********************
PubNub Configuration
********************

For Alexa Plus Unity to fuction, we need to configure AWS to allow the Unity project to communicate to DynamoDB.

Prerequisites
=============

-  An `AWS Account <https://aws.amazon.com/>`_

Obtain an Identity Pool ID using Amazon Cognito
================================================

1. Log in to the `Amazon Cognito Console <https://console.aws.amazon.com/cognito/home>`_ and click **Create new identity pool**.
2. Enter a name for your Identity Pool and check the checkbox to enable access to unauthenticated identities. Click **Create Pool** to create your identity pool.
3. Click **Allow** to create the two default roles associated with your identity pool--one for unauthenticated users and one for authenticated users. These default roles provide your identity pool access to Cognito Sync and Mobile Analytics.

The next page displays code. Take note of the displayed **Identity Pool ID** and the **Region** you set up the Identity Pool in as you will need them when setting up Alexa Plus Unity.

Attach Polices to the Identity Pool default roles in AWS IAM
============================================================

1. Log in to the `AWS IAM Console <https://console.aws.amazon.com/iam/home?region=us-east-1#/home>`_ and click **Roles** in the left navigation bar.
2. Find and click your **Unauthenticated** Identity Pool role. It should look similar to ``Cognito_[YOUR IDENTITY POOL]Unauth_Role``.
3. Click **Attach Policies**.
4. Find and check the **AmazonDynamoDBFullAccess** policy.
5. Click **Attach Policy**.

Your Identity Pool is now configured to use the required AWS services for Alexa Plus Unity to function.