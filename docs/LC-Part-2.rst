************************
Creating the Alexa Skill
************************

Let's setup the Alexa Plus Unity SDK!

If you get lost, stuck, or just want a working demo right now, you can clone the complete sample project from `here <>_`

Prerequisites
=============

-  Node.js (v4.5 or above)
-  Have an `AWS Account <https://aws.amazon.com/>`_
-  Have an `Amazon Developer Account <https://developer.amazon.com/>`_
-  Install the `ASK CLI <https://developer.amazon.com/docs/smapi/quick-start-alexa-skills-kit-command-line-interface.html>`_

Creating the skill
==================

1. Clone the templete:



Adding AlexaPlusUnity to the skill
==================================

1. Lorem Ipsolm ::

        var alexaGaming = require('./alexa-gaming-cookbook.js');

2. Lorem Ipsolm ::

        if(attributes.SETUP_STATE == "STARTED") {
            var launchSetUpResult = await launchSetUp(speechText, reprompt, handlerInput, attributes);
            attributes = launchSetUpResult.attributes;
            response = launchSetUpResult.response;
        }

3. Lorem Ipsolm ::

        var payloadObj = { 
            type: "State",
            message: state
        };

4. Lorem Ipsolm ::

        var response = await alexaGaming.publishEventSimple(JSON.stringify(payloadObj), attributes.SQS_QUEUE_URL).then((data) => {
            return handlerInput.responseBuilder
                .speak(speechText + reprompt)
                .reprompt(reprompt)
                .getResponse();
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

5. Lorem Ipsolm ::

        var payloadObj = { 
            type: "Color",
            message: color
        };

6. Lorem Ipsolm ::

        var response = await alexaGaming.publishEventSimple(JSON.stringify(payloadObj), attributes.SQS_QUEUE_URL).then((data) => {
            return handlerInput.responseBuilder
                .speak(speechText + reprompt)
                .reprompt(reprompt)
                .getResponse();
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

7. Lorem Ipsolm ::

        var response = await alexaGaming.createQueue(attributes.SQS_QUEUE).then(async (data) => {
            attributes.SQS_QUEUE_URL = data.QueueUrl.toString();

            var responseToReturn = responseBuilder
                .speak(speechText)
                .reprompt(reprompt)
                .withSimpleCard('Alexa Plus Unity', "Here is your Player ID: " + attributes.SQS_QUEUE)
                .getResponse();

            var userId = handlerInput.requestEnvelope.session.user.userId;
            return await sendUserId(userId, attributes, handlerInput, responseToReturn);
            }).catch((err) => {
                return ErrorHandler.handle(handlerInput, err);
            }
        );

8. Lorem Ipsolm ::

        var payloadObj = { 
            type: "AlexaUserId",
            message: userId
        };

9. Lorem Ipsolm ::

        return await alexaGaming.publishEventSimple(JSON.stringify(payloadObj), attributes.SQS_QUEUE_URL).then((data) => {
            return response;
        }).catch((err) => {
            return ErrorHandler.handle(handlerInput, err);
        });

10. Lorem Ipsolm ::

        attributes.SETUP_STATE = "STARTED";
        attributes.SQS_QUEUE = await alexaGaming.uniqueQueueGenerator();
        attributes.SQS_QUEUE_URL = null;
