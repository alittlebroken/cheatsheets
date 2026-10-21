# MOSCA Broker

MQTT is a protocol used in IoT ( Internet of Things ) to allow devices to communicate easily in lower power mode, which is ideal for devices such as Arduino.

The Mosca broker (Server) is a package that allows you to utilise nodeJs as a self hosted Broker.

It is a good idea to secure the mqtt broker with usernames and passwords and any API endpoints with tokens and keys

## Installation

```bash
mkdir <project_folder>
cd <project_folder>
npm init -y
npm i mosca
```

NOTE: if node needs to act as a client which publishes or subscribes to topics then the following packages are needed as well

```bash
npm i mqtt
```

## Setup

### Broker

This is the service that listens for new messages from publishers and sends them to the applicable subscribers

- Create a file in the project root called broker.js and add the following code
  
```javascript
import mosca from 'mosca';

const settings = {
  port: 1883
};

const server = new mosca.server(settings);

server.on('ready', () => {
  console.log('MQTT Broker ready');
});
```

### Publisher

The publisher is a device which sends a message to a topic

```javascript
/** publisher.js */
import mqtt from 'mqtt';

// Connect to the broker
const client = mqtt.connect(<broker_address>);

// Listen for client events
client.on('connect', () => {

  /** Send a message every x number of seconds as a test.
   *  On a real system the client.publish could be used
   *  to send data back to a device to switch and led on
   *  for example */
   setInterval(() => {
    client.publish('device/led', Math.random() * 1000);
    console.log('Message sent to topic: device/led');
   }, 5000); 

});

```

### Subscribers

The subscriber, subscribes to a topic to receive all of its messages. On some devices it could be a publisher as well

```javascript
/** subscriber.js */
import mqtt from 'mqtt';

// Connect to a broker
const client = mqtt.connect(<broker_address>);

// Subscribe to applicable topics on conenction
client.on('connect', () => {
  client.subscribe('device/led');
});

// Listen for any message from the topics
client.on('message', (topic, message) => {
  // The message comes in as a buffer so we should convert to a string
  const msg = message.toString();
  console.log(msg);
};

```
