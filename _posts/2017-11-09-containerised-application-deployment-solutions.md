---
layout: post
title: Deployment solutions of a containerised application
date: 2017-11-07
---

I have spent part of last week losing hairs over container deployments. If you have read my previous posts, you know by now that I have developed a very simple application and packaged into a Docker container. I am currently trying to deploy this application and I never thought deployment would be the challenging part. I looked at container deployments offered by cloud providers because I thought it would be the easiest and cheapest solutions. It probably is the cheapest but not necessarily the easiest!

In this post I am sharing my experience with four cloud providers that offer container deployment service. If you are reading this article, bear in mind that I am no expert in cloud deployments and I am looking at a very particular use case: deployment of a Docker container that needs good availability (it is okay if it goes down once in a while) and does not need to be scalable (one instance running is enough).


#### Microsoft Azure

[Microsoft Azure](https://azure.microsoft.com/) offers very good [accounts](https://azure.microsoft.com/en-gb/free/free-account-faq/) that allow users to try many different services for free.
In terms of container deployments it provides a wide range of solutions. It makes things a bit confusing to be honest:

* [Azure Container Service](https://azure.microsoft.com/en-gb/services/container-service/) (AKS)
<br/>This service seems to be great for complex containerised applications that require load-balancing, easy scaling, availability and container orchestration.
<br/>In my case, while I want my service to be always available, the rest is not going to be a problem and the complexity this service seems to be adding is not worth it.

* [Container Instance](https://azure.microsoft.com/en-gb/services/container-instances/)
<br/>This service addresses a different type of needs to AKS. It offers like a computing farm where users can send jobs on-demand and paid for the processing time.

* [Web App for Containers](https://azure.microsoft.com/en-gb/services/app-service/containers/)
<br/>As its name indicates, it is a service dedicated to the deployment of containerised web applications.
<br/>It looks a lot simpler than AKS as it doesn't have, or at least expose, the orchestration layer AKS offers. I am not sure how it works behind the scene because it still needs to do some kind of orchestration I would have thought :/


#### Google Cloud

My experience with [Google Cloud](https://cloud.google.com) was a lot shorter than anticipated.
<br/>
As a non-business European user, Google Cloud is not available:

> If you are located in the European Union and the sole purpose for which you want to use Google Cloud Platform services has no potential economic benefit you should not use the service. If you have already started using Google Cloud Platform, you should discontinue using the service. See Create, modify, or close your billing account to learn how to disable billing on your projects.
> [Google Cloud](https://cloud.google.com/free/docs/frequently-asked-questions)

Googling the topic a bit it looks like it has something to do with taxes. Google Cloud needs a VAT number so you must be a business to use Google Cloud Platform.
<br/>

Oh well... Next!


#### Amazon Web Service (AWS)

AWS offers a service similar to Microsoft Azure AKS service called EC2 Container Service (ECS) (I think ECS may have been there before AKS so I shall probably say "Microsoft Azure offers a service similar to ECS" :)). It also offers free accounts so it is great for testing.
<br/>
I was able to deploy my application to AWS (kind of, I stopped before being completely finished) but I didn't really enjoy the experience. I followed this tutorial [here](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted.html) and it took me quite some time to create all the things that were required: cluster, service, task, IAM, etc... It felt really overhelming to do and learn about so many components for deploying something as simple as my application. I feel like I barely scratched the surface but it was already too much. So I decided to give up and move on.


#### IBM Bluemix

As I was searching for new alternatives to deploy my application I came across this great [video](https://www.youtube.com/watch?v=TfCj2qOXb1g) by David Barnes, IBM technology evangelist. It offered a very simple solution to deploy containerised applications, I was very excited about it. However, I quickly realised that the service was being deprecated [:'(](https://console.bluemix.net/docs/containers/cs_classic.html).
<br/>
IBM new alternative is a service similar to Microsoft AKS and Amazon ECS called [IBM Cloud Container Service](https://www.ibm.com/cloud/container-service). As I said before, this type of services are great but add too much complexity to my simple use case.


#### A few notes

If you go into your own cloud adventure, bear in mind that although many providers offer free accounts, you still need to give them your credit card number. The official reason being make sure you are not a robot.


<br/>

TODO: reformulate


In conclusion, I spent a lot of time googling, trying, having fun, not having fun, losing my hairs and not progressing a lot in term of deployment but learning a lot about container deployment to the cloud.
<br/>
I thought deploying my application via a cloud provider was the cheapest and easiest solution but I realise now that it isn't and it makes sense.
<br/>First, it isn't easy because the deployment use case that cloud providers are targeting is the deployment of large and complex applications that need high availability and easy scalability. For instance, a website with high volume traffic at peak hours. This is a complex but very common problem so from a business perspective it makes sense for cloud providers to tailor to that use case (rather than mine where clearly they won't make much money, haha).
<br/>
Second, using a cloud solution is not easy. It is complex and there is a lot to learn and to know. Even if cloud providers provide an abstraction layer, it is important to understand what happens behind the scene. And in my opinion, this is a full-time job. So for companies with complex applications it is likely cheaper to use a cloud provider instead of having a full IT department for machine provisioning, maintenance, support, etc... and they can get away with a couple of people, however for individuals like me who wants something up and running quickly I feel like it's not!

This being said, I will still take a look at Microsoft Azure Web App for Container deployment solutions because it is free but if it doesn't work I will explore getting a virtual machine and handling the rest myself.