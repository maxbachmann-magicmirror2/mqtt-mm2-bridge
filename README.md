# Snips-MM2-Gateway
This is an extension for the [MagicMirror²](https://github.com/MichMich/MagicMirror). It gives an easy option to connect a MQTT broker and the MM2 internal notification system. This way you can send MQTT messages to any module and let modules easily talk to other smart home components that support MQTT (MQTT is a quite popular protocol). 
Other than the other MQTT modules that already exist it can:

-   publish and subscribe with an easy way to connect it with any module
-   forward the messages to the other modules, so they can react to the messages
-   subscribe to as many topics as you want
-   send messages to any topic from any module without extra config
-   use username and password for the MQTT broker

This way not every module that wants to use MQTT has to implement a MQTT client

## Supported Modules
1.  [MMM-SnipsHideShow](https://github.com/maxbachmann-magicmirror2/MMM-SnipsHideShow)

Since the module just acts as a gateway for mqtt messages the internal notifications of MM2 it´s really easy to add the module to control any MM2 module with MQTT messages even usernameand password for the mqtt broker.

Any incoming messages on topics that got subscribed (it can subscribe as many topics as you want at once) will get forwarded using `sendNotification(notification, payload)` with the mqtt topic as notification and the message in the payload.
To send messages just use `sendNotification("MQTT_SEND", payload)`, with `payload = {topic: <topicname>, message: <mqtt_message>}`.

## Installation
1.  Navigate into your MagicMirror's `modules` folder and execute `git clone https://github.com/maxbachmann-magicmirror2/mqtt-mm2-gateway.git`. A new folder will appear, likely called `mqtt-mm2-gateway`.  Navigate into it.
2.  Execute `npm install` to install the node dependencies.

## Using the module
To use this module, add this to the modules array in the `config/config.js` file:
````javascript
modules: [
	{
		module: 'mqtt-mm2-gateway',
		config: {
			// See 'Configuration options' for more information.
		}
	}
]
````

## Configuration options
The following options can be configured:

### Required
| Option               | Description                                                                                             |
|----------------------|---------------------------------------------------------------------------------------------------------|
| `host`               | IP Address or Hostname of the mqtt broker. The default value is `'localhost'`                           |
| `port`               | Port of the mqtt broker. The default value is `1883`                                                    |
| `topics`             | Topics the Bridge should subscribe to. The default value is a empty array `[]`                          |

### username and password
By default username and password is deactivated

| Option               | Description                                                                                             |
|----------------------|---------------------------------------------------------------------------------------------------------|
| `username`           | Username of the mqtt broker when using username + password.                                             |
| `password`           | Password of the mqtt broker when using username + password.                                             |

## Testing whether the gateway works is quite easy
1.  set the config parameters as described in [Using the module](#using-the-module)
2.  when running MagicMirror2 there will be a Notification Alert when the mqtt server can´t be reached or the authentification fails
3.  The module prints log messages into the console aswell, so you can check these for example when running MM2 with npm start dev
4.  to check wether it correctly receives the messages on the selected topics it is for example possible to manually test using mosquitto_pub (`mosquitto_pub -h <host> -p <port> -u <username> -P <password> -t <topic> -m <message>`). The message will be logged by the module aswell
5.  the same way it´s possible to test sending mm2 notifications as mqtt messages. Just subscribe the topic using (`mosquitto_sub -h <host> -p <port> -u <username> -P <password> -t <topic>`) then send a message by sending a mm2 notification as described in [Supported Moduled](#supported-modules)

## Dependencies
-  [mqtt](https://www.npmjs.com/package/mqtt) (installed via `npm install`)

## Contributing Guidelines
Contributions of all kinds are welcome, not only in the form of code but also with regards bug reports and documentation.

Please keep the following in mind:

## Roadmap
-   [ ] tls support for mqtt
