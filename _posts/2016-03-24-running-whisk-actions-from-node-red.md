---
published: true
layout: post
title: "Running OpenWhisk Actions From Node-RED"
categories:
  - Cloud
  - Bluemix
---

At IBM InterConnect 2016, IBM announced a new experimental compute runtime for Bluemix
called [OpenWhisk](https://new-console.ng.bluemix.net/openwhisk/?cm_sp=bluemixblog-_-content-_-cta&cm_mc_uid=99069416036614567943703&cm_mc_sid_50200000=1458847907).  What is OpenWhisk?

> OpenWhisk is an event-driven compute platform that executes application logic in response to events or through direct
> invocations–from web/mobile apps or other endpoints.

If you are familiar with Amazon's Lambda feature, it is very similar.  If you
want to learn more about OpenWhisk there are number of resources to help you get started.  First is the [documentation on Bluemix](https://new-console.ng.bluemix.net/docs/openwhisk/index.html).  The documentation provides a great explanation of the technology as well as
some examples to get you started.  You can also read this
[blog post](https://developer.ibm.com/bluemix/2016/02/25/bluemix-openwhisk-overview/)
on the Bluemix developerWorks site for some high level information.  In addition, OpenWhisk
is actually an open source technology, so the [GitHub repo](https://github.com/openwhisk/openwhisk)
also has a bunch of useful information.

At the core of OpenWhisk is this concept of [actions](https://new-console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html).  Actions are the objects you
run in response to events.  There are a number of ways to run them.  For example,
they can be run via the [OpenWhisk CLI](https://console.ng.bluemix.net/openwhisk/cli).

~~~sh
$ wsk action invoke hello-world --blocking --result
{
    "payload": "Hello, World!"
}
~~~

There is also a [Swift API](https://new-console.ng.bluemix.net/docs/openwhisk/openwhisk_mobile_sdk.html)
to run OpenWhisk actions.

~~~Swift
// In this example, we are invoking an action to print a message to the OpenWhisk Console
var params = Dictionary<String, String>()
params["payload"] = "Hi from mobile"

do {
    try whisk.invokeAction(name: "helloConsole", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: false, callback: {(reply, error) -> Void in
        if let error = error {
            //do something
            print("Error invoking action \(error.localizedDescription)")
        } else {
            print("Action invoked!")
        }

    })
} catch {
    print("Error \(error)")
}
~~~

There
is also a [REST API](https://new-console.ng.bluemix.net/apidocs/98) you can use which
can be used from any runtime you might be using.  As I continue to explore OpenWhisk,
I wanted to experiment with the REST API and decided an easy way to do this was with
using [Node-RED](http://nodered.org).  I have
[already discussed](http://ryanjbaxter.com/2015/01/13/sample-node-red-flow-using-websockets/)
how easy it is to invoke REST APIs from Node-RED,
so using the OpenWhisk REST APIs with Node-RED should be super simple, and it was!

Once you have some actions created (you can try this out with the samples from
the documentation) you can invoke those actions using the [`/invokeAction` endpoint](https://new-console.ng.bluemix.net/apidocs/98#invokeactionPOST).
Here is an example of the URL you would make the POST request to in order to do this.

`https://openwhisk.ng.bluemix.net/api/v1/namespaces/ET_Demo_org_demo/actions/language-profile?blocking=true`

To invoke your own action you will need to change the `namespaces` and `actions` path parameters in the above URL.  Use your namespace and your action name instead.

If your action takes parameters, than the POST body would be a JSON object containing
those parameters.  For example

~~~JSON
{
  "twitter": "ryajbaxter",
  "github": "ryanjbaxter"
}
~~~

Finally you will need to authenticate with the OpenWhisk service in order to make the request.
Unfortunately, right now OpenWhisk on Bluemix does not have the concept of API Keys
and Tokens for use by applications so you need to use your own credentials.  To retrieve your Bluemix OpenWhisk credentials
you can run a OpenWhisk CLI command.

~~~sh
$ wsk property get --auth
~~~

If you want to get a base 64 encoded version of your credentials suitable to include
in an HTTP request you can run

~~~sh
$ wsk property get --auth | awk '{printf("%s", $3)}' | base64
~~~

The output of the above command can be included in the `Authorization` header in your
HTTP request.

Putting this all together you could invoke this action using the REST API by issuing
this cURL command

~~~sh
$ curl -X "GET" "https://openwhisk.ng.bluemix.net/api/v1/namespaces/ET_Demo_org_demo/actions/language-profile?blocking=true" \
	-H "Authorization: Basic <base64 encoded credentials>" \
	-H "Content-Type: application/json" \
	-d "{\"twitter\":\"ryajbaxter\",\"github\":\"ryanjbaxter\"}"
~~~

OK, so what about Node-RED?  Well now that we know how to invoke an action using
the OpenWhisk REST API we can easily invoke that action via a Node-RED flow as well.
The http request node in Node-RED makes calling any HTTP endpoint extremely easy.
Lets take a look at a simple flow that replicates the cURL command above.

In your Node-RED instance drag and drop and http request node from the function section
onto your sheet.  Once you have the node on your sheet double click it to open the configuration
properties.  In the Method drop down select POST.  In the URL field enter the URL to invoke
your OpenWhisk task.  Once you have done that you can click OK to close the configuration
dialog.

![HTTP Request](/wp-content/uploads/2016/03/Screen Shot 2016-03-25 at 12.31.09 PM.png)

You will also need to set the `Authorization` header on the request the http request node
makes.  The documentation specifies that you can set headers on the request by setting
`msg.headers` on the incoming msg object.  To set the `msg.headers` property we can
use a function node.  Drag and drop a function node from the function section to your sheet.
Double click the function node to open the editor and add the following code.

~~~javascript
msg.headers = {
      "Authorization": "Basic <base 64 encoded credentials>"  
    };
return msg;
~~~

Just make sure you replace `<base64 encoed credentials>` with the result of running
`$ wsk property get --auth | awk '{printf("%s", $3)}' | base64`.

If you need to pass parameters to your action you will need to provide a POST body to
the http request node.  The documentation for the http request node specifies that this can be done
by setting the `payload` property on the `msg` object that is passed to the node.  To
set the `payload` property we can add some additional code to our function node.
In addition, since we are passing a POST body we need to set the `Content-Type` header to
`application/json`.  If you don't
need to pass any parameters to your action than you can skip this step.

Double click the function node to open the editor.  You can add your parameters simply by
setting `msg.payload`, for example

~~~javascript
msg.headers = {
  "Authorization": "Basic <base 64 encoded credentials>",
  "Content-Type": "application/json"  
};
msg.payload = {
  "twitter":"ryanjbaxter",
  "github":"ryanjbaxter"
};
return msg;
~~~

Connect the function node to the http request node.

![Function and HTTP Request](/wp-content/uploads/2016/03/Screen Shot 2016-03-25 at 11.51.54 AM.png)

We will want to see
the output of our HTTP request, so drag and drop a debug node to your sheet and connect
the http request node to the debug node.

Finally, we need a way of kicking off the flow, so drag and drop an inject node
to the sheet and connect it to the function node.  If you deploy this flow and
click the inject button it should invoke the action in OpenWhisk on Bluemix!  That's
it, four nodes and you are able to invoke OpenWhisk actions from Node-RED.

![Complete Flow](/wp-content/uploads/2016/03/Screen Shot 2016-03-25 at 12.02.02 PM.png)

You can easily start to build upon this flow to take advantage of other features in
Node-RED as well.  For example, you can invoke an action as the result of making
an HTTP request to your own endpoint you setup in Node-RED.  Or you can invoke an action
as the result of an event published to an MQTT topic you are subscribed to.

### Update

After I initially posted this blog post someone on Twitter pointed out to me that there were
actually [nodes for Node-RED](http://flows.nodered.org/node/node-red-node-openwhisk)
for running OpenWhisk actions and tasks!

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr"><a href="https://twitter.com/ryanjbaxter">@ryanjbaxter</a> Nice post. And/or you could use the Openwhisk node directly - <a href="https://t.co/aB8fMSyGbK">https://t.co/aB8fMSyGbK</a> <a href="https://twitter.com/NodeRED">@NodeRED</a></p>&mdash; Dave CJ (@ceejay) <a href="https://twitter.com/ceejay/status/714495301638295552">March 28, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

After a little investigation I found out that these nodes were already available in
the [Node-RED community boilerplate](https://console.ng.bluemix.net/catalog/starters/node-red-starter/)
on Bluemix!  Under the covers I am sure they are probably using the REST APIs I mention
in the post below but they encapsulate everything.

As far as using them, the only tricky part is getting the auth token for the configuration
node.  To do that just run `wsk property get --auth`.  Copy the resulting token into
the OpenWhisk configuration node and you should be all set.

![OpenWhisk Config](/wp-content/uploads/2016/03/Screen Shot 2016-03-28 at 5.04.57 PM.png)
