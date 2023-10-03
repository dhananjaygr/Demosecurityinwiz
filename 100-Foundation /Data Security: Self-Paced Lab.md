# Data Security: Self-Paced Lab

## Overview

Welcome to the Wiz DSPM lab. In this lab, you will:
- Review key concepts from the 100 DSPM in Wiz Overview and Demo session.
- Gain hands on experience with the  **Explorer > Data Findings** page to find specific information.
- Gain hands on experience with the  **Explorer > Security Graph** page to find specific information.
- Learn to filter views and answer specific questions.

## Pre-requisites

The following is required to perform this lab: 
- A laptop with internet access.
- A user account with access to demo.wiz.io tenant.

Before you begin, verify that you have access to a Wiz tenant with a user account and user role that has permission to query the graph and save queries across all environments. The following roles can save a graph query: 

-  Project Graph Reader
-  Project Member
-  Project Admin
-  Global Graph Reader
-  Global Responder
-  Global Contributor
-  Global Admin

## Overall time commitment
The individual participant should be able to complete this session in approximately 55 minutes. However, reviewing the results with your cohort may take additional time.

## Instructions


## Exercise 1. Concept Review

1. What two types of settings need to be configured to enable DSPM in Wiz?
2. How do the Wiz DSPM features fit within the broader context of data security?
3. What are the key graph resource objects that contain a senstive data findig?
4. What is the difference between the two data-related risk factors: Unprotected data vs. Data exfiltration?
5. What are the two considerations in a data finding's severity level?


## Exercise 2. Seek & Find
1. In the Wiz portal, where can you review
2. How do you isolate the data volumes on a VM to see them in the Security Graph?
3. TBW
4. TBW
5. TBW

## Exercise 3. Graph Query

1. How many buckets contain sensitive data?
2. How many occurrences of sensitive data in publicly accessible buckets can you find?
3. Build a query that finds all buckets that host sensitive data where that data is not encrypted on the bucket.
4. Building on the last question,  what can you add to the query to find all buckets that host sensitive data where that data is not encrypted on the bucket and it is validated as accessible from the internet from a web browser?
5. How many buckets and database servers contain sensitive data?
6. Find all resources with secrets. Consider the resources scoped for sensitive data scans (as opposed to Workload scans)
7. How many resources have secrets where the resources are scoped for sensitive data scans as opposed to Workload scans?
8. When scanning for sensitive data on buckets, Wiz looks for PII, PCI, PHI, and secrets.
9. A sensitive data finding includes secret instances.

# Exercise Answers

## Exercise 1. Concept Review

1. What two types of settings need to be configured to enable DSPM in Wiz?
A: 1. Turn on the desired DSPM scanner settings to instruct the Wiz backend as to which types of resources to scan for senstive data. 2. Ensure that you enable DSPM permissions settings for the connector. This setting enables permissions in the customer tenant that allow Wiz to perform the data scans.
2. How do the Wiz DSPM features fit within the broader context of data security?
A: Wiz helps detect sensitive data and contextualize the risk to that data so you can proactively porrect sensitive and regulated data. It is not a data governance solution.
3. What are the key graph resource objects that contain a senstive data findig?
A: Answers include: data store, data schema, buckets, databases, database servers, data resources (generic term to collect them all).
BONUS: Why doesn't a volume have a data finding?  Becasue all findings other than CCR findings, are attached to the workload itself.`
4. What is the difference between the two data-related risk factors: Unprotected data vs. Data exfiltration?
A: Data exfiltration is a risk that has been futher qualified with real-time attack data from either a CSP Scanner Sevice (such as Azure Defender for Cloud, Amazon GuardDuty, or Google Security Command Center).
5. What are the two considerations in a data finding's severity level?
A: Wiz assigns critical, high, medium, or low severities to Data Findings based on two factors:
- Data Classifier severity
- Number of unique matches within the file (e.g., a leaked file with 1000 email addresses poses a greater risk than a file with just three addresses)
The higher both of these factors are, the higher the severity. If the same data classifier appears in more than one file, Wiz will generate the finding severity according to the maximum number of unique matches.
For more information, see: https://docs.wiz.io/wiz-docs/docs/data-security#severity

## Exercise 2. Seek & Find

## Exercise 3. Graph Query

1. Using the bucket node, look for data findings. Be sure to aggregate on the findings, since we just care about the bucket count.

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true~aggregate~true)))))

