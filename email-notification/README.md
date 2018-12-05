# Send an email to the approvers when a review is created

This is a tutorial about setting up Zapier to send an email to the review approvers when a review is created.

## Create a Zap.

The Zap uses three steps. 
- The first step catches the webhook. 
- The second step is a Code step that extracts the date we need from the  JSON in the webhook, in particular, the approver's email address. 
- The third step sends an email to the approver. 

### Catch the webhook

- Create a webhook step in Zapier. 
- Choose to Catch a hook. Copy the URL of the webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a webhook, and paste the copied URL.
- Set the filters as "Create" and "Review". 
- Add the webhook.
- Navigate to the WCH "All content and assets" view and create a new review for a dummy draft item. 

Zapier should detect the webhook and make the data from the webhook available for use while adding the next steps. 

### Add a code step

- Add a Zapier code step.
- Select JavaScript.
- Set the input data. Extract the fields from the webhook that you want to include in your email. In this example, we'll extract the fields `<docName>` and `<docApprovers>`. So, enter docName as the input data field name, and select the Doc Name variable from the webhook. Repeat this for DocApprovers.
- Add the code:

`var emailAddress = inputData.docApprovers.split("\n")[0].split(" ")[1];
output = [{emailAddress: emailAddress, docName: inputData.docName}];`

Zapier converts the docApprovers field from an object in JSON to a string with properties separated by a new line and the keys and values separated by a space. The code parses that string to get the email address out. There doesn't seem to be a way to get the original JSON from the webhook. This is an example of where Zapier feels much more awkward than using the IBM Cloud and just writing code. Extracting all of the approvers (in case there are multiple) would be more awkward again. Once things get more complicated, using a coded solution such as the IBM Cloud rather than a low-code solution like Zapier is going to be more efficient for those with access to a developer.

### Add a send email step

- Add a Zapier send email step.
- Set the To field as `<emailAddress>`, where the property emailAddress comes from the previous Code step. 
- Set the subject field as "Your approval required for `<docName>`", where the property docName comes from the previous Code step. 
- Set the body field as:

`"A Review had been requested for <docName>
<a href="https://wch-dev.au.ibm.com/#/Content/My-Work">Click here</a> to review the items."` 
where the property docName comes from the previous Code step. 

And, you're done. Turn on your Zap, and give it a test!

## Going further

This example can be used for lots of other similar use cases. For example, an email could be sent when to the creator of an item when another user modifies or deletes the item. 

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
