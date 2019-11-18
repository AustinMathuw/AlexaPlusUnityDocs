**************************
Creating the Unity Project
**************************

Let's setup the Unity Project!

If you get lost or want to see the final project, the light control demo scene is located in ``Games SDK for Alexa\Examples``. You will need to create a project and add the Games SDK for Alexa asset package from the Unity Asset Store to gain access to this folder. 

Prerequisites
=============

-  `Unity <https://unity3d.com/>`_ version 4.x or above.
-  Configure AWS and Pubnub as explained `in configuration <https://games-sdk-for-alexa.readthedocs.io/en/latest/GS-Configuration.html>`_.

Creating the project
====================

1. Open Unity.
2. Create a new 3D project.
3. Add a cube to the SampleScene (``GameObject -> 3D Object -> Cube``).

Integrating the Games SDK for Alexa Package
========================================

1. Download Games SDK for Alexa from the `Unity Asset Store <http://u3d.as/1kfP>`_.
2. Add the asset package to your Unity project.
3. Make sure everything is checked and click **Import**.

You should now see a new folder in you Assets folder, **Games SDK for Alexa**.

Adding the LightControl script into the Unity project
=====================================================

1. With your project **open**, create a new script (``Assets -> Create -> C# Script``) and name it **LightControl**.
2. Open your LightControl script by double-clicking it.
3. Delete everything in the script.
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
            //Step 6: Add Games SDK for Alexa initialization

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

        private void GetObjectInDirection(string type, string message)
        {
            //Step 11: Get the object in a specific direction (Note: For this demo, there is only one object, the cube)

        }
        
        public void UpdateLight(string type, string value, GetSessionAttributesEventData eventData)
        {
            //Step 12: Update the light based on the incoming message, then save the state of the light through the skill's session attributes
            
        }

        public void SetAttributesCallback(SetSessionAttributesEventData eventData)
        {
            //Step 13: Callback for when session attributes have been updated
            
        }

        
        public void OnMessageSent(MessageSentEventData eventData)
        {
            //Step 14: Callback for when a message is sent
            
        }
    }

The above code is our skeleton for our script. We will fill this skeleton step by step. The steps below corrospond to the step numbers in the skeleton. Place the code for each of the below steps under their step number in the skeleton.

.. Note:: There may be IDE errors as we continue, but those will be resolved at the end when the skeleton is complete.

5. Define the class variables: ::

    public string publishKey;
    public string subscribeKey;
    public string channel;
    public string tableName;
    public string identityPoolId;
    public string AWSRegion = RegionEndpoint.USEast1.SystemName;
    public bool debug = false;
    public GameObject lightCube;
    public GameObject camera;

    private Dictionary<string, AttributeValue> attributes;
    private AmazonAlexaManager alexaManager;

These variables are necessary to preform initialization and enable reusablity of the Alexa Manager within our LightControl script.

6. Initialize the Alexa Manager: ::

        UnityInitializer.AttachToGameObject(gameObject);
        AWSConfigs.HttpClient = AWSConfigs.HttpClientOption.UnityWebRequest;
        alexaManager = new AmazonAlexaManager(publishKey, subscribeKey, channel, tableName, identityPoolId, AWSRegion, this.gameObject, OnAlexaMessage, null, debug); //Initialize the Alexa Manager

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

        if (!PlayerPrefs.HasKey("alexaUserDynamoKey")) //If the AlexaUserId has not been recieved from Alexa (If the user has not opened the skill)
        {
            Debug.LogError("'alexaUserDynamoKey' not found in PlayerPrefs. We must establish connection from Alexa to set this. Please open the skill to set the 'AlexaUserId' PlayerPref.");
        } else {
            alexaManager.GetSessionAttributes((result) =>
            {
                if (result.IsError)
                    Debug.LogError(result.Exception.Message);
                UpdateLight("Color", "blue", result);
            });
        }

10. Listen for new messages from the Alexa skill: ::

        Debug.Log("OnAlexaMessage");

        Dictionary<string, object> message = eventData.Message;

        //Get Session Attributes with in-line defined callback
        switch (message["type"] as string)
        {
            case "AlexaUserId":
                Debug.Log("AlexaUserId: " + message["message"]);
                alexaManager.alexaUserDynamoKey = message["message"] as string;
                break;
        }

        alexaManager.GetSessionAttributes((result) =>
        {
            if (result.IsError)
                Debug.LogError(eventData.Exception.Message);

            switch (message["type"] as string)
            {
                case "AlexaUserId":
                    ConfirmSetup(result);
                    break;
                case "Color":
                    Debug.Log("Requested Light Color: " + message["message"]);
                    UpdateLight(message["type"] as string, message["message"] as string, result);
                    break;
                case "State":
                    Debug.Log("Requested Light State: " + message["message"]);
                    UpdateLight(message["type"] as string, message["message"] as string, result);
                    break;
                case "GetObject":
                    Debug.Log("Requested object direction: " + message["message"]);
                    GetObjectInDirection(message["type"] as string, message["message"] as string);
                    break;
                default:
                    break;
            }
        });

11. Get object in a direction: ::

        RaycastHit hit;
        Dictionary<string, string> messageToAlexa = new Dictionary<string, string>();
        Vector3 forward = camera.transform.forward * 10;
        messageToAlexa.Add("object", "nothing");

        if (Physics.Raycast(camera.transform.position, forward, out hit, (float)15.0))
        {
            if (hit.rigidbody)
            {
                messageToAlexa.Remove("object");
                messageToAlexa.Add("object", hit.rigidbody.name);
            }
        }

        alexaManager.SendToAlexaSkill(messageToAlexa, OnMessageSent);

12. Update the light: ::

        attributes = eventData.Values;
        if (type == "Color")
        {
            attributes["color"] = new AttributeValue { S = value }; //Set color attribute to a string value
        }
        else if (type == "State")
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

13. Let's be notified when there is a error setting the attributes: ::

        Debug.Log("OnSetAttributes");
        if (eventData.IsError)
            Debug.LogError(eventData.Exception.Message);

14. Let's be notified when there is a error deleting a message: ::

        Debug.Log("OnMessageSent");
        if (eventData.IsError)
            Debug.LogError(eventData.Exception.Message);

15. Be sure to save this file!

Adding the Alexa Manager GameObject in Unity
============================================

1. Create a new **Empty GameObject** (``GameObject -> Create Empty``) and name it **Amazon Alexa**.
2. With your new GameObject selected, click **Add Component**, type **LightControl** and select the LightControl script.
3. Fill the ``Publish Key`` with the Pubnub publish key you made note of during configuration.
4. Fill the ``Subscribe Key`` with the Pubnub subscribe key you made note of during configuration.
5. Fill the ``Channel`` with the code sent from the Alexa skill when it launches.

.. Note:: You will have to fill this in later, as we have not set up the Alexa skill yet.

6. Fill the ``Table Name`` with **AlexaPlusUnityTest**.
7. Fill the ``Identity Pool Id`` with the one you created during configuration.
8. Fill the ``AWS Region`` with the one you made note of during configuration.
9. Check the box next to ``Debug`` to enable detailed logging.
10. Drag the **Cube** from the hierarchy into the box next to ``Light Cube``.
11. Drag the **Main Camera** from the hierarchy into the box next to ``Camera``.

Wrapping Up
===========

Aside from a few minor updates, we have finished the Unity project! Next Step: The Alexa Skill!
