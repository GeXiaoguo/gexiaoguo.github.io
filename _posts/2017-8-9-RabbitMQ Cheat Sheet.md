---
layout: post
title: RabbitMQ Cheat Sheet
---

# Terminologies
- Queue: what you receive messages from  
- Exchange: what you send messages to  
- Binding: binds queues to exchanges  
- Routing Key:  
      - binding routing key: routing key is associated with a Binding    
      - message routing key: routing key is specified when publishing a message  
       
# Exchange Types  
- direct: sends messages to all bindings that the binding routing key matches the message routing key exactly  
- fanout: sends messages to all bindings it knows about regardless of the routing key  
- topic: is a more complex direct exchange where  
binding routing key can contain   
    - \*(star) which matches exactly one word
    - \#(hash) which matchs zero or more word
    - example binding key: *.orange.*, lazy.#
message routing key: doted words for example lazy.orange.dog
message are routed from exchange to bindings that matches the routing key according to the * and # rule

## The default exchange:   
An direct exchange with an empty string name which binds to all queues and a binding routing key set to the name of the queue.

#Message patterns:  

## Producer Consumer:  
- message is only delivered to one consumer
- producer publish to a direct exchange(can also be the default exchange)
- consumer attached its queue to the exchange that producer is publishing
- can also use topic exchange
## PubSub:  
- message delivered to multiple subscribers  
- publisher publishes message to a fanout exchange  
- subscriber creates a queue and bind it to the exchange   
- subscriber listens to the queue  


## Topic:   
In case of fanout exchange. the name of the exchange is the topic. Any listener interested in the topic connects its own queue to the exchange.

## RPC:  
can be done with any exchange as long as the message can arrive at the server's listening queue
client sends a request with a replyQueue and a correlationId  
problems:  
  
- PRC hides remote call cost. solution is to make it clear what call is RPC and what call is Local  
- how should client react if client is not running      
- request time out    
- error feed back    
- how should client react to a response with a not recognized correlationId( should be ignored because of duplicated response from server is a valid case)  

