## Add a messaging extension to your app

Building a messaging extension involves implementing familiar Microsoft Teams developer platform concepts like bot APIs, cards, and tabs.

At its core, a messaging extension is a cloud-hosted service that listens to user requests and responds with structured data, such as cards. You integrate your service with Microsoft Teams via Bot Framework `Activity` objects. Our .NET and Node.js [extensions for the Bot Builder SDK](~/get-started/code#microsoft-teams-extensions-for-the-bot-builder-sdk) can help you add messaging extension functionality to your app.

![Diagram of message flow for messaging extensions](~/assets/images/compose-extensions/ceflow.png)

### Register in the Bot Framework

If you haven’t done so already, you must first register a bot with the Microsoft Bot Framework. (See [Create a bot](~/concepts/bots/bots-create) for instructions.) The Microsoft app ID and callback endpoints for your bot, as defined there, will be used in your messaging extension to receive and respond to user requests. Remember to enable the Microsoft Teams channel for your bot.

Record your bot’s app ID and app password—you will need to supply the app ID in your app manifest.

### Update your app manifest

As with bots and tabs, you update the [manifest](~/resources/schema/manifest-schema#composeextensions) of your app to include the messaging extension properties. These properties govern how your messaging extension appears and behaves in the Microsoft Teams client. Messaging extensions are supported beginning with v1.0 of the manifest.

#### Declare your messaging extension

To add a messaging extension, include a new top-level JSON structure in your manifest with the `composeExtensions` property. Currently, you are limited to creating a single messaging extension for your app.

> [!NOTE]
> The manifest refers to messaging extensions as `composeExtensions`. This is to maintain backward compatibility.

The extension definition is an object that has the following structure:

| Property name | Purpose | Required? |
|---|---|---|
| `botId` | The unique Microsoft app ID for the bot as registered with the Bot Framework. This should typically be the same as the ID for your overall Teams app. | Yes |
| `scopes` | Array declaring whether this extension can be added to `personal` or `team` scopes (or both). | Yes |
| `canUpdateConfiguration` | Enables **Settings** menu item. | No |
| `commands` | Array of commands that this messaging extension supports. You are limited to 10 commands. | Yes |

#### Define commands

Your messaging extension should declare one command, which appears when the user selects your app from the **More options** (**&#8943;**) button in the compose box.

![Screenshot of list of messaging extensions in Teams](~/assets/images/compose-extensions/compose-extension-list.png)

In the app manifest, your command item is an object with the following structure:

| Property name | Purpose | Required? |
|---|---|---|
| `id` | Unique ID that you assign to this command. The user request will include this ID. | Yes |
| `title` | Command name. This value appears in the UI. | Yes |
| `description` | Help text indicating what this command does. This value appears in the UI. | Yes |
| `type` | Set the type of command. Possible values include `query` and `action`. If not present the default value is set to `query` | No |
| `initialRun` | Optional parameter, used with `query` commands. If set to **true**, indicates this command should be executed as soon as the user chooses this command in the UI. | No |
| `fetchTask` | Optional parameter, used with `action` commands. Set to **true** to fetch the adaptive card or web url to display within the [task module](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/task-modules/task-modules-overview). This is used when the inputs to the `action` command is dynamic as opposed to a static set of parameters. Note that the if set to **true** the static parameter list for the command is ignored | No |
| `parameters` | Static list of parameters for the command. | Yes |
| `parameter.name` | The name of the parameter. This is sent to your service in the user request. | Yes |
| `parameter.description` | Describes this parameter’s purposes or example of the value that should be provided. This value appears in the UI. | Yes |
| `parameter.title` | Short user-friendly parameter title or label. | Yes |
| `parameter.inputType` | Set to the type of input required. Possible values include `text`, `textarea`, `number`, `date`, `time`, `toggle`. Default is set to `text` | No |