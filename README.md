# Stateless vs stateful research

### Cookie based authentication is stateful.
 This means that an authentication record or session must be kept both server and client-side. The server needs to keep track of active sessions in a database, while on the front-end a cookie is created that holds a session identifier, thus the name cookie based authentication. Let's look at the flow of traditional cookie based authentication:

- User enters their login credentials
- Server verifies the credentials are correct and creates a session which is then stored in a database
- A cookie with the session ID is placed in the users browser
- On subsequent requests, the session ID is verified against the database and if valid the request processed
- Once a user logs out of the app, the session is destroyed both client and server side

![pic](http://robmclarty.com/system/pictures/sources/65/flow-cookie-session_large.jpg?1450223782)

#### A few major problems arose with this method of authentication.

**Extensive memory usage**: Every time a user is authenticated, the server will need to create a record somewhere on our server. This is usually done in memory and when there are many users authenticating, the overhead on your server increases.

**Bad Scalability**: Since sessions are stored in memory, this provides problems with scalability. As our cloud providers start replicating servers to handle application load, having vital information in session memory will limit our ability to scale.

**CORS**: As we want to expand our application to let our data be used across multiple mobile devices, we have to worry about cross-origin resource sharing (CORS). When using AJAX calls to grab resources from another domain (mobile to our API server), we could run into problems with forbidden requests.


### WHAT IS TOKEN BASED AUTHENTICATION ?

- session Based auth: when the user log in, the server creats the sessionand send the id for this user sessionto thebrowser. After that, any request from the user will take with it the session id for that user.
Baesd on the id, the server retrieve the data from the database.

**Stateful Diagram**
![](http://robmclarty.com/system/pictures/sources/64/flow-basic_large.jpg?1450223652)

- Token Based: the server create the token for the user and send it back to the browser and then each request will be attached with the token id and the server will use it to retrieve the data.

**Stateless Diagram**
![](http://hamdiceylan.com/wp-content/uploads/2015/12/accessToken.png)


### Stateless advantages:
1. **Reduces memory usage** Image if google stored session information about every one of their users.

2. **Easier to support server farms** If you need session data and you have more than 1 server, you need a way to sync that session data across servers. Normally this is done using a database.

3. **Reduce session expiration problems** Sometimes expiring sessions cause issues that are hard to find and test for. Sessionless applications don't suffer from these.

4. **Url linkability** Some sites store the ID of what the user is looking at in the sessions. This makes it impossible for users to simply copy and paste the URL or send it to friends.

### Stateless disadvantages:
1. **Compromised Secret Key :** The best and the worst thing about JWT is that it relies on just one Key. Consider that the Key is leaked by a careless or a rogue developer/administrator, the whole system is compromised!

2. **Cannot manage client from the server:** We had several cases where we wanted the users at HelpTap to logout by cleaning up the cookies, but we cannot ask them to do so every time.
As well consider the case that a user’s mobile is stolen, and he wants to logout of all existing sessions(e.g. Gmail’s logout other sessions feature). Well its not possible in case of JWT.

3. **Cannot push Messages to clients** (Identifying clients from server) : As we have no record about the logged-in clients on the DB end, we cannot push messages to all the clients.

4. **Crypto-algo can be deprecated:** JWT relies completely on the Signing algorithm. Now, though it is not frequent, but in the past many Encryption/Signing algorithms have been deprecated.

5. **Data Overhead :** The size of the JWT token will be more than that of a normal Session token. The more data you add in the JWT token, the longer it gets linearly.

6. **Complicated to understand:** JWT uses cryptographic Signature algorithms to verify the data and get the user-id from the token. Understanding the Signing Algo in itself requires basics of cryptography. So, in case if the developer is not completely educated s/he might introduce security loopholes in the system.
