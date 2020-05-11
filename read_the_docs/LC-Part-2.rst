************************
Creating the Alexa Skill
************************

Let's setup the Alexa Skill!

If you get lost, stuck, or just want a working demo right now, you can clone the complete sample project from `here <https://github.com/AustinMathuw/AlexaPlusUnityExampleSkillComplete.git>`_.

Prerequisites
=============

-  Node.js (v4.5 or above).
-  Have an `AWS Account <https://aws.amazon.com/>`_.
-  Have an `Amazon Developer Account <https://developer.amazon.com/>`_.
-  Install the `ASK CLI v1 <https://www.npmjs.com/package/ask-cli/v/1.7.23>`_.
.. Note:: The npm command to install the ASK CLI v1: `npm i -g ask-cli@1`

Creating the Skill
==================

1. Clone the example skill templete: ``git clone https://github.com/AustinMathuw/AlexaPlusUnityExampleSkillTemplate.git``
2. Open the template in a editor such as Visual Studio code
3. Open a command prompt or terminal and navigate to ``<Template Location>/lambda/custom/``
4. In the command prompt or terminal, run ``npm install``

Example Skill Template Overview
===============================

Our Example Skill has the following Intents:

* FlipSwitchIntent

    * Handles turning our light on and off

* ChangeColorIntent

    * Handles changing the color of our light

* GetColorIntent

    * Returns our lights current color

* AMAZON.HelpIntent

    * Returns help message to guide the user

* AMAZON.CancelIntent

    * Closes the skill

* AMAZON.StopIntent

    * Closes the skill


Adding Games SDK for Alexa to the Skill
=======================================

The steps below corrospond to the step numbers in the skeleton. Place the code for each of the below steps under their step number in the templete code.

.. Note:: There may be IDE errors as we continue, but those will be resolved at the end when the skeleton is complete.

1. Our cloned templete already has the ``alexaplusunity`` node module, but we still need to include it. Open ``index.js`` under ``lambda/custom/`` and add the following: ::

        var alexaPlusUnityClass = require('alexaplusunity');
        var alexaPlusUnity = new alexaPlusUnityClass("<YOUR_PUBNUB_PUB_KEY>", "<YOUR_PUBNUB_SUB_KEY>", true); //Third parameter enables verbose logging

2. In our example skill, we will use state management to confirm that the user has connected to our game. Insert the code below inside of the LaunchRequestHandler: ::

        if(attributes.SETUP_STATE == "STARTED") {
            var launchSetUpResult = await launchSetUp(speechText, reprompt, handlerInput, attributes);
            attributes = launchSetUpResult.attributes;
            response = launchSetUpResult.response;
        }

In the code block above, we are checking to see if we are still in the ``SETUP_STATE``. If we are, then run the method ``launchSetUp()`` and build our response. If we are not in the ``SETUP_STATE``, then use the response we already built.

3. In the next steps, we will add Games SDK for Alexa to the FlipSwitchIntent (CompletetedFlipSwitchIntentHandler). We will start by creating a payload object for the intent: ::

        var payloadObj = { 
            type: "State",
            message: state
        };

4. Then we will send the payload to our game and reply with a success or error if the payload failed to send: ::

        var response = await alexaPlusUnity.publishMessage(payloadObj, attributes.PUBNUB_CHANNEL).then((data) => {
            return handlerInput.responseBuilder
                .speak(speechText + reprompt)
                .reprompt(reprompt)
                .getResponse();
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

5. Now, we will add Games SDK for Alexa to the ChangeColorIntent (CompletedChangeColorIntentHandler). We will create our payload object: ::

        var payloadObj = { 
            type: "Color",
            message: color
        };

6. Send the payload to our game and reply with a success or error if the payload failed to send: ::

        var response = await alexaPlusUnity.publishMessage(payloadObj, attributes.PUBNUB_CHANNEL).then((data) => {
            return handlerInput.responseBuilder
                .speak(speechText + reprompt)
                .reprompt(reprompt)
                .getResponse();
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

7. Add Games SDK for Alexa to the GetObjectInDirectionIntent (CompletedGetObjectInDirectionIntentHandler). We will create our payload object: ::

        var payloadObj = { 
            type: "GetObject",
            message: direction_id
        };

8. Send a the payload to our game and reply with a success or error if the payload failed to send: ::

         var response = await alexaPlusUnity.publishMessageAndListenToResponse(payloadObj, attributes.PUBNUB_CHANNEL, 4000).then((data) => {
            speechText = 'Currently, ' + data.message.object + ' is ' + direction + ' of you!';
        
            return handlerInput.responseBuilder
               .speak(speechText + reprompt)
               .reprompt(reprompt)
               .getResponse();
         }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
         });

9. Create the user's unique channel: ::

        var response = await alexaPlusUnity.addChannelToGroup(attributes.PUBNUB_CHANNEL, "AlexaPlusUnityTest").then(async (data) => {
        var responseToReturn = responseBuilder
            .speak(speechText)
            .reprompt(reprompt)
            .withSimpleCard('Games SDK for Alexa', "Here is your Player ID: " + attributes.PUBNUB_CHANNEL)
            .getResponse();

        var userId = handlerInput.requestEnvelope.session.user.userId;
        return await sendUserId(userId, attributes, handlerInput, responseToReturn);
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

10. Now, we need to send the game the user's Alexa ID so we can access their persistant session attributes. Create our payload object: ::

        var payloadObj = { 
            type: "AlexaUserId",
            message: userId
        };

11. Send the payload to the game: ::

        return await alexaPlusUnity.publishMessage(payloadObj, attributes.PUBNUB_CHANNEL).then((data) => {
            return response;
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

12. Lastly, we need to initialize the skills attributes ::

        attributes.SETUP_STATE = "STARTED";
        var newChannel = await alexaPlusUnity.uniqueQueueGenerator("AlexaPlusUnityTest");
        
        if(newChannel != null) {
            attributes.PUBNUB_CHANNEL = newChannel;
        } else {
            return null;
        }

Deploying the Skill
===================

1. Open a command prompt or terminal and navigate to <Template Location>
2. Type ``ask deploy`` to deploy the skill.
3. In a browser, navigate to your newly created `Lambda Function <https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions/ask-custom-AlexaPlusUnityTest-default?tab=configuration>`_
4. Scroll down to the **Execution Role** and click on ``View the ask-lambda-Unity-Light-Control-AlexaPlusUnityTest role``. This takes you to the IAM role in IAM.
5. Click **Attach Policies**.
6. Find and check the **AmazonDynamoDBFullAccess** policy.
7. Click **Attach Policy**.

.. Note:: This will only work if you set up the `ASK CLI v1 <https://www.npmjs.com/package/ask-cli/v/1.7.23>`_ correctly! Will not work with v2 of the ASK CLI.

Wrapping Up
===========

At this point, you should be able to test the skill by saying, "Alexa, open unity light".

.. Note:: You will likely get an error the first couple of times initially opening the skill. This is because the skill needs to create the DynamoDB table and it can take a couple of minutes to do so. See `this issue <https://github.com/alexa/alexa-skills-kit-sdk-for-nodejs/issues/292>`_ for more information.

We have finished the Alexa Skill!
