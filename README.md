# Stateless vs stateful research

### Cookie based authentication is stateful.
 This means that an authentication record or session must be kept both server and client-side. The server needs to keep track of active sessions in a database, while on the front-end a cookie is created that holds a session identifier, thus the name cookie based authentication. Let's look at the flow of traditional cookie based authentication:

- User enters their login credentials
- Server verifies the credentials are correct and creates a session which is then stored in a database
- A cookie with the session ID is placed in the users browser
- On subsequent requests, the session ID is verified against the database and if valid the request processed
- Once a user logs out of the app, the session is destroyed both client and server side

![pic](https://cdn.scotch.io/scotchy-uploads/2014/11/tokens-traditional.png)

#### A few major problems arose with this method of authentication.

**Sessions**: Every time a user is authenticated, the server will need to create a record somewhere on our server. This is usually done in memory and when there are many users authenticating, the overhead on your server increases.

**Scalability**: Since sessions are stored in memory, this provides problems with scalability. As our cloud providers start replicating servers to handle application load, having vital information in session memory will limit our ability to scale.

**CORS**: As we want to expand our application to let our data be used across multiple mobile devices, we have to worry about cross-origin resource sharing (CORS). When using AJAX calls to grab resources from another domain (mobile to our API server), we could run into problems with forbidden requests.
