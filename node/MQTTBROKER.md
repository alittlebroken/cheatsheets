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

## Examples

### Express & MQTT

A simple and quick express app showing how it can connect to mqtt, receive messages for one or more topics and publish messages to a topic as well

The examples below assume you are using an existing mqtt broker

```javascript
/** app.js */
import express from 'express';
import mqtt from 'mqtt';
import cors from 'cors';

// Setup the base app
const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// Setup and configure cors
app.use(cors({
  origin: 'http://localhost, https://localhost',
  credentials: true,
}));

// Setup and configure the MQTT client
const mqclient = mqtt.connect('http://localhost:8341');

// Subscribe to the relevant topics
mqclient.on('connect', () => {
  client.subscribe('device_id/led');
  client.subscribe('device_id/temp');
  client.subscribe('device_id/humidity');
});

// Listen for messages
mqclient.on('message', (topic, message) => {
  // Check to ensure the message is handed off to the correct topic handler
  if(topic === 'device_id/led'){
    handleLedTopic(JSON.parse(message.toString()));
  }
});

// Routes
// Route to toggle the led on or off
app.use('/led/:toggle', async (req, res, next) => {
  const toggle = req.params.toggle;

  if(['on', 'off'].includes(toggle)){
    client.publish('device_id/led', toggle);
    return res.status(200).json({ message: `LED set to ${toggle}` });
  } else {
    return res.status(400).json({ message: `LED can't be switched to ${toggle}` });
  }

});

```
