**************************
Frequently Asked Questions
**************************

On `ask deploy`: [Warn]: Please make sure you are in the root directory of the project.
===================================================================================================

If you get this error, please try the following:

-  Make sure the skill.json file is in the same folder where you are running the `ask deploy` command.
-  Make sure you are using the `ASK CLI v1 <https://www.npmjs.com/package/ask-cli/v/1.7.23>`_.
.. Note:: The npm command to install the ASK CLI v1: `npm i -g ask-cli@1.7.23`

GetSessionAttributes: The service returned an error with HTTP Body: Cannot resolve destination host
===================================================================================================

If you get this error, please try the following:

-  Make sure you are connected to the internet and the Unity Project can access the internet.
-  Make sure the DynamoDB table name matches in both the Alexa Skill and Unity Project.
-  Run the Alexa Skill and make sure the DynamoDB table was created.
