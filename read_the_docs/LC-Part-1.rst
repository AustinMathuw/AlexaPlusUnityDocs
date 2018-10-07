**************************
Creating the Unity Project
**************************

Let's setup the Unity Project!

If you get lost or want to see the final project, the light control demo scene is located in ``Amazon Alexa\Examples``. You will need to create a project and import ``AlexaPlusUnity.unitypackage`` to gain access to this folder. 

Prerequisites
=============

-  `Unity <https://unity3d.com/>`_ version 4.x or above.
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
4. Find and check the **AmazonSQSFullAccess** and **AmazonDynamoDBFullAccess** polices.
5. Click **Attach Policy**.

Your Identity Pool is now configured to use the required AWS services for Alexa Plus Unity to function.

Creating the project
====================

1. Open Unity.
2. Create a new 3D project.
3. Add a cube to the SampleScene (``GameObject -> 3D Object -> Cube``).

Integrating the Alexa Plus Unity Package
========================================

1. Download the ``AlexaPlusUnity.unitypackage`` from the `Releases <https://github.com/AustinMathuw/AlexaPlusUnity/releases>`_ tab in the GitHub project.
2. With your project **open**, double click the package file you just downloaded.
3. Make sure everything is checked and click **Import**.

You should now see two new folders in you Assets folder, **Amazon Alexa** and **Plugins**.

Adding the LightControl script into the Unity project
=====================================================

