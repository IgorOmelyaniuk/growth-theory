## MITM attacks:

Man-in-the-middle attacks come in two forms, one that involves physical proximity to the intended target, and another that involves malicious software, or malware. This second form, like fake bank website, is also called a man-in-the-browser attack
Cybercriminals typically execute a man-in-the-middle attack in two phases — interception and decryption.

With a traditional MITM attack, the cybercriminal needs to gain access to an unsecured or poorly secured Wi-Fi router. These types of connections are generally found in public areas with free Wi-Fi hotspots, and even in some people’s homes, if they haven’t protected their network. Attackers can scan the router looking for specific vulnerabilities such as a weak password. A successful man-in-the-middle attack does not stop at interception. The victim’s encrypted data must then be unencrypted, so that the attacker can read and act upon it.

With a man-in-the-browser attack (MITB), an attacker needs a way to inject malicious software, or malware, into the victim’s computer or mobile device. One of the ways this can be achieved is by phishing.

Phishing is when a fraudster sends an email or text message to a user that appears to originate from trusted source, such as a bank, as in our original example. By clicking on a link or opening an attachment in the phishing message, the user can unwittingly load malware onto their device.

The malware then installs itself on the browser without the user’s knowledge. The malware records the data sent between the victim and specific targeted websites, such as financial institutions, and transmits it to the attacker.

7 types of man-in-the-middle attacks:
- IP spoofing

Every device capable of connecting to the internet has an internet protocol (IP) address, which is similar to the street address for your home. By spoofing an IP address, an attacker can trick you into thinking you’re interacting with a website or someone you’re not, perhaps giving the attacker access to information you’d otherwise not share.

- DNS spoofing

Domain Name Server, or DNS, spoofing is a technique that forces a user to a fake website rather than the real one the user intends to visit. If you are a victim of DNS spoofing, you may think you’re visiting a safe, trusted website when you’re actually interacting with a fraudster. The perpetrator’s goal is to divert traffic from the real site or capture user login credentials.

- HTTPS spoofing

When doing business on the internet, seeing “HTTPS” in the URL, rather than “HTTP” is a sign that the website is secure and can be trusted. In fact, the “S” stands for “secure.” An attacker can fool your browser into believing it’s visiting a trusted website when it’s not. By redirecting your browser to an unsecure website, the attacker can monitor your interactions with that website and possibly steal personal information you’re sharing.

- SSL hijacking

When your device connects to an unsecure server — indicated by “HTTP” — the server can often automatically redirect you to the secure version of the server, indicated by “HTTPS.” A connection to a secure server means standard security protocols are in place, protecting the data you share with that server. SSL stands for Secure Sockets Layer, a protocol that establishes encrypted links between your browser and the web server.

In an SSL hijacking, the attacker uses another computer and secure server and intercepts all the information passing between the server and the user’s computer.

- Email hijacking

Cybercriminals sometimes target email accounts of banks and other financial institutions. Once they gain access, they can monitor transactions between the institution and its customers. The attackers can then spoof the bank’s email address and send their own instructions to customers. This convinces the customer to follow the attackers’ instructions rather than the bank’s. As a result, an unwitting customer may end up putting money in the attackers’ hands.

- Wi-Fi eavesdropping

Cybercriminals can set up Wi-Fi connections with very legitimate sounding names, similar to a nearby business. Once a user connects to the fraudster’s Wi-Fi, the attacker will be able to monitor the user’s online activity and be able to intercept login credentials, payment card information, and more. This is just one of several risks associated with using public Wi-Fi. You can learn more about such risks here.

- Stealing browser cookies

To understand the risk of stolen browser cookies, you need to understand what one is. A browser cookie is a small piece of information a website stores on your computer.

For example, an online retailer might store the personal information you enter and shopping cart items you’ve selected on a cookie so you don’t have to re-enter that information when you return.

A cybercriminal can hijack these browser cookies. Since cookies store information from your browsing session, attackers can gain access to your passwords, address, and other sensitive information.

How to help protect against a man-in-the-middle attack:

- Make sure "HTTPS" — with the S — is always in the URL bar of the websites you visit.
- Be wary of potential phishing emails from attackers asking you to update your password or any other login credentials. Instead of clicking on the link provided in the email, manually type the website address into your browser.
- Never connect to public Wi-Fi routers directly, if possible. A VPN encrypts your internet connection on public hotspots to protect the private data you send and receive while using public Wi-Fi, like passwords or credit card information.
- Since MITB attacks primarily use malware for execution, you should install a comprehensive internet security solution on your computer. Always keep the security software up to date.
- Be sure that your home Wi-Fi network is secure. Update all of the default usernames and passwords on your home router and all connected devices to strong, unique passwords. 

