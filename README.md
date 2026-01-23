# **Cloud OSINT**

**Cloud OSINT** is a centralized knowledge base for security professionals who need to identify, enumerate, and assess cloud infrastructure using publicly available information. As organizations increasingly migrate critical workloads to multi-cloud environments, the attack surface expands across dozens of services with predictable patterns that can be discovered through passive reconnaissance.
This repository provides actionable intelligence on official IP range publications and BGP/ASN references for major cloud providers, DNS naming conventions and subdomain patterns for storage, compute, and serverless services, certificate transparency mining techniques for discovering cloud-hosted assets, tools and databases for detecting misconfigured buckets, containers, and exposed services, HTTP header signatures and metadata endpoints for cloud provider fingerprinting, Shodan, Censys, and specialized search engine queries optimized for cloud discovery, and provider-specific reconnaissance methodologies for AWS, Azure, GCP, and secondary providers.
Whether you are conducting authorized penetration tests, building threat intelligence capabilities, performing attack surface management, or hunting for bug bounties, this resource helps you systematically discover cloud assets that may be invisible to traditional reconnaissance approaches.

## **IP ranges and Network Intelligence**

Cloud providers publish authoritative IP range data that forms the foundation of cloud attribution. These official sources enable accurate IP-to-provider mapping for security monitoring and threat analysis.

### **Official IP range publications**

* AWS - https://ip-ranges.amazonaws.com/ip-ranges.json
* Azure - https://www.microsoft.com/en-us/download/details.aspx?id=56519
* GCP - https://www.gstatic.com/ipranges/cloud.json
* GCP (All cloud solutions) - https://www.gstatic.com/ipranges/goog.json
* Oracle Cloud - https://docs.oracle.com/iaas/tools/public_ip_ranges.json
* Cloudfare - https://api.cloudflare.com/client/v4/ips
* DigitalOcean - https://digitalocean.com/geo/google.csv

### **AWS IP Ranges Parsing Example**

```
curl https://ip-ranges.amazonaws.com/ip-ranges.json | jq -r '.prefixes[] | select(.service=="S3") | .ip_prefix'
curl https://ip-ranges.amazonaws.com/ip-ranges.json | jq -r '.prefixes[] | select(.region=="us-east-1") | .ip_prefix'
```

### **GCP Ranges Parsing**

```
curl https://www.gstatic.com/ipranges/cloud.json | jq '.prefixes[].ipv4Prefix'
```

### **BGP and ASN**

| Provider | Primary ASN | Additional ASNs |
|----------|-------------|-----------------|
| AWS | AS16509 | AS14618 |
| Microsoft Azure | AS8075 | AS8069 |
| Google Cloud | AS15169 | AS396982, AS19527 |
| Oracle Cloud | AS31898 | — |
| Alibaba Cloud | AS45102 | AS37963, AS134963 |
| IBM Cloud | AS36351 | — |
| Cloudflare | AS13335 | — |
| DigitalOcean | AS14061 | — |
| Linode/Akamai | AS63949 | AS12222 |
| Vultr | AS20473 | — |
| OVHcloud | AS16276 | — |
| Hetzner | AS24940 | — |
| Scaleway | AS12876 | AS29447 |

**BGP lookup Tools**

* Hurricane Electric BGP Toolkit — https://bgp.he.net/ — Query `https://bgp.he.net/AS16509` for AWS prefixes
* BGP.Tools — https://bgp.tools/ — Real-time BGP data and ASN search
* PeeringDB — https://www.peeringdb.com/ — Network interconnection data and peering locations

**Third-party IP attribution services**

* IPinfo.io — https://ipinfo.io/
    * Provides `is_hosting` flag for datacenter/cloud detection
    * API: `https://api.ipinfo.io/{ip}?token={TOKEN}`
    * Free tier available with ASN data

* MaxMind GeoIP2 — https://www.maxmind.com/
    * Anonymous IP Database detects VPNs, proxies, hosting providers
    * `is_hosting_provider` field in Enterprise tier
    * GeoLite2 free version available