Additionally, you can look at all data findings that alerted on a Bucket. You can hide the finding or aggregate. 

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET)~select~true)))))

2. How many occurrences of sensitive data in publicly accessible buckets can you find?

To assess internet accessibility for a bucket, use the Application Endpoint node. 

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true))~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true)))))

You can also achieve the same result by using:

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'ENDPOINT)~select~true~relationships~(~(type~(~(type~'SERVES~reverse~true))~with~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true))))))))

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true))))))))

3. Build a query that finds all buckets that host sensitive data where that data is not encrypted on the bucket.

The attribute about encryption at rest is one that applies to the bucket, not to the data. 
https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true)))~where~(encrypted~(EQUALS~false))))

4. Building on the last question,  what can you add to the query to find all buckets that host sensitive data where that data is not encrypted on the bucket and it is validated as accessible from the internet from a web browser?

Full query:

https://demo.wiz.io/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true))~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true~where~(portValidationResult~(EQUALS~(~'Open))~httpGETStatus~(EQUALS~(~'200))))))~where~(encrypted~(EQUALS~false))))

5. How many buckets and database servers contain sensitive data?

A database server is a first class object representing a workload that hosts one or more databases.

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET~'DB_SERVER)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true~aggregate~true)))))

You can also achieve the same result by running this query:

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET~'DB_SERVER)~select~true)))))

6. Find all resources with secrets. Consider the resources scoped for sensitive data scans (as opposed to Workload scans)

NOTE from Raph: each acceptable query above generates a different result. Need to narrow down to what we want to see.

As usual, there are multiple queries that result in the correct answer.  The key is that we need to consider the resources that we have scoped for scans for sensitive data. The resources that we scan today for secrets are buckets, databases, and database servers. In the near future, we will also scan  hosted data volumes (as opposed to OS volumes).

The fastest way to get this answer is actually off of the Data Security dashboard. 

https://demo.wiz.io/graph#~(queryTitle~'Resources*20With*20Secrets~query~(type~(~'BUCKET~'DATABASE~'DB_SERVER)~select~true~relationships~(~(type~(~(type~'CONTAINS))~with~(type~(~'SECRET_INSTANCE)~select~true~aggregate~true))~(type~(~(type~'SERVES))~optional~true~with~(type~(~'ENDPOINT)~select~true)))))

Tip: We've thrown in the optional application endpoint here to show which ones are internet exposed and which are not. However, Application Endpoint is the only way to uniformly assess internet exposure on all three resource types. Buckets do not have the flags of Internet Exposure or Wide Internet Exposure. 

7. How many resources have secrets where the resources are scoped for sensitive data scans as opposed to Workload scans?

Tricky question for sure.  The new object "Data Resource" can be used, but it also scans OS volumes, so how do we get the Data Scan security tool results? Find the results and open one that is a data scan and filter to objects like this. 

https://demo.wiz.io/p/ebclabs/graph#~(queryTitle~'Publicly*20Exposed~query~(type~(~'DATA_RESOURCE)~select~true~relationships~(~(type~(~(type~'SCANNED~reverse~true))~with~(type~(~'SECURITY_TOOL_SCAN)~where~(dataSource_name~(EQUALS~(~'Wiz*20Data*20Scanner)))))~(type~(~(type~'CONTAINS))~with~(type~(~'SECRET_INSTANCE)~relationships~(~(type~(~(type~'INSTANCE_OF))~with~(type~(~'SECRET_DATA)~where~(type~(EQUALS~(~'SecretTypePrivateKey~'SecretTypeDBConnectionString~'SecretTypeGitCredential~'SecretTypePresignedURL~'SecretTypeSaasAPIKey~'SecretTypeCloudKey)))))))))))

8. When scanning for sensitive data on buckets, Wiz looks for PII, PCI, PHI, and secrets. (T/F)

True. Wiz does scan for malware on storage buckets. For buckets, this functionaly requires enabling DSPM, and it is subject to DSPM bucket scanning process and limits. The detections are based on etag (MD5 hash).

9. A sensitive data finding includes secret instances. (T/F)

False. Data findings do not include secrets, only the PII, PHI, PCI type information. For secret instances, you must still look for actual instances on the correct resources.

While Data Security includes secrets in the sensitive data concept, queries match against secrets using the secret instance node. To ensure you find the correct results,  change the scope of the objects/resources in question--for secret instances on on the "data resources" as opposed to the workloads. 
