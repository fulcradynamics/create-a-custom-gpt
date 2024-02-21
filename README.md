# 🤖 Setting up a Custom GPT with Fulcra Integration

You can create & configure your own ChatGPT custom GPT via the following process:

## ⚙️ [OAuth] Get Application Credentials

OAuth applications allow ChatGPT to communicate with the Fulcra Life API on your behalf. Currently new applications are provisioned exclusively by Fulcra staff members. Reach out to a staff member on the Fulcra discord in [channel] and they can create a new OAuth application for you. You will receive two values that you will use when configuring your custom GPT, a **Client ID** and a **Client Secret**.

*Should we call out to building application/api key self service in the future?*

## ⚙️ [ChatGPT] Configure Your Custom GPT

Under **My GPTs**, click **Create a GPT**. From the **Configure** tab, enter the GPT Name and Description.

### GPT Instructions

Instructions allow you to influence the behavior of your custom GPT. You can copy & paste the following starter instructions into the **Instructions** text box and edit them to your own liking:

```
[to be determined]
```

### Configure Fulcra Life API Action

Under **Actions**, click on the **Create new action** button.

#### Authentication

In the **Authentication** section, click the gear on the right and select `OAuth` authentication. Enter the **Client ID** & **Client Secret** values you received earlier.

- Authorization URL is `https://auth.fulcradynamics.com/authorize`.
- Token URL is `https://auth.fulcradynamics.com/oauth/token`.
- Scope is `openid profile name email`.
- Token Exchange Method is `Default (POST request)`.

Click the save button to save the Authentication configuration.

#### Schema

In order to allow your custom GPT to know how to communicate with the Fulcra Life API, you'll need to import the Life API's OpenAPI schema. This schema tells ChatGPT what HTTP API endpoints it can talk to, as well as providing context for each endpoint.

Click the **Import from URL** button and import the schema from `https://api.fulcradynamics.com/data/v0/llm/openapi.json`. This will load the JSON spec into the Schema text box and populate a list of available actions.

**Note** Whenever Fulcra makes changes to the Life API, you will need to re-import the schema.

## ⚙️ [OAuth] Update OAuth application with redirect URL

After configuring & saving the action, the **Configure** tab will display a **Callback URL** value similar to `https://chat.openai.com/aip/<hash>/oauth/callback`.

Your OAuth application will need this value in order to complete the authentication process. Reach out and provide the redirect URL to a Fulcra team member and they will update your OAuth application. Once confirmed, this will complete your custom GPT setup and your custom GPT should be able to make requests to the Fulcra Life API on your behalf.
