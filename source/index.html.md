---
title: Quickstart Guide: How to build a Connector for LiveEngage

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

LivePerson provides a unified messaging platform for connecting your consumers with human agents and bot automations. As part of our open platform, we provide a growing number of out-of-the-box connectors, e.g., for SMS, Facebook Messenger, WhatsApp, and more. Please see **here** for a complete list of standard connectors available today. 

In cases where an out-of-the-box connector is not available you have the opportunity to build your own custom connector.

<aside class="success">
If you are not sure if you need to build a custom connector, contact your account manager to discuss the best solution fit for your application!
</aside>

This guide includes examples in different programming languages to help you get up and running quickly. If you are using Postman you can download a collection **here**

# Use Cases

A common use case for building a connector to LiveEngage Messaging are SMS Gateway integrations. LivePerson offers a SMS Twilio connector out-of-the-box, which is an excellent solution for most applications. However, you may have your own SMS Gateway or a custom Messaging app to integrate with the LiveEngage platform.

Connectors are exclusively server-side applications. If you are looking to build your own client-side application, please refer to the Messaging Window API documentation.

# Core concepts

LiveEngage Messaging is built around conversations. When a consumer first connects, they will do so by creating a conversation. A new conversation is assigned to an agent or bot. Not every Messaging app has a notion of conversation&mdash; in the case of SMS messages are of exchanged in a sequence. 

Conversations can be created or closed, and one consumer can only have one open conversation at a time. Agents and bots, on the other hand, can respond to multiple conversations. A conversation is closed when it ends, e.g., the agent has resolved the case and closes the conversation. There are also cases when a conversation is closed automatically, for example, when a consumer is no longer responding ('auto close'). Conversations can be split into dialogs of different types. The 'main' dialog is the one that holds the actual messaging exchange, however, depending on the application there could be more dialog types such as CoBrowse or Survey.

Consumers, agents, agent managers, or bots are all identifiable participants in a conversation. Often consumers are not only introduced using an ID but carry additional information, such as name and contact data. This information is sent by using <abbr title="In order to collect specific consumer information such as product viewed, purchase information, errors the visitor encountered, and search results, you can send this information by using Engagement Attributes.">Engagement Attributes</abbr>. 

# Prerequisites

Before getting started please make sure that Messaging is configured for your account, and a campaign with engagement is set up. Follow **this guide** for more details.

Filesharing enabled and tested (if used)

Installing an app

## Install a LivePerson app


If the initial app was deployed you can change the configuration of your webhook endpoints as self-service using the Sample Connector app (requires admin permissions)

## Set up a WebHook endpoint for development

WebHook endpoint

# Required LivePerson APIs

Implementing a custom connector requires several LivePerson APIs&mdash;depending on functional scope of your server application. Most of the required functionality is covered by Connector API, which is responsible for receiving and sending messages. Consumer messages are encoded as JSON and sent via HTTPS. If an agent or bot answers a notification is sent to your configured Webhook endpoint.

<aside class="info">
In the remainder of this tutorial, the LivePerson APIs listed below will be introduced by example. In order to learn more about the APIs click on the links in the table to see the developer documentation.
</aside>

API name | Purpose | API docs
-------------- | -------------- | --------------
Connector API | Send and receive messages on behalf of consumers | [API docs](https://developers.liveperson.com/connector-api-first-steps-overview.html)
Domain API | Retrieve domains for API services, e.g., authentication | [API docs](https://developers.liveperson.com/agent-domain-domain-api.html)
File Sharing API | Upload images and other media to to storage for file sharing |[API docs](https://developers.liveperson.com/file-sharing-file-sharing-for-web-messaging.html)
Monitoring API | Create visitor and session id to provide additional conversation context | [API docs](https://developers.liveperson.com/monitoring-api-overview.html)
Consumer Messaging History API | Query current and past conversations of a consumer | [API docs](https://developers.liveperson.com/consumer-messaging-history-api-overview.html)

<aside class="warning">
 The Domain API is used for retrieving service domains, e.g., for authentication servers and for sending messages. For development purposes domains can be retrieved once, however, server domains can be subject to change.
</aside>

# Authentication


> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

