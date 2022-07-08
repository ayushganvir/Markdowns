## RabbitMQ
RabbitMQ is a message broker: it accepts and forwards messages.

RabbitMQ, and messaging in general, uses some jargon.

1. __Producing__ means nothing more than sending. A program that sends messages is a producer
2. Queue is how the message is send from producer to consumer.A queue is only bound by the host's memory & disk limits, it's essentially a large message buffer.

3. Consuming has a similar meaning to receiving. A consumer is a program that mostly waits to receive messages:

## Sending

```python

# sender.py
 

import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()
channel.queue_declare(queue='hello') # Declaring - 'hello'(queue name)

channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello World!')
print(" [x] Sent 'Hello World!'")
connection.close()

```

broker connected to the localhost(local computer), external computer can also be connected by givving the IP address.

> RabbitMQ a message can never be sent directly to the queue, it always needs to go through an exchange. 

Default exchange identified by an empty string. It allows us to specify exactly to which queue the message should go. The queue name needs to be specified in the routing_key parameter.

Before exiting we make sure that the network buffers were flushed and our message was actually delivered to RabbitMQ. 

## Receiving

```python
## receiver.py


import pika, sys, os

def main():
    connection = pika.BlockingConnection(pika.ConnectionParameters(host='localhost')) # declaring location of the queue
    channel = connection.channel()

    channel.queue_declare(queue='hello') #declaring the queue

    def callback(ch, method, properties, body):
        print(" [x] Received %r" % body)

    channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

    print(' [*] Waiting for messages. To exit press CTRL+C')
    channel.start_consuming()

if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        print('Interrupted')
        try:
            sys.exit(0)
        except SystemExit:
            os._exit(0)
```
 First we need to connect to RabbitMQ server. The code responsible for connecting to Rabbit is the same as previously.

Creating a queue using queue_declare is idempotent ‒ we can run the command as many times as we like, and only one will be created.

> We're not yet sure which program to run first. In such cases it's a good practice to repeat declaring the queue in both programs.

Receiving messages from the queue works by subscribing a callback function to a queue. Whenever we receive a message, this callback function is called by the Pika library. 
