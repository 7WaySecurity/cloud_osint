# **Cloud OSINT**

A repository with information related to different resources, tools, and techniques associated with Cloud OSINT

## **Cloud Infrastructure**

### **Azure Storage**

* Blob storage: http://*mystorageaccount*.blob.core.windows.net
* Table storage: http://*mystorageaccount*.table.core.windows.net
* Queue storage: http://*mystorageaccount*.queue.core.windows.net
* Azure Files: http://*mystorageaccount*.file.core.windows.net
* Database: http://*mystorageaccount*.database.windows.net

### **AWS Regions**

* af-south-1
* ap-east-1
* ap-northeast-1
* ap-northeast-2
* ap-northeast-3
* ap-south-1
* ap-south-2
* ap-southeast-1
* ap-southeast-2
* ap-southeast-3
* ap-southeast-4
* ap-southeast-5
* ap-southeast-7
* ca-central-1
* ca-west-1
* cn-north-1
* cn-northwest-1
* eu-central-1
* eu-central-2
* eu-north-1
* eu-south-1
* eu-south-2
* eu-west-1
* eu-west-2
* eu-west-3
* il-central-1
* me-central-1
* me-south-1
* mx-central-1
* sa-east-1
* us-east-1
* us-east-2
* us-gov-east-1
* us-gov-west-1
* us-west-1
* us-west-2

### **AWS S3 Buckets**

* https://[bucketname].s3.amazonaws.com
* https://s3-[region].amazonaws/[bucketname]/
* https://[bucketname].s3-website-[region].amazonaws.com/

### **AWS SQS**

https://sqs.[region].amazonaws.com

### **GCP Technologies**
* Technologies Cheatsheet - https://googlecloudcheatsheet.withgoogle.com
* GCP Regions and Zones - https://cloud.google.com/compute/docs/regions-zones

### **IBM Cloud**
* IBM Global Cloud Data Centers - https://www.ibm.com/cloud/data-centers
* IBM Cloud IP ranges - https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges

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
* Azure SAS Tokens - "bfqt&srt"
```
### **AWS**

```
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
```
### **Google Cloud**

```
* site:googleapis.com +commondatastorage
* site:.firebaseio.com "COMPANY NAME" 
* inurl:bc.googleusercontent.com intitle:index of  
* site:storage.googleapis.com
* Bucket list for a project - site:console.cloud.google.com/storage/browser
* Details for an object - site:console.cloud.google.com/storage/browser/_details
* site:firebasestorage.googleapis.com
```
### **IBM Cloud**
```
* site:appdomain.cloud
* site:appdomain.cloud +s3
* site:*cloud-object-storage.appdomain.cloud
* site:codeengine.appdomain.cloud
* site:containers.appdomain.cloud
* site:clb.appdomain.cloud
* site:apiconnect.appdomain.cloud
* site:cdn.appdomain.cloud
* site:lb.appdomain.cloud
* site:vmware.cloud.ibm.com - VMware Cloud Director Availability
* site:appid.cloud.ibm.com - IBM Cloud App ID Management Configuration APIs and AppID Authentication Portals
* site:site:ibmmarketingcloud.com
```
### **Miscellaneous Services**

* site:notion.site "keyword"

## **Shodan Dorks**

### **Filter Reference**

* cloud.provider
* cloud.region
* cloud.service

### **Azure**

* cloud.service:"azureCloud"
* cloud.service:"azureCloud" country:GB,US http.title:"swagger" http.status:200 - API Documentation
* cloud.service:"azureCloud" http.status:200 country:GB,US -http.title:"Your Azure Function App is up and running." -http.title"IIS Windows Server“ - Web Services that are not default splash pages
* cloud.provider:"Azure" country:GB,US http.status:200 http.title:"Index of /" ssl:true - Web Apps with directory listings enabled and SSL
* cloud.provider:"Azure" country:GB,US http.status:200 http.title:"Index of /" - Web Apps with directory listings enabled
* cloud.provider:"Azure" hostname:"cloudapp.net" http.status:200,302 - Cloud Apps
* cloud.service:"AzureCloud" http.status:200 http.title:"api" - APIs

### **Amazon**

* cloud.provider:"Amazon"
* cloud.provider:"Amazon" http.status:200,302 http.title:"Index of /"

### Other Cloud Services

* site:vps-*.vps.ovh.net

## **Web Cloud OSINT Resources**

1. Search Open Buckets - https://buckets.grayhatwarfare.com/ 
2. Search cloud storage and buckets in different cloud providers - https://cse.google.com/cse?cx=002972716746423218710:veac6ui3rio#gsc.tab=0&gsc.q=
3. FullHunt - https://fullhunt.io/search?query=is_cloud%3Atrue+*domain*
4. Comand to download the results of URLs and buckets that contain a specific word and the file is .docx, xlsx and pdf - curl "https://buckets.grayhatwarfare.com/api/v1/files/[WORD TO SEARCH]?access_token=[access_token]&extensions=docx,xlsx,pdf"
5. Azure Tenant Information including subdomains and configuration - https://aadinternals.com/osint/
6. Misconfigured servers containing sensitive data, including Azure Blob Storage, Amazon AWS S3 Buckets, and Google Buckets - https://socradar.io/labs/bluebleed
7. Cloud and other services key exposure - https://forager.trufflesecurity.com/explore
8. AWS Eye is an OSINT (Open Source Intelligence) tool designed to investigate Amazon Web Services (AWS) configurations. It specializes in identifying and analyzing publicly misconfigured resources such as S3 buckets, helping security researchers and OSINT enthusiasts uncover potential cloud exposures efficiently - https://awseye.com/


## **Cloud OSINT Tools**

1. CloudEnum - https://github.com/initstring/cloud_enum
2. S3 Browser - https://s3browser.com - This is not properly a tool for OSINT tasks but is a Windows client for Amazon S3 and Amazon CloudFront that could help to browse some files.

## **Domain Identification**

1. https://spyse.com/tools/subdomain-finder domain and subdomain enumeration 
2. https://crt.sh Finding domains and subdomains by SSL certificates through certificate transparency
3. https://dnsdumpster.com/ domain and subdomain enumeration 
4. https://osint.sh/subdomain/ domain and subdomain enumeration 
5. https://search.censys.io/?q= queries by domain, host, SSL certificate, among others
6. https://www.zoomeye.org/ domains and host exposed on the internet similar to Shodan
7. https://osint.sh/subdomain/ find subdomains 
8. https://osint.sh/dnshistory/ History of a DNS record

## **Help to create your own dorks**
1. https://dorksearch.com/
2. https://www.dorkgpt.com/ dorks with chatgpt

## **Others**

1. https://www.dedigger.com/# find exposed files in Google Drive, try search terms: AWS, azure, gcp, etc.

