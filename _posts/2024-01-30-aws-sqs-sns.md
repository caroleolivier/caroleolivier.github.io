---
layout: post
title: AWS SQS vs SNS
date: 2024-01-30
---

A little refresher on [AWS SQS](https://aws.amazon.com/sqs/) vs [AWS SNS](https://aws.amazon.com/sns/) and my current mental model on it.

## AWS SQS

![AWS SQS mental model]({{ "/assets/blog/aws_sqs.png" | absolute_url }}){:height="30%" width="30%"}

With SQS, there's 1 queue, so consumers "competes" or shares the load from the queue.  
Whenever a consumer pulls a message from the queue and successfully acknowledges it, then it's not visible to any other consumers.

So the idea is that there's 1 bag of messages **shared** by consumers.  
An example of using SQS would be: a system that process smart meter readings and receive most of them at the same time.  
To the processing up, you might want to spin off a few AWS lambda behind the AWS SQS queue to process these meter readings in parallel.  
This way you **spread the load** and the meter readings are processed faster.

### Note on message replay

SQS queues have visibility time out configured.  
It means that if a consumer takes longer than the configured time out to process a message, then the message becomes visible again and is visible to all consumers again.
That's one of the case where a message could be replayed.

Another case of message replay is down to AWS internal as they only guarantee "at least once" delivery.

## AWS SNS

![AWS SNS mental model]({{ "/assets/blog/aws_sns.png" | absolute_url }}){:height="30%" width="30%"}

AWS SNS is used when you want to deliver the same message to **multiple subscribers**.

An example could be if you want to save the meter readings you processed earlier to different storage. You could have a subscription for storing to S3 and another for storing to PostgreSQL.  
Or maybe a better example: one subscription for storage and another one for metrics.

AWS SNS duplicates message on each subscription.
