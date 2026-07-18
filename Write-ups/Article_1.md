# **🚨 How I “Hacked” a Company’s Instagram Account (And Got Rewarded for an Out-of-Scope Bug!) 📸🔥**

> **Author:** Vansh Rathore  
> **LinkedIn:** https://www.linkedin.com/in/vansh-rathore-cybersecurity

In the bug bounty community, it is incredibly easy to get tunnel vision. We spend hours hunting for complex vulnerabilities — trying to bypass WAFs, chaining exploits, or digging for that elusive RCE. But nowadays, almost no one is thinking about the simpler, logical bugs hiding right in plain sight.

When I am hunting, I always prioritize hands-on, practical testing over simply firing up automated scanners. It is often the manual, eagle-eyed observations that yield the most interesting results. Recently, this approach led me to a Broken Link Hijacking vulnerability that resulted in a complete Social Media Account Takeover.

Even though this specific bug is frequently marked as “Out of Scope” on many programs, it is a critical threat to a company’s reputation. And as I found out, if you report it professionally, companies will still reward you! 🏆

Here is a breakdown of how I found the bug, the weird technical twist involved, and my approach to reporting it. 🧵👇

## **🕵️‍♂️ The Reconnaissance & The Approach**

While conducting a vulnerability assessment on a target platform (let’s call it ktik.me), I started with my usual manual walkthrough. Before diving into Burp Suite or fuzzing parameters, I like to map out the business logic and interact with the application exactly as a normal user would.

I scrolled down to the footer of the homepage and started checking the social media links. When I clicked the Instagram icon, instead of landing on a glossy corporate profile, I was greeted by Instagram’s classic error message:

<img width="678" height="399" alt="image" src="https://github.com/user-attachments/assets/aa59ae98-eb02-4325-a17b-908589777f95" />

The link was pointing to a handle that simply did not exist.

## **💥 The Exploit: A One-Click Account Takeover**

The impact here is immediate and severe. Because the link originated directly from the company’s official domain, any user clicking it implicitly trusts the destination.

I immediately went to Instagram and registered the unclaimed username. Just like that, the official link on the company’s website was now redirecting all of their traffic directly to an account that I completely controlled.

<img width="676" height="439" alt="image" src="https://github.com/user-attachments/assets/18000f4f-d424-4755-b60c-a59f39c50e2b" />

### **What could a malicious actor do with this?**

#### **Brand Impersonation**
Post fake updates or damaging content under the company’s name.

#### **Phishing Campaigns**
Message users directly, tricking them into giving up credentials or financial info.

#### **Malware Distribution**
Place malicious links in the Instagram bio.

To prevent any actual bad actors or automated bots from scraping and claiming the handle, I secured the account temporarily and began drafting my report.

## **🧩 The Technical Twist: A Tale of Two Typos**

After I reported the vulnerability, the team was incredibly responsive and pushed a fix. However, when I went to verify the patch, I noticed something strange.

On the homepage, the Instagram link had been pointing to kti.kme (a typo). But when I navigated to their blog and bug bounty sub-pages, the broken link was pointing to ktik.me (a different typo).

I reached out to the team with a video PoC of this discrepancy. It turned out this wasn’t just a simple HTML error — it was a caching issue. The developers had pushed an initial fix, but an older version of the website’s layout was still being served from the cache on specific page paths!

### **Takeaway for developers:**
Always remember to completely purge your cache across all servers when deploying security patches! 🧹

## **🎁 The Reward: Out of Scope, but Not Out of Mind**

In many bug bounty programs, Social Media Account Takeover is explicitly listed in the “Out of Scope” section. Because of this, a lot of hunters simply ignore dead links.

However, you have to think about the business impact. A company’s reputation is everything. I submitted a polite, detailed report outlining the exact risks and offered to hand over the credentials to the secured account.

Even though the platform was a 100% free community project and the bug technically fell outside traditional bounty scopes, the Head of the platform was thrilled. They patched the code, purged their servers, and as a token of appreciation, permanently added my name and LinkedIn profile to their Bug Bounty Hall of Fame! 🎉

<img width="1105" height="676" alt="image" src="https://github.com/user-attachments/assets/78f803ee-dfc4-424f-bf5f-205fd796f58e" />

## **💡 Final Thoughts for Fellow Hunters**

### **Don’t rely solely on scanners.**
Automated tools often miss the nuances of broken external links or business logic flaws.

### **Report the “small” things.**
Even if a bug is low severity or out of scope, a good company will appreciate your effort in protecting their users. Build that positive relationship!

### **Always secure the asset.**
If you find an unclaimed social handle or expired domain, secure it yourself before reporting it so a malicious bot doesn’t snatch it while the security team is triaging your report.

Keep your eyes peeled, check those footers, and happy hunting! 💻🔒