* IP2Location — https://www.ip2location.com/
    * Usage type includes "DCH" (Data Center/Web Hosting)
    * IP2Proxy database for proxy/VPN detection
    * Daily updates

* Shodan InternetDB — https://internetdb.shodan.io/{ip}
    * Free IP enrichment without API key
    * Returns open ports, hostnames, cloud provider tags

* AbuseIPDB — https://www.abuseipdb.com/
    * `usageType` field returns "Data Center/Web Hosting/Transit"
    * Free tier: 1,000 API requests/day
 
**Aggregated IP range repositories**

* tobilg/public-cloud-provider-ip-ranges — https://github.com/tobilg/public-cloud-provider-ip-ranges
    * Includes DuckDB database for SQL queries
    * Coverage: AWS, Azure, Cloudflare, DigitalOcean, Fastly, GCP, Oracle, Linode

* lord-alfred/ipranges — https://github.com/lord-alfred/ipranges
    * Daily updated lists for Google, AWS, Microsoft, Oracle, DigitalOcean, Linode, Vultr, Cloudflare GitHub

## **DNS and domain intelligence**

Cloud services follow predictable DNS naming patterns that enable passive discovery through subdomain enumeration, certificate transparency mining, and pattern matching.

### **Cloud service DNS patterns**

**Amazon Web Services**

| Pattern | Service |
|---------|---------|
| `{bucket}.s3.amazonaws.com` | S3 Storage |
| `{bucket}.s3.{region}.amazonaws.com` | S3 Regional |
| `*.ec2.{region}.compute.amazonaws.com` | EC2 Instances |
| `*.elasticbeanstalk.com` | Elastic Beanstalk |
| `*.cloudfront.net` | CloudFront CDN |
| `*.execute-api.{region}.amazonaws.com` | API Gateway |
| `{id}.lambda-url.{region}.on.aws` | Lambda URLs |
| `*.awsapps.com` | WorkMail, WorkDocs |
| https://sqs.[region].amazonaws.com | SQS |

**AWS Regions**

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

**Microsoft Azure**

| Pattern | Service |
|---------|---------|
| `{account}.blob.core.windows.net` | Blob Storage |
| `{account}.table.core.windows.net` | Table Storage |
| `{account}.queue.core.windows.net` | Queue Storage |
| `{account}.file.core.windows.net` | Azure Files |
| `{account}.database.windows.net` | Azure SQL |
| `{app}.azurewebsites.net` | App Service |
| `*.cloudapp.azure.com` | Cloud Services |
| `*.azureedge.net` | Azure CDN |

**Google Cloud Platform**

| Pattern | Service |
|---------|---------|
| `{bucket}.storage.googleapis.com` | Cloud Storage |
| `storage.googleapis.com/{bucket}` | Cloud Storage (path) |
| `{service}.run.app` | Cloud Run |
| `{region}-{project}.cloudfunctions.net` | Cloud Functions |
| `{project}.appspot.com` | App Engine |
| `{project}.firebaseio.com` | Firebase |
| `{project}.web.app` | Firebase Hosting |

**GCP Technologies**
* Technologies Cheatsheet - https://googlecloudcheatsheet.withgoogle.com
* GCP Regions and Zones - https://cloud.google.com/compute/docs/regions-zones

**Other Providers**

 Provider | Storage Pattern |
|----------|----------------|
| Oracle | `{bucket}.objectstorage.{region}.oraclecloud.com` |
| DigitalOcean | `{bucket}.{region}.digitaloceanspaces.com` |
| Alibaba | `{bucket}.oss-{region}.aliyuncs.com` |
| IBM | `{bucket}.s3.{region}.cloud-object-storage.appdomain.cloud` |
| Cloudflare R2 | `pub-{hash}.r2.dev` |
| Linode | `{cluster}.linodeobjects.com` |

### **Certificate Transparency Sources**

