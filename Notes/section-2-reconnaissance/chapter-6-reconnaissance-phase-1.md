---
description: >-
  The more targets you find in this phase the better your  odds will be in
  finding a vulnerability in the exploitation phase
---

# Chapter 6: Reconnaissance Phase 1

## <mark style="color:yellow;">CIDR Range</mark>

CIDR ranges can be used to help identify assets belonging to an organization.

#### ASN

An Autonomous System Number (ASN) is a way to represent a collection of IPs and who owns them.

The IP address pool is spread across five Regional Internet Registries (RIRs) AFRINIC, APNIC, ARIN, LACNIC, and RIPE NCC.&#x20;

The providers allocate IP ranges to different organizations.

**ASN Lookup:**

* &#x20;use ([https://mxtoolbox.com/asn.aspx](https://mxtoolbox.com/asn.aspx)) to find a company’s ASN as well as their correlating CIDR ranges

## <mark style="color:yellow;">Reverse Whois</mark>

used to find other domains that are owned by the same organization and then find more assets.

**For reverse whois lookup**

* use [https://viewdns.info/reversewhois](https://viewdns.info/reversewhois)

## <mark style="color:yellow;">Reverse DNS</mark>

DNS records can be used to tie domains together.&#x20;

If domains share the same A, NS, or MX record we can assume they are owned by the same entity.

* Search for them online using [https://domaineye.com/](https://domaineye.com/)
* or use the nslookup command to find them

```bash
# DNS nameserver lookup
nslookup -type = NS target.com

# DNS mail server lookup
nslookup -type = MX target.com
```

## <mark style="color:yellow;">Google Dork</mark>

Using the “intext” Google dork with an organization's copyright text we can find sites owned by the same company.

example:

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

## Reconnaissance Phase 1 using Amass Tool

Amass is one of the best tools for asset discovery.

<pre class="language-bash"><code class="lang-bash"># Amass ASN search
amass intel -org &#x3C;company name here>
<strong>
</strong><strong># Find CIDR range from ASN number
</strong>whois -h whois.radb.net -- '-i origin &#x3C;ASN Number Here>' | grep -Eo "([0-
9.]+){4}/[0-9]+" | sort -u

# Amass find domains hosted on an ASN
amass intel -asn &#x3C;ASN Number Here>

# Amass find domains hosted in a CIDR range
amass intel -cidr &#x3C;CIDR Range Here>

# Amass reverse whois search
amass intel -whois -d &#x3C;Domain Name Here>

</code></pre>
