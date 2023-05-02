---
description: >-
  the more assets and endpoints you can uncover the better your odds of  finding
  a vulnerability
---

# Chapter 7: Reconnaissance Phase 2

## Wordlist

Using the right word list will instantly increase your success rate when hunting for vulnerabilities.

| Wordlist        | Repo link                                                                                                                                                                                                              | Usage                                                                               |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Seclists        | [https://github.com/danielmiessler/SecList](https://github.com/danielmiessler/SecLists)                                                                                                                                | a very popular source of different wordlists                                        |
| Robots Disallow | [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/RobotsDisallowed-Top1000.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/RobotsDisallowed-Top1000.txt) | used to tell scraping bots such as google to not crawl certain files and file paths |
| RAFT            | [https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-files.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-files.txt)                 | for directory brute forcing                                                         |
| Common Speak    | [https://github.com/assetnote/commonspeak2](https://github.com/assetnote/commonspeak2)                                                                                                                                 | subdomain brute forcing                                                             |
| jhaddix         | [https://gist.github.com/jhaddix/86a06c5dc309d08580a018c66354a056](https://gist.github.com/jhaddix/86a06c5dc309d08580a018c66354a056)                                                                                   | subdomain brute forcing                                                             |
| Internetwache   | [https://github.com/internetwache/CT\_subdomains](https://github.com/internetwache/CT\_subdomains)                                                                                                                     | subdomain brute force                                                               |

## Subdomain Enumeration workflow

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:orange;">Ways to find subdomains:-</mark>

1. #### Certification Transparency Logs

Certificate transparency logs contain a list of all websites that request an SSL certificate for their domain.&#x20;

These logs were created to help spot forged certificates but we can use them in our subdomain enumeration process utilizing the site CERT.SH

2. **Search Engine**

Google dork to find subdomains using (site: ) dork:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

3. **Forward DNS**

Note that gzip searches based on a regex so you must escape the “.” characters with a forward slash “\”.

```bash
# Parse subdomains from the forward DNS dataset using the zgrep tool:

zgrep ‘\.domain\.com”,’ path_to_dataset.json.gz 

```

4. **GitHub**

Scraping subdomains from GitHub is an excellent way to find hidden endpoints that other methods would miss, this can be done by using the [gwen001](https://github.com/gwen001/github-search/blob/master/github-subdomains.py) tool.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Subdomain brute force is one of the most popular and best ways to find subdomains using the [Gobuster](https://github.com/OJ/gobuster) tool:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

6. **Subdomain Permutation**

A permutation is a way a set of words can be rearranged.

* performed as the last step in your subdomain enumeration process.
* Using [altdns](https://github.com/infosec-au/altdns) we can pass in a list of found subdomains and a list of words and the tool will output a huge list of permutations.

```bash
# Altdns subdomain permutations
altdns -i found_subdomains.txt -o permutation_output -w words.txt -r -s 
resolved_output.txt
```

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

7. **A small list of resources to find subdomains:**

* Shodan, DNSdb, Pastebin, Netcraft, DNSdumpster, Threat crowed and Virus Total.

8. **Tools**

* [Amass](https://github.com/OWASP/Amass)

```bash
# Amass subdomain enumeration
amass enum -passive -d <Domain Name Here>
```

* [Knock.py](http://localhost:5000/s/uPoozq67B4qyE1lvqouT/networking-basics/ip-addresses)

```bash
# Knock.py subdomain enumeration
knockpy.py <Domain Name Here>
```

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:yellow;">DNS Resolutions</mark>

we can perform a DNS lookup against a domain to see if it contains an A record, If it does then we know the subdomain is live.

* If you have a list of subdomains you can use [Massdns](https://github.com/blechschmidt/massdns.git) to determine which ones are live domains.
* we will need to parse the tool's output using the [JQ](https://github.com/stedolan/jq) command line json parser.

```bash
# Starting Massdns tool
git install https://github.com/blechschmidt/massdns.git
cd massdns
make

# Parse out the live domains
# Resolvers.txt should hold your list of DNS resolvers
# Subdomains.txt holds the domains you want to check
./bin/massdns -r resolvers.txt -t A -o J subdomains.txt | jq 
'select(.resp_type=="A") | .query_name' | sort -u


```

## <mark style="color:yellow;">Screenshot using</mark> [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness)

It takes a screenshot of each domain in the list that was passed to it, Once the tool is finished you can scroll through each of the screenshots to find interesting endpoints.

```bash
# Running Eyewitness screenshot
Python3 EyeWitness.py -f subdomains.txt --web
```

## <mark style="color:yellow;">Content Discovery</mark>

The main purpose behind the content discovery is to find endpoints on a target domain.

Crawling a website involves recursively visiting each link and saving each link on a web page recursively.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

1. **Self Crawl**

Crawling is useful for finding public endpoints.

* can be done using the [Crawler.py](https://github.com/ghostlulzhacks/crawler/tree/master) tool

```bash
# Crawl the website to get URLs
python3 crawler.py -d <URL> -l <Levels Deep to Crawl>
```

2. **Wayback machine crawl data**

The [Wayback Machine](https://web.archive.org/) is an archive of the entire internet.

It goes to every website and crawls it while taking screenshots and logging the data into a database.

Once you get the data start looking for interesting files and GET parameters that might be vulnerable.

Interesting filters include:

* .config, /admin/, /api/, .zip, .bak

3. [**Common crawl**](http://commoncrawl.org/) **data**

Like the Wayback Machine, this data is publicly available and we can use it to get a list of endpoints on a site passively.

Using [Common crawl URLs script](https://github.com/ghostlulzhacks/commoncrawl) to query the data provided by common crawl:

```bash
python cc.py -d <Domain>
```

4. **Directory brute force**

Directory brute forcing is perfect for finding hidden endpoints just make sure you're using a high-quality wordlist.

[Gobuster](https://github.com/OJ/gobuster) directory brute force:

```bash
./gobuster dir -k -w <Wordlist> -u <URL>
```

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:yellow;">Inspecting JavaScript Files</mark>

There are interesting things in JavaScript files such as AWS keys, S3 bucket endpoints, and API keys.

1. [**Link Finder**](https://github.com/GerbenJavado/LinkFinder)

one of the best tools for parsing endpoints from JavaScript files.

works by using jsbeautifier with a list of regexes to find URLs.

```bash
#  Linkfinder parses URLS from JavaScript files
python linkfinder.py -i <JavaScript File URL> -o cli
```

2. [**Jssearch**](https://github.com/incogbyte/jsearch)

another JavaScript parser except this tool is primarily used to find sensitive or interesting strings.

Jsearch regexes:

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Using the[ jsearch.py](https://github.com/incogbyte/jsearch/blob/master/regex\_modules/regex\_modules.py) command line tool to do this, make sure you are using Python 3.7 or above:

```bash
# ex: Jsearch parse URLs, API keys, and other information
python3.7 jsearch.py -u https://starbucks.com -n Starbucks
```

## <mark style="color:yellow;">Google Dorks</mark>

Google Dorks can be used to find anything and everything about your target (hidden assets, credentials, vulnerable endpoints).

A huge list of interesting dorks can be found on the [exploit-db](https://www.exploit-db.com/google-hacking-database) website.

[List of Google Dorks](https://gbhackers.com/latest-google-dorks-list/).

1. **Dork Basics**

One of the most used Google dorks is the “site:” command, this can be used to filter the search engine results so only a specific URL is shown.

* EX: site:\<Domain Name>

“inurl:” Dork query is used to match a URL with a specific word.

“intitle:” Dork query will filter results that have a specific title.

2. **Third-Party Vendors**

A typical dork when looking for third-party vendors looks like this:

* site:\<Third Party Vendor> \<Company Name>

3. **Content**

You can search for specific file extensions with the “ext:” dork.

* can be used to find all kinds of things such as backup files, PDFs, databases, zip files, and anything else.
* ex: site:starbucks.com ext:PDF

