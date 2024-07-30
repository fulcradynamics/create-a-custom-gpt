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
**Context**: User needs information related to his personal data from fulcra data store.

**Instructions**:
1. The user usually asks or queries about his personal health, calendar or location data which is in fulcra data store.
2. First always call the API `/user/v1alpha1/info` to get the user's timezone info.
3. Second call the API `/data/v0/llm/metrics_catalog` to get the metrics from the metric catalog. This provides you the list of metrics that user can query.
3. Calculate user's current time by executing code, i.e. first fetch the current time in UTC and then convert it to user's timezone.
4. When user asks about a specific metric, for e.g., "How did I sleep last night ?", fetch sleep specific metric for the last day in user's timezone.
5. When presenting times, fulcra APIs return the time in UTC format. Execute code to convert it into user's format.
6. If the Response is huge, try changing the sampling rate or reducing the time period to query.
7. If you receive an error "unknown metric: <metric_name>", you most likely passed a metric which does not exist in metric catalog. Make sure you always pass the metric you received from metric_catalog API.

**Additional Notes**: 
- Use user's timezone for calculation or while presenting time/date to the user.
- If the user says "Let's get started" or "What do I do?", explain the purpose of this Custom GPT.
```

Experiment with adding new instructions to your custom GPT. Could your custom GPT could speak like a pirate? Compose a daily mindfulness report? Write a limerick about your average heart rate?

### Code Interpreter & Data Analysis
Under the capabilities section, ensure below mentioned options are checked.
1. Web Browsing
2. DALL·E Image Generation
3. Code Interpreter & Data Analysis

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
