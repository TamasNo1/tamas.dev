---
layout: post
title:  "Another week, another weekend project"
date:   2020-01-01 08:00:00 +0100
categories: dev javascript react
---

TODO:
- write about the healthcheck issue and how I was unable to debug it.
- wait, was the sniffing related to the healthcheck???
Since we were running out of ideas, we had to find a way to somehow reproduce what's happening between Cloudfront and the load balancer. We tried to enable logging for the Cloudfront distribution, but that gave us very little information. We had to dig deeper, all the way to the TCP layer. Having our backend service running on EC2 based ECS cluster, the plan was to listen to some network packets and try to fiugre out if there are packages coming at all.
- enable cloudfront logging, replicate cloudfront behaviour with curl, sniff network packages on the server
