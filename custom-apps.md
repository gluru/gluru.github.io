# How to implement custom applications

Customers and integrators can extend the platform by creating their own client
applications and either use them on their account or publish them in their
own ecosystem.

# Authorization and permission scopes

Applications are exclusively authorized to interact with the system for the
purpose of communicating with the dialog engine, meaning that they can only:
 *  read the default app configuration
 *  start conversations
 *  post queries (i.e. ask questions) into a conversation
 *  post click events into a conversation
 *  connect with the dialog websocket

Customers willing to perform any other action, for instance injecting data
or controlling an account, should instead authenticate with credentials
granting the required permissions. See the tutorial on
[how to create a custom fetcher](./custom-fetchers) for more details.  

An administrator with permission to write the domain settings can create a new
application using the rest APIs. Applications ID (or `app_id`s) are used by the
system to allow access to a client application.
The main purpose of an `app_id` is to identify an app and to revoke its access
in case it gets compromised. From a security perspective the app key should
be kept secret if possible but it should not considered as a password.

In order to use the REST APIs applications have to request the system for a
short lived access token which is given with restricted access acknowledging
that the application is recognized but the user using it is not authenticated.
This application tokens expire after a short amount of time, when that happens
the client is required to get a new one.

Customers are discouraged from re-using the same `app_id` for multiple purposes
because it would reduce the ability to counter malicious attacks. Imagine for
example that the same Kare MIND account would be used to power two portal one
internal and one public facing with two different applications. If the client
application deployed in the public portal is then targeted by a DDoS attack the
domain administrator will be able to disable it without impacting the internal
portal.

Reference in APIs v2.1:
 *  Creating a new app [POST /settings/application](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/settings/postApplication)
 *  Getting an app token [GET /apps/token](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/apps/getToken)


# Getting the default app settings

Administrators with permission to write domain settings can configure the
default for how clients should behave. This is normally done from the console
but it can also done using the APIs.

Applications have the option to load such configuration or to ignore it. Using
the default configuration is useful to allow administrator to remotely control
the look and feel of all applications at once.
To load the default configuration applications can use the get
configuration endpoint.  

Reference in APIs v2.1:
 *  Get configuration [GET /apps/config](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/apps/getConfig)


# Starting a new conversation

As soon a user is ready to start interacting with the dialog engine the
application should start a conversation. That can be done by calling the
start conversation endpoint.

It is the client responsibility to start new conversations when necessary. This
will however affect how messages are aggregated and counted in the rest of the
platform.
Ideally clients should start a new conversation every time
the user is opening the application and start a new one if the user comes back
after having closed the app.

Clients are meant to cache the conversation until they need it. It is possible
for a client to download all previous messages from a given
conversation. However, to prevent malicious attempts of stealing data,
downloading historical messages requires permission to read the knowledge which
is only granted upon authentication.

Reference in APIs v2.1:
 *  Starting a new conversation [POST /dialog/conversation](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/dialog/startConversation)
 *  Downloading historical messages [GET /activity/conversations/{conversation_id}/events](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/activity/listConversationEvents)

# Asynchronous interaction

The recommended way for interacting with the dialog engine is by using a
websocket. Once the websocket is connected client and server can exchange
messages asynchronously.

```
curl 'wss://socket.stg.karehq.com/ws?app_id={{ YOUR APP ID }}&conversation_id={{ YOUR CONV ID }}&host_locale=&selected_locale=&user_id=' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Language: en-US,en;q=0.9' -H 'Sec-WebSocket-Key: {{ KEY }}' -H 'Upgrade: websocket' -H 'Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits' -H 'Cache-Control: no-cache' -H 'Connection: Upgrade' -H 'Sec-WebSocket-Version: 13' --compressed
```

For convenience the WS can also authenticate the client implicitly using the
`app_id` instead of a bearer token.

## Messages and communication flow.

The communication flow happens by exchanging messages between the client and
the dialog engine. The conversation progress by exchanging messages. Messages
have a `uri` which defines what they are, an `aspect` which defines how they are
meant to be rendered, an `id` which uniquely identifies them and a `parent`
which identify where they are coming from. Inside a conversation messages are
organized as a forest with each query starting a new tree.

The communication flow can be simplified as follows:
 1. The client sends a `in/hi` to start the conversation.
 2. The engine replies with a `out/hi` to acknowledge the conversation and with
    a series of `out/*` depending ho the configuration.
 3. The client sends a `in/query` message to submit a question or a `in/click`
    to trigger a given action. Sending a query or clicking on a given action
    are the only two ways with which the user can interact with the engine.
 4. The engine replies with a series of `out/*`.
 5. The previous two steps are repeated until the user closes the application.
    If the server decides to terminate the conversation it can do so by sending
    an `out/bye` message.

## Actions and Commands

Outbound messages can contain trigger actions which the user can activate by
clicking them. Actions have triggers which are executed either when the action
is shown to the user (`on_load`) or when the user is clicking on it (`on_click`).

