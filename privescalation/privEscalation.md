# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

   ```
       http://localhost:3000/send-form
   ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   Lack of Authentication: The server trusts the userId provided in the request body without verifying the identity of the requester.

   Improper Authorization Logic: It checks if the user associated with the input userId is an admin, rather than checking if the actual requester is an admin.

   Privilege Escalation Risk: Any user can submit a request claiming to be an admin and change roles, bypassing proper access control.

2. Briefly explain how a malicious attacker can exploit them.
   A non-admin user can craft a POST request like:
   {
   "userId": 1,
   "newRole": "admin"
   }
   Since insecure.ts checks the role of the userId in the request and not the actual requester, the attacker impersonates an admin (e.g., userId = 1), and the server allows them to update any user’s role.

   This leads to unauthorized elevation of privileges, letting attackers become admins or promote others.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
   Session-Based Authentication: Tracks the logged-in user using req.session.userId, preventing users from faking identities via input.

   Proper Authorization Check: Verifies the role of the authenticated user (from session) rather than trusting data from the request body.

   Role-Based Access Control: Only allows users with the admin role to perform role updates, ensuring access is restricted to authorized personnel.
