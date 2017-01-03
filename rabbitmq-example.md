# RabbitMQ example in Node.js

## Preparing environment

Install & run RabbitMQ.

File `package.json`:
```json
{
  "name": "rabbitexample",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "producer": "node --harmony producer",
    "consumer": "node --harmony worker"
  },
  "keywords": [],
  "author": "Umidjons <almatov.us@gmail.com>",
  "license": "ISC",
  "dependencies": {
    "amqplib": "^0.5.1"
  }
}
```

Install dependencies:
```sh
npm install
```

## Common module to connect to the RabbitMQ

File `queue.js`:
```js
const amqp = require('amqplib');
const URL = 'amqp://guest:guest@localhost:5672';

module.exports = class Queue {
    /**
     * Creates RabbitMQ channel.
     * @param {string} queue name of the queue.
     * @returns {Promise.<*>} {conn, channel} object on success, error object on failure.
     */
    static async create(queue) {
        try {
            let conn = await amqp.connect(URL);
            let channel = await conn.createChannel();
            await channel.assertQueue(queue, {durable: true});
            return {conn, channel};
        } catch (err) {
            return err;
        }
    }

    /**
     * Encode given data.
     * @param {*} data given data
     * @returns {Buffer} JSON formatted buffer object.
     */
    static encode(data) {
        return new Buffer(JSON.stringify(data));
    }

    /**
     * Decode JSON encoded buffer object.
     * @param {*} data data to decode.
     */
    static decode(data) {
        return JSON.parse(new Buffer(data).toString());
    }
};
```

## Producer part

File `producer.js`:
```js
const Queue = require('./queue');
const queue = 'queue';

(async() => {
    try {
        let MQ = await Queue.create(queue);
        console.log('Channel and queue created.');
        let work = {test: 'make me a sandwich'};
        await MQ.channel.sendToQueue(queue, Queue.encode(work), {persistent: true});
    } catch (err) {
        console.error(err.stack);
    }
})();
```

## Consumer part

File `worker.js`:
```js
const Queue = require('./queue');
const queue = 'queue';

(async() => {
    try {
        let MQ = await Queue.create(queue);

        await consume();

        async function consume() {
            let msg = await MQ.channel.get(queue, {});
            if (msg) {
                console.log('Consuming %j', Queue.decode(msg.content));
                setTimeout(async() => {
                    await MQ.channel.ack(msg);
                    await consume();
                }, 1000);
            } else {
                console.log('No message, waiting...');
                setTimeout(consume, 1000);
            }
        }
    } catch (err) {
        console.error(err.stack);
    }
})();
```

## Running producer & consumer

Run producer:
```
npm run producer

> rabbitexample@1.0.0 producer F:\nodeprojects\rabbitexample
> node --harmony producer

Channel and queue created.
```

Run consumer:
```
npm run consumer

> rabbitexample@1.0.0 consumer F:\nodeprojects\rabbitexample
> node --harmony worker

No message, waiting...
Consuming {"test":"make me a sandwich"}
No message, waiting...
No message, waiting...
```