1. With your project **open**, create a new script (``Assets -> Create -> C# Script``) and name it **LightControl**.
2. Open your LightControl script by double-clicking it.
3. Deleate everything in the script.
4. Copy and paste the code below into your LightControl script: ::

    using Amazon;
    using Amazon.DynamoDBv2.Model;
    using AmazonsAlexa.Unity.AlexaCommunicationModule;
    using System.Collections.Generic;
    using UnityEngine;

    public class LightControl : MonoBehaviour
    {
        //Step 5: Add variables below


        // Use this for initialization
        void Start () {
            //Step 6: Add AlexaPlusUnity initialization

        }

        public void ConfirmSetup(GetSessionAttributesEventData eventData)
        {
            //Step 7: Notify the skill that setup has completed by updating the skills persistant attributes (in DynamoDB)
            
        }

        void Update() {
            //Step 8: Add Keyboard event listener (For "gameplay")
            
        }
        
        public void HandleSpacePress()
        {
            //Step 9: Handle the keyboard input
            
        }

        //Callback for when a message is recieved
        public void OnAlexaMessage(HandleMessageEventData eventData)
        {
            //Step 10: Listen for new messages from the Alexa skill
            
        }
        
        public void UpdateLight(string type, string value, GetSessionAttributesEventData eventData)
        {
            //Step 11: Update the light based on the incoming message, then save the state of the light through the skill's session attributes
            
        }

        public void SetAttributesCallback(SetSessionAttributesEventData eventData)
        {
            //Step 12: Callback for when session attributes have been updated
            
        }

        
        public void OnMessageDeleted(ErrorEventData eventData)
        {
            //Step 13: Callback for when a message is deleted
            
        }
    }

The above code is our skeleton for our script. We will fill this skeleton step by step. The steps below corrospond to the step numbers in the skeleton. Place the code for each of the below steps under their step number in the skeleton.
**Note**: There may be IDE errors as we continue, but those will be resolve at the end when the skeleton is complete.

5. Define the class variables: ::

    public string sqsQueue;
    public string identityPoolId;
    public string AWSRegion = RegionEndpoint.USEast1.SystemName;
    public string tableName;
    public GameObject lightCube;
    private Dictionary<string, AttributeValue> attributes;
    private AmazonAlexaManager alexaManager;

These variables are necessary to preform initialization and enable reusablity of the Alexa Manager within our LightControl script.

6. Find and initialize the Alexa Manager: ::

        alexaManager = GetComponent<AmazonAlexaManager>(); //Get the manager script
        StartCoroutine(alexaManager.StartAlexa(sqsQueue, tableName, identityPoolId, AWSRegion, OnAlexaMessage)); //Initialize the Alexa Manager

7. Tell the skill that the game has completed setup and is ready to play: ::

        attributes = eventData.Values;
        attributes["SETUP_STATE"] = new AttributeValue { S = "COMPLETED" }; //Set SETUP_STATE attribute to a string, COMPLETED
        alexaManager.SetSessionAttributes(attributes, SetAttributesCallback);

8. Listen for a spacebar keypress: ::

        if (Input.GetKeyDown(KeyCode.Space))
        {
            Debug.Log("Space pressed");
            HandleSpacePress();
        }

9. Update the light to blue when the spacebar is pressed: ::

        if (!PlayerPrefs.HasKey("AlexaUserId")) //If the AlexaUserId has not been recieved from Alexa (If the user has not opened the skill)
            Debug.LogError("'AlexaUserId' not found in PlayerPrefs. We must establish connection from Alexa to set this. Please open the skill to set the 'AlexaUserId' PlayerPref.");

        alexaManager.GetSessionAttributes((result) =>
        {
            if (result.IsError)
                Debug.LogError(result.Exception.Message);
            UpdateLight("Color", "blue", result);
        });

10. Listen for new messages from the Alexa skill: ::

        Debug.Log("OnAlexaMessage");

        AlexaIncomingMessage messageBody = JsonUtility.FromJson<AlexaIncomingMessage>(eventData.Message.Body);

        //Get Session Attributes with in-line defined callback 
        alexaManager.GetSessionAttributes((result) =>
        {
            if (result.IsError)
                Debug.LogError(eventData.Exception.Message);

            switch (messageBody.type)
            {
                case "AlexaUserId":
                    Debug.Log("AlexaUserId: " + messageBody.message);
                    ConfirmSetup(result);
                    goto case "delete";
                case "Color":
                    Debug.Log("Requested Light Color: " + messageBody.message);
                    UpdateLight(messageBody.type, messageBody.message, result);
                    goto case "delete";
                case "State":
                    Debug.Log("Requested Light State: " + messageBody.message);
                    UpdateLight(messageBody.type, messageBody.message, result);
                    goto case "delete";
                case "delete":
                    var receiptHandle = eventData.Message.ReceiptHandle;
                    alexaManager.DeleteMessage(receiptHandle, OnMessageDeleted);
                    break;
                default:
                    break;
            }
        });

11. Update the light: ::

        attributes = eventData.Values;
        if(type == "Color")
        {
            attributes["color"] = new AttributeValue { S = value }; //Set color attribute to a string value
        } else if(type == "State")
        {
            attributes["state"] = new AttributeValue { S = value }; //Set state attribute to a string value
        }

        switch (value)
        {
            case "white":
                lightCube.GetComponent<Renderer>().material.color = Color.white;
                break;
            case "red":
                lightCube.GetComponent<Renderer>().material.color = Color.red;
                break;
            case "green":
                lightCube.GetComponent<Renderer>().material.color = Color.green;
                break;
            case "yellow":
                lightCube.GetComponent<Renderer>().material.color = Color.yellow;
                break;
            case "blue":
                lightCube.GetComponent<Renderer>().material.color = Color.blue;
                break;
            case "on":
                lightCube.GetComponent<Renderer>().enabled = true;
                break;
            case "off":
                lightCube.GetComponent<Renderer>().enabled = false;
                break;
        }
        alexaManager.SetSessionAttributes(attributes, SetAttributesCallback);  //Save Attributes for Alexa to use

12. Let's be notified when there is a error setting the attributes: ::

        Debug.Log("OnSetAttributes");
        if (eventData.IsError)
            Debug.LogError(eventData.Exception.Message);

13. Let's be notified when there is a error deleting a message: ::

        Debug.Log("OnDeleteMessage");
        if (eventData.IsError)
            Debug.LogError(eventData.Exception.Message);

14. Be sure to save this file!

Adding the Alexa Manager GameObject in Unity
============================================

1. Create a new **Empty GameObject** (``GameObject -> Create Empty``) and name it **Amazon Alexa**.
2. With your new GameObject selected, click **Add Component**, type **AlexaAlexaManager** and select the AlexaAlexaManager script.
3. Click **Add Component** again, type **LightControl** and select the LightControl script.
4. Fill the ``SQS Queue`` with the code sent from the Alexa skill when it launches.

**Note**: You will have to fill this in later, as we have not set up the Alexa skill yet.

5. Fill the ``Identity Pool Id`` with the one you created earlier.
6. Fill the ``AWS Region`` with the one you made note of earlier.
7. Fill the ``Table Name`` with the one your Alexa skill created.

**Note**: You will have to fill this in later, as we have not set up the Alexa skill yet.

8. Drag the **Cube** from the hierarchy into the box next to ``Light Cube``.

Wrapping Up
===========

Aside from a few minor updates, have finished the Unity project! Next Step: The Alexa Skill!