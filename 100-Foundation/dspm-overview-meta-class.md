## Exercise: Create a metadata classifier

### Scope

In this exercise, we will create a custom metadata classifier that finds any files with "draft-patent-filing" in the title. The rule should generate data finding if such a file is detected. We will define the classifier severity level as "Critical" to reflect the sensitve nature of the content. 

Once this rule is completed, we will rescan a bucket resource to see if our rule fires on any of its contents.

### Expected Outcomes

Once the rule is defined and the bucket is rescanned, we will see some findings for the metadata classifier. 

### Steps

1. In the Wiz portal, click **Policies > Data Classification Rules**, and then click the **Create New Data Classification Rule** button.
<br/><ins>Expeted Result:</ins> The New Data Classification Rule page appears. 
2. Under Classification Type, select **Metadata match**.
3. In the Name box, enter a name for this rule using the following format *<login-username>-dspmlab-meta* (for example, odl_user_#####-dspmlab-meta).
4. (Optional) In the Description box, enter a description for the rule.
<br/>This field is useful for providing context for other users. While not necessary for this lab, it is best practice to state the purpose of the rule and use cases that it is expected to address. For example, "Identify working patent documents that are not submitted or granted. The expectation is to prevent any accidental public exposure of this material prior to patent submission when we can lock in the timestamp for submission. Expected to scan Word, PDF, and text file titles with the required title string 'draft-patent-filing' anywhere in the title of the file."
5. From the Data Type dropdown , select **Other**. <br/>
As we are focused on proprietariy information, this rule does not match any known defintions, such as PHI or PII. As these types are used as filters in other pages, you should strive to keep them as accurate as possible.
6. (Optional) Under Framework categories, select the framework and category to which this rule should be aligned.
<br/> Aligning to a compliance framework may be part of your orgnaization-specific policies and governance. You will need to select an existing or custom compliance framework and then align to the correct category, which in this case, is usually Data Security or a sensitive data tracking category.
--INSERT IMAGE HERE--
7. From the Severity dropdown box, select **Critical**.
<br/>Severity of the data classifier is only part of the formula that is used to designate the sevrity of any relatred data finding. Remember that it is the number of unique occurences. 

