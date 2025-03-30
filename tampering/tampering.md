# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

   `npm install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

   ```
       <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
   ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   Session Tampering: The script lets users set req.session.user purely from their input (e.g., by entering "Admin" as their name). There is no authentication or server-side validation to confirm their real identity or role, allowing malicious users to escalate privileges.

   Possible XSS (Cross-Site Scripting): Since user input is directly inserted into the session and potentially rendered back without proper sanitization, attackers could inject HTML/JavaScript, leading to client-side attacks.

2. Briefly explain how a malicious attacker can exploit them.
   Privilege Escalation: A user can submit name="Admin" in the registration form and thereby set req.session.user = "Admin", bypassing checks on the /sensitive route.

   Injecting Malicious Scripts: Without sanitization, an attacker could enter a script tag (e.g., <script>alert('XSS')</script>) as their name, causing the browser to execute unauthorized code if that input were ever re-displayed in an HTML context.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
   Input Sanitization:In secure.ts, the function escapeHTML() is used to sanitize user inputs. This prevents XSS attacks by escaping special characters (<, >, &, etc.), so malicious HTML/JavaScript cannot be injected.

   Stricter Session Configuration:The cookie settings include httpOnly: true and sameSite: 'strict', which help guard against common session-based attacks such as XSS-based session theft and CSRF.

   Reduced Tampering Risk:Because input is sanitized, attackers cannot inject malicious HTML/JS into the session. While secure.ts does still allow any user to call themselves "Admin", it addresses tampering with HTML/JS input. A full fix would also include server-side role verification (i.e., an actual login system), but secure.ts does remove the direct script-injection (XSS) vector and makes session cookies harder to steal or misuse.
