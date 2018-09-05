title: Homework 4
description: Recursive DNS Resolver
due: Friday, April 6 
assigned: Tuesday, March 20
additional_css: [syntax.css]
---

## {{ page.title }}: {{ page.description }}

---------
##### **This assignment is due at 11:59pm on April 06, 2018.**

##### **You can download the homework zip archive [here]({{ site.url }}{{ site.baseurl }}/downloads/hw4.zip).**

---------

Your goal is to recreate the functionality of a recursive DNS resolver.
Your program will not be allowed to perform any recursive queries (i.e.
request that another server perform recursion for it); it must perform all
recursion itself.


### Getting Setup

Most of what you'll need in this assignment is included in the `resolve.py`
skeleton file included in this archive. You'll need to install
the [dnspython](http://www.dnspython.org/) library though.  You can do so
using [pip](https://pip.pypa.io/en/stable/).  In most cases you can
do so by running one of the following commands on your system:

 * `pip3 install dnspython`
 * `pip install dnspython`
 * `python -m pip install dnspython`

You're responsible for getting `dnspython` installed on your system.  If you
have any problems getting the library installed, please ask on Piazza.


### The Assignment

You'll find a `resolve.py` file in your repo.  This file contains
code that approximates the functionality of the `host` program.  The program
takes a domain name, and returns a summary of DNS information about the domain.
For example, given the domain "yahoo.com", `host` returns the "A", "AAAA"
and "MX" records for the domain.  You can test this out by running
the `resolve.py` command as so: `python resolve.py yahoo.com`.

You can lookup multiple domains in the same execution by passing multiple
domains to the program (e.g. `python resolve.py first.com second.edu third.org`).

`resolve.py` currently queries a DNS server (`8.8.8.8`) to find information
about domains.  That DNS server handles the recursive part of DNS for you
(i.e. `8.8.8.8` by default doesn't know where to find `yahoo.com`, so it
asks a root server, and the root server tells `8.8.8.8` where to find
information about `.com`, so then `8.8.8.8` asks the new server where to
find `yahoo.com`, etc.)

Your task in this assignment is to implement this recursive functionality
on your own.  When your version of `resolve.py` is run, it will not
query `8.8.8.8` (or any other recursive resolver).  Your `resolve.py` will
query a root server itself, as well as performing any further needed
queries.

The IP addresses of the root servers are hard coded into the `resolve.py`
in the global `ROOT_SERVERS` list. Your program should start by querying one
of these servers. **A very good first step** in your solution will be
to find where `8.8.8.8` is in the code currently, and make sure you instead
query one of the root servers.


### Handling Errors

Your code should be able to handle cases where DNS servers are down or slow
to respond. The
[rules governing DNS and how to handle errors](https://tools.ietf.org/html/rfc1034)
are very complex.  For the purposes of this assignment, we'll be using a
simplified set of rules for handling errors.

 * Use a timeout value of 3 seconds for all queries.  Any request taking
   more than 3 seconds to respond should be treated as non-responsive.
 * You should exhaustively try all available servers when trying to answer
   a query.  For example, if a request to a root server gives you
   13 servers for the ".com" zone, you should try each of those 13 servers
   before giving up.
 * If you receive an error or non-response from a server, you should not
   retry the server.  Only query each server once.
 * Only query servers over IPv4.  You should not query servers over IPv6 when
   trying to resolve domains.

Your code should never throw an exception.  Dumping a stacktrace is not
an appropriate response for any query.  If you are unable to get a result
for a domain, your code should not print nothing out.

If you have any questions regarding how to handle errors and non-responsive
servers, please ask on Piazza.


### Grading

 * **5 points each**: Perform A record lookups without querying a recursive
   nameserver (two tests, for a total of 10 possible points).
 * **2 points**: Perform lookups in the case of CNAME restarts (e.g.
   `www.internic.net`).  **Note** that your code should run succesfully even
   when the CNAME returns no "Additional" section.
 * **2 points**: Perform lookups of "unglued" nameservers (e.g.
   `www.yahoo.com.tw`)
 * **4 points**: Correctly handle errors, so that your code correctly
   deals with non-responsive or slow servers.
 * **3 points**: Implement a caching system, so that exact duplicate lookups for
   the same domain do not result in additional queries.  (e.g. running
   `python resolve.py uic.edu uic.edu`, sould result in the same number of
   queries as running `python resolve.py uic.edu`).
 * **3 points**: Implement a more sophisticated caching system, that caches
   intermediate results too.  (e.g. if you run
   `python resolve.py uic.edu illinois.edu`, you sould only query for `edu`
   once).
 * **1 point**: [PEP 8](https://www.python.org/dev/peps/pep-0008/) compliant
   code that earns at least 10 of the above points.

There are 25 possible points. This homework will be graded out of 20 points.

## Submission Instructions
You will be making the submission for this homework through the OK client. When you first download 
the homework, make sure you authenticate using `python3 ok --authenticate`. To save your work, feel free to create as many backups as you like.

You will be making changes to `resolve.py` and that is the only file that will be submitted for grading by the ok client:

To submit your work, run `python3 ok --submit`. Like previously, you are allowed to make multiple submissions. The most recent submission before the deadline will be graded.

## Due Date and Logistics

* This assignment is due at the at 11:59 pm on Friday, April 6, 2018.

* If you have any questions please use the [Piazza](https://piazza.com/class/j9oqs0y7d01k0) discussion forum.

### Helpful Links
 * [TCP IP Guide](http://www.tcpipguide.com/free/t_TCPIPDomainNameSystemDNS.htm)
 * [IANA DNS Parameters](http://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml)
