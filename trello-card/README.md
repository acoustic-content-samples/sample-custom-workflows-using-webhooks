# Create a Trello card to translate a content item

This is a tutorial about setting up Zapier to send an email to add a Trello card when a translator needs to translate some content.

## Create a Zap.

The Zap uses three steps. 
- The first step catches the webhook. 
- The second step creates a Trello card to add a TODO for the French translator to translate the new item into French. 
- The third step does the same for German.

### Catch the webhook

- Create a webhook step in Zapier. 
- Choose to Catch a hook. Copy the URL of the webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a webhook, and paste the copied URL.
- Set the filters as event type "Create" and "Update" and item type "Content".
- Set the tag filter as "Translate". When the creator of the new item is ready for the item to be translated, they add this tag. 
- Add the webhook.
- Navigate to the WCH "All content and assets" view and create a new content item and add the tag "Translate" for a dummy item. 

Zapier should detect the webhook and make the data from the webhook available for use while adding the next steps. 

### Add the step to create a Trello card for the French translation

- Sign up to a free Trello account or use your existing Trello account.
- Create a new Zapier Trello step. 
- Select to create a Card.
- Connect Zapier to your Trello account. 
- Select a board and the TODO list.
- Set a card title such as "Translate `<docName>` into French" where docName is a variable from the webhook - the content name.
- set a card description such as 
"Go to `https://www.customer-engagement.ibm.com/content-hub/Authoring/Content/All-Content-And-Assets/Edit/?contentId=<docId>`.
- Translate the content into French" where docId is a variable from the webhook.
- the content Id. 
- Select other options such as the label color, member (the user who owns the Trello action) and due date.

### Add the step to create a Trello card for the German translation

- Repeat these steps for the German translation.
- Use a different label color and member (the user who owns the Trello action).

And, you're done! Give it a test. 

## Going further

You could use the Send notification example in this repo to also send an email to the translator, or the Slack notification example to send them a slack notification as well. 

Other parts of the translation workflow could be automated in the same way. For example, when all the translations are completed, a Trello card could be created to initiate an approval workflow or publish all of the items. That could happen manually in the WCH Hub, or the WCH APIs could be used to make that happen automatically. 

In this way, you can build a fully flexible translation workflow based on your companies processes and tools.

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
