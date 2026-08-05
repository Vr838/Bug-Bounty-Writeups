# 🚀 How I Got My NASA Hall of Fame Recognition: A Unique Twist on Google Dorking 🌌

> **Author:** Vansh Rathore  
> **LinkedIn:** https://www.linkedin.com/in/vansh-rathore-cybersecurity/

---

# Introduction

Every bug bounty hunter dreams of seeing their name recognized by an organization like NASA.

When people hear "NASA Hall of Fame," they often imagine discovering a critical vulnerability such as:

- Remote Code Execution (RCE)
- SQL Injection
- Authentication Bypass
- Server-Side vulnerabilities

But sometimes, impactful security research comes from something much simpler.

A different perspective, careful observation, and understanding how small mistakes can create real-world security risks can be just as valuable.

This is the story of how I earned a NASA Vulnerability Disclosure Program (VDP) Letter of Recognition by combining **Google Dorking** with a classic issue: **Broken Link Hijacking**.

---

# 📉 The Road Before Success

The journey was not instant.

Before this recognition, I submitted multiple reports to the NASA VDP through Bugcrowd.

The results:

| Report | Result |
|--------|--------|
| Report #1 | Informative |
| Report #2 | Informative |
| Report #3 | Duplicate |
| Report #4 | Accepted P4 |

<img width="1468" height="479" alt="image" src="https://github.com/user-attachments/assets/85ee32d1-71ca-4ec1-9fd8-8b4104116c7a" />


---

NASA has an enormous attack surface.

When testing an organization of this size, a traditional automated workflow can quickly become overwhelming:

- Enumerating thousands of subdomains
- Checking live hosts
- Running automated scanners
- Fuzzing large numbers of endpoints

The amount of noise can make it difficult to focus on meaningful findings.

I realized that improving my methodology was more important than simply increasing the number of tools I used.

---

# 🧠 Changing the Strategy: Rethinking Google Dorking

While studying security research approaches, I noticed many hunters use Google Dorks for reconnaissance.

Common searches focus on:

- Exposed files
- Public directories
- Admin panels
- Log files
- Configuration leaks

  <img width="1263" height="606" alt="image" src="https://github.com/user-attachments/assets/479b9867-9582-4ba4-bade-647e58fb8cdd" />


Some of my earlier informative findings also came from this style of reconnaissance.

However, I wanted to approach Google Dorking differently.

Instead of searching only for obvious technical vulnerabilities, I focused on discovering forgotten content and overlooked assets.

The idea was simple:

> What happens if I combine targeted Google Dorking with a basic vulnerability class?

That led me toward **Broken Link Hijacking**.

---

# 🕸️ Discovery: Finding a 13-Year-Old Forgotten Link

Using targeted search queries, I looked for old NASA pages containing social media references.

Example methodology:

```
site:nasa.gov ("facebook.com" OR "instagram.com" OR "twitter.com" OR "youtube.com")
```

For deeper subdomain discovery:

```
site:*.nasa.gov ("facebook.com" OR "instagram.com" OR "twitter.com" OR "youtube.com")
```

```
site:*.*.nasa.gov ("facebook.com" OR "instagram.com" OR "twitter.com" OR "youtube.com")
```

The search led me to an old article hosted on a trusted NASA laboratory subdomain.

The page was approximately 13 years old.

While reviewing the article, I reached the social media section:

```
Find Solar System Exploration at...
```

The link appeared legitimate at first glance.

However, I noticed a small typo in the URL.

---

# 🔎 The Broken Link

Incorrect URL:

```
https://social-media-page.com/oursolarssytem
```

Expected URL:

```
https://social-media-page.com/oursolarsystem
```

Because of the spelling mistake, the destination page did not exist.

The username/page name was available for registration.

This created a potential **Broken Link Hijacking** scenario.

---

# ⚠️ Security Impact: Brand Impersonation Risk

At first glance, a broken social media link might seem harmless.

However, the context changes everything.

The link originated from:

- A trusted NASA domain
- An official article
- A highly reputable organization

An attacker could potentially register the abandoned page and create a malicious account or landing page using:

- Official branding
- Similar logos
- Misleading content

Users clicking from the official NASA website could believe they were interacting with a legitimate NASA-related resource.

This creates a risk of:

- Brand impersonation
- Phishing campaigns
- Social engineering attacks
- Reputation damage

---

# 🛡️ Responsible Validation

To demonstrate the risk safely, I did not create a malicious page or attempt to deceive users.

Instead, I validated the issue responsibly by registering the page and setting up a clear security research disclaimer explaining that it was created only for vulnerability disclosure validation.

The purpose was only to demonstrate the ownership risk.

---

# 📩 Disclosure Process

After confirming the issue, I submitted a detailed report through the NASA VDP.

The report included:

- Affected URL
- Description of the broken link
- Security impact
- Proof of ownership risk
- Remediation recommendation

---

# 🏆 Result

The report was reviewed by the Bugcrowd triage team.

NASA successfully fixed the issue by correcting the broken link.

A few days later, I received an official:

# 📜 NASA VDP Letter of Recognition

---

<img width="625" height="862" alt="image" src="https://github.com/user-attachments/assets/5273e354-36d6-4f68-8153-76b3a443ca79" />


---

# 🔥 Key Lessons Learned

This experience reinforced an important lesson:

You do not always need the most complicated vulnerability to create meaningful security impact.

Sometimes success comes from:

- Looking where others do not
- Understanding trust relationships
- Finding forgotten assets
- Combining simple concepts creatively

A small typo on a trusted website can create a real security concern.

---

# Final Thoughts

Many researchers focus only on finding:

- RCE
- SQL Injection
- Authentication bypasses

Those vulnerabilities are valuable, but security research is also about understanding the bigger picture.

A simple broken link combined with the trust of a major organization can create a meaningful security risk.

Keep learning.

Keep experimenting.

Keep looking at problems from different angles.

Sometimes the simplest findings create the biggest opportunities.

---

# Tools Used

- Google Search
- Google Dorking
- Browser Developer Tools
- Manual Reconnaissance
- Bugcrowd Platform

---

# Responsible Disclosure

This research was performed under responsible disclosure guidelines.

Testing was limited to validation purposes only.

No users were targeted, no phishing activity was performed, and no harm was caused during testing.

---

⭐ If you enjoyed this write-up, consider starring the repository and following for more bug bounty research.
