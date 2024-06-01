---
layout: post
title:  "[React] Gitlab CI auto send email with information from Git release tag"
comments: true
date:  2023-04-01 15:07:33 +0700
categories: React
---

# Gitlab CI auto send email with information from Git release tag


I) Send email with AWS Lambda & AWS SES

1) Go to AWS SES and verify email identities ( source sender email)

!http://rubykiwi.com/content/images/2024/01/Verified_identities___Amazon_Simple_Email_Service___us-east-1.png

2) Create AWS Lambda function

```
const aws = require("aws-sdk")

function generateEmailParams(subject, message) {
  const bodyHtml = `<html>
    <head></head>
      <body>
          <h1>Message from {SENDER_NAME}</h1>
          <h3>From: Sender</h3>
          <br/>
          <p>${message}</p>
      </body>
    </html>`

  return {
    Destination: {
      ToAddresses: [
        "[email protected]"
      ],
    },
    ReplyToAddresses: ["[email protected]"],
    Source: "[email protected]",
    Message: {
      Body: {
        Html: {
          Charset: "UTF-8",
          Data: bodyHtml,
        },
        Text: {
          Charset: "UTF-8",
          Data: message,
        },
      },
      Subject: {
        Charset: "UTF-8",
        Data: subject,
      },
    },
  }
}

function errorResponse(errors) {
  return {
    statusCode: 400,
    body: {
      success: false,
      errors,
    },
  }
}

function successResponse(message) {
  return {
    statusCode: 200,
    body: {
      success: true,
      message,
    },
  }
}

exports.handler = async event => {
  try {
    const { subject, message } = JSON.parse(event.body)
    const params = generateEmailParams(subject, message)
    const data = await new aws.SES()
      .sendEmail(params)
      .promise()
      .then(() => {
        return successResponse("Email sent")
      })
      .catch(error => {
        return errorResponse([error.message])
      })

    return data
  } catch (e) {
    return errorResponse([e.message])
  }
}
```

3) Generate Lambda function URL to use to invoke from CI

!http://rubykiwi.com/content/images/2024/01/deployCiSendEmail_-_Lambda.png

4) Navigate to IAM role and edit permission for sending email permission

!http://rubykiwi.com/content/images/2024/01/Edit_policy___IAM___Global.png

5) Go to postman and test lambda function before

!http://rubykiwi.com/content/images/2024/01/Screenshot_2024-01-11_at_12_53_08_PM.png

6) Create `gitlab-ci.yml`

```
#======INIT=========
stages:
  - test

#======JOBS=========

email_notify_prod_deployment:
  stage: test
  tags:
    - linux
    - autopilot-builder
  before_script:
    - apk update && apk add --no-cache git && apk add --no-cache curl
    - echo "Execute email_notify_prod_deployment section starts."
  script:
    - git fetch --tags
    - LATEST_TAG=$(git describe --tags $(git rev-list --tags --max-count=1))
    - echo $LATEST_TAG
    - TAG_AUTHOR=$(git show $LATEST_TAG --format='%an <%ae>' --no-patch | awk '{printf "%s<br>", $0}' | sed 's/"/\\"/g')
    - echo $TAG_AUTHOR
    - |
      curl --request POST \
      -H "Content-Type:application/json" \
      -d "{ \"subject\":\"$LATEST_TAG\", \"message\":\"$TAG_AUTHOR\" }" \
      "$SEND_EMAIL_LAMBDA_FUNCTION"

  after_script:
    - echo "Execute email_notify_prod_deployment section completes."
  only:
    - /^main$/

```