## CORS:

The Cross-Origin Resource Sharing standard works by adding new HTTP headers that let servers describe which origins are permitted to read that information from a web browser. Additionally, for HTTP request methods that can cause side-effects on server data, the specification mandates that browsers "preflight" the request, soliciting supported methods from the server with the HTTP OPTIONS request method, and then, upon "approval" from the server, sending the actual request. Servers can also inform clients whether "credentials" (such as Cookies and HTTP Authentication) should be sent with requests.
CORS failures result in errors, but for security reasons, specifics about the error are not available to JavaScript. All the code knows is that an error occurred. The only way to determine what specifically went wrong is to look at the browser's console for details.

Some requests don’t trigger a CORS preflight. Those are called “simple requests”. 

Simple requests:

- One of the allowed methods: GET, POST, HEAD
- The only allowed values for the Content-Type header are: application/x-www-form-urlencoded, multipart/form-data, text/plain
- the only headers which are allowed to be manually set are those which the Fetch spec defines as a “CORS-safelisted request-header”, which are: Accept, Accept-Language, Content-Language, Content-Type
- No ReadableStream object is used in the request.
- No event listeners are registered on any XMLHttpRequest.upload object used in the request; these are accessed using the XMLHttpRequest.upload property.

The pattern of the Origin and Access-Control-Allow-Origin headers is the simplest use of the access control protocol.

Preflighted requests:

Unlike “simple requests” (discussed above), for "preflighted" requests the browser first sends an HTTP request using the OPTIONS method to the resource on the other origin, in order to determine if the actual request is safe to send. Cross-site requests are preflighted like this since they may have implications to user data.

The HTTP response headers:
- Access-Control-Allow-Origin: <origin> | *
- Access-Control-Expose-Headers
- Access-Control-Max-Age: <delta-seconds> - indicates how long the results of a preflight request can be cached. For an example of a preflight request
- Access-Control-Allow-Credentials: true -  indicates whether or not the response to the request can be exposed when the credentials flag is true
- Access-Control-Allow-Methods: <method>[, <method>]*
- Access-Control-Allow-Headers - indicate which HTTP headers can be used when making the actual request

The HTTP request headers:
- Origin: <origin> - indicates the origin of the cross-site access request or preflight request
- Access-Control-Request-Method: <method> - used when issuing a preflight request to let the server know what HTTP method will be used when the actual request is made.
- Access-Control-Request-Headers - used when issuing a preflight request to let the server know what HTTP headers will be used when the actual request is made

## CSP:

Content Security Policy (CSP, политика защиты контента) — это механизм обеспечения безопасности, с помощью которого можно защищаться от атак с внедрением контента, например, межсайтового скриптинга (XSS). CSP описывает безопасные источники загрузки ресурсов, устанавливает правила использования встроенных стилей, скриптов, а также динамической оценки JavaScript — например, с помощью eval. Загрузка с ресурсов, не входящих в «белый список», блокируется. Для использования политики страница должна содержать HTTP-заголовок Content-Security-Policy с одной и более директивами, которые представляют собой «белые списки». Простейший пример политики, разрешающей загрузку ресурсов только указанного домена: Content-Security-Policy: default-src 'self'. На данный момент в перечне URL нельзя прописывать пути, можно лишь перечислять сами домены и поддомены: Content-Security-Policy: default-src 'self' trusted.com *.trusted.com

## OWASP Top 10:

### Injection:

A code injection happens when an attacker sends invalid data to the web application with the intention to make it do something that the application was not designed/programmed to do. Perhaps the most common example around this security vulnerability is the SQL query consuming untrusted data. Anything that accepts parameters as input can potentially be vulnerable to a code injection attack.

Preventing SQL injections requires keeping data separate from commands and queries:
  - The preferred option is to use a safe API, which avoids the use of the interpreter entirely or provides a parameterized interface or migrate to use Object    Relational Mapping Tools (ORMs). Note: Even when parameterized, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec().
  - Use positive or “allowlist” server-side input validation. This is not a complete defense as many applications require special characters, such as text areas or APIs for mobile applications.
  - For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter. Note: SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.
  - Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection.

From these recommendations you can abstract two things:
  - Separation of data from the web application logic.
  - Implement settings and/or restrictions to limit data exposure in case of successful injection attacks.

### Broken Authentication:

