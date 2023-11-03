# DSPM: Defining Custom Classifiers

We’re going to work through several custom classifiers to develop an understanding of what Wiz can detect and how to configure the various rule tyles. We use several scenarios for background use cases of a data match or metadata match rule. We also learn to leverage findings from these rules to create controls that solve more complex use cases. So let’s get started!

In this lab, we will develop custom data classifiers to generate findings in the WizLabs environment. 

## Instructions

Consider the scenarios and exercises and follow the steps to define the correct regular expression (RegEx) for a matching rule. For RegEx, we recommend using a tool like [RegEx 101](https://regex101.com/) (select the Golang flavor) to define valid syntax. Because we perform data sampling, we'll set the alert threshold to 1 to ensure our lab results are successful.

For a refresher on DSPM features in Wiz, see the following:
- [Data Security Docs](https://docs.wiz.io/wiz-docs/docs/data-sec)
- [Data Scan Objects on the Graph](https://docs.wiz.io/wiz-docs/docs/data-security#data-scan-objects)

## Scenario

You work for a military contractor that is required to tag all content with an appropriate sensitivity level. As they do not work on top secret projects, they are only required to tag  content using a "Secret", "Confidential", or "Unclassified" sensitivity label. There is an S3 bucket, named s3-fileshare-rbm, where they have been storing files.
