***********************
Security Best Practices
***********************

In our demo, we attach the ``AmazonDynamoDBFullAccess`` policy to both the Lambda function and the Cognito Identity Pool. In a full deployment of a game, we should restrict this to only allow specific access to resources in AWS. Instead of adding ``AmazonDynamoDBFullAccess``, follow the steps below to create policies for your Lambda Function and Cognito Identity Pool.

Cognito Identity Pool Policy
============================

1. Navigate to the `Policies section in IAM <https://console.aws.amazon.com/iam/home?#/policies>`_.
2. Click ``Create Policy``
3. Click ``JSON``
4. Copy and paste the following: ::

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "SpecificTable",
                "Effect": "Allow",
                "Action": [
                    "dynamodb:GetItem",
                    "dynamodb:UpdateItem",
                    "dynamodb:PutItem"
                ],
                "Resource": "arn:aws:dynamodb:*:*:table/AlexaPlusUnityTest"
            }
        ]
    } 

.. Note:: Replace ``AlexaPlusUnityTest`` with the name of your table.

5. Click ``Review Policy``
6. Give the Policy a name and click ``Create Policy``

Attach this policy to the UnAuth IAM Role instead of the ``AmazonDynamoDBFullAccess`` policy.

Lambda Policy
=============

1. Navigate to the `Policies section in IAM <https://console.aws.amazon.com/iam/home?#/policies>`_.
2. Click ``Create Policy``
3. Click ``JSON``
4. Copy and paste the following: ::

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "SpecificTable",
                "Effect": "Allow",
                "Action": [
                    "dynamodb:CreateTable",
                    "dynamodb:GetItem",
                    "dynamodb:UpdateItem",
                    "dynamodb:PutItem"
                ],
                "Resource": "arn:aws:dynamodb:*:*:table/AlexaPlusUnityTest"
            }
        ]
    } 

.. Note:: Replace ``AlexaPlusUnityTest`` with the name of your table.

5. Click ``Review Policy``
6. Give the Policy a name and click ``Create Policy``

Attach this policy to thr Alexa Skill's Lambda IAM Role instead of the ``AmazonDynamoDBFullAccess`` policy.