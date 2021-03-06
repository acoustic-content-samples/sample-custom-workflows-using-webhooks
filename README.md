# Creating custom workflows using Acoustic Content Webhooks

These days, the workflow for managing content in different channels can involve multiple systems. Content can be created in Acoustic Content (formerly Watson Content Hub or WCH) and translated in a translation tool. You can upload an asset to WCH and then use a cognitive system to add metadata. Approvals can be multi-levelled and managed by using a task management tool such as Trello or Microsoft Flow. Notification of events and user collaboration can be sent using Slack, email, or mobile push notifications. In other words, content management in WCH is part of a larger workflow. WCH Webhooks can help you deliver this extended workflow to other systems.

This repo contains tutorials that make use of WCH Webhooks to demonstrate how to set up some common custom workflows. 

WCH Webhooks send messages when an event occurs in WCH. For more background information on Webhooks, see the WCH Webhooks documentation here:

https://help.goacoustic.com/hc/en-us/articles/360039881833-Creating-custom-workflows-with-webhooks

In particular, note that a user nees to enable the WCH Webhooks UI needs in order to set up WCH Webhooks, and needs to be an admin to do so.

To make use of WCH Webhooks, you need a Webhook consumer. The consumer listens for webhook events and performs an action based on those events. For complete custom control and flexibility, the webhook consumer can be a service running on a cloud provider such as the Amazon AWS, Microsoft Azure, or Google GCP. These options require a developer who can build and deploy a service, such as a JavaScript node app. Function as a service solutions like AWS lambda are also well suited to such integration and data processing solutions.

It's also possible to use a no-code or low-code cloud iPaas platform such as Zapier, Tray.io, or IFTTT. Zapier has a free plan, is the easy to get started, and is specifically designed for integrating systems with little to no coding required. For that reason, the following examples use Zapier.

## Demo videos

Demo videos are here https://youtu.be/T4x-y4IfMFA and https://youtu.be/2OgPEyh7XvE.

## Create Slack notification when items are updated

See: https://github.com/acoustic-content-samples/sample-custom-workflows-using-webhooks/tree/master/slack-notification

## Send an email to the approvers when a review is created

See: https://github.com/acoustic-content-samples/sample-custom-workflows-using-webhooks/tree/master/email-notification

## Create a Trello card to translate a content item

See: https://github.com/acoustic-content-samples/sample-custom-workflows-using-webhooks/tree/master/trello-card

## Check or expire content or assets on a schedule

See: https://github.com/acoustic-content-samples/sample-custom-workflows-using-webhooks/tree/master/check-expire-content

## Create a custom workflow by integrating Acoustic Content with a task management system

See: https://github.com/acoustic-content-samples/sample-custom-workflows-using-webhooks/tree/master/external-task-management

## Going further

These are just some examples of the custom workflows you can create using Acoustic Content Webhooks. They can be mixed and matched; for example, you can use a Trello card when an approval is required. Generally, a workflow contains multiple workflow states, with corresponding actions when entering these states. WCH content items have draft, approval, and published states. Use a tag to create an additional custom workflow state. For example "Translate" for awaiting translation, or "Check" for needs checking or "Expire" for pending removal from the website. These workflow actions are the actions that the webhook consumer uses when an item enters the state. 

Approval workflows are a common example of a workflow that frequently needs to be customised. Perhaps approval of new content items can be mandatory or optional, depending on the content type. Perhaps approval can be granted by a single user, or perhaps approval requires that _any_ of the selected users can approve. Perhaps it is sufficient that any two of the selected user approve the item or perhaps all of the selected user must approve. Perhaps the approval process is that content authors tag content with a category, and the approver is selected based on that category. Perhaps a 'multi-level' approval is required whereby a content creation team lead must approve, and then a user from the design team approves, before finally the site owner approves. All of these can be implemented using tags for the various stages, a task system such as Trello to assign and track the approvals required, and notification in the appropriate channels, such as email, Slack, SMS or mobile push.

Some more use cases that could be implemented:

- Schedule a facebook post or twitter tweet featuring the newly published content item or webpage.
- Invalidating the cache of your mobile app to make sure users see the latest content when it's published.
- Running a test suite for your website using a CI service such as Travis when the website content is changed.
- Updating an external search index when content was updated.

It should be possible to create any custom workflow imaginable with any system on the internet with an API by following this approach. 


