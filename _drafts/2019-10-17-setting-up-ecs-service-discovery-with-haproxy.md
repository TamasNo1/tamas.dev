---
layout: post
title:  "Setting up ECS service discovery with HAProxy"
date:   2019-10-17 9:30:00 +0100
categories: aws ecs haproxy certbot microservices containers route53 srv nginx
---

## Keeping things simple
When working on side projects I always try to keep things simple and focus on the important things at the beggining as delivering values is key. I don't want to deal with clusters, HA, pipelines etc. because it's more than alright to deploy to production from your laptop in the early days. In this particular example, the infrastructure is basically an RDS instance (no multi AZ at this point) and an EC2 box with Ubuntu and docker installed on it manually. The deploy process is a simple shell script on my laptop that builds the container, uploads it to the registry, dials in to the remote box via SSH and does a docker pull. Super simple. However as time passes and the project gets tracktion, it's getting more and more pressing to come up with at least a semi-scalable solution which is what this post is about.

I recently got to a point where I wanted to introduce multiple containers (Django web server, celery worker, celery beat). Sure, I could just add another build and pull command to my existing script, but I figured maybe there's a simple way to do this in a more proper fashion without blowing the budget. ECS seemed like a good choice (EC2 based on Fargate seems to be more expensive at this small scale) because it deals with deployment, container scheduling, logging, monitoring, etc. My only worry was that putting a load balancer in front of the services would increase the monthly budget by 50% (again, we're talking very small scale), therefore I started looking for alternate solutions. And that's when I found a solution provided by ECS itself.

## Service discovery à la ECS
As it turns out, ECS supports service discovery out of the box, which is great as it solves half of my problem. If service discovery is enabled, a private Route53 zone is created (and managed by the service discovery service) and whenever a new container is scheduled, a new SRV and A record will be created automagically. I didn't know this before, but using an SRV DNS record, you can define not just the target location, but also the target port. This solution works beautifully with dynamic port allocation which allows running multiple instances of the same service on a single physical box. The other huge benefit of this implementation is that there's no Elastic Load Balancer involed, it all relies on DNS records and them being up to date. Think of it as a client side load balancing. And this is where I need a solution for the other half of the roblem. I will need a reverse proxy in front of the services in order to resolve the DNS entries and to provide a single port exposed to the outside world. And this is where HAProxy comes to play.

I'll be honest here, I don't have any prior experience with HAProxy so please don't take any of the remaining post as a guide or as a best practice. The goal here is to explore possibilities that can save you a couple of bucks (literally a couple) at the early phase of your project. My first choice was Nginx as I have some experience with it. Unfortunately it does not support this use case only in their Nginx Plus offering, therefore HAProxy it is.

As you'll see later it's fairly straight forward to set it up and configure, also it's still just a proxy server at the end of the day with load balancing, so if you're familiar with the concept, there's nothing new here really.

One more important thing is that if you get to a point where you need a more reliable solution, it's very easy to just reconfigure the ECS service to use and actial load balancer.

## Scope of this post
In this post I'd like to walk you through a simple concept, which I'm sure is not perfect, so please do let me know if I missed something!

The goal will be to:
- Create an EC2 spot instance based ECS cluster
- Create a simple http hello-world ECS service with service discovery enabled
- Create an EC2 box that'll host HAProxy
- Configure HAProxy and set up SSL termination using Let's Encrypt's certbot

## Setting up ECS

**1.,** Without going into too much details on how to use ECS, let's create a new ECS cluster, select Linux + Networking (todo: check) and use the following configuration:
- Provisioning model: Spot
- Allocation strategy: Lowest price
- EC2 instance types: t2.micro, t2.small, t3.micro, t3.small
- Number of instances should just be 1 at this point
- Create a new VPC
- Either select an existing or create new container instance and spot fleet IAM roles
- No need to enable Container Insights 

**2.,** Once the cluster is ready, create a task definition for our hello-world service. Again, without going into too much details, these are the key configuration points:
- Name: hello-world
- Compatibilities / Requires compatibilities: EC2
- Container definitions:
    - image: strm/helloworld-http
    - port mappings:
        - containerPort: 80
        - hostPort: 0 <= This will specify dynamic port allocation on the host

**3.,** With a cluster and a task definition we can now create the service:
- Launch type: EC2
- Task definition / revision: hello-world (latest)
- Cluster: select your cluster created in step 1
- Service name: hello-world-service
- Service type: Replica
- Number of tasks: 3 <= In order to test load balancing
- Deployment type: Rolling update
- Load balancer type: None <= Whole point of this post
- Enable service discovery integration: checked
- Namespace: create new private namespace
- Namespace name: internal
- Cluster VPC: select the VPC that belongs to the cluster created in step 1
- Configure service discovery service: Create new service discovery service
- Service discovery name: _hello-rowld-service **<= Very important: HAProxy will only resolve the SRV DNS correctly if it starts with an undescore**
- Enable ECS task health propagation: checked
- Service Autio Scaling: Don not adjust the service's desired count

[![SRV and A records created by service discovery service][dns_records]][dns_records]{:target="_blank"}


## Setting up HAProxy

Create box
- Add security group
- Add IAM role
Test DNS records
Install everything
Run certbot
Configure HAProxy
restart HJAProxy
profit




[dns_records]: https://tamas.dev/static/2019-10-17/service_discovery_records.png


[cloudfront_502_image]: https://tamas.dev/static/cloudfront_502s.png
[cloudfront_502_link]: https://aws.amazon.com/premiumsupport/knowledge-center/resolve-cloudfront-connection-error/
[shakespeare_image]: https://external-asdasdpreview.redd.it/E896abucqJeMw3XvoqpZ4RMHbKBjRoqqDlpFwqECmtg.jpg?width=960&crop=smart&auto=webp&s=52d8c00bf39e5759cec1be9938d515f2d81f4b63
[shakespeare_link]: https://www.reddit.com/r/me_irl/comments/86tb5k/meirl/