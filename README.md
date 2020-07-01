# [Create-Manage-Cloud-Resources](https://google.qwiklabs.com/focuses/10258?parent=catalog)

## Overview
You must complete a series of tasks within the allocated time period. Instead of following step-by-step instructions, you'll be given a scenario and a set of tasks - you figure out how to complete it on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

To score 100% you must complete all tasks within the time period!

When you take a Challenge Lab, you will not be taught GCP concepts. To build the solution to the challenge presented, use skills learned from the labs in the quest this challenge lab is part of. You will be expected to extend your learned skills; you will be expected to change default values, but new concepts will not be introduced.

This lab is only recommended for students who have completed the labs in the Getting Started: Create and Manage Cloud Resources Quest. Are you up for the challenge?

>> Please make sure you review the labs in the [Getting Started: Create and Manage Cloud Resources](https://google.qwiklabs.com/quests/120) quest before starting this lab!

Topics tested:

- Create an instance.
- Create a 3 node Kubernetes cluster and run a simple service.
- Create an HTTP(s) Load Balancer in front of two web servers.

## Challenge scenario
You have started a new role as a Junior Cloud Engineer for Jooli Inc. You are expected to help manage the infrastructure at Jooli. Common tasks include provisioning resources for projects.

You are expected to have the skills and knowledge for these tasks, so don't expect step-by-step guides to be provided.

Some Jooli Inc. standards you should follow:

- Create all resources in the default region or zone, unless otherwise directed.
- Naming is normally ___team-resource___, e.g. an instance could be named __nucleus-webserver1__
- Allocate cost effective resource sizes. Projects are monitored and excessive resource use will result in the containing project's termination (and possibly yours), so beware. This is the guidance the monitoring team is willing to share; unless directed use __f1-micro__ for small Linux VMs and __n1-standard-1__ for Windows or other applications such as Kubernetes nodes.

## Your challenge
As soon as you sit down at your desk and open your new laptop you receive several requests from the Nucleus team. Read through each description, then create the resources.

#### Task 1: Create a project jumphost instance
We will use this instance to perform maintenance for the project.

Make sure you:

- name the instance `nucleus-jumphost`
- use the machine type of f1-micro
- use the default image type (Debian Linux)

#### Task 2: Create a Kubernetes service cluster

> You have a limit to the resources you are allowed to create in your project, if you don't get the result you expected please delete the cluster before you create another cluster or the lab might exit and you might get banned.

The team is building an application that will use a service. This service will run on Kubernetes. You need to:

- Create a cluster (in the `us-east1` region) to host the service
- Use the Docker container hello-app (`gcr.io/google-samples/hello-app:2.0`) as a place holder, the team will replace the container with their own work later
- Expose the app on port `8080`

#### Task 3: Setup an HTTP load balancer
We will serve the site via nginx web servers, but we want to ensure we have a fault tolerant environment, so please create an HTTP load balancer with a managed instance group of __two nginx web servers__. Use the following to configure the web servers, the team will replace this with their own configuration later.

> You have a limit to the resources you are allowed to create in your project, so do not create more than two instances in your managed instance group or the lab might exit and you might get banned. 

``` cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF 
```

You need to:

- Create an instance template
- Create a target pool
- Create a managed instance group
- Create a firewall rule to allow traffic (80/tcp)
- Create a health check
- Create a backend service and attach the manged instance group
- Create a URL map and target HTTP proxy to route requests to your URL map
- Create a forwarding rule
