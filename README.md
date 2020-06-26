# azure-service-bus-uptime-test

I wrote this code to do some testing against Azure Service Bus, which has an uptime SLA of 99.9%.  A downtime of 0.01% works out to 43 minutes every month.  

## Scenario

My real-world scenario is that I have an API that accepts a high volume of requests that must be processed asynchronously.  Because my processing is done async, I want to simply write the inbound request to a queue and return 200.  The message can then be picked up and processed asynchronously.  The risk in this scenario is that my API is unable to write to the queue to persist the data.  As a result, it must return a failure response to the caller, which is undesireable.

## Test Details

To test out the availability of Azure Service Bus queues, I want to simulate a high volume of incoming messages, which I can do by using an Azure Function that runs every few seconds and writes messages to a SB queue.  If the Service Bus uptime is truly 99.9%, I expect that occasionally the queue will be unavailable to my function.  

## Goals

In a high-volume service that's using SB queues to store incoming asynchronous requests, I'm hoping to figure out a few things:

1. At high volume, will I see failures writing messages to a Service Bus Queue.
2. When failures occur, will retry logic be able mitigate the failures?
3. Or, for failures, is it possible to write to a second instance of Azure Service Bus as a backup?  

> for goal #3 I'm really curious if my two Service Bus instances fail simulaneously or at different times.