# Data Security: Self-Paced Lab

## Overview

Welcome to the Wiz DSPM lab. In this lab, you will:
- Review key concepts from the 100 DSPM in Wiz Overview and Demo session.
- Gain hands on experience with the **Explorer > Data Findings** page to find specific information.
- Gain hands on experience with the **Explorer > Security Graph** page to find specific information.
- Learn to filter views and answer specific questions.

## Pre-requisites

The following is required to perform this lab: 
- A laptop with internet access.
- A user account with access to demo.wiz.io tenant.

Before you begin, verify that you have access to a Wiz tenant with a user account and user role that has permission to query the graph and save queries across all environments. The following roles can save a graph query: 

- Project Graph Reader
- Project Member
- Project Admin
- Global Graph Reader
- Global Responder
- Global Contributor
- Global Admin

## Overall time commitment
The individual participant should be able to complete this session in approximately 55 minutes. However, reviewing the results with your cohort may take additional time.

## Instructions
Complete each section accordingly:
- **Seek & Find.** Write down where in the Wiz portal you can find the answer. You can specify to the nearly page and section.  Review with your cohort.
- **Graph Query.** Review each task and navigate to the answer in demo.wiz.io. Because the cloud is dynamic and new findings are inevitable, answers should not be "specific." Instead, they should be in the form of navigation Condition + Property filter(s) + narrowing condition + property filter(s) + etc. (if helpful).  You may use any accelerators or tips to initiate your queries.


## Exercise 1. Seek & Find
1. In the Wiz portal, where can you review data findings by country?
2. How do you find which data volumes on a VM actually have a data finding? 
3. Where do you define a custom classifier?
4. I wanted to exclude scanning of private buckets in a specific region. Where can specify this?
5. How do I quickly see issues that identify data is being actively exfiltrated?

## Exercise 2. Graph Query

1. How many buckets contain sensitive data?
2. How many occurrences of sensitive data in publicly accessible buckets can you find?
3. Build a query that finds all buckets that host sensitive data where that data is not encrypted on the bucket.
4. Building on the last question, what can you add to the query to find all buckets that host sensitive data where that data is not encrypted on the bucket and it is validated as accessible from the internet from a web browser?
5. How many buckets and database servers contain sensitive data?
6. Find all resources with secrets. Consider the resources scoped for sensitive data scans (as opposed to Workload scans)
7. How many resources have secrets where the resources are scoped for sensitive data scans as opposed to Workload scans?
8. When scanning for sensitive data on buckets, Wiz looks for PII, PCI, PHI, and secrets.
9. A sensitive data finding includes secret instances.

# Exercise Answers