* **crt.sh** — https://crt.sh
    * Primary CT log search interface
    * Web query: `https://crt.sh/?q=%.example.com`
    * Direct PostgreSQL: `psql -h crt.sh -p 5432 -U guest certwatch`
    * Cloud discovery: `https://crt.sh/?q=%.s3.amazonaws.com&output=json`

* **Censys** — https://search.censys.io/
    * Certificate and host search with cloud provider detection
    * Query: `services.tls.certificates.leaf_data.subject.common_name: *.amazonaws.com`

* **Certstream** — https://certstream.calidog.io/
    * Real-time CT log feed via WebSocket
    * Python library: `pip install certstream`
    * Use case: Monitor for new cloud certificates as they're issued

### **Passive DNS Services**

* **Farsight DNSDB** — https://www.domaintools.com/
    * Largest passive DNS database with 100+ billion records
    * Commercial with community edition available

* **SecurityTrails** — https://securitytrails.com/
    * Historical DNS, subdomain API
    * Free tier: 50 requests/month
    * API: `https://api.securitytrails.com/v1/domain/{domain}/subdomains`

* **Chaos by ProjectDiscovery** — https://chaos.projectdiscovery.io/
    * Continuously updated DNS dataset
    * Free API key via cloud.projectdiscovery.io

## **Misconfiguration detection**

Exposed cloud storage buckets and misconfigured services represent significant security risks. These tools and databases help identify vulnerable assets.

### Bucket discovery tools

* **GrayhatWarfare** — https://buckets.grayhatwarfare.com/
    * Database of **470,600+ buckets** and **15.2 billion+ files**
    * Coverage: 313,218 S3 buckets, 64,323 Azure containers, 81,303 GCP buckets
    * API for automation, regex search support
    * Free tier with premium options (~$25/month)

* **S3Scanner** — https://github.com/sa7mon/S3Scanner
    * Multi-provider scanner: AWS, GCP, DigitalOcean, Linode, Scaleway
    * Commands:
```bash
s3scanner -bucket-file names.txt -enumerate  # List objects
s3scanner -provider gcp -bucket-file names.txt  # GCP buckets
```

* **GCPBucketBrute** — https://github.com/RhinoSecurityLabs/GCPBucketBrute
    * GCS bucket enumeration with privilege escalation detection
    * Command: `python3 gcpbucketbrute.py -k keyword -u` (unauthenticated)

* **CloudBrute** — https://github.com/0xsha/CloudBrute
    * Multi-cloud: AWS, GCP, Azure, DigitalOcean, Alibaba, Vultr, Linode
    * Command: `cloudbrute -d domain.com -k keyword -w wordlist.txt -c aws`

* **BucketLoot** — https://github.com/redhuntlabs/BucketLoot
    * Automated S3-compatible bucket inspector for secrets
    * Featured at Black Hat Arsenal 2023-2024

* **BlobHunter** — https://github.com/cyberark/BlobHunter
    * Azure-specific blob container scanner
    * Requires Azure credentials with storage permissions

* **MicroBurst** — https://github.com/NetSPI/MicroBurst
    * Comprehensive Azure security toolkit
    * Commands:
```powershell
Import-Module .\MicroBurst.psm1
Invoke-EnumerateAzureBlobs -Base companyname
Invoke-EnumerateAzureSubDomains -Base companyname
```

* **AWS Eye** — https://awseye.com/
    * Web-based OSINT tool for misconfigured S3 buckets
    * Free search interface
 
### Exposure databases and services

* **LeakIX** — https://leakix.net/
    * Exposed services database with cloud plugins
    * Query syntax (YQL): `plugin:GitConfigPlugin`, `protocol:mysql`, `asn:16509`
    * 15-day responsible disclosure delay

**PublicWWW** — https://publicwww.com/
- Source code search for cloud storage references
- Search: `"s3.amazonaws.com"`, `"blob.core.windows.net"`

### **Google dorks**

**Azure**
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
**AWS**
```
* site:"s3-external-1.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3.dualstack.us-east-1.amazonaws.com" and intext:CONFIDENTIAL
* site:"s3-external-1.amazonaws.com" and intext:"TOP SECRET"
* site:"s3.amazonaws.com" and intext:"tlp:red"
* site:s3.amazonaws.com example
* site:s3.amazonaws.com example.com
* site:s3.amazonaws.com example-com
* site:s3.amazonaws.com com.example
* site:s3.amazonaws.com com-example
* site:s3.amazonaws.com filetype:xls password
* site:http://s3.amazonaws.com intitle:index.of.bucket
* site:http://amazonaws.com inurl:".s3.amazonaws.com/"
* s3 site:amazonaws.com filetype:log
* site:s3.amazonaws.com "keyword"
* site:s3.amazonaws.com intitle:index.of.bucket
* site:s3.amazonaws.com filetype:sql
* site:"s3.amazonaws.com" intext:CONFIDENTIAL
* s3 site:amazonaws.com filetype:xls password
```
**Google Cloud**
```
* site:googleapis.com +commondatastorage
* site:.firebaseio.com "COMPANY NAME" 
* inurl:bc.googleusercontent.com intitle:index of  
* Bucket list for a project - site:console.cloud.google.com/storage/browser
* Details for an object - site:console.cloud.google.com/storage/browser/_details
* site:firebasestorage.googleapis.com
* site:storage.googleapis.com "keyword"
* site:storage.cloud.google.com
* site:*.appspot.com
* site:firebaseio.com
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
```
* site:notion.site "keyword"
* site:http://trello.com "aws.amazon.com" "password"
```
## **Cloud Medata and Fingerprint**

HTTP headers, certificates, and metadata endpoints provide reliable indicators for identifying cloud providers and services.

### HTTP headers by provider

**AWS headers:**
| Header | Indicates |
|--------|-----------|
| `x-amz-request-id` | S3/AWS service (16-char alphanumeric) |
| `x-amz-id-2` | S3 extended request ID (Base64) |
| `X-Amz-Cf-Id` | CloudFront CDN |
| `Server: AmazonS3` | S3 bucket hosting |
| `Server: awselb/2.0` | Elastic Load Balancer |
| `X-Amzn-Trace-Id` | AWS X-Ray tracing |

**Azure headers:**
| Header | Indicates |
|--------|-----------|
| `x-ms-request-id` | Azure service (GUID format) |
| `x-ms-version` | Azure API version (date format) |
| `x-azure-ref` | Azure Front Door |
| `x-ms-blob-type` | Azure Blob Storage type |

**GCP headers:**
| Header | Indicates |
|--------|-----------|
| `x-goog-*` | GCP services |
| `x-cloud-trace-context` | Cloud Trace |
| `Server: Google Frontend` | GCP infrastructure |
| `Metadata-Flavor: Google` | GCP metadata (required header) |

**Cloudflare headers:**
| Header | Indicates |
|--------|-----------|
| `cf-ray` | Cloudflare request ID (hash + datacenter) |
| `cf-cache-status` | Cache behavior (HIT/MISS/DYNAMIC) |
| `Server: cloudflare` | Cloudflare proxy |

### Cloud metadata endpoints

**AWS IMDS:**
* Endpoint: `http://169.254.169.254/latest/meta-data/`
* Key paths: `/ami-id`, `/instance-id`, `/iam/security-credentials/`
* IMDSv2 requires token: `curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`

**Azure IMDS:**
* Endpoint: `http://169.254.169.254/metadata/instance?api-version=2021-02-01`
* Required header: `Metadata: true`

**GCP Metadata:**
* Endpoint: `http://metadata.google.internal/computeMetadata/v1/`
* Required header: `Metadata-Flavor: Google`

**DigitalOcean:**
* Endpoint: `http://169.254.169.254/metadata/v1/`

**Alibaba Cloud:**
* Endpoint: `http://100.100.100.200/latest/meta-data/`

### Certificate patterns

**AWS Certificate Authority:**
* Issuer: `CN=Amazon Root CA 1-4, O=Amazon, C=US`
* Types: RSA 2048, RSA 4096, ECDSA P-256, ECDSA P-384

**Detection with Censys:**
```
services.tls.certificates.leaf_data.issuer.organization: "Amazon"
services.tls.certificates.leaf_data.issuer.organization: "Microsoft Corporation"
services.tls.certificates.leaf_data.issuer.organization: "Google Trust Services"
```

### Fingerprinting tools

* **WhatWeb** — https://github.com/urbanadventurer/WhatWeb
    * 1800+ plugins for technology detection
    * Command: `whatweb --aggression=3 target.com`

* **httpx** — https://github.com/projectdiscovery/httpx
    * Fast HTTP probing with tech detection
    * Command: `httpx -u https://target.com -tech-detect -server -title`

* **Nuclei** — https://github.com/projectdiscovery/nuclei-templates
    * YAML-based detection templates
    * Cloud templates: `nuclei -u https://target.com -tags cloud,aws,azure,gcp`

## Search engines and specialized tools

### **Shodan Queries**

* **Cloud Providers Filters**

  * cloud.provider
    * cloud.provider:aws
    * cloud.provider:azure
    * cloud.provider:gcp
  * cloud.region
  * cloud.service

* **Azure**

    * cloud.service:"azureCloud"
    * cloud.service:"azureCloud" country:GB,US http.title:"swagger" http.status:200 - API Documentation
    * cloud.service:"azureCloud" http.status:200 country:GB,US -http.title:"Your Azure Function App is up and running." -http.title"IIS Windows Server“ - Web Services that are not default splash pages
    * cloud.provider:"Azure" country:GB,US http.status:200 http.title:"Index of /" ssl:true - Web Apps with directory listings enabled and SSL
    * cloud.provider:"Azure" country:GB,US http.status:200 http.title:"Index of /" - Web Apps with directory listings enabled
    * cloud.provider:"Azure" hostname:"cloudapp.net" http.status:200,302 - Cloud Apps
    * cloud.service:"AzureCloud" http.status:200 http.title:"api" - APIs
    * org:"Microsoft Azure"
    * hostname:*.azurewebsites.net
    * hostname:*.cloudapp.azure.com

* **AWS Specific**

    * cloud.provider:"Amazon"
    * cloud.provider:"Amazon" http.status:200,302 http.title:"Index of /"
    * org:"Amazon.com"
    * org:"Amazon Web Services"
    * hostname:*.amazonaws.com
    * Server:AmazonS3
    * html:"ListBucketResult"
 
* **GCP**
    
  * org:"Google Cloud"
  * hostname:*.appspot.com
  * hostname:*.googleusercontent.com

* **Exposed Services**
  * product:kubernetes port:6443
  * port:2375 product:docker
  * port:9200 org:"Amazon.com"  # Elasticsearch
  * port:27017 org:"Amazon.com"  # MongoDB

* **Other Cloud Services**

    * site:vps-*.vps.ovh.net

 ### **Censys Queries**

* **Cloud Providers Detection**
  * services.cloud.provider: "AWS"
  * services.cloud.provider: "AZURE"
  * services.cloud.provider: "GCP"
  * autonomous_system.asn: 16509  # AWS
  * autonomous_system.asn: 8075   # Azure
  * autonomous_system.asn: 15169  # GCP

* **Certificate Based**
  * services.tls.certificates.leaf_data.issuer.organization: "Amazon"
  * services.http.response.headers.Server: "AmazonS3"

### **Other search engines**

* **BinaryEdge** — https://www.binaryedge.io/
  * Free: 250 queries/month
  * Query: `asn:AS16509`, `type:elasticsearch`

* **ZoomEye** — https://www.zoomeye.org/
  * China-based, ~1.2 billion indexed devices
  * Query: `app:"Amazon Web Services"`, `os:"Amazon Linux"`

* **FOFA** — https://fofa.so/
  * Query: `server="AmazonS3"`, `header="x-amz-"`, `cert.issuer="Amazon"`

* **Netlas** — https://netlas.io/
  * Python SDK, Maltego integration
  * Query: `http.headers.server:"AmazonS3"`, `cve.name:"CVE-2021-*" AND tag:aws`

* **GreyNoise** — https://www.greynoise.io/
  * Identifies cloud provider scanning traffic
  * GNQL: `metadata.asn:AS16509`, `classification:malicious metadata.cloud_provider:aws`

* **URLScan.io** — https://urlscan.io/
  * Website scanning with ASN identification
  * Query: `asn:AS16509 AND domain:example.com`
 
* **Google CSE** - https://cse.google.com/cse?cx=002972716746423218710:veac6ui3rio#gsc.tab=0&gsc.q=
 
## **Specialized Cloud OSINT Tools**

1. **CloudEnum** - https://github.com/initstring/cloud_enum
- Multi-cloud enumeration for AWS, Azure, GCP
- Command: `./cloud_enum.py -k companyname -k brand`
- Discovers S3 buckets, Azure blobs, GCP buckets, Firebase, App Engine

2. **S3 Browser** - https://s3browser.com - This is not properly a tool for OSINT tasks but is a Windows client for Amazon S3 and Amazon CloudFront that could help to browse some files.

3. **Prowler** — https://github.com/prowler-cloud/prowler
- Cloud security posture management for AWS, Azure, GCP, Kubernetes
- 500+ controls, CIS/NIST/PCI-DSS compliance
- Commands:
```bash
prowler aws  # Full AWS audit
prowler azure  # Azure audit
prowler gcp  # GCP audit
prowler aws --compliance cis_2.0  # Specific framework
```

4. **ScoutSuite** — https://github.com/nccgroup/ScoutSuite
- Multi-cloud security auditing by NCC Group
- Commands:
```bash
scout aws --profile myprofile
scout azure --cli
scout gcp --service-account /path/to/sa.json
```

5. **CloudFox** — https://github.com/BishopFox/cloudfox
- Attack path enumeration for AWS/Azure/GCP
- Commands:
```bash
cloudfox aws all-checks
cloudfox aws principals
cloudfox aws permissions
cloudfox aws secrets
```

6. **Pacu** — https://github.com/RhinoSecurityLabs/pacu
- AWS exploitation framework by Rhino Security Labs
- 36+ modules for enumeration, privilege escalation, persistence
- Commands:
```bash
python3 pacu.py
# In Pacu:
run iam__enum_permissions
run iam__privesc_scan
run s3__bucket_finder -d domain.com
```

7. **ROADtools** — https://github.com/dirkjanm/ROADtools
- Azure AD/Entra exploration toolkit
- Commands:
```bash
roadrecon auth -u user@tenant.onmicrosoft.com
roadrecon gather
roadrecon gui
```

8. **AADInternals** — https://github.com/Gerenios/AADInternals
- Azure AD reconnaissance (requires authentication as of Sept 2025)
- Commands:
```powershell
Invoke-AADIntReconAsOutsider -DomainName "company.com"
Get-AADIntTenantID -Domain company.onmicrosoft.com
```

9. **Cartography** — https://github.com/cartography-cncf/cartography
- Infrastructure graph analysis with Neo4j
- CNCF Sandbox project, originated at Lyft
- Query: `MATCH (ec2:EC2Instance) WHERE ec2.exposed_internet = true RETURN ec2`

10. **Checkov** — https://github.com/bridgecrewio/checkov
- IaC security scanner for Terraform, CloudFormation, Kubernetes
- 750+ built-in policies
- Command: `checkov -d /path/to/terraform`

11. **Cloudsplaining** — https://github.com/salesforce/cloudsplaining
- AWS IAM security assessment
- Detects privilege escalation, data exfiltration permissions


## **Create your own Dorks**
1. https://dorksearch.com/
2. https://www.dorkgpt.com/ dorks with chatgpt

## **Others**

1. https://www.dedigger.com/# find exposed files in Google Drive, try search terms: AWS, azure, gcp, etc.
