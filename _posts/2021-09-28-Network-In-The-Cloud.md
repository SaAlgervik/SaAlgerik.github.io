---
layout: post
title: Network In The Cloud
subtitle: Make it Private
cover-img: https://images.unsplash.com/photo-1618767451283-c9705dff0874?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1171&q=80 
thumbnail-img: https://s7280.pcdn.co/wp-content/uploads/2020/06/virtual-network.jpg
share-img: /assets/img/path.jpg
tags: [Azure Service Bus , VPN]
---

#### Let me change you minde...
So i wanted to fill you in on the benefits from moving all of our services in to the cloud. I know that you have the security in the back of your mind, as of course all of us have. But i been doing some research and Azure have someting called [Azure Private Link (APL)](https://docs.microsoft.com/en-us/azure/private-link/private-link-overvieprivat) where we can have use an [Azure Service Bus (ASB)](https://www.serverless360.com/azure-service-bus). Whit this features our application can be run on a [Virtual Private Cloud (VPC)](https://awsfeed.com/whats-new/certification/azure-virtual-private-cloud-guide). With this in mind we can both have the security we need but still have the good stuff from having it in the cloud. 

***Let me explain!***

If we would use the APL we won't have to use the standard way of creating a public IP and our traffic will never travel through the internet. Instead we will have Private Endpoint with a Private IP adress within a [VNet](https://www.bmc.com/blogs/virtual-network/) that makes sure our traffic stays within our VNet. If we do this we will now be granted access to a specific PaaS resource within our VNet. Meaning, we can control the egress to the PaaS resource. We can also make sure that the traffic from us **always** stays within our VNet and have it secured.

***Lets get a ride on this bus!***
The good thing with this is that you can decuple you application and have things working smoother for the user. When you send a message to the application you don't have to think abaout the cost of it to scale up. The bus will put it in a queue for when it's enough power to process it and then send a message back to the user when it's done. And if we have a bug or if something gets lost on the way the bus will put it in something called the Dead Lettering. The message will stay there until we handel it, either by looikng in to the problem and fix it, or put it back in the queue to bo processed again. The good thing about this is even if something gets lost on the way, we wont loose it for good and be able to make a decision about what to do with it. 
We can also schedule when we want things to happen for exampel if we want something to start working or get sent at 2am, we wont have to have someone go up and do that in the middle of the night. An outher really good thing is that if something gets duplicated in the request the systmen will notis and just discard one of the duplicates for us.

Whit this we can have Virtual Privat Cloud (VPC) and don't have to think about any downtime on the application and be able to grow when we need to, and have shared VPC for the entire organization. This means teams isolated within projects can maintain a shared private IP space to access commonly used services. And all this whitout scratching your head thinking about security, with a VPN this i all taken care of. A virtual private network (VPN) uses encryption to create a private network over the top of a public network. VPN traffic passes through publicly shared Internet infrastructure – routers, switches, etc. – but the traffic is scrambled and not visible to anyone.

Best Regards Team bus drivers!
_______________________


<https://scomandothergeekystuff.com/2020/06/22/azure-service-endpoints-versus-azure-private-links/>    
<https://medium.com/awesome-azure/azure-difference-between-azure-private-links-and-azure-service-endpoints-private-links-vs-service-endpoints-8fb0f80ca196>  
<https://www.cloudflare.com/learning/cloud/what-is-a-virtual-private-cloud/>