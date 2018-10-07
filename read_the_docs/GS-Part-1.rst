***************
Setup for Unity
***************

Let's setup the Alexa Plus Unity SDK!

Prerequisites
=============

-  `Unity <https://unity3d.com/>`_ version 4.x or above.
-  An `AWS Account <https://aws.amazon.com/>`_

Integrating the Alexa Plus Unity SDK into your Unity project
============================================================

1. Download the ``AlexaPlusUnity.unitypackage`` from the `Releases <https://github.com/AustinMathuw/AlexaPlusUnity/releases>`_ tab in the GitHub project.
2. Open your project in Unity.
3. Open your newly downloaded ``AlexaPlusUnity.unitypackage`` and import the package.
4. In a scene, create an empty GameObject.
5. Attach the *Amazon Alexa Manager* script to your newly created GameObject.

That's it! However, in order to handle the communication to and from Alexa, you need to create your own script to initalize the manager. See the Tutorial for a more in-depth implementation.