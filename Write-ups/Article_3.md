# 🚀 From False Positive to Hall of Fame: Exploiting Web Cache Poisoning in 4 Hours

> **Author:** Vansh Rathore  
> **LinkedIn:** https://www.linkedin.com/in/vansh-rathore-cybersecurity

---

## Introduction

Balancing bug bounty hunting with my third-year Computer Science coursework means every hunting session has to count. One lesson I've learned is that relying solely on automated scanners creates a false sense of security. The most impactful vulnerabilities are often discovered through manual testing, understanding application behavior, and exploring business logic.

Recently, that approach led to discovering a **Web Cache Poisoning** vulnerability through **Cache Entanglement** on **Shout Me Crunch**. The issue was acknowledged, patched within **4 hours**, and ultimately earned me a place in their **Security Hall of Fame**.

This write-up explains the complete discovery process, including how I avoided a common testing mistake, bypassed a WAF limitation, proved the vulnerability safely, and responsibly disclosed it.

---

# What is Web Cache Poisoning?

Web caches improve website performance by storing responses and serving them to multiple visitors requesting the same resource.

To determine whether two requests are identical, caches use a **Cache Key**, which commonly consists of:

- URL
- Host header
- Selected request headers

Problems occur when an application processes input that **isn't included in the cache key**.

An attacker can send a crafted request that causes the backend to generate a modified response. Since the malicious parameter isn't part of the cache key, the poisoned response becomes cached and later served to users requesting the normal URL.

This behavior is commonly referred to as **Web Cache Poisoning**.

---

# Target Overview

Target:

```
www.shoutmecrunch.com
```

Technology observed during reconnaissance:

- WordPress
- WP-Super-Cache
- Cloudflare CDN
- Cloudflare WAF

During parameter fuzzing, I noticed that the parameter:

```
content=
```

was reflected directly into the DOM.

At first glance this resembles a reflected XSS, but because the page was aggressively cached, I suspected something much more interesting.

---

# Initial Testing

My first attempt was the standard XSS payload.

```http
GET /?1uzvno4p0w=1&content="><script>alert(document.domain)</script> HTTP/1.1
Host: target
```

> **Note:** I always use a random cache-buster parameter (for example `?1uzvno4p0w=1`) during testing to avoid accidentally poisoning content that could impact real visitors.

Instead of reaching the origin server, Cloudflare immediately returned its anti-bot page.

```html
<title>One moment, please...</title>
```

The WAF blocked the payload before WordPress could generate a cache entry.

At this point many researchers stop testing.

However, a WAF blocking JavaScript does **not** mean the cache vulnerability has been fixed.

It simply means the payload needs to change.

---

# Changing the Strategy

Rather than proving JavaScript execution, I only needed to demonstrate that I could modify the cached content delivered to other users.

Instead of XSS, I switched to **Content Spoofing / Cache Defacement**.

Requirements:

- Payload must look harmless to Cloudflare
- Backend must still cache the response
- Fresh cache-buster must be used

---

# Step 1 — Poisoning the Cache

I sent the following request:

```http
GET /?xyz=99&content=CACHE_POISONED_BY_VANSH_RATHORE HTTP/1.1
Host: target
User-Agent: Mozilla/5.0
Accept: text/html
Connection: close
```

Cloudflare allowed the request.

The response returned:

```
HTTP/1.1 200 OK
```

While reviewing the HTML response inside Burp Suite, I observed my payload reflected inside a Jetpack analytics JSON object.

Additionally, the WP-Super-Cache timestamp confirmed that a **fresh cache file** had been generated.

---

### Screenshot


<img width="683" height="442" alt="image" src="https://github.com/user-attachments/assets/0d5991fe-5716-480d-8750-8e97e4d09f14" />


---

# Step 2 — Verifying Cache Poisoning

The important part of proving Web Cache Poisoning is demonstrating that another visitor receives the poisoned response **without including the malicious parameter**.

I opened a completely new Incognito browser window.

Visited:

```
https://www.shoutmecrunch.com/?xyz=99
```

Notice that the request no longer contained:

```
content=
```

I opened **View Source** and searched for:

```
VANSH_RATHORE
```

The payload appeared inside the cached HTML served from WP-Super-Cache.

This demonstrated that the backend ignored the `content` parameter when generating its cache key, resulting in **Cache Entanglement**.

---

### Screenshot


<img width="677" height="440" alt="image" src="https://github.com/user-attachments/assets/6054b4be-687a-4942-bc54-d6c6f2e2e644" />


---

# Why This Matters

Although Cloudflare blocked obvious JavaScript payloads, arbitrary HTML content could still be cached and delivered to future visitors.

Potential impacts include:

- Website defacement
- Content spoofing
- Phishing messages
- Fake login prompts
- Brand reputation damage
- Social engineering campaigns

If a future WAF bypass became available, the same weakness could potentially escalate into:

- Stored XSS
- Zero-click XSS
- Mass compromise of visitors

---

# Responsible Disclosure

After confirming the vulnerability, I immediately stopped testing and prepared a detailed report including:

- Raw Burp Suite requests
- Reproduction steps
- Screenshots
- Impact assessment
- Safe Harbor considerations

The report was submitted to the Shout Me Crunch security team.

---

# Response Timeline

## Acknowledgement

The report was acknowledged almost immediately.

---

## Patch

Within **4 hours**, the team:

- Replaced the vulnerable caching configuration
- Hardened their `.htaccess`
- Updated cache behavior
- Requested verification testing

---

## Verification

I re-tested the affected pages.

The cache poisoning vulnerability was completely remediated.

---

## Recognition

Following verification, I was officially added to the **Shout Me Crunch Security Hall of Fame**.

---

### Hall of Fame


<img width="554" height="187" alt="image" src="https://github.com/user-attachments/assets/92e20fea-995f-48d9-8058-44c73be736a3" />


---

# Lessons Learned

- Never rely entirely on automated scanners.
- WAF blocks do not necessarily eliminate vulnerabilities.
- Always isolate testing with cache-buster parameters.
- Understand how cache keys are generated.
- Manual testing consistently uncovers issues that scanners miss.
- Respect responsible disclosure policies and stop once sufficient proof has been obtained.

---

# Tools Used

- Burp Suite Professional
- Firefox
- Cloudflare
- WP-Super-Cache
- Manual HTTP analysis

---

# Timeline

| Event | Time |
|--------|------|
| Vulnerability discovered | Day 1 |
| Report submitted | Immediately |
| Vendor acknowledgement | Same day |
| Patch deployed | Within 4 hours |
| Verification completed | Same day |
| Hall of Fame recognition | After validation |

---

# Acknowledgements

Special thanks to **Dr. Toufiq Hassan Shawon** and the **Shout Me Crunch** team for their fast response, transparent communication, and commitment to improving security.

Working with responsive organizations makes responsible disclosure rewarding for everyone involved.

---

# Disclaimer

This research was conducted under responsible disclosure principles. Testing was intentionally limited to avoid affecting real users, and no unauthorized access or exploitation beyond proof-of-concept validation was performed.

---

## Tags

```
Bug Bounty
Web Cache Poisoning
Cache Entanglement
WordPress
Cloudflare
Responsible Disclosure
Security Research
Penetration Testing
Application Security
```

---

⭐ If you enjoyed this write-up, consider starring the repository.
