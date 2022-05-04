# **Cloud OSINT**

A repository with information related to differnet resources, tools and techniques related with Cloud OSINT

## **Cloud Infrastructure**

### **Azure Storage**

* Blob storage: http://*mystorageaccount*.blob.core.windows.net
* Table storage: http://*mystorageaccount*.table.core.windows.net
* Queue storage: http://*mystorageaccount*.queue.core.windows.net
* Azure Files: http://*mystorageaccount*.file.core.windows.net
* Database: http://*mystorageaccount*.database.windows.net

### **AWS S3 Buckets**

* https://[bucketname].s3.amazonaws.com
* https://s3-[region].amazonaws/[bucketname]/
* https://[bucketname].s3-website-[region].amazonaws.com/

## **Google dorks**

### **Azure**

```
* site:blob.core.windows.net “keyword” 
* site:"blob.core.windows.net" and intext:"CONFIDENTIAL"
* site:*.core.windows.net intext:"TLP:RED"
* site:*.core.windows.net
* site:*.core.windows.net +blob
* site:*.core.windows.net +files -web -blob
* site:*.core.windows.net -web
* site:*.core.windows.net -web -blob -files
* site:*.core.windows.net inurl:dsts.dsts
* site:*.core.windows.net inurl:"term" -web
* site:*.blob.core.windows.net ext:xls | ext:xlsx (login | password | username)
* intext:connectionstring blob filetype:config
* intext:accountkey windows.net filetype:xml
* intext:storageaccountkey windows.net filetype:txt
```

### **AWS**

* site:"s3-external-1.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3.dualstack.us-east-1.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3-external-1.amazonaws.com" and intext:"TOP SECRET"
* site:"s3.amazonaws.com" and intext:"tlp:red"
* site:"s3.amazonaws.com" and intext:"tlp:amber"
* site:s3.amazonaws.com example
* site:s3.amazonaws.com example.com
* site:s3.amazonaws.com example-com
* site:s3.amazonaws.com com.example
* site:s3.amazonaws.com com-example
* site:s3.amazonaws.com filetype:xls password
* site:http://s3.amazonaws.com intitle:index.of.bucket
* site:http://amazonaws.com inurl:".s3.amazonaws.com/"

### **Google Cloud**

* site:.firebaseio.com "COMPANY NAME" : Find endpoints of especific company
* inurl:bc.googleusercontent.com intitle:index of  : Find dir list of servers

### Other Cloud Services

* site:vps-*.vps.ovh.net

## Tools to find buckets

### **Tools**

1. https://buckets.grayhatwarfare.com/ : Search open Buckets
2. https://cse.google.com/cse?cx=002972716746423218710:veac6ui3rio#gsc.tab=0&gsc.q= : search cloud storage and buckets in diferent cloud providers

## **OSINT to domains**

1. https://spyse.com/tools/subdomain-finder domain and subdomain enumeration 
2. https://crt.sh Finding domains and subdomains by ssl certificates through certificate transparency
3. https://dnsdumpster.com/ domain and subdomain enumeration 
4. https://osint.sh/subdomain/ domain and subdomain enumeration 
5. https://search.censys.io/?q= queries by domain, host , ssl certificate , among others
6. https://www.zoomeye.org/ domains and host exposed in internet similar to shodan
7. https://osint.sh/subdomain/ find subdomains 
8. https://osint.sh/dnshistory/ History of a dns record
