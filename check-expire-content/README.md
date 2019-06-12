# Check or expire content or assets on a schedule

This is a tutorial on setting up WCH Webhooks and Zapier to create a Trello card on a schedule that prompts a user to check or expire a content item or asset. For background information on all the tutorials, see: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks.

Common use cases include:
- Where content items and assets should be checked every month, say, for currency.
- Where an expiry date is set on content item when created. When the expiry date arrives, the content item should be removed from the site.

WCH Webhooks can be used to initiate a notification to action the check or expiry. For example, a Trello card can be created and assigned to a user, or an email sent, or a Slack announcement created. The notifications prompt the user to check the content or asset for currency or remove the content or asset from the site. For the expiry case, the content or asset maybe 'in use' in some way, in which case the user may replace the item with some other content or asset, or just remove it - this will depend on the way in which the content or asset is used. WCH Webhooks can also be used to automatically update or delete the asset or content item, by calling back into the WCH APIs to do the update or delete. Whether the update or expire is manual or automated will depend on the way in which the content item or asset is used. Doing the action 'manually' has the advantage that this user can use the draft and approval process to check that the removal of the content item or asset does not cause any issues in the website or application. 

## Example - Auto Expiring Content

This example will show how you can automatically update Trello items and WCH Content via Zapier and WCH Webhooks. It will require a Trello account with a Dashboard already created. I'm using the following:

![Trello Dashboard](/images/trelloDashboard.jpg)

When a Content is created, it will be added to the list **Content Set To Expire**. When the Content expires (as defined when the user creates it), it will be moved over to the list **Expired Content**. 

We'll also need a WCH Content Type with a Date field added to it. The Content Type can contain any other fields that would describe it, but we'll be using the Date field to set the expire time. I'm using the following:

![WCH Content Type](/images/TimeLimitedContent.jpg)

## Create a new Zap in Zapier

The Zap will consist of 6 steps that perform the following actions:
1. Catch the WCH Webhook. 
1. Using the call from the WCH Webhook, create a Trello card under a specified dashboard and list.
1. Delay until the time specified from the WCH Webhook.
1. Find the card we added in Trello from Step 2.
1. Move the card into our "Completed" list.
1. Using WCH APIs, retire the Content item in WCH


Create a new Zap and name it something like 'Expiring Content Webhook'.

### Catch the WCH Webhook

- Add a Webhook step to the Zap.
- Choose to Catch a hook. Copy the URL of the Webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a Webhook, and paste the copied URL.
- Set the filters as event type "Create" and "Update" and item type "Content" and "Assets".
- Set the tag filter as "ExpiringContent". When the creator or updater of an item wants their item to be automatically expired, they need to add this tag to the item. 
- Add the Webhook
- Navigate to the WCH "All content and assets" view and create a dummy new content item and add the tag "ExpiringContent". Zapier should detect the Webhook and make the data from the Webhook available for use while adding the next steps. 

### Add a step to create a Trello card

- Create a new Zapier Trello step. 
- Select to create a Card.
- Connect Zapier to your Trello account. 
- Select the board and list you have created.
- Set a card title such as "Check `<docName>`" where docName is a variable from the Webhook - the content name.
- Set a card description that includes the fields from the WCH Content item. You can get creative as Trello supports Markdown.
- Add a Due Date to the card. We can pull this directly from the previous WCH Catch Hook i.e.  `Doc Elements Expiry Date Value`

### Add a delay step

- Add a Zapier delay step.
- Select the **Delay Until** option
- Set the value to pull from the `Expiry Date` field that we retrieved from the Webhook.

### Add a step to find the previous Trello card

- Create a new Zappier **Find Card** Trello step.
- Query the same Board, List and Card Name. The Card Name should be an available field from step 2. 

### Add a step to move the card to the Expired list

- Create a new Zappier **Move Card to List** Trello step.
- The correct Board, From List and To List should all be available via dropdowns. 
- To pick the correct Card, set the dropdown to **Use a Custom Value**
- Then the correct Card should be available from the previous search step. 

### Add a step to send an authenticated request back to WCH to Retire the content

- Create a new Zappier **Webhooks** step, selecting **Post** as the option. 
- The request only needs to include:
  - the correct URL
  - your WCH authenication credentials **(see note on security below)**
  - Payload Type set to JSON
- The URL should be of the form *\<API URL\>\<Retire URL from step 1\>*
  - The Base Authoring URL of your request can be retrieved from your WCH account. See **Hub Information -> API URL**
  - The Retire URL was included in the Webhook from Step 1 and should be available as a field
- Your WCH authentication will be entred under the **Basic Auth** section. This should take either the form `<username>|<password>` or `apikey|<api key value>`
-----

#### Note on Security

In this example, we are entering Authentication information in plain text into the Zapier product, which would be visible to anyone with access to your Zapier account. This approach is **NOT** advised for anything beyond testing and learning. At this time, Zapier does not appear to provide a means of storing private account information to be used in Zap fields. A more secure approach may become available in future as either a native Zapier feature or WCH integration. 

-----
You're done! Give it a test. 

## Going further

As already mentioned, this example could be adapted to create any other kind of notification, or to apply any other type of time-based check or expiry. 

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
