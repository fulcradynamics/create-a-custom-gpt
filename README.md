# 🤖 Setting up a Custom GPT with Fulcra Life API Integration

Fulcra Users who are already sending data to the Fulcra Platform can integrate the Fulcra Life API with ChatGPT's Custom GPT functionality to create new and exciting ways to interact with their data.

You can create & configure your own ChatGPT custom GPT via the following process:

## ⚙️ [OAuth] Get Application Credentials

OAuth applications allow ChatGPT to communicate with the Fulcra Life API on your behalf.

Currently new applications are provisioned exclusively by Fulcra staff members. Reach out to a staff member on the Fulcra discord in `#custom-gpt-fulcra` and they can create a new OAuth application for you. You will receive two values that you will use when configuring your custom GPT, a **Client ID** and a **Client Secret**.

## ⚙️ [ChatGPT] Configure Your Custom GPT

Under **My GPTs**, click **Create a GPT**. From the **Configure** tab, enter the GPT Name and Description.

### GPT Instructions

Instructions allow you to influence the behavior of your custom GPT. You can copy & paste the following starter instructions into the **Instructions** text box:

```
1. At the beginning of a chat session make two initial requests to the Fulcra Life API before any other requests: First, make a request to the /user/v1/alpha1/info endpoint to retrieve the authenticated user's information. Second, make a request to the /data/v0/llm/metrics_catalog endpoint to retrieve a list of metrics that can be queried from the Fulcra Life API.

2. Assume all times specified by a user are given in the user's local time zone. You will know the user's time zone from the information retrieved in the first instruction. When making requests to the Fulcra Life API that take date/time arguments, convert the time given by the user to UTC and use that value in the Life API request.
```

Experiment with adding new instructions to your custom GPT. Could your custom GPT could speak like a pirate? Compose a daily mindfulness report? Write a limerick about your average heart rate?

### Configure Fulcra Life API Action

Under **Actions**, click on the **Create new action** button.

#### Authentication

In the **Authentication** section, click the gear on the right and select `OAuth` authentication. Enter the **Client ID** & **Client Secret** values you received earlier.

**Authorization URL**
```
https://auth.fulcradynamics.com/authorize
```

**Token URL**
```
https://auth.fulcradynamics.com/oauth/token
```

**Scope**
```
openid profile name email
```

**Token Exchange Method**
```
Default (POST request)
```

Click the save button to save the Authentication configuration.

#### Schema

In order to allow your custom GPT to know how to communicate with the Fulcra Life API, you'll need to import the Life API's OpenAPI schema. This schema tells ChatGPT what HTTP API endpoints it can talk to, as well as providing context for each endpoint.

Click the **Import from URL** button and import the schema from `https://api.fulcradynamics.com/data/v0/llm/openapi.json`. This will load the JSON spec into the Schema text box and populate a list of available actions.

> [!IMPORTANT]
> Whenever Fulcra makes changes to the Life API, you will need to re-import the schema.

You can now click back to return to the main GPT configuration.

## ⚙️ [OAuth] Update OAuth application with redirect URL

After configuring the action, the GPT configuration section will show a **Callback URL** value similar to `https://chat.openai.com/aip/<hash>/oauth/callback`. You will need this value to complete your custom GPT's ability to communicate with the Fulcra Life API.

Reach out and provide the redirect URL to a Fulcra team member and they will update your OAuth application. Once confirmed, this will complete your custom GPT setup and your custom GPT should be able to make requests to the Fulcra Life API on your behalf.

> [!CAUTION]
> If you make any changes to the Fulcra Life API Action of your custom GPT, it's highly likely the **Callback URL** will change. If authentication through your custom GPT starts giving `invalid_request` errors, double check the **Callback URL** value in your configuration and reach out to Fulcra staff with the new value.