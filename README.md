# shimla-push-notifications-ios
### Integration:
Add `GoogleService-Info.plist` to the root of your project. Make sure the bundle identifier matches your projects.

### Simple Usage: 
```
var client = ShimlaClient(form: yourPrivisioningCredentials)
client.connect()
client.subscribe(topic: "demo", withHandler: topicHandler)
client.publish(topic: "demo", message: "Hello, World!")
```

`topicHander` in the above is an object that implements the TopicHandler protocol
Specifically, the `onMessageRecieved(topic: String, message: String)` method.

Note: **All handlers will be run on a DispatchQueue**, so UI manipulation in any delegate method need to done via `DispatchQueue.main.async {...}` or similar.

Note: **`.connect()` will block the caller until completion**

### For Push Notifications:

```
client.enablePushNotifications(app: UIApplication)
client.setPushNotificationHandler(handler: pushNotificationHandler)
client.pushNotificationSubscribe(topic: "demo")
```

Anything published to the topic "demo" will be recieved by the client via APNS, this may happen alongside the standard subscribe method.

Be sure to set the PushNotificationHandler before subscribing if you wish to add more behavior. There is only one instance for all topics, instead of per topic.

### Unsubscribing

```
client.unsubscribe(topic: "demo")
client.pushNotificationUnsubscribe(topic: "demo")
```

When called, the client will no longer recieve messages for the provided topic and will remove references to any handlers for the topic.

### Publishing

```
client.publish(topic: "demo", message: "Hello Demo")
```

Client will queue a message to be sent to the broker for distribution.

```
client.publish(topic: "demo", message: "Hello Demo, but with a callback/delegate/handler", delegate: PublishDelegate)
```

`delegate` argument for the above call is an object that conforms to the PublishDelegate protocol.

PublishDelegates are called once a message is sent to the broker, it is message specific.
