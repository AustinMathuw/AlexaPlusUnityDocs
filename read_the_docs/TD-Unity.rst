***********************
Unity C# Technical Docs
***********************

Classes

AlexaBaseData
=============

Syntax :: 

    public abstract class AlexaBaseData : BaseEventData

Constructors
~~~~~~~~~~~~

AlexaBaseData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public AlexaBaseData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Properties
~~~~~~~~~~

Exception
^^^^^^^^^

Declaration :: 

    public Exception Exception { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Exception
      - 

IsError
^^^^^^^

Declaration :: 

    public bool IsError { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Boolean
      - 

Methods
~~~~~~~

BaseInitialize(Boolean, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    protected virtual void BaseInitialize(bool isError, Exception exception)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Boolean
      - isError
      - 
    * - System.Exception
      - exception
      - 

AmazonAlexaManager
==================

Syntax :: 

    public class AmazonAlexaManager

Constructors
~~~~~~~~~~~~

AmazonAlexaManager(String, String, String, String, String, String, GameObject, Action<HandleMessageEventData>, Action<ConnectionStatusEventData>, Boolean)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AmazonAlexaManager Constructor.

Declaration :: 

    protected virtual void BaseInitialize(bool isError, Exception exception)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.String
      - publishKey
      - Your PubNub publish key.
    * - System.String
      - subscribeKey
      - Your PubNub subscribe key.
    * - System.String
      - channel
      - Your player's channel. (Should be unique to the player)
    * - System.String
      - tableName
      - Name of your skill's DynamoDB table where the persistant attributes are stored.
    * - System.String
      - identityPoolId
      - Identifier of your AWS Cognito identity pool.
    * - System.String
      - AWSRegion
      - The AWS Region where your DynamoDB table and Cognito identity pool are hosted.
    * - GameObject
      - gameObject
      - The GameObject you are attaching this manager instance to.
    * - System.Action<HandleMessageEventData>
      - messageCallback
      - The callback for when a message is recieved from your Alexa Skill.
    * - System.Action<ConnectionStatusEventData>
      - connectionStatusCallback
      - 
    * - System.Boolean
      - debug
      - (Optional) True to debug.

Fields 
~~~~~~

handleConnectionStatusCallback
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The connection status recieved callback.

Declaration :: 

    public Action<ConnectionStatusEventData> handleConnectionStatusCallback

Field Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Action<ConnectionStatusEventData>
      - 

handleMessageCallback
^^^^^^^^^^^^^^^^^^^^^

The message recieved callback.

Declaration :: 

    public Action<HandleMessageEventData> handleMessageCallback

Field Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Action<HandleMessageEventData>
      - 

Properties
~~~~~~~~~~

alexaUserDynamoKey
^^^^^^^^^^^^^^^^^^

Gets or Resets the player's DynanoDB table key.

Declaration :: 

    public string alexaUserDynamoKey { get; set; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.String
      - The alexa user dynamo key.

channel
^^^^^^^

Resets your player's channel. (Should be unique to the player)

Declaration :: 

    public string channel { set; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.String
      - The channel.

Methods
~~~~~~~

GetSessionAttributes(Action<GetSessionAttributesEventData>)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gets the Skill's persistant session attributes from DynamoDB.

Declaration :: 

    public void GetSessionAttributes(Action<GetSessionAttributesEventData> callback)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Action<GetSessionAttributesEventData>
      - callback
      - The callback.

SendToAlexaSkill(Object, Action<MessageSentEventData>)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Sends a message to Alexa Skill. NOTE: Skill will only recieve the message if it is listening for a response.

Declaration :: 

    public void SendToAlexaSkill(object message, Action<MessageSentEventData> callback)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Object
      - message
      - The message.
    * - System.Action<MessageSentEventData>
      - callback
      - The callback.

SetSessionAttributes(Dictionary<String, AttributeValue>, Action<SetSessionAttributesEventData>)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Sets the Skill's persistant session attributes in DynamoDB.

Declaration :: 

    public void SetSessionAttributes(Dictionary<string, AttributeValue> attributes, Action<SetSessionAttributesEventData> callback)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Collections.Generic.Dictionary<System.String, AttributeValue>
      - attributes
      - The attributes to set.
    * - System.Action<SetSessionAttributesEventData>
      - callback
      - The callback.

ConnectionStatusEventData
=========================

Syntax :: 

    public class ConnectionStatusEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

ConnectionStatusEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public ConnectionStatusEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Properties
~~~~~~~~~~

Category
^^^^^^^^

Declaration :: 

    public PNStatusCategory Category { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - PNStatusCategory
      - 

Methods
~~~~~~~

Initialize(Boolean, PNStatusCategory, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(bool isError, PNStatusCategory category, Exception exception = null)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Boolean
      - isError
      - 
    * - PNStatusCategory
      - category
      - 
    * - System.Exception
      - exception
      - 

ErrorEventData
==============

Syntax :: 

    public class ErrorEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

ErrorEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public ErrorEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Methods
~~~~~~~

Initialize(Boolean, PNStatusCategory, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(Exception exception, bool isError = true)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Exception
      - exception
      - 
    * - System.Boolean
      - isError
      - 

GetSessionAttributesEventData
=============================

Syntax :: 

    public class GetSessionAttributesEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

GetSessionAttributesEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public GetSessionAttributesEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Properties
~~~~~~~~~~

Values
^^^^^^

Declaration :: 

    public Dictionary<string, AttributeValue> Values { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Collections.Generic.Dictionary<System.String, AttributeValue>
      - 

Methods
~~~~~~~

Initialize(Boolean, Dictionary<String, AttributeValue>, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(bool isError, Dictionary<string, AttributeValue> values, Exception exception = null)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Exception
      - exception
      - 
    * - System.Collections.Generic.Dictionary<System.String, AttributeValue>
      - values
      - 
    * - System.Boolean
      - isError
      - 

HandleMessageEventData
======================

Syntax :: 

    public class HandleMessageEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

HandleMessageEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public HandleMessageEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Properties
~~~~~~~~~~

Message
^^^^^^^

Declaration :: 

    public Dictionary<string, object> Message { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Collections.Generic.Dictionary<System.String, System.Object>
      - 

Methods
~~~~~~~

Initialize(Boolean, Dictionary<String, Object>, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(bool isError, Dictionary<string, object> message, Exception exception = null)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Boolean
      - isError
      - 
    * - System.Collections.Generic.Dictionary<System.String, System.Object>
      - message
      - 
    * - System.Exception
      - exception
      - 

MessageSentEventData
====================

Syntax :: 

    public class MessageSentEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

MessageSentEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public MessageSentEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Properties
~~~~~~~~~~

Message
^^^^^^^

Declaration :: 

    public object Message { get; }

Property Value

.. list-table:: 
    :widths: 20 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Description
    * - System.Object
      - 

Methods
~~~~~~~

Initialize(Boolean, Object, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(bool isError, object message, Exception exception = null)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Boolean
      - isError
      - 
    * - System.Object
      - message
      - 
    * - System.Exception
      - exception
      - 

SetSessionAttributesEventData
=============================

Syntax :: 

    public class SetSessionAttributesEventData : AlexaBaseData

Constructors
~~~~~~~~~~~~

SetSessionAttributesEventData(EventSystem)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public SetSessionAttributesEventData(EventSystem eventSystem)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - EventSystem
      - eventSystem
      - 

Methods
~~~~~~~

Initialize(Boolean, Exception)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declaration :: 

    public void Initialize(bool isError, Exception exception = null)

Parameters

.. list-table:: 
    :widths: 10 10 20
    :header-rows: 1
    :stub-columns: 0

    * - Type
      - Name
      - Description
    * - System.Boolean
      - isError
      - 
    * - System.Exception
      - exception
      - 