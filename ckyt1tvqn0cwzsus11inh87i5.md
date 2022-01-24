## My Mental Model of a Server

Something I struggled with when I was beginning web development and I see many new devs struggle with as well: What is a "Server" actually capable of?

I've already given it away with the cover photo; but why not look at it top-to-bottom instead of left-to-right 😅 (and I will talk about each part in more detail)

# Simplest Mental Model of a Server

![Diagram of a Simple Server: inputs are a Request and Storage; outputs are a Response and updated Storage](https://cdn.hashnode.com/res/hashnode/image/upload/v1643050469850/-4cQsqMjj.png)

---

**A server is this: software that starts upon request, runs using the information in the request and data from longer term storage without further input, completing execution by updating the stored data and returning a response.**

---

This model eradicates three main misconceptions:
1. No processing or execution _after_ the response
2. No processing or execution _before_ the request
3. No interrupting or user interaction _during_ the process

The above three things can _only_ be achieved using the "storage" aspect in the diagram to coordinate _something else_ to achieve it. The better we understand that _something else_ is a **completely separate process**, the better prepared we are to handle the hazards in those patterns. The _only_ way to do any of the above three things is with "storage".

## What is "Storage"?

I'm using this as a stand-in for any non-volatile data store.

Most commonly a relational database. But it may also be a document database, key-value store, even an in-memory store like [Redis](https://redis.io)* or just files on the local hard drive. In large or needy applications, more than one may be used, but we can mentally lump them all together into "storage".

This current state gets put in the top and updates come out the other side (if anything needs to update).

_* Memory stores are usually considered "volatile" in comparison to hard drives, but to the length of an average request-response, a store like Redis comparably keeps data for eons._

## The Request

The first big half of the "request-response lifecycle". Nothing, I repeat _nothing_, happens in HTTP without a request to start it.

A request can have all sorts of information, but the highlights are:
1. Verb - aka `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, etc. each has a meaning; please use them appropriately
2. Host - like [`www.buymeacoffee.com`](buymeacoff.ee/kallmanation)
3. Path - the slashy bits, like [`/kallmanation`](buymeacoff.ee/kallmanation); but also contains the raw query parameters like [`?via=kallmanation`](buymeacoff.ee/?via=kallmanation)
4. Various Headers - all sorts of meta-data about the request, mostly look these up as needed; but one is worth mentioning here:
5. Cookies - the thing we always have to "accept"; just a bunch of little pieces of text the requester keeps track of for the server
6. Body - not on all requests; usually JSON, but can be any text for the server to use

Without data in either the storage or somewhere in this request (mainly in cookies), the server can not know about it and in its opinion that information _does not exist_ and has never existed.

## The Response

The end of the "request-response lifecycle". Truly, _The End_, Finito. Nothing happens in HTTP after the response has been returned.

Like requests, responses have all sorts of information, but the big parts are:
1. Status - [200](https://http.cat/200) for OK, [401](https://http.cat/401) for unauthorized, [418](https://http.cat/418) I'm a teapot, etc.; please use the appropriate status
2. Headers - again all sorts of meta-data, look up as needed
3. Cookies - a specific header can tell the requester to send the given cookie with any future requests (they can also be set by JavaScript)
4. Body - HTML/CSS/JS/JSON/XML/etc./etc. whatever we want to send back

# Nuanced Mental Model of a Server

To be slightly more pedantic and technical: an application server does not exactly have the entire database stuffed into it on each request. Instead some pre-processing needs to use the request information to formulate a query to storage to load only the needed data.

![Diagram of a Server: input Request goes to the main process and a side process; the side process queries storage and passes data into the main process; the main process outputs a Response and updated Storage](https://cdn.hashnode.com/res/hashnode/image/upload/v1643050471508/fgBpnPxf4.png)

This "Query" might be a regular old SQL query. But it might be just finding the right file on disk by name and path. Or it could be a request to a separate web service managing storage (like in a Database-as-a-Service). Once the data is fetched, the rest of the server continues as it did in the simpler mental model.

To get even more technical; I've never seen a server application written in such distinct steps of fetch all data, process data, update data, return response. The fetching, processing, and updating usually muddles all together (sometimes rightly, oftentimes wrongly). But still, from an external perspective, a requester could not perceive a real difference between that technicality and the more controlled mental model diagrammed above. So I believe it to be a very useful model of an application server.