## Exercise 1. Seek & Find
1. In the Wiz portal, where can you review data findings by country?
A: You can get there a few ways. On the Data Security dashbaord there is a locations widget. You can also go to **Explorer > Data Findings** and filter by **Location**. 
https://demo.wiz.io/data-findings#~(groupBy~'location~filters~())
2. How do you find which data volumes on a VM actually have a data finding? 
A: Looking at the workload, you can see the data findings and the path where the finding lives. However, you cannot directly determine which volume is affected; it requires working knowledge of the environment. For example, if the path is /share/users/*, then it is likely an NFS share and a noo-boot volume. 
3. Where do you define a custom classifier?
A: Policies > Data Classification Rules > Create Data Classification Rule
4. I wanted to exclude scanning of private buckets in a specific region. Where can specify this?
A: Settings > Scanners > Data Security. Under Buckets, select Scope = Scan all resources, excluding ones matching tag, subscription, region or project and specify the regions to exclude.
5. How do I quickly see issues that identify data is being actively exfiltrated?
A: Go to Issues and select Risk = Data Leakage.

## Exercise 2. Graph Query

1. Using the bucket node, look for data findings. Be sure to aggregate on the findings, since we just care about the bucket count.
   A: Start with the Bucket node, add data finding (as a bucket is a workload), aggregate on the findings. 

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true~aggregate~true)))))

Additionally, you can look at all data findings that alerted on a Bucket. You can hide the finding or aggregate. 

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET)~select~true)))))

2. How many occurrences of sensitive data in publicly accessible buckets can you find?

To assess publically accessible, do one of two things: add the Application Endpoint node to the bucket or filter to Internet Exposure property = True (Currently, there is an issue on buckets where only Wide Internet Exposure is getting set correctly --- so Application endpoint is probably more accruate.)

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true))~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true)))))

You can also achieve the same result by using:

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'ENDPOINT)~select~true~relationships~(~(type~(~(type~'SERVES~reverse~true))~with~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true))))))))

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true))))))))

3. Build a query that finds all buckets that host sensitive data where that data is not encrypted on the bucket.

The attribute about encryption at rest is one that applies to the bucket, not to the data. 
https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true)))~where~(encrypted~(EQUALS~false))))

4. Building on the last question, what can you add to the query to find all buckets that host sensitive data where that data is not encrypted on the bucket and it is validated as accessible from the internet from a web browser?

Full query:

https://demo.wiz.io/graph#~(query~(type~(~'BUCKET)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true))~(type~(~(type~'SERVES))~with~(type~(~'ENDPOINT)~select~true~where~(portValidationResult~(EQUALS~(~'Open))~httpGETStatus~(EQUALS~(~'200))))))~where~(encrypted~(EQUALS~false))))

5. How many buckets and database servers contain sensitive data?

A database server is a first-class object representing a workload that hosts one or more databases.

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'BUCKET~'DB_SERVER)~select~true~relationships~(~(type~(~(type~'HAS_DATA_FINDING))~with~(type~(~'DATA_FINDING)~select~true~aggregate~true)))))

You can also achieve the same result by running this query:

https://demo.wiz.io/p/ebclabs/graph#~(query~(type~(~'DATA_FINDING)~relationships~(~(type~(~(type~'HAS_DATA_FINDING~reverse~true))~with~(type~(~'BUCKET~'DB_SERVER)~select~true)))))

6. Find all resources with secrets. Consider the resources scoped for sensitive data scans (as opposed to Workload scans)

NOTE from Raph: each acceptable query above generates a different result. Need to narrow down to what we want to see.

As usual, there are multiple queries that result in the correct answer. The key is that we need to consider the resources that we have scoped for scans for sensitive data. The resources that we scan today for secrets are buckets, databases, and database servers. In the near future, we will also scan hosted data volumes (as opposed to OS volumes).

The fastest way to get this answer is actually off of the Data Security dashboard. 

https://demo.wiz.io/graph#~(queryTitle~'Resources*20With*20Secrets~query~(type~(~'BUCKET~'DATABASE~'DB_SERVER)~select~true~relationships~(~(type~(~(type~'CONTAINS))~with~(type~(~'SECRET_INSTANCE)~select~true~aggregate~true))~(type~(~(type~'SERVES))~optional~true~with~(type~(~'ENDPOINT)~select~true)))))

Tip: We've thrown in the optional application endpoint here to show which ones are internet exposed and which are not. However, Application Endpoint is the only way to uniformly assess internet exposure on all three resource types. Buckets do not have the flags of Internet Exposure or Wide Internet Exposure. 

7. How many resources have secrets where the resources are scoped for sensitive data scans as opposed to Workload scans?

Tricky question for sure. The new object "Data Resource" can be used, but it also scans OS volumes, so how do we get the Data Scan security tool results? Find the results and open one that is a data scan and filter to objects like this. 

https://demo.wiz.io/p/ebclabs/graph#~(queryTitle~'Publicly*20Exposed~query~(type~(~'DATA_RESOURCE)~select~true~relationships~(~(type~(~(type~'SCANNED~reverse~true))~with~(type~(~'SECURITY_TOOL_SCAN)~where~(dataSource_name~(EQUALS~(~'Wiz*20Data*20Scanner)))))~(type~(~(type~'CONTAINS))~with~(type~(~'SECRET_INSTANCE)~relationships~(~(type~(~(type~'INSTANCE_OF))~with~(type~(~'SECRET_DATA)~where~(type~(EQUALS~(~'SecretTypePrivateKey~'SecretTypeDBConnectionString~'SecretTypeGitCredential~'SecretTypePresignedURL~'SecretTypeSaasAPIKey~'SecretTypeCloudKey)))))))))))

8. When scanning for sensitive data on buckets, Wiz looks for PII, PCI, PHI, and secrets. (T/F)

True. Wiz does scan for malware on storage buckets. For buckets, this functionaly requires enabling DSPM, and it is subject to DSPM bucket scanning process and limits. The detections are based on etag (MD5 hash).

9. A sensitive data finding includes secret instances. (T/F)

False. Data findings do not include secrets, only the PII, PHI, PCI type information. For secret instances, you must still look for actual instances on the correct resources.

While Data Security includes secrets in the sensitive data concept, queries match against secrets using the secret instance node. To ensure you find the correct results, change the scope of the objects/resources in question--for secret instances on on the "data resources" as opposed to the workloads. 
