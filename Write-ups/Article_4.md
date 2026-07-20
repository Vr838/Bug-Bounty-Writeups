# 🕵️‍♂️ How I Find Critical Data Leaks Using Google Search (Google Dorking)

> **Author:** Vansh Rathore  
> **LinkedIn:** https://www.linkedin.com/in/vansh-rathore-cybersecurity

---

# Introduction

When people think about hacking, they often imagine complex exploits, custom tools, and endless lines of code.

In reality, some of the most impactful security issues can be discovered with nothing more than a search engine.

One technique I frequently use during reconnaissance is **Google Dorking**—leveraging advanced Google search operators to identify publicly exposed resources that organizations have unintentionally indexed.

This write-up explains how Google Dorking works, the types of accidental data exposure it can reveal, and how security researchers can use it responsibly during authorized security assessments and bug bounty programs.

> **Disclaimer:** Only use these techniques against organizations you are authorized to assess (e.g., under a bug bounty or responsible disclosure program). Never access, modify, or misuse exposed information.

---

# What is Google Dorking?

Google Dorking (also known as Google Hacking) is the practice of using advanced search operators to locate publicly indexed resources that may expose sensitive information.

Examples include:

- Public Google Drive folders
- Shared Google Docs
- Google Sheets
- Presentations
- Public code snippets
- Configuration files
- Backup files
- Log files

These resources usually become exposed because someone accidentally shared them publicly or linked them from a website that Google's crawlers indexed.

---

# How Do These Data Leaks Happen?

Most exposures are caused by simple configuration mistakes rather than software vulnerabilities.

Common scenarios include:

- Sharing a Google Drive folder publicly
- Setting a document to "Anyone with the link"
- Accidentally publishing internal documentation
- Posting sensitive files on public websites
- Leaving development notes accessible
- Uploading credentials to public paste sites

Employees often assume that a long Google Drive URL is impossible to discover.

However, if that link appears on a public webpage, source code, issue tracker, or documentation site, Google may index it.

---

# Common Targets During Reconnaissance

During authorized assessments, I often look for publicly indexed resources such as:

- Google Drive folders
- Google Sheets
- Google Docs
- Google Slides
- Public paste sites
- Documentation portals
- Internal PDFs
- Employee onboarding material

These resources sometimes reveal:

- Internal documentation
- Architecture diagrams
- Employee contact lists
- API documentation
- Configuration information
- Deployment procedures

---

# Useful Google Search Operators

Below are examples of search patterns commonly used during reconnaissance.

Replace:

```
target.com
```

with the domain you are authorized to assess.

---

## 1. Public Google Drive Folders

Entire folders are occasionally exposed.

```
site:target.com "drive.google.com/drive/folders"
```

Searching specifically for PDFs inside exposed folders:

```
site:target.com "drive.google.com/drive/folders" filetype:pdf
```

Possible findings:

- Internal manuals
- Policies
- Architecture documents
- Employee onboarding guides

---

## 2. Public Google Sheets

Google Sheets often contain operational information because organizations use spreadsheets for collaboration.

```
site:target.com "docs.google.com/spreadsheets"
```

Potential discoveries include:

- Inventory lists
- Project tracking
- Employee schedules
- Publicly shared business data

Always avoid viewing or handling sensitive personal or credential information beyond what is necessary to verify an exposure.

---

## 3. Public Google Documents

```
site:target.com "docs.google.com/document/"
```

Possible findings:

- Documentation
- SOPs
- Internal notes
- Technical documentation

---

## 4. Public Google Slides

```
site:target.com "docs.google.com/presentation/d"
```

Sometimes presentations contain:

- Product roadmaps
- Internal diagrams
- Training material
- Organizational workflows

---

## 5. Public Paste Sites

Developers occasionally share code snippets publicly during troubleshooting.

Examples include:

```
site:pastebin.com "target.com"
```

```
site:codebeautify.org "target.com"
```

Public snippets may contain:

- Example code
- Configuration files
- Debug information
- Sample API requests

---

## 6. Narrowing Search Results

To reduce noise, additional keywords can help locate potentially relevant resources.

Examples:

```
site:target.com "internal"
```

```
site:target.com "confidential"
```

```
site:target.com "staging"
```

```
site:target.com "documentation"
```

These keywords simply help prioritize documents for manual review during authorized testing.

---

# My Recon Workflow

A typical workflow looks like this:

1. Identify an in-scope domain.
2. Search for publicly indexed documents.
3. Verify whether the resource is intentionally public.
4. Avoid modifying or interacting with sensitive content.
5. Collect minimal evidence required for validation.
6. Report the issue responsibly.

The objective is to confirm exposure—not to access or exploit sensitive information.

---

# Responsible Disclosure

If you discover an unintentionally exposed resource:

- Stop once you've confirmed the exposure.
- Do not edit, download excessively, or share the contents.
- Capture only the minimum evidence required.
- Record the affected URL.
- Report the issue through the organization's bug bounty or vulnerability disclosure program.

Responsible disclosure helps organizations remediate issues quickly while minimizing potential harm.

---

# Example Findings

<img width="1101" height="829" alt="image" src="https://github.com/user-attachments/assets/d096fa36-aae1-4b79-acf5-6dbb34688a6f" />

<img width="1101" height="848" alt="image" src="https://github.com/user-attachments/assets/058bf9b5-3d2e-462f-aab7-cd9a99c54bdf" />


---

# Best Practices for Researchers

- Stay within the authorized scope.
- Verify exposures without causing impact.
- Never modify public documents.
- Minimize access to exposed data.
- Follow the organization's disclosure policy.
- Respect privacy and legal boundaries.

---

# Key Takeaways

- Google indexes far more than most organizations realize.
- Small sharing mistakes can lead to significant information exposure.
- Manual reconnaissance often uncovers issues that automated scanners miss.
- Responsible disclosure is essential for improving security.
- Ethical research benefits both organizations and the security community.

---

# Disclaimer

This article is intended for educational purposes and authorized security testing only.

Only perform these techniques against systems you own or have explicit permission to assess. Always comply with applicable laws, bug bounty program rules, and responsible disclosure guidelines.

---


⭐ If you found this write-up helpful, consider starring the repository and following for more bug bounty research and recon techniques.
