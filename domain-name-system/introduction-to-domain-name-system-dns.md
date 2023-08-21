---
description: Learn how domain names get translated to IP addresses through DNS.
---

# Introduction to Domain Name System (DNS)

#### The origins of DNS <a href="#the-origins-of-dns" id="the-origins-of-dns"></a>

Let’s consider the example of a mobile phone where a unique number is associated with each user. To make calls to friends, we can initially try to memorize some of the phone numbers. However, as the number of contacts grows, we’ll have to use a phone book to keep track of all our contacts. This way, whenever we need to make a call, we’ll refer to the phone book and dial the number we need.

Similarly, computers are uniquely identified by IP addresses—for example, `104.18.2.119` is an IP address. We use IP addresses to visit a website hosted on a machine. Since humans cannot easily remember IP addresses to visit domain names (an example domain name being [educative.io](http://educative.io/)), we need a phone book-like repository that can maintain all mappings of domain names to IP addresses. In this chapter, we’ll see how DNS serves as the Internet’s phone book.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.29.26 AM.png" alt=""><figcaption></figcaption></figure>

### What is DNS? <a href="#what-is-dns" id="what-is-dns"></a>

The **domain name system (DNS)** is the Internet’s naming service that maps human-friendly domain names to machine-readable IP addresses. The service of DNS is transparent to users. When a user enters a domain name in the browser, the browser has to translate the domain name to IP address by asking the DNS infrastructure. Once the desired IP address is obtained, the user’s request is forwarded to the destination web server.

The slides below show the high-level flow of the working of DNS:

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.30.56 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.31.23 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.31.55 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.32.40 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.33.03 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.33.26 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.33.47 AM (1).png" alt=""><figcaption></figcaption></figure>



The entire operation is performed very quickly. Therefore, the end user experiences minimum delay. We’ll also see how browsers save some of the frequently used mappings for later use in the next lesson.

### Important details <a href="#important-details" id="important-details"></a>

Let’s highlight some of the important details about DNS, some of which we’ll cover in the next lesson:

* **Name servers:** It’s important to understand that the DNS isn’t a single server. It’s a complete infrastructure with numerous servers. DNS servers that respond to users’ queries are called **name servers**.
* **Resource records:** The DNS database stores domain name to IP address mappings in the form of resource records (RR). The RR is the smallest unit of information that users request from the name servers. There are different types of RRs. The table below describes common RRs. The three important pieces of information are _type_, _name_, and _value_. The _name_ and _value_ change depending upon the _type_ of the RR.

### Common Types of Resource Records

| **Type** | **Description**                                                       | **Name**    | **Value**      | **Example (Type, Name, Value)**                          |
| -------- | --------------------------------------------------------------------- | ----------- | -------------- | -------------------------------------------------------- |
| A        | Provides the hostname to IP address mapping                           | Hostname    | IP address     | (A, relay1.main.educative.io,104.18.2.119)               |
| NS       | Provides the hostname that is the authoritative DNS for a domain name | Domain name | Hostname       | (NS, educative.io, dns.educative.io)                     |
| CNAME    | Provides the mapping from alias to canonical hostname                 | Hostname    | Canonical name | (CNAME, educative.io, server1.primary.educative.io)      |
| MX       | Provides the mapping of mail server from alias to canonical hostname  | Hostname    | Canonical name | (MX, mail.educative.io, mailserver1.backup.educative.io) |

* **Caching:** DNS uses caching at different layers to reduce request latency for the user. Caching plays an important role in reducing the burden on DNS infrastructure because it has to cater to the queries of the entire Internet.
* **Hierarchy:** DNS name servers are in a hierarchical form. The hierarchical structure allows DNS to be highly scalable because of its increasing size and query load. In the next lesson, we’ll look at how a tree-like structure is used to manage the entire DNS database.

Let's explore more details of the above points in the next lesson to get more clarity.
