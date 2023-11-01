## Exercise: Create a metadata classifier

### Scope

In this exercise, we will create a custom metadata classifier that finds any files with "draft-patent-filing" in the title. The rule should generate data finding if such a file is detected. We will define the classifier severity level as "Critical" to reflect the sensitve nature of the content. 

Once this rule is completed, we will rescan a bucket resource to see if our rule fires on any of its contents.

### Expected Outcomes

Once the rule is defined and the bucket is rescanned, we will see some findings for the metadata classifier. 

### Steps

1. In the Wiz portal, click **Policies > Data Classification Rules**, and then click the **Create New Data Classification Rule** button.

<u>Expeted Result</u> The New Data Classification Rule page appears. 
