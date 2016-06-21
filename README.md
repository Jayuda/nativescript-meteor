# Nativescript-Meteor
Plugin to connect via DDP Meteor with NativeScript

### How to Install On Nativescript Project
```js
npm install nativescript-meteor --save
```


### How to use this package
```js
import DDPClient = require("nativescript-meteor");

var ddpclient = new DDPClient({
    host: "localhost",
    port: 3000,
    ssl: false,
    autoReconnect: true,
    autoReconnectTimer: 15000,
    maintainCollections: true,
    ddpVersion: '1',
    useSockJs: true
});

ddpclient.connect(function (error, wasReconnect) {
    if (error) {
        console.log('DDP connection error!');
        return;
    }

    if (wasReconnect) {
        console.log('Reestablishment of a connection.');
    }

    console.log('connected!');

    // CALL METHOD METEOR
    ddpclient.call(
                'deleteSinglePost',             // name of Meteor Method being called
                ['foo', 'bar'],            // parameters to send to Meteor Method
                function (err, result) {   // callback which returns the method call results
                    console.log('deleteSinglePost result: ' + result);
                },
                function () {              // callback which fires when server has finished
                    console.log('deleteSinglePost finished');  // sending any updated documents as a result of
                });

    // SUBSCRIBE COLLECTIONS
    ddpclient.subscribe(
        'todos',                  // name of Meteor Publish function to subscribe to
        [],                       // any parameters used by the Publish function
        function () {             // callback when the subscription is complete
            console.log('posts complete:');
            console.log(ddpclient.collections.todos);
            for (var _id in ddpclient.collections.todos) {
                var todos = ddpclient.collections.todos[_id];
                console.log("Adding available todos " + _id + " name: " + todos.judul);
            }
        });
});

```