# Bug Bounty Writeups & Security Research
Welcome to my repository! I'm Vansh Rathore, a Certified Ethical Hacker and an independent security researcher.

This repository is more than just a collection of proof-of-concepts; it's a deep dive into my thought process, hunting methodology, and the techniques I use to uncover complex vulnerabilities like IDORs, Race Conditions, chained XSS exploits, etc.

# 🎯 What to Expect
Here, I share detailed writeups on real-world vulnerabilities I've discovered and responsibly disclosed (including findings reported to the NCIIPC). My focus is heavily centered on the OWASP Top 10 and uncovering critical business logic flaws that automated scanners entirely miss.

# 🧠 My Hunting Methodology
I strongly believe in practical, hands-on testing over heavily relying on automated tools. My general approach follows these steps:

## Deep Reconnaissance: 
Mapping out the target's attack surface using advanced Google Dorks, Nmap, and careful endpoint enumeration to find hidden or deprecated assets.

Understanding Business Logic: Before firing any payloads, I spend time interacting with the application as a normal user. Understanding how the application is supposed to function is the key to breaking its logic.

## Manual VAPT: 
Shifting away from standard scanner reliance. I proxy all traffic through Burp Suite to manually manipulate parameters, test for state-machine bypasses, and look for nuanced flaws like TOCTOU race conditions.

## Impact & Chaining: 
I look for ways to chain lower-level bugs (like HTML injection or content spoofing) into higher-impact vulnerabilities.

## Responsible Disclosure: 
Clearly documenting the impact and precise steps to reproduce, ensuring the organization can patch the flaw effectively.

# 🛠️ Tools of the Trade
While a hacker's mindset is the most important tool, my typical workflow involves:

Burp Suite (My primary driver for manual testing and request manipulation)

Nuclei (For targeted, template-based validation)

Nmap & sqlmap (For infrastructure mapping and database testing)

Advanced search engine dorking

# 🤝 Let's Connect
I'm always open to discussing vulnerabilities, sharing knowledge, and collaborating with fellow bug hunters. Feel free to explore the writeups, learn from my methodology, and reach out!

