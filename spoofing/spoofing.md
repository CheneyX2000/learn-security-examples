# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

   `$ npx install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

   `npx ts-node mal.ts`

4. Open **http://localhost:8000** in a browser, type a name and Submit.

5. Open the **Application** tab in the Browser's inspect pane. Find the **Cookies** under **Storage**. You should see a **connect.sid** cookie being set.

6. Open the HTML file **mal-steal-cookie.html** file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file **mal-csrf.html** file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
   No Real Authentication: In insecure.ts, users can set their session user simply by submitting a form (e.g., /register with name="Admin"). There is no username/password check or identity verification.

   Impersonation: Because the app decides that anyone who sets req.session.user = "Admin" is effectively the admin, a malicious user can impersonate admin privileges by just declaring themselves as "Admin".

2. Briefly explain different ways in which vulnerability can be exploited.
   Crafted Form Submission: An attacker can fill out the /register form with "Admin" as the username. The server sets req.session.user = "Admin", allowing them to pass authorization checks.

   Direct HTTP Request: An attacker can skip the form and send a direct POST request to /register with name=Admin. Since there’s no validation against a real user database, they’re instantly “logged in” as Admin.

   Session Hijacking (If Not Properly Secured): If cookies are not protected (httpOnly, sameSite, etc.), an attacker could steal or manipulate session cookies and alter the user identity without detection.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
   Stricter Session Configuration: In secure.ts, session cookies are set with httpOnly: true and sameSite: true, which helps prevent client-side script access and Cross-Site Request Forgery attacks.

   Controlled User Identity: In a truly secure version, the server would validate a user’s credentials (e.g., username/password) against a trusted store before assigning req.session.user. Simply put, the user identity is not determined by arbitrary input.

   Real Authentication & Authorization Logic: Instead of allowing a user to set their own role or username in an unverified way, the secure approach ensures that user roles (like “Admin”) are assigned by the server after a successful login or token validation.
