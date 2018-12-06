# Check or expire content or assets on a schedule

This is a tutorial on setting up Zapier to create a Trello card on a schedule that prompts a user to check or expire a content or asset.

Common use cases include:
- Where content and assets should be checked every month, say, for currency.
- Where content has an expiry date, at which point it should be removed from the site.

Webhooks can be used to initiate a notification that launches these actions. For example, a Trello card can be created, or an email sent, or a Slack announcement created. All these notifications prompt a user to check the content or asset for currency or remove the content or asset from the site. If the content or asset is in use, the user may replace it with some other content or asset, or just remove it. Doing this manually means that this user can check that the removal of the content or asset does not break the site. Alternatively, a scheduled update or deletion can be automated by calling back into the WCH APIs to do the update or delete.

## Create a Zap.

The Zap consists of three steps:
- The first step catches the webhook. 
- The second step creates a delay until the specified time. 
- The third step creates a Trello card to add a TODO for the user to check or remove the given content or asset.

This example implements a monthly check of content and assets. This model can easily be adjusted to deliver the other similar use cases listed above.

### Catch the webhook

- Create a webhook step in Zapier. 
- Choose to Catch a hook. Copy the URL of the webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a webhook, and paste the copied URL.
- Set the filters as event type "Create" and "Update" and item type "Content" and "Assets".
- Set the tag filter as "CheckMonthly". When the creator of the new item wants their item to be checked monthly, they add this tag. 
- Add the webhook
- Navigate to the WCH "All content and assets" view and create a new content item and add the tag "CheckMonthly" for a dummy item. Zapier should detect the webhook and make the data from the webhook available for use while adding the next steps. 

### Add a delay step

- Add a Zapier delay step:
- Set the delay as a month.
- The delay here could be based on a tag in the content or some other logic. If the logic is more complicated, a Code step would need to be added before the delay step to calculate the delay.

### Add the step to create a Trello card

- Sign up to a free Trello account or use your existing Trello account.
- Create a new Zapier Trello step. 
- Select to create a Card.
- Connect Zapier to your Trello account. 
- Select a board and the TODO list.
- Set a card title such as "Check `<docName>`" where docName is a variable from the webhook - the content name.
- set a card description such as 
"Go to `https://www.customer-engagement.ibm.com/content-hub/Authoring/Content/All-Content-And-Assets/Edit/?contentId=<docId>` and check the content for currency" where docId is a variable from the webhook - the content ID. 
- Select other options such as the label color, member (the user who owns the Trello action) and due date.

You're done! Give it a test. 

## Going further

As already mentioned, this example could be adapted to create any other kind of notification, or to apply any other type of time-based check or expiry. 

Implementing an automated callback into WCH is possible. For example, to expire content. For this, you will need to implement a Code step that will authenticate to the WCH Authoring APIs and then use the Rest APIs to make the desired changes. Check out the sample hhttps://github.com/ibm-wch/sample-custom-workflows-using-webhooks/sample-article-create to see how to do this. If there is some demand, I can spend some time doing a simple example of that in Zapier. 

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
