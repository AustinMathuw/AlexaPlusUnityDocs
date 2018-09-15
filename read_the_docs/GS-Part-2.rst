**********************
Setup for Alexa Skills
**********************

Now that we have configured our Unity3D workspace, it's time to set up the Alexa Skill!

Prerequisites
=============

-  An `NPM <https://www.npmjs.com/>`_ project. For information on how to set up a NPM project, please see `this <https://docs.npmjs.com/getting-started/creating-node-modules>`_.
-  A suitable Node.js development environment. The ASK SDK v2 for Node.js requires Node 4.3.2 or above.

Integrating the Alexa Plus Unity SDK into your Alexa Skill
==========================================================

1. Download the ``alexa-gaming-cookbook.js`` from the `Releases <https://github.com/AustinMathuw/AlexaPlusUnity/releases>`_ tab in the GitHub project.
2. Put the newly downloaded file in the root directory of your Alexa Skill
3. Include the script in your skill with::

    var alexaGaming = require('./alexa-gaming-cookbook.js');

That's it! However, in order to handle the communication to and from your game, you need to use the script's methods. See the Tutorial for a more in-depth implementation.