# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?id[$ne]=
   ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
   No Input Validation for \_id: The script directly uses req.query.id in the database query (User.findOne({ \_id: uid })) without checking whether it is a valid MongoDB ObjectId. This can lead to malformed or expensive queries that overwhelm the database or block the Node.js event loop.

   Unlimited Requests: The insecure script does not implement any rate limiting, meaning an attacker can send large numbers of requests in rapid succession to overload the server.

2. Briefly explain how a malicious attacker can exploit them.
   Malicious Payloads: By sending invalid or excessively large \_id values (e.g., deeply nested objects or specially crafted strings), the attacker can force MongoDB or Mongoose to spend excessive time parsing and handling the query.

   Request Flooding: Without rate limiting, an attacker can flood the endpoint with hundreds or thousands of requests per second, potentially consuming all available server or database resources, thereby causing a Denial of Service.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
   Rate Limiting: Uses express-rate-limit to limit the number of incoming requests from a single IP address in a specific time window, preventing automated request floods.

   Input Validation: Ensures that the user-provided id is a valid ObjectId before querying the database, thus rejecting malformed input and reducing the risk of database overload or parsing errors.

   Error Handling: Proper try-catch blocks and appropriate HTTP status codes help mitigate the potential for unexpected behavior and communicate errors more effectively, further hardening against DoS scenarios.
