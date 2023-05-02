---
description: >-
  The fingerprinting phase is all about determining what's running on  each
  asset.
---

# Chapter 8: Fingerprint Phase

### &#x20;Fingerprinting workflow

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

## IP

Once you have a list of IPs you will want to discover the ports and services running on that endpoint.

1. [**Shodan**](https://www.shodan.io/)

If you have your target CIDR range you can use that to query Shodan. This will display all assets in that CIDR range that has an open port.

Shodan CIDR search:

* net:<”CIDR,CIDR,CIDR”>

Shodan org search:

* org:<”Organization Name”>

Shodan search by SSL cert name ( for endpoints hosted on a cloud provider):

* ssl:<”ORGANIZATION NAME”>&#x20;

2. **Cencys**

Censys does the same thing Shodan does and we can find assets on Censys that aren't on Shodan and vice versa.

* Censys search: [https://censys.io/ipv4](https://censys.io/ipv4)

3. **Nmap**

Nmap performs best when scanning a single or small range of IPs.

4. [**Masscan**](https://github.com/robertdavidgraham/masscan)

A good scanner if you have a large target range and it's good at detecting a single port across a huge range of IPs.

```bash
# Masscan internet scan on port 80:

sudo masscan -p<Port Here> <CIDR Range Here> --exclude <Exclude IP> --
banners -oX <Out File Name>
```

To search through the results, you can use grep or you can use [Masscan web UI](https://github.com/offensive-security/masscan-web-ui).

## Web Application

Web applications can be found on both IPs and domains.

We want to know the technology stack, programming languages used, firewalls used, and more.

1. [**Wappalyzer**](https://github.com/vincd/wappylyzer)

This tool can only scan one domain at a time but with a little bash scripting, you can create a wrapper script to scan multiple domains.

* Wappalyzer Python script:

<pre class="language-bash"><code class="lang-bash"><strong># set up
</strong><strong>git clone https://github.com/vincd/wappylyzer.git
</strong><strong>cd wappylyzer
</strong><strong># script usage
</strong>python3 main.py analyze -u &#x3C;URL HERE>
</code></pre>

ex: If you know an application is running WordPress version 2.0 you can use this information to find related vulnerabilities and misconfiguration.

2. **Firewall**

Before you start throwing a bunch of XSS payloads at a target you should check to see if there is a WAF using the [Wafw00f](https://github.com/EnableSecurity/wafw00f) tool.

If you discover an application is behind a WAF you need to adjust your payloads so they are able to bypass the firewall.

* [bypassing WAFs](https://github.com/0xInfection/Awesome-WAF#known-bypasses).
