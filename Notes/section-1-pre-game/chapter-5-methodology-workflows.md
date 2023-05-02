---
description: >-
  The easiest way to map out your pentest, bug bounty, or hacking process is to
  create a flowchart or workflow that describes everything
---

# Chapter 5: Methodology - Workflows

## <mark style="color:yellow;">Recon Workflow</mark>

### Traditional Workflow

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

#### Domain

1. pick a company that has a bug bounty program.
2. locate all domains belonging to that company (Root Domains).
3. determine the subdomains belonging to each root domain.
4. perform DNS resolution to determine each target's A, NS, MX, and CNAME records.
5. add A records to a list of IPs belonging to the company.

#### CIDR

Classless Inter-Domain Routing (CIDR) is just a range of IP addresses belonging to an organization.

Large companies tend to have their own CIDR ranges but smaller companies won’t have a CIDR range.

#### IP

Once you have gathered a list of IPs, perform a port scan.&#x20;

You can perform port scans passively using third-party scanners or scan the target yourself.

#### Web Applications

take the list of subdomains and IPs running a web application and perform fingerprinting and content discovery on them.

1. fingerprint what technology runs on each endpoint (e.g. if you see a site is running WordPress you might run a WordPress scanner on it).
2. perform content discovery by crawling or directory brute force to figure out what pages are on the target domain.



### GitHub Workflow

Almost every developer uses GitHub to manage and store their source code.

With a little recon, you can find these repositories and if you get lucky you will find some hard-coded credentials that can be used to log in to their application or server.

<figure><img src="../.gitbook/assets/Screenshot 2023-04-15 164500.png" alt=""><figcaption></figcaption></figure>

### Cloud Workflow

Companies are starting to ditch the idea of hosting their servers and they are moving to the cloud.

check each cloud provider to see if your target has any assets with a misconfigured storage bucket you should look for exposed sensitive data.

<figure><img src="../.gitbook/assets/Screenshot 2023-04-15 195715.png" alt=""><figcaption></figcaption></figure>

### Google Dork Workflow

Google Dorks allows you to filter through the massive amount of data Google collects to find specific things.

used to find remote code execution (RCE) or working credentials.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

### Leaked Credentials Workflow

This workflow might be out of scope.

If database leaks are posted online, we can perform searches against them to find user credentials belonging to our target.

These credentials could then be used to log in to an organization's SSH, VPN, email, or any other service that is exposed to the internet

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:yellow;">Exploit Workflows</mark>

### New CVE Workflow

You aren't looking for lame vulnerabilities here, you are looking for high-impact vulnerabilities like SQL injection, and RCE.

it revolves around pouncing on a target the second a new CVE with a working POC comes out.

you have to fingerprint your targets so you can easily search which domains are impacted by the new CVE. If not, you will move directly to this phase to determine which targets to go after.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### Known Exploit/Misconfiguration Workflow

The key to this workflow is knowing how to google information and experience.

* You need to fingerprint both the target domains and IPs.&#x20;
* Once that is completed you can search for public CVEs with working POCs.
* CVEs get patched while misconfigurations can happen at any time to anyone.
* Each technology stack has its misconfigurations so you need to be familiar with everything.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### CMS Workflow

We are targeting content management systems (CMS).

* as a tester, all you need to know is what tool to run for which CMS.
* You don’t typically perform manual testing against a CMS, you normally run some type of scanner against the host which looks for known CVEs and misconfigurations.

<figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

### OWASP Workflow

This workflow tends to be a mix of manual and automated testing.

You want to look at the endpoint manually so you can cover it in depth, find unique vulnerabilities, and test for vulnerabilities that scanners can’t do such as architecture and logic flows that automated scanners can’t pick up.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

### Brute Force Workflow

This workflow involves brute forcing exposed admin interfaces, SSH services, FTP, services, and anything else that accepts credentials (make sure this technique is in scope).

* If you find working credentials on GitHub or if you find a database leak with the target's credentials you could spray those across all of their login services to see if they work.
* You could also build a list of default or common usernames and passwords and spray those across the target’s login services.

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>
