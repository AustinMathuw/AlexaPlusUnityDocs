**********************
Setup for Alexa Skills
**********************

Now that we have configured our Unity workspace, it's time to set up the Alexa Skill!

Prerequisites
=============

-  An `NPM <https://www.npmjs.com/>`_ project. For information on how to set up a NPM project, please see `this <https://docs.npmjs.com/getting-started/creating-node-modules>`_.
-  A suitable Node.js development environment. The ASK SDK v2 for Node.js requires Node 4.3.2 or above.

Integrating the Games SDK for Alexa into your Alexa Skill
==========================================================

1. Navigate to your skill fuction in an command prompt or terminal
2. Install the AlexaPlusUnity package into yor skill's function: ``npm install alexaplusunity``
3. Include the package in your skill with::

    var alexaPlusUnityClass = require('alexaplusunity');

4. Create an instance of the class with::

    var alexaPlusUnity = alexaPlusUnityClass("<YOUR_PUBNUB_PUB_KEY>", "<YOUR_PUBNUB_SUB_KEY>", true); //Third parameter enables verbose logging

That's it! However, in order to handle the communication to and from your game, you need to use the script's methods. See a Tutorial or the `package page <https://www.npmjs.com/package/alexaplusunity>`_ for more in-depth information.