# **🏆 Hall of Fame Unlocked: Bypass Authentication**

> **Author:** Vansh Rathore  
> **LinkedIn:** https://www.linkedin.com/in/vansh-rathore-cybersecurity
> 
Balancing CSE coursework with late-night bug hunting always makes for an interesting schedule, but hitting a Hall of Fame makes the grind completely worth it! 🚀

Recently, I was hunting on a target (let’s call it example.com) and decided to dive deep into their authentication mechanisms. I always emphasize to other hunters in our community that relying solely on automated scanners will make you miss the most critical business logic flaws. You have to get your hands dirty and understand how the application actually thinks.

Here is the full breakdown of how I chained an Insufficient Rate Limiting vulnerability with HTTP Response Manipulation to completely bypass their email verification process, earning a Certificate of Appreciation and a spot on their Security Acknowledgements page. 🛡️💻

## **🎯 The Target & The Recon**

While poking around example.com, I navigated to their standard signup portal at `https://admin.example.com/signup`.

The onboarding flow was standard:
1. Enter an email address.
2. The server sends a 6-digit OTP to that email, valid for 10 minutes.
3. Enter the OTP to verify the email and access the dashboard.

At first glance, it looked secure. But as a bug hunter, whenever I see a 4- or 6-digit OTP, my immediate thought is: *Is there a rate limit?*

## **⚙️ The Attack Chain: Intruder Meets Response Manipulation**

To test the endpoint, I entered a random, incorrect 6-digit number (111111) into the verification field and intercepted the `POST /api/auth/complete-signup` request using Burp Suite.

### **Step 1: Testing for Rate Limits**

I sent the intercepted request to Burp Intruder to see if the server would block me after a few failed attempts. To strictly adhere to the program’s “Minimize Impact” policy and avoid causing a Denial of Service (DoS), I didn’t blast the server with thousands of requests. I simply configured a numbers payload to test a small range of 200 numbers where I knew the correct OTP was hidden.

I fired up the attack. Attempt 1… `400 Bad Request`. Attempt 50… `400 Bad Request`. Attempt 100… `400 Bad Request`.

No `429 Too Many Requests`. No account lockout. The endpoint had absolutely zero rate limiting.

<img width="680" height="400" alt="Screenshot 2026-07-18 at 01 00 27" src="https://github.com/user-attachments/assets/23fedebc-6f99-4cd9-b6a1-80c13166a985" />
        
### **Step 2: Extracting the Golden Ticket (JWT)**

Because the endpoint lacked rate limits, my Intruder payload eventually hit the correct OTP. Instead of the standard `400 Bad Request`, the server returned an `HTTP/2 200 OK` with a larger content length (~1370 bytes).

Looking closely at that `200 OK` response, I found exactly what I needed: a brand new, fully valid JSON Web Token (JWT) issued by the server for that session.

### **Step 3: Response Manipulation**

Having the JWT is great, but I needed to prove I could hijack the actual browser session.

I went back to my proxy and intercepted the server’s response to my original failed `111111` request. The server was trying to send a `400 Bad Request` back to my browser to tell it the OTP was wrong.

I right-clicked, chose to intercept the response, and manually modified it:
* Changed the HTTP status from `400 Bad Request` to `200 OK`.
* Swapped out the failure JSON body with the successful JSON body I extracted from Intruder (which contained the valid JWT).

I forwarded this manipulated response to my browser. The frontend completely trusted the injected response, accepted the JWT, bypassed the email verification screen entirely, and dropped me right into the authenticated onboarding dashboard! 🎉

<img width="679" height="451" alt="image" src="https://github.com/user-attachments/assets/a9cfef0d-3c3a-4987-9665-9833d2aa73f1" />

## **💥 The Impact**

A 6-digit OTP has exactly 1,000,000 possible combinations. Because the code was valid for 10 minutes and there were no rate limits, an attacker could easily script an attack to guess the correct code within the time window.

By chaining the lack of rate limits with response manipulation, a malicious actor could completely bypass the email verification mechanism, allowing them to register unauthorized accounts and bypass intended access controls.

## **📝 Takeaways for Fellow Hunters**

### **Scanners Miss Logic**
Automated vulnerability scanners won’t catch this. This exploit required understanding the authentication flow, manipulating the response, and manually injecting a valid token. Always test business logic!

### **Read the Policy**
I stopped my Intruder attack at 200 requests. Had I blasted the server with 10,000 requests, I might have violated their out-of-scope rules regarding DoS attacks. Prove the concept, document it clearly, and stop.

### **PGP is Your Friend**
When dealing with authentication bypasses, the method is highly sensitive. Encrypting the report and the video PoC using the company’s PGP key ensures responsible and secure disclosure.

## **🏆 Conclusion**

I promptly reported this to the example.com security team. They were incredibly professional, acknowledged the report swiftly, and deployed a remediation patch to secure the endpoint.

As a token of gratitude, they awarded me a Certificate of Appreciation and featured me on their Hall of Fame page!

Always learning, always hunting. On to the next target! 🎯

If you enjoyed this write-up, feel free to connect with me on LinkedIn and let’s talk cybersecurity!

<img width="1310" height="839" alt="image" src="https://github.com/user-attachments/assets/d65b8e60-ed1b-495b-8858-0c923d88246d" />
<img width="563" height="397" alt="image" src="https://github.com/user-attachments/assets/94c921d4-209c-4e06-92c8-8aeb172094e6" />


***

*Tags: #BugBounty #CyberSecurity #EthicalHacking #InfoSec #VulnerabilityDisclosure #HallOfFame #BurpSuite #VAPT*
