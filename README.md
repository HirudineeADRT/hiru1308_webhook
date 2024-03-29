<!--
title: 'AWS Serverless Github Webhook Listener example in NodeJS'
description: 'This service will listen to github webhooks fired by a given repository.'
layout: Doc
framework: v1
platform: AWS
language: nodeJS
authorLink: 'https://github.com/adambrgmn'
authorName: 'Adam Bergman'
authorAvatar: 'https://avatars1.githubusercontent.com/u/13746650?v=4&s=140'
-->
# Serverless Github webhook listener

This service will listen to github webhooks fired by a given repository.

## Use Cases

* Custom github notifications
* Automatically tagging github issues
* Pinging slack on new Pull requests
* Welcoming new stargazers
* etc.

## How it works

```
âââââââââââââââââ               âââââââââââââ
â               â               â           â
â  Github repo  â               â   Github  â
â   activity    âââââTriggerââââ¶â  Webhook  â
â               â               â           â
âââââââââââââââââ               âââââââââââââ
                                      â
                     âââââPOSTâââââââââ
                     â
          ââââââââââââ¼ââââââââââ
          â ââââââââââââââââââ â
          â â  API Gateway   â â
          â â    Endpoint    â â
          â ââââââââââââââââââ â
          âââââââââââ¬âââââââââââ
                    â
                    â
         ââââââââââââ¼âââââââââââ
         â ââââââââââââââââââ  â
         â â                â  â
         â â     Lambda     â  â
         â â    Function    â  â
         â â                â  â
         â ââââââââââââââââââ  â
         âââââââââââââââââââââââ
                    â
                    â
                    â¼
         ââââââââââââââââââââââ
         â                    â
         â      Do stuff      â
         â                    â
         ââââââââââââââââââââââ
```

## Setup

1. Set your webhook secret token in `serverless.yml` by replacing `REPLACE-WITH-YOUR-SECRET-HERE` in the environment variables `GITHUB_WEBHOOK_SECRET`.

  ```yml
  provider:
    name: aws
    runtime: nodejs8.10
    environment:
      GITHUB_WEBHOOK_SECRET: REPLACE-WITH-YOUR-SECRET-HERE
  ```

2. Deploy the service

  ```yaml
  serverless deploy
  ```

  After the deploy has finished you should see something like:
  ```bash
  Service Information
  service: github-webhook-listener
  stage: dev
  region: us-east-1
  api keys:
    None
  endpoints:
    POST - https://abcdefg.execute-api.us-east-1.amazonaws.com/dev/webhook
  functions:
    github-webhook-.....github-webhook-listener-dev-githubWebhookListener
  ```

3. Configure your webhook in your github repository settings. [Setting up a Webhook](https://developer.github.com/webhooks/creating/#setting-up-a-webhook)

  **(1.)** Plugin your API POST endpoint. (`https://abcdefg.execute-api.us-east-1.amazonaws.com/dev/webhook` in this example). Run `sls info` to grab your endpoint if you don't have it handy.

  **(2.)** Plugin your secret from `GITHUB_WEBHOOK_SECRET` environment variable

  **(3.)** Choose the types of events you want the github webhook to fire on

  ![webhook-steps](https://cloud.githubusercontent.com/assets/532272/21461773/db7cecd2-c911e6-936bbf4661fe14.jpg)


4. Manually trigger/test the webhook from settings or do something in your github repo to trigger a webhook.

  You can tail the logs of the lambda function with the below command to see it running.
  ```bash
  serverless logs -f githubWebhookListener -t
  ```

  You should see the event from github in the lambda functions logs.

5. Use your imagination and do whatever you want with your new github webhook listener! ð

Let us know if you come up with a cool use case for this service =)
