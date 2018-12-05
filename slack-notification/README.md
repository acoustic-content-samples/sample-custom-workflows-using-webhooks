# Create Slack notification when items are updated

This is a tutorial on setting up Zapier to listen to WCH webhook events and to send a Slack notification.

## Create a Slack app.

Create an 'incoming webhook' Slack app. The documentation on creating Slack apps is located here:

https://api.slack.com/slack-apps

Incoming webhook Slack app documentation is located here:

https://api.slack.com/incoming-webhooks

Click the green button to create your incoming webhook Slack app. You get to set some options, such as the channel to post to and an icon for your app. Your administrator may need to approve the app before it is allowed to post in your Slack organisation.

Copy the incoming webhook URL provided by Slack. You'll need that when you create your Zap. 

## Create a Zap.

The Zap uses three steps. 
- The first step catches the webhook from WCH. 
- The second step extracts some fields from the webhook and puts them into a JSON structure expected by Slack. 
- The third step sends an outbound webhook onto Slack. 

### Catch the webhook

- Create a webhook step in Zapier. 
- Choose to Catch a hook. Copy the URL of the webhook.
- In a new browser tab, go to the Webhooks UI in WCH. 
- Add a webhook, and paste the copied URL.
- For this example, we don't set any filters. All events will appear in Slack. In reality, you would want to apply some filters such as event type "Create" and item type "Content" and perhaps a tag named "MarchUpdates" to only see events related to a particular team or project.
- Add the webhook.
- Navigate to the WCH "All content and assets" view and create a new review for a dummy draft item. 

Zapier should detect the webhook and make the data from the webhook available for use while adding the next steps. 

### Add a Code step to create the JSON to send to the Slack app

- Add a Zapier Code step.
- Select JavaScript.
- Set the input data. Extract the fields from the webhook that you want to include in your Slack notification. In this example, extract the fields `<docName>`, `<event>`, `<docClassification>` and `<docCreator>`. So, enter `docName` as the input data field name, and select the `Doc Name` variable from the webhook. Repeat this for the other four fields.
- Add the Code:

`output = [{
    "text": "*"+inputData.docCreator+"* "+inputData.event+" a "+inputData.docClassification+" item called *"+inputData.docName+"*"
}];`

The * makes the creator and item name bold. 

### Add a Post step to send a webhook to slack

- Add a Zapier webhook step.
- Choose to send a POST.
- Enter the URL from the Slack app. 
- Select a payload type of JSON.

And, you're done. Turn on the Zap and give it a test!

## Going further

The sample shows how to send all WCH events within a hub to a fixed Slack channel. It's not difficult to customise this to send the events to different channels based on the classification or tags applied to a given item. It's also possible to have an action button in slack notifications. These could be used to perform actions back in the Hub
such as approve content items.

## Go back to see all the custom workflow using webhooks tutorials

Back: https://github.com/ibm-wch/sample-custom-workflows-using-webhooks