A broken authentication vulnerability can allow an attacker to use manual and/or automatic methods to try to gain control over any account they want in a system – or even worse – to gain complete control over the system. Broken authentication usually refers to logic issues that occur on the application authentication’s mechanism, like bad session management prone to username enumeration. To minimize broken authentication risks avoid leaving the login page for admins publicly accessible to all visitors of the website.

 A web application contains a broken authentication vulnerability if it:
   - Permits automated attacks such as credential stuffing, where the attacker has a list of valid usernames and passwords.
   - Permits brute force or other automated attacks.
   - Permits default, weak, or well-known passwords
   - Uses weak or ineffective credential recovery and forgot-password processes, such as “knowledge-based answers,” which cannot be made safe.
   - Uses plain text, encrypted, or weakly hashed passwords.
   - Has missing or ineffective multi-factor authentication.
   - Exposes session IDs in the URL
   - Does not rotate session IDs after successful login.
   - Does not properly invalidate session IDs. User sessions or authentication tokens (particularly single sign-on (SSO) tokens) aren’t properly invalidated during logout or a period of inactivity.

Preventing:
   - Where possible, implement multi-factor authentication to prevent automated, credential stuffing, brute force, and stolen credential reuse attacks.
   - Do not ship or deploy with any default credentials, particularly for admin users.
   - Implement weak-password checks, such as testing new or changed passwords against a list of the top 10,000 worst passwords.
   - Align password length, complexity and rotation policies with NIST 800-63 B’s guidelines in section 5.1.1 for Memorized Secrets or other modern, evidence-based password policies.
   - Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes.
   - Limit or increasingly delay failed login attempts. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected.
   - Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL. Ids should also be securely stored and invalidated after logout, idle, and absolute timeouts.

### Sensitive Data Exposure:

Sensitive data exposure is one of the most widespread vulnerabilities on the OWASP list. It consists of compromising data that should have been protected.

Some sensitive data that requires protection is:
  - Credentials
  - Credit card numbers
  - Social Security Numbers
  - Medical information
  - Personally identifiable information (PII)
  - Other personal information

Responsible sensitive data collection and handling have become more noticeable especially after the advent of the General Data Protection Regulation (GDPR). This is a new data privacy law that came into effect May 2018. It mandates how companies collect, modify, process, store, and delete personal data originating in the European Union for both residents and visitors.

There are two types of data:
   - Stored data – data at rest
   - Transmitted data – data that is transmitted internally between servers, or to web browsers

Some of the ways to prevent data exposure:
   - Classify data processed, stored, or transmitted by an application.
   - Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.
   - Apply controls as per the classification.
   - Don’t store sensitive data unnecessarily.
   - Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data that is not retained cannot be stolen.
   - Make sure to encrypt all sensitive data at rest.
   - Ensure up-to-date and strong standard algorithms, protocols, and keys are in place; use proper key management.
   - Encrypt all data in transit with secure protocols such as TLS with perfect forward secrecy (PFS) ciphers, cipher prioritization by the server, and secure parameters.
   - Enforce encryption using directives like HTTP Strict Transport Security
   - Disable caching for responses that contain sensitive data.
   - Store passwords using strong adaptive and salted hashing functions with a work factor (delay factor), such as Argon2, scrypt, bcrypt, or PBKDF2.
   - Verify independently the effectiveness of configuration and settings.

### XML External Entities (XXE):

XML External Entity attack is a type of attack against an application that parses XML input. This attack occurs when XML input containing a reference to an external entity is processed by a weakly configured XML parser.

Preventing:
- Whenever possible, use less complex data formats ,such as JSON, and avoid serialization of sensitive data.
- Patch or upgrade all XML processors and libraries in use by the application or on the underlying operating system.
- Use dependency checkers
- Implement positive server-side input validation, filtering, or sanitization to prevent hostile data within XML documents, headers, or nodes.

### Broken Access Control:

In website security, access control means putting a limit on what sections or pages visitors can reach, depending on their needs.
For example, if you own an ecommerce store, you probably need access to the admin panel in order to add new products or to set up a promotion for the upcoming holidays. However, hardly anybody else would need it. Allowing the rest of your website’s visitors to reach your login page only opens up your ecommerce store to attacks. And that’s the problem with almost all major content management systems (CMS) these days. By default, they give worldwide access to the admin login page. Most of them also won’t force you to establish a two-factor authentication method (2FA).

Examples of Broken Access Control:
- Access to a hosting control / administrative panel
- Access to a server via FTP / SFTP / SSH
- Access to a website’s administrative panel
- Access to other applications on your server
- Access to a database

