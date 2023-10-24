# DSPM: Defining Custom Classifiers

In this lab, we will develop custom data classifiers to generate findings in either the CSAPROD or WizLabs test environment. Consider the scenarios and exercises and use ChatGPT or another RegEx tool to define matching text. Because we perform data sampling, set the alert threshold to 1 and specify the data classifier severity at high or critical, as you see fit in the scenario.

For a refresher on some of the DSPM features in Wiz, see the following:

- [Data Security Docs](https://docs.wiz.io/wiz-docs/docs/data-sec)

*Scenario A:* You work for an organization that tags it's intellectual property with "Company Confidential – Internal Only".

*Scenario B:* You work for a military contractor that is required to tag all content with a "Secret", "Classified", or "Unclassified" sensitivity label. 


## Exercise:

1. *Scenario A:*  Create one ore more custom classifiers to generate a data finding when protected data is detected.
2. *Scenario B:* Generate one or more custom classifiers that generates a data finding when a file does not contain a required sensitivity level.
3. *Scenario B:* Your managers asks you to ensure all files that stored with a sensitivity label. Generate a control that alerts when a bucket contains a file that does not contain one of the required sensitivity levels.
