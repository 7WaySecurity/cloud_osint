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

### **GCP Technologies**
* Technologies Cheatsheet - https://googlecloudcheatsheet.withgoogle.com
* GCP Regions and Zones - https://cloud.google.com/compute/docs/regions-zones

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
* s3 site:amazonaws.com filetype:log
* site:http://trello.com "aws.amazon.com" "password"

### **Google Cloud**

* site:googleapis.com +commondatastorage
* site:.firebaseio.com "COMPANY NAME" 
* inurl:bc.googleusercontent.com intitle:index of  
* site:storage.googleapis.com
* Bucket list for a project - site:console.cloud.google.com/storage/browser
* Details for an object - site:console.cloud.google.com/storage/browser/_details
* site:firebasestorage.googleapis.com

### Other Cloud Services

* site:vps-*.vps.ovh.net

## **Web Cloud OSINT Resources**

1. Search Open Buckets - https://buckets.grayhatwarfare.com/ 
2. Search cloud storage and buckets in diferent cloud providers - https://cse.google.com/cse?cx=002972716746423218710:veac6ui3rio#gsc.tab=0&gsc.q=
3. FullHunt - https://fullhunt.io/search?query=is_cloud%3Atrue+*domain*
4. Comand to download the results of urls and buckets that contain a especific word and the file is .docx, xlsx and pdf - curl "https://buckets.grayhatwarfare.com/api/v1/files/[WORD TO SEARCH]?access_token=[access_token]&extensions=docx,xlsx,pdf"
5. Azure Tenant Information including subdomains and configuration - https://aadinternals.com/osint/
6. Misconfigured servers containing sensitive data, including Azure Blob Storage, Amazon AWS S3 Buckets, and Google Buckets - https://socradar.io/labs/bluebleed

## **Cloud OSINT Tools**

1. CloudEnum - https://github.com/initstring/cloud_enum
2. S3 Browser - https://s3browser.com - Is not properly a tool for OSINT task but is a Windows client for Amazon S3 and Amazon CloudFront that could help to browse some files.

## **Domain Identification**

1. https://spyse.com/tools/subdomain-finder domain and subdomain enumeration 
2. https://crt.sh Finding domains and subdomains by ssl certificates through certificate transparency
3. https://dnsdumpster.com/ domain and subdomain enumeration 
4. https://osint.sh/subdomain/ domain and subdomain enumeration 
5. https://search.censys.io/?q= queries by domain, host , ssl certificate , among others
6. https://www.zoomeye.org/ domains and host exposed in internet similar to shodan
7. https://osint.sh/subdomain/ find subdomains 
8. https://osint.sh/dnshistory/ History of a dns record

## **Help creating your own dorks**
1. https://dorksearch.com/
2. https://www.dorkgpt.com/ dorks with chatgpt