Reducing the Risks of Broken Access Control:
- Employ least privileged concepts – apply a role appropriate to the task and only for the amount of time necessary to complete said task and no more.
- Get rid of accounts you don’t need or whose user no longer requires it.
- Audit your servers and websites – who is doing what, when, and why.
- If possible, apply multi-factor authentication to all your access points.
- Disable access points until they are needed in order to reduce your access windows.
- Remove unnecessary services off your server.
- Check applications that are externally accessible versus applications that are tied to your network.

To prevent broken access control:
- With the exception of public resources, deny by default.
- Implement access control mechanisms once and reuse them throughout the application, including minimizing CORS usage.
- Log access control failures, alert admins when appropriate
- Rate limit API and controller access to minimize the harm from automated attack tooling.
- JWT tokens should be invalidated on the server after logout.

### Security Misconfigurations:

The most common:
- Unpatched flaws
- Default configurations
- Unused pages
- Unprotected files and directories
- Unnecessary services

In order to prevent security misconfigurations:
- A repeatable hardening process that makes it fast and easy to deploy another environment that is properly locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. Automate this process in order to minimize the effort required to set up a new secure environment.
- A minimal platform without any unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.
- A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process. In particular, review cloud storage permissions.
- A segmented application architecture that provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups.
- Sending security directives to clients, e.g. Security Headers.

### Cross-Site Scripting (XSS):

XSS is a widespread vulnerability that affects many web applications. XSS attacks consist of injecting malicious client-side scripts into a website and using the website as a propagation method. The risks behind XSS is that it allows an attacker to inject content into a website and modify how it is displayed, forcing a victim’s browser to execute the code provided by the attacker while loading the page.

Types of XSS:
- Reflected XSS: The application or API includes unvalidated and unescaped user input as part of HTML output. A successful attack can allow the attacker to execute arbitrary HTML and JavaScript in the victim’s browser. Typically the user will need to interact with some malicious link that points to an attacker-controlled page, such as malicious watering hole websites, advertisements, or similar.
- Stored XSS: The application or API stores unsanitized user input that is viewed at a later time by another user or an administrator. Stored XSS is often considered high or critical risk.
- DOM XSS: JavaScript frameworks, single-page applications, and APIs that dynamically include attacker-controllable data to a page are vulnerable to DOM XSS. Ideally, the application would not send attacker-controllable data to unsafe JavaScript APIs. Typical XSS attacks include session stealing, account takeover, MFA bypass, DOM-node replacement or defacement

Preventing:
- Using frameworks that automatically escape XSS by design, such as the latest Ruby on Rails, React JS. Learn the limitations of each framework’s XSS protection and appropriately handle the use cases which are not covered.
- Escaping untrusted HTTP request data based on the context in the HTML output (body, attribute, JavaScript, CSS, or URL) will resolve Reflected and Stored XSS vulnerabilities. 
- Applying context-sensitive encoding when modifying the browser document on the client side acts against DOM XSS
- Enabling a content security policy (CSP) is a defense-in-depth mitigating control against XSS

### Insecure Deserialization:

If an attacker is able to deserialize an object successfully, then modify the object to give himself an admin role, serialize it again. This set of actions could compromise the whole web application.

The best way to protect your web application from this type of risk is not to accept serialized objects from untrusted sources.

Preventing:
- Implementing integrity checks such as digital signatures on any serialized objects to prevent hostile object creation or data tampering.
- Isolating and running code that deserializes in low privilege environments when possible.
- Logging deserialization exceptions and failures, such as where the incoming type is not the expected type, or the deserialization throws exceptions.
- Restricting or monitoring incoming and outgoing network connectivity from containers or servers that deserialize.
- Monitoring deserialization, alerting if a user deserializes constantly.

### Using Components with Known Vulnerabilities:

Every time you disregard an update warning, you might be allowing a now known vulnerability to survive in your system. Whatever the reason for running out-of-date software on your web application, you can’t leave it unprotected.

- Webmasters/developers cannot keep up with the pace of the updates; after all, updating properly takes time.
- Legacy code won’t work on newer versions of its dependencies.
- Webmasters are scared that something will break on their website.
- Webmasters don’t have the expertise to properly apply the update.

Preventing:
- Remove all unnecessary dependencies.
- Have an inventory of all your components on the client-side and server-side
- Obtain components only from official sources.
- Get rid of components not actively maintained.

### Insufficient Logging and Monitoring:

Not having an efficient logging and monitoring process in place can increase the damage of a website compromise. Keeping audit logs are vital to staying on top of any suspicious change to your website. An audit log is a document that records the events in a website so you can spot anomalies and confirm with the person in charge that the account hasn’t been compromised.
