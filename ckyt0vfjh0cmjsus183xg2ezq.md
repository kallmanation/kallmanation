## Immutable ActiveRecord

I just merged something at [my job](https://root.engineering) that I'm proud of. I was inspired by a [post on dev.to](https://dev.to/rodreegez/immutable-activerecord-models-11dp), but I'm especially happy with mine as I think I've covered a few holes Adam Rogers left in their example as well as added some needed niceties for my use-case. Ready? Let's get stuck into it:

# Why?

If you already know why you want an immutable model; then go ahead to the next sections for the goodies. This section is for the rest of you.

Immutability is a bit of a buzzword around tech these days. All it means is unchangingness: once something exists it cannot be altered; a tattoo or stone tablet if you will. Only new things can be added, old things cannot be changed.

Without immutability our records can be something of a Schrödinger's cat; always either the last state we observed it in or some other random state (and we won't know until we observe it again). It also means there is no record of history; we have no clue how we arrived at the current state.

Root, being an insurance company, needs to regularly prove, to intense detail, how it arrived at the current prices it gives to customers, how it handled claims and payouts on policies, and really every detail of the business.

Have you tried _proving_ that your history record is accurate when anyone or anything can change it? "It's Immutable" becomes your favorite word when an auditor starts asking questions.

# A Word on Adam's Approach

_You can read their article [here](https://dev.to/rodreegez/immutable-activerecord-models-11dp)._

If you are familiar with Rails you'll probably notice a few holes in that article's example (which is fine for exemplary purposes, but not for me in production): 
1. It doesn't protect against `.destroy` 'ing records
2. It doesn't protect against updating with `.save`

Now, it's easy enough to add the needed `before_save` and `before_destroy` hooks and prevent those too. But the other word I have for why I took my approach is that [Root's](https://root.engineering) codebase generally shuns ActiveRecord's hooks (because of the [side-effects and coupling problems](https://dev.to/mickeytgl/the-good-and-bad-of-activerecord-callbacks-p4a)) and my solution is much simpler to implement and test (I don't even need to `extend ActiveSupport::Concern` !)

# Readonly?

Yes, `.readonly?`. Which if you didn't know is an ActiveRecord method on all models that by default returns `false` and returns `true` if you've previously called `.readonly!` on the object.

But what makes this especially interesting for us is that ActiveRecord checks `.readonly?` before any `.create`, `.update`, `.save`, or `.destroy` call and raises an error (`ActiveRecord::ReadOnlyRecord`) if `.readonly?` returns `true`. ActiveRecord also handily provides a `.new_record?` method to clue us in on if this record has been saved to the database yet. So maybe all we need to do is write our `.readonly?`

```ruby
module ImmutableRecord
  def readonly?
    !new_record?
  end
end
```

Done! ...well not exactly. Remember what I said about `.readonly!` being able to mark any object as... well readonly? We've kind of broken that functionality by so naively overriding `.readonly?`. That's not very nice, so let's restore it to default behavior:

```ruby
module ImmutableRecord
  def readonly?
    return true unless new_record?
    super
  end
end
```

And that finishes our use of `.readonly?` (mostly). But I wasn't done. I still had a few changes in mind.

# Allow Mutation!

It was great that I could now have immutable records; but sometimes I still need to update them (ever added a column to a table and needed to set the value for existing records?).

Escape hatch coming up! But we don't want too broad of an escape hatch (opening doors for accidental abuse and mutation). I chose to utilize blocks so the window for mutation has a distinct, language enforced, start and end.

```ruby
module ImmutableRecord
  def allow_mutation!
    current_mutation_allowed = @mutation_allowed
    begin
      @mutation_allowed = true
      result = yield self
    ensure
      @mutation_allowed = current_mutation_allowed
    end
    result
  end

  def readonly?
    return true if !new_record? && !@mutation_allowed
    super
  end
end
```

Stash the current value of a new attribute (`current_mutation_allowed = @mutation_allowed`), and use an `ensure` block to ensure, even if the given block raises an error, the attribute is set back to its previous state. Two more niceties: return the result of the given block and yield `self` (aka the current model) for easier chaining. Wrap up the changes by making `.readonly?` account for our override attribute and we have a little escape hatch! Usage looks something like `immutable_model.allow_mutation! { |m| m.update!(...) }`.

# Immutable?

I had one last interesting use case I needed. On a particular model, it is usually safe to do any updating or even deletion; but in a particular scenario, we risk serious data loss if the record is touched at all[*](#asterisk). I needed one more tool to allow the model to switch on and off immutability.

So I came up with `.immutable?`. By default this always returns `true`, but the model is free to override the definition when it needs to turn off immutability based on current state.

```ruby
module ImmutableRecord
  def allow_mutation!
    current_mutation_allowed = @mutation_allowed
    begin
      @mutation_allowed = true
      result = yield
    ensure
      @mutation_allowed = current_mutation_allowed
    end
    result
  end

  def immutable?
    true
  end

  def readonly?
    return true if !new_record? && immutable? && !@mutation_allowed
    super
  end
end
```

Of course I could have skipped `.immutable?` and had my model implement `.readonly?` for itself. But that means re-implementing the knowledge of the interaction between `super,` `new_record?`, and `@mutation_allowed`. Adding `.immutable?` lets the model focus more tightly on the things it actually concerns itself with and leaves the rest to this concern (I guess that's why ActiveRecord calls these `concern` 's).

# Remaining Gaps

I mentioned earlier that ActiveRecord checks `.readonly?` during `.create`, `.update`, `.save`, and `.destroy`; essentially all the methods that also call other lifecycle hooks like validations and `before_` / `after_`.

What ActiveRecord does not do is check `.readonly?` before methods like `.delete` or `.update_columns`. Meaning there are still easy ways to mutate our "immutable" records. For my purposes these holes were acceptable and these methods should be expected to bypass lifecycle checks as that is their purpose.

If you'd rather not have these bypasses to immutability, then you'll need to override each method in this concern to first check `.readonly?` (and raise an error if it is) and then call `super`.

---

<a id="asterisk">*</a> So you are curious about this strange model that is sometimes immutable sometimes not? Well alright, I'll tell you:

The gist of it is backfilling. There was an older model that we just updated on certain events. That turned out to not be working well. So I created a new model wherein we simply take the full information we needed from the models triggering those events and idempotently generate the full history in the format we needed for tracking; only then comparing to what was already stored in the database and making the minimal creations/updates to store the full history.

Since that's an idempotent process and the source of truth comes from other tables; we are usually free to update and even delete these new records however we please.

The problem came when I needed to backfill the information from the old table into this new table. I needed to keep the historical records exactly the same (and I mean **_exactly_**). So I simply one-for-one copied data from the old table to the new, and marked those records with a column as being backfilled. While it is normally safe to do any mutation against this table; these backfilled records really should not be mutated (because they came from the old table; not the normal process).

To add a guardrail against us accidentally mutating those backfilled records, I used this `ImmutableRecord` concern with the `.immutable?` method overridden to simply return `.backfilled?`.