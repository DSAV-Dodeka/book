# Frontend vs Backend

What is a "frontend", what is a "backend", why do we have this separation?

First, what is the difference? There is no perfect definition, but in general the "frontend" is the part that is exposed to the end user, so what they actually see and interact with. So the "frontend" is about the _user_, it's what actually runs in the browser. 

The "backend" is the part that cannot run in the browser, for example because it needs to have dynamic access to the database. It runs on a server, away from the user. It's job is to store things that need to be secure not visible to everyone, like passwords or personal information. For that, it needs a database.

To use the backend, it exposes a so-called "API", which is basically a list of functions which can be called from the internet. In particularly, it mostly adheres to the principles of a RESTful API (there are many resources on the internet about it).

In general, when the frontend wants data, it sends a JSON request (a specific format used for structuring data) ... #TODO

There are two main reasons. The first, more technical one, is that by separating the frontend from the backend, you can develop them separately. So someone who wants to update how something looks, doesn't have to worry about any logic that should happen on the server. This allows teams to work more independently. It's also a "separation of concerns", which is ensures that there's not too much tangling of functionality. It also allows fully replacing one of the two components without worrying about the other. This might be useful in the future.

However, the more important ... #TODO