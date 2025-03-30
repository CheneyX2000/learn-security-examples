# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?username[$ne]=
   ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   Information Disclosure: The code sends back the entire user object, including sensitive fields such as the password and internal database metadata, exposing private information.

   NoSQL Injection: There is no input validation or sanitization for the username parameter. An attacker could inject malicious queries (e.g., special characters or MongoDB operators) to manipulate the database query.

2. Briefly explain how a malicious attacker can exploit them.
   Harvesting Sensitive Data: By requesting GET /userinfo?username=someUser, the server returns the user’s password and other internal fields. Attackers can collect these for credential stuffing or other exploits.

   Crafting Malicious Queries: Attackers can insert special characters or operators (e.g., $ne, $gt) in the username parameter to alter the database query logic and potentially gain unauthorized access or enumerate users.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
   Input Validation: It verifies that username is a string and sanitizes it by removing non-alphanumeric characters, greatly reducing the risk of NoSQL injection.

   Safer Querying: The sanitized username is used to query the database, preventing malicious payloads from altering the query in unexpected ways。
