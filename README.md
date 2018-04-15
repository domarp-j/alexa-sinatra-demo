# Alexa Sinatra Demo - Number Facts

This is a quick primer for setting up an Alexa skill with Ruby! Go through the [AWS custom skills documentation](https://developer.amazon.com/docs/custom-skills/understanding-custom-skills.html) to see the full depth of Alexa's capabilities.

This guide will go through building an Alexa skill called "Fun Number Facts", which you can ask for interesting facts about numbers.

Based on a great guide by Maker's Academy: https://developer.amazon.com/alexa-skills-kit/makers-academy

## Setup

Note: These instructions are based on the developer console beta that exists as of *20 March 2018*.

### Part 1 - Initialize the skill

1. Create an AWS account, if you don't have one already
    - Note: it'll be easier to test your skill on your Alexa device if the AWS account has the same email as the Amazon account registered with the device
2. Log in and go to the AWS Developer Console: https://developer.amazon.com/edw/home.html#/
3. Select "Get Started" in the Alexa Skills Kit widget
4. Click "Create Skill"
5. Enter skill name, then press "Next"
6. Select "Custom" as the skill type, then select "Create skill"

### Part 2 - Create the interaction model

1. On the right side, you should see a skill builder checklist. Select the first step "Invocation Name".
2. Enter the **invocation name** of your skill
    - The invocation name is the name that Alexa will use to recognize your skill
    - "Alexa, ask **fun number facts** *about the number 11*" => The invocation name is **fun number facts**
3. Click "Save Model", then go back to the dashboard by clicking the "Build" tab on top
4. Under the skill builder checklist, select the second step "Intents, Samples, and Slots"
5. Create an **intent** and select "Create custom intent"
    - Intents are just the requests that will be sent to Alexa, allowing it to send a response
6. Enter one or more **sample utterances**
    - Sample utterances are essentially the specific commands you have for a skill
    - "Alexa, ask **fun number facts** *about the number 11*"" => The sample utterance is *about the number 11*
7. Optionally, add one or more **slots** to any sample utterance
    - Slots are parameters that you can add to sample utterances to make them dynamic
    - Add curly braces around the slot in the sample utterance field => "about the number {someNumber}"
    - Define the slot under "Intent Slots", defining both the name and slot type
8. Optionally, you can create a **custom slot type**
    - In the left side panel, select "Add" next to Slot Types
    - Enter a name for the slot type (like "FactType"), then press "Create custom slot type"
    - Add any possible values for that slot type, i.e. "trivia", "math", etc
    - Create an intent that uses that slot type, adding curly braces like you would for a normal slot => "tell me a {factType} fact about {someNumber}"
    - Define the new slot under "Intent Slots", defining both the name and setting its type to the custom slot type, which is whichever custom slot type you just created
    - Now, you can ask Alexa to "tell me a math fact about 11" or "tell me a trivia fact about 89"
9. Click "Save Model", then "Build Model"
10. Enter more intents, if you would like
11. Click on the "Build" tab to go back to the skill builder checklist, and wait for the third step "Build Model" to go green
12. Hold off on setting an endpoint for now

### Part 3 - Set up Sinatra & ngrok

1. If you haven't already, fork and clone this repo
2. Run `bundle install`
3. Download ngrok: https://ngrok.com/download
4. Unzip the downloaded ngrok contents and move the `ngrok` executable to this repo
5. Run `./ngrok http 4567` to run the Sinatra app locally on port 4567
    - We need ngrok to give us an HTTPS URL that we can use as an endpoint for Alexa
    - Alexa uses SSL to validate endpoints, so an HTTPS URL is mandatory
    - ngrok will give us a convenient public URL that will directly hit `localhost:4567` on our local machine
6. Copy the HTTPS path somewhere. We will use it shortly.
    - Look for a URL with the format https://e094a487.ngrok.io
    - Do not close this terminal while ngrok is running
7. Open a new terminal and run `ruby server.rb`
    - Note that `server.rb` already has a POST request defined, ready to accept requests passed along by Alexa
    - Note the format of the JSON response. This is how the Sinatra app tells Alexa how to reply.
    - Do not close this terminal while `server.rb` is running

### Part 4 - Endpoint setup

1. Go back to the AWS Developer Console for Alexa
2. Under the skil builder checklist, select the fourth and final step "Endpoint"
3. For the service endpoint type, select "HTTPS"
    - AWS Lambda does not yet support Ruby, which is why we need to create our own Sinatra web service in the first place
4. You should see two fields next to "Default Region". In the first field, enter the HTTPS ngrok URL copied above.
5. For the second field, the SSL certificate type, select the following:
    - "My development endpoint is a sub-domain of a domain that has a wildcard certificate from a certificate authority"
6. Click "Save Endpoints"
7. Select the "Build" tab at the top, and verify that all four skill builder checklist items are green

### Part 5 - Testing the skill

1. Select the "Test" tab at the top, and enable testing
2. Make sure that `ngrok` and `server.rb` are both running locally, in separate terminals
3. Test Alexa by doing one of the following:
    - Type in the commands to get Alexa's responses on your local machine
    - Speak to your Alexa-enabled device
        - If you have an Alexa that is already connected to the same email as your AWS account, you're good to go!
        - If not, follow these instructions to link the device to your developer account: https://developer.amazon.com/docs/custom-skills/test-a-custom-skill.html#h2_register