When triggered actions request the client to execute a certain command. The dialog
engine uses commands to describe the desired behaviour to the client, and ideally
clients should try to comply with the request to the best of their capabilities.

### Print

`print` instructs the client to print a message in the conversation on behalf
of the user. This is intended to give the user a visual aid on what his actions
are doing.

Params:
 * `m`: the message to echo.

Example: `print -m="Some text"`.

### Send

`send` instructs the client to send a message to the server. This is used so
that the server can log the action and respond to it.

Params:
 * `m`: the message to post.

Example: `send -m=<msg>`.

### Close

`close` instructs the client to close and terminate. This normally indicates
that the conversation has been escalated or is ended.

Example: `close`.

### Open 

`open` instructs the widget to open a resource.

Params:
 * `href`: the url target to open in a new window.
 * `mailto`: an email address to open with the system mail client.
 * `ext`: an extension to open. These are hardcoded in the widget.
 * `escalate`: used to run the escalation script.
 * `zopim`: used to open Zopim, if present.

Example: `open -href="https://foo.biz"`

### Update 

`update` changes the property of an action.

Params:
 * `context`: update the context of an action.
 * `enabled`: enables or disables an action.
 * `icon`: sets the value of an icon.
 * `text`: sets the text of an action.
 
Example: `update -context="warning"`.  

## Icons

The dialog engine assumes that each client will have an icon set of its choice
according with the desired look and feel. When an action requires an icon
the engine will suggest which icon to use using an identifier.

The dialog requires the following icons:
 *  `empty`: no icon required
 *  `contact`:contact us
 *  `done`: all done
 *  `link`: opening a link
 *  `thumb_up`: thumb up
 *  `thumb_down`: thumb down
 *  `false`: false
 *  `resolution`: opening a resolution
 *  `attachment`: opening a document

## `out/text` aspects

When receiving a `out/text` message clients are supposed to be able to render
the following aspects.

 *  `resolution/missing`: a message indicating that no suitable answer is found.
 *  `resolution/answer`: the parent message for an answer. Usually the empty
     parent node for all the elements of the answer.
 *  `resolution/answer/text`: a markdown text block, child of an answer node.
 *  `resolution/answer/quote`: a quote block used used to quote documents.
 *  `resolution/answer/choice`: a choice block used to compose dialogs.
 *  `resolution/answer/image`: an image block containing the url of a picture.
 *  `resolution/answer/video`: a video block containing the url of a video.
 *  `resolution/answer/feedback`: a feedback block asking the suer for feedback.
 *  `resolution/disambiguation`: a disambiguation block used to ask the user
    to refine it's question.
 *  `text/plain`: a general purpose markdown text message. Generally used for
    adding conversational glue into the dialog.
 *  `language/unsupported`: a message stating that the language used by the user
    is not supported.
 *  `language/unknown`: a message stating a problem while detecting which
    language was used.

## Advanced tracking

The protocol facilitates the collection of signals to better understand the
user and to monitor their behaviour. 

### User tracking

It is possible to trace the user across multiple conversations by passing
the `user_id` url parameter when establishing the connection.

### Metadata

The engine automatically logs as metadata all url parameters starting with
the `kare-meta-` prefix as indicated in the [JavascriptSDK](./javascript-sdk)
documentation. 


### Tracking custom events

Clients can track their own events by sending `in/click` messages with the 
right aspect. The following is an example with a `open/link` aspect. 

When creating a custom event it is important to assign the `parent_id` of
the message used to show the content to the user. The client can assign the
new events an `id` as long as it is a valid uuid, otherwise the engine will
auto assign one.

```
{
  "id":"458d130b-633c-42bd-91d3-bfce6c7f0006",
  "parent_id":"f2975053-0a74-4948-9a80-63ba157157c1",
  "uri":"in/click",
  "header":{},
  "body":{
    "target":{
      "aspect":"open/link",
      "choice_id":"https://developer.karehq.com/custom-apps",
      "text":"Kare developer portal"
    }
  }
}
```

# Using the REST interface

It is also possible to interact with the dialog engine synchronously by using
the REST APIs instead of the websocket. The endpoints are synchronous and wait
for the dialog to have published all the messages before returning them.

Customers are advised to use the websocket if possible.     

## Submitting a query

Applications can submit a query by using the post query endpoint. This has
the same effect of sending and `in/query` message.

The response is the list of messages that the dialog engine has generated
to answer the question.

Reference in APIs v2.1:
 *  Submit a query [POST /dialog/query](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/dialog/postQuery)


## Submitting a click event

Applications can submit click events by using the post event endpoint. This has
the same effect of sending and `in/click` message.

The response is the list of messages that the dialog engine has generated
to continue the conversation.

Reference in APIs v2.1:
 *  Submit a click event [POST /dialog/click](http://swaggerui.herokuapp.com/?url=https://s3-eu-west-1.amazonaws.com/gluru-docs/swagger-specs/core/mind/api/public/v2.1/swagger.json#!/dialog/postClick)
