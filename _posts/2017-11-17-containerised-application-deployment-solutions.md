---
layout: post
title: Deployment solutions for a containerised application
date: 2017-11-17
---

I have spent part of last week pulling my hair out over container deployments. If you have read my previous posts, you know by now that I have developed a very simple application and packaged it into a Docker container. I am currently trying to deploy this application and I never thought deployment would be the challenging part. I looked at container deployments offered by cloud providers because I thought it would be the easiest and cheapest solution. It probably is the cheapest but not necessarily the easiest!

In this post I will share my experience with four cloud providers that offer container deployment services. If you are reading this article, bear in mind that I am no expert in cloud deployments and I am looking at a very particular use case: deployment of a Docker container that needs good availability (it is okay if it goes down once in a while) and does not need to be scalable (one instance running is enough).

#### Search criteria

Obviously, the solution needs to cover my use case, i.e. deployment of a web application packaged into a Docker container. On top of that here are my criteria:

* **Price**
<br/>The project I am working on is just a pet project and I won't be making any money with it, so the cheaper, the better.

* **Configuration**
<br/>I am after a solution easy to configure. My intention is not to become a cloud deployment expert and spend ages learning how to use a particular deployment service, so the easier to configure and learn the better.

* **One-click deployment**
<br/>The deployment itself must be easy, ideally one click from a web interface.

* **Programmatic deployment**
<br/>Eventually I want to be able to deploy whenever the code changes, so I need to be able to integrate the solution with my continuous integration pipeline.


#### Microsoft Azure

[Microsoft Azure](https://azure.microsoft.com/) offers very good [accounts](https://azure.microsoft.com/en-gb/free/free-account-faq/) that allow users to try many different services for free.
In terms of container deployments it provides a wide range of solutions. It makes things a bit confusing to be honest:

* [Azure Container Service](https://azure.microsoft.com/en-gb/services/container-service/) (AKS)
<br/>This service seems to be great for complex containerised applications that require load-balancing, easy scaling, availability and container orchestration.
<br/>In my case, while I want my service to be always available, the rest is not going to be a problem and the complexity this service is adding is not worth it. As an example, it uses [Kubernetes](https://kubernetes.io/) as the default orchestrator, do I really need it for my one container application? ^^

* [Container Instance](https://azure.microsoft.com/en-gb/services/container-instances/)
<br/>This service addresses a different type of need to AKS. It offers a sort of computing farm where users can send jobs on-demand and pay for the processing time.
<br/>I guess I could use it but it is definitely not meant for my use case. I want my service up all the time even if no one uses it.

* [Web App for Containers](https://azure.microsoft.com/en-gb/services/app-service/containers/)
<br/>As its name indicates, it is a service dedicated to the deployment of containerised web applications.
<br/>It looks a lot simpler than AKS as it doesn't have, or at least expose, the orchestration layer.
<br/>This solution could be a good candidate for my use case. However, looking at the [price details](https://azure.microsoft.com/en-gb/pricing/details/app-service/), it is not free and requires at least a Basic Service Plan.


#### Google Cloud

My experience with [Google Cloud](https://cloud.google.com) was a lot shorter than anticipated.
<br/>
As a non-business European user, Google Cloud is not available:

> If you are located in the European Union and the sole purpose for which you want to use Google Cloud Platform services has no potential economic benefit you should not use the service. If you have already started using Google Cloud Platform, you should discontinue using the service. See Create, modify, or close your billing account to learn how to disable billing on your projects.
> [Google Cloud](https://cloud.google.com/free/docs/frequently-asked-questions)

Googling the topic a bit, it looks like it has something to do with taxes. Google Cloud needs a VAT number so you must be a business to use Google Cloud Platform.
<br/>

Oh well... Next!


#### Amazon Web Service (AWS)

[AWS](https://aws.amazon.com) offers free accounts like Azure so it is really great for testing. It also provides many deployment solutions for containerised applications, here are the ones I found:

* [EC2 Container Service (ECS)](https://aws.amazon.com/ecs)
<br/>ECS is similar to AKS (I think ECS may have existed before AKS so I should probably say "Microsoft Azure offers a service similar to ECS" :)).
<br/>I actually tested this one and was able to deploy my application to AWS but I didn't really enjoy the experience. I followed this tutorial [here](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted.html) and it took me quite some time to create all the things that were required: cluster, service, task, IAM, etc... Having spent so much time looking at deployment solutions I realise now that I shouln't even have considered this service. It is for applications way more complex than mine!

* [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk)
<br/>I somehow missed this solution, one of my friends mentioned it to me.
<br/>Elastic Beanstalk is a simple service to deploy and scale applications. I haven't looked into it yet but it looks like it supports Docker and it could a good candidate for my use case. However, I must admit I am not a big fan of Amazon documentation.


#### IBM Bluemix

As I was searching for new alternatives to deploy my application I came across this great [video](https://www.youtube.com/watch?v=TfCj2qOXb1g) by David Barnes, IBM technology evangelist. It offered a very simple solution to deploy containerised applications, I was very excited about it. However, I quickly realised that the service was being deprecated [:'(](https://console.bluemix.net/docs/containers/cs_classic.html).
<br/>
IBM's new alternative is a service similar to Microsoft AKS and Amazon ECS called [IBM Cloud Container Service](https://www.ibm.com/cloud/container-service). As I said before, this type of service is great but add too much complexity for my simple use case.


<br/>

In conclusion, I spent a lot of time googling, testing, having fun, not having fun, pulling my hair out and not progressing a lot in term of deployment but learning a lot about container deployment to the cloud.
<br/>
To be honest at this stage, I still feel overwhelmed by the amount of solutions available. Also, it looks like my search led me to more complex solutions than I need. I don't need a cluster of machines with a container orchestrator, in its simplest form I need a server with Docker installed and a way to reach this server.
<br/>
I am going to pause deployment for a few days, I could use a break! Then, I will look at single container deployment services such as [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk) and [Azure Web App for Containers](https://azure.microsoft.com/en-gb/services/app-service/containers/). I will also look at server provisioning providers, it may well be the best solution in my case!