# Image scanning<a name="image-scanning"></a>

Amazon ECR image scanning helps in identifying software vulnerabilities in your container images\. The following scanning types are offered\.
+ **Enhanced scanning**—Amazon ECR integrates with Amazon Inspector to provide automated, continuous scanning of your repositories\. Your container images are scanned for both operating systems and programing language package vulnerabilities\. As new vulnerabilities appear, the scan results are updated and Amazon Inspector emits an event to EventBridge to notify you\.
+ **Basic scanning**—Amazon ECR uses the Common Vulnerabilities and Exposures \(CVEs\) database from the open\-source Clair project\. With basic scanning, you configure your repositories to scan on push or you can perform manual scans and Amazon ECR provides a list of scan findings\.

**Topics**
+ [Enhanced scanning](image-scanning-enhanced.md)
+ [Basic scanning](image-scanning-basic.md)