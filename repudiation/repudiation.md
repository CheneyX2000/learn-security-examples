# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Run the server **insecure.ts**.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
   The primary vulnerability is repudiation: there is no reliable way to prove which user performed a given action. Because the original (insecure) script does not authenticate users or log their actions, anyone can impersonate someone else (e.g., by forging the user field) and later deny having sent a particular message.

2. Briefly explain why the vulnerability is addressed in **secure.ts**.
   Audit Logging is introduced, so every request and error is written to a log file with timestamps, IP addresses, and user information.

   Authentication Checks (though simulated) ensure only authenticated users can send and retrieve messages.

   Error Handling writes errors to the log as well, creating a traceable record of all interactions.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
   A series of middleware functions form a chain (e.g., the logging middleware, the authentication check, and so on).

   Each middleware can either handle the request/response (e.g., log it, authenticate it) or pass the request to the next function in the chain.

   By placing the logging middleware early, all incoming requests are captured and logged before moving forward, thus creating an audit trail that mitigates repudiation.
