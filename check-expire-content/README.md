# Check or expire content or assets on a schedule

This is a tutorial on setting up WCH Webhooks and Zapier to create a Trello card on a schedule that prompts a user to check or expire a content item or asset. For background information on all the tutorials, see: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks.

Common use cases include:
- Where content items and assets should be checked every month, say, for currency.
- Where an expiry date is set on content item when created. When the expiry date arrives, the content item should be removed from the site.

WCH Webhooks can be used to initiate a notification to action the check or expiry. For example, a Trello card can be created and assigned to a user, or an email sent, or a Slack announcement created. The notifications prompt the user to check the content or asset for currency or remove the content or asset from the site. For the expiry case, the content or asset maybe 'in use' in some way, in whihch case the user may replace the item with some other content or asset, or just remove it - this will depend on the way in which the content or asset is used. WCH Webhooks can also be used to automatically update or delete the asset or content item, by calling back into the WCH APIs to do the update or delete. Whether the update or expire is manula or automated will depend on the way in which the content item or asset is used. Doing the action 'manually' has the advantage that this user can use the draft and approval process to check that the removal of the content item or asset does not cause any issues in the website or application. 

Proceed as follows:

## Create a new Zap in Zapier

The Zap will consist of three steps:
- The first step catches the WCH Webhook. 
- The second step creates a delay for an appropriate time period.
- The third step creates a Trello card to add a TODO for the user to check or remove the given content or asset.

For this example we will implement a monthly check of content item and assets. This model can easily be adjusted to deliver the other similar use cases listed above.

Create a new Zap and name it something like 'Monthly content check'.

### Catch the WCH Webhook

- Add a Webhook step to the Zap.
- Choose to Catch a hook. Copy the URL of the Webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a Webhook, and paste the copied URL.
- Set the filters as event type "Create" and "Update" and item type "Content" and "Assets".
- Set the tag filter as "CheckMonthly". When the creator or updater of an item wants their item to be checked monthly, they need to add this tag to the item. 
- Add the Webhook
- Navigate to the WCH "All content and assets" view and create a dummy new content item and add the tag "CheckMonthly". Zapier should detect the Webhook and make the data from the Webhook available for use while adding the next steps. 

### Add a delay step

- Add a Zapier delay step.
- Set the delay as a month.

Note that delay here could be based on properties of the content item or asset or some other logic. For example, content item could have an 'Expiry date' element. The content creator sets a specific expiry date. The Zap could read that value of the element and use that to set the delay. In this case, you will need to add a Zapier Code step before the delay step to calculate the delay using some simple Javascript and to make the delay available to the delay step as an input parameter.

### Add a step to create a Trello card

- Sign up to a free Trello account or use your existing Trello account.
- Create a new Zapier Trello step. 
- Select to create a Card.
- Connect Zapier to your Trello account. 
- Select a board and the TODO list.
- Set a card title such as "Check `<docName>`" where docName is a variable from the webhook - the content name.
- set a card description such as 
"Go to `https://www.customer-engagement.ibm.com/content-hub/Authoring/Content/All-Content-And-Assets?bookmark=%2FAuthoring%2FContent%2FAll-Content-And-Assets%2FEdit%2F%3FcontentId%3D<docId>` and check the content for currency." where docId is a variable from the Webhook - the content ID. 
- Select other options such as the label color, member (the user who owns the Trello action) and due date.

Alternatively, a different notification method, or multiple notifications could be sent here, or a Code step used to send API requests back to the WCH APIs. For this, you will need to implement a Code step that will authenticate to the WCH Authoring APIs and then use the Rest APIs to make the desired changes. Check out the sample hhttps://github.com/ibm-wch/sample-custom-workflows-using-webhooks/sample-article-create to see how to do this.

You're done! Give it a test. 

## Going further

As already mentioned, this example could be adapted to create any other kind of notification, or to apply any other type of time-based check or expiry. 

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
