# DSPM: Defining Custom Classifiers

We’re going to work through several custom classifiers so that you get a good feel for what we can detect and how to configure the rules. We will have several scenarios where we need to define either a data match rule or a metadata match rule. We will also see how to leverage findings from these rules to create controls that solve for more complex problems. So let’s get started!

In this lab, we will develop custom data classifiers to generate findings in the WizLabs environment. 

## Instructions

Consider the scenarios and exercises and either use the correct regular expression (RegEx) to define a matching rule. For RegEx, be sure to use a tool like [RegEx 101](https://regex101.com/) (select the Golang flavor) to define valid syntax. Because we perform data sampling, set the alert threshold to 1 and specify the data classifier severity at high or critical, as you see fit in the scenario.

For a refresher on some of the DSPM features in Wiz, see the following:
- [Data Security Docs](https://docs.wiz.io/wiz-docs/docs/data-sec)
- [Data Scan Objects on the Graph](https://docs.wiz.io/wiz-docs/docs/data-security#data-scan-objects)

## Scenario

You work for a military contractor that is required to tag all produced content with with an appropriate senstivity level. As they do not work on top secret projects, they are only required to tag using a "Secret", "Confidential", or "Unclassified" sensitivity label. 
