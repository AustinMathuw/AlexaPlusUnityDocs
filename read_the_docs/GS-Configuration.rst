*************
Configuration
*************

AWS Configuration
=================

For Games SDK for Alexa to fuction, we need to configure AWS to allow the Unity project to communicate to DynamoDB (The Alexa Skill's persistant attributes).

Prerequisites
^^^^^^^^^^^^^

-  An `AWS Account <https://aws.amazon.com/>`_.

Obtain an Identity Pool ID using Amazon Cognito
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Log in to the `Amazon Cognito Console <https://console.aws.amazon.com/cognito/home>`_ and click **Create new identity pool**.
2. Enter a name for your Identity Pool and check the checkbox to enable access to unauthenticated identities. Click **Create Pool** to create your identity pool.
3. Click **Allow** to create the two default roles associated with your identity pool--one for unauthenticated users and one for authenticated users. These default roles provide your identity pool access to Cognito Sync and Mobile Analytics.

The next page displays code. Take note of the displayed **Identity Pool ID** and the **Region** you set up the Identity Pool in as you will need them when setting up Games SDK for Alexa.

Attach Polices to the Identity Pool default roles in AWS IAM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Log in to the `AWS IAM Console <https://console.aws.amazon.com/iam/home?region=us-east-1#/home>`_ and click **Roles** in the left navigation bar.
2. Find and click your **Unauthenticated** Identity Pool role. It should look similar to ``Cognito_[YOUR IDENTITY POOL]Unauth_Role``.
3. Click **Attach Policies**.
4. Find and check the **AmazonDynamoDBFullAccess** policy.
5. Click **Attach Policy**.

Your Identity Pool is now configured to use the required AWS services for Games SDK for Alexa to function.

PubNub Configuration
====================

For Games SDK for Alexa to fuction, we need to configure PubNub to allow the Unity project to communicate with the Alexa Skill.

Prerequisites
^^^^^^^^^^^^^

-  A `PubNub Account <https://www.pubnub.com/>`_.

Create a New App on PubNub
^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Log in to the `PubNub Admin Console <https://admin.pubnub.com/#/>`_ and click **Create new app**.
2. Enter a name for your new app. Click **Create** to create your new app.
3. Click on your new app in the admin console.

The next page displays your keysets. You can create as many as you keysets as you wish, but for our purposes, we can just use the Demo Keyset.

4. Click the Demo Keyset.
5. Make note of both the publish and subscribe keys as you will need them when setting up Games SDK for Alexa.
6. Enable the **Stream Controller** and the **Storage and Playback** Application add-ons.

Your PubNub App is now configured for Games SDK for Alexa.