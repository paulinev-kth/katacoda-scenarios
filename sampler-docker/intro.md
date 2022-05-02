
# Introduction
## Motivation
A container is a piece of software that gathers all the code and the dependencies to be able to run an application on any environment. A container contains code, runtime, system tools, system libraries and settings, all packed in a so called container image. The application is isolated from its environment preventing any conflict and allowing a limit on resources to consume to be set.
Contrary to virtual machines, containers can share commune layers and use fewer resources as operating above the operating system.

Docker containers are the most popular ones.

Containers are dynamic, and monitoring containers is important to :
- catch problems early
- optimize resource allocation

## What is sampler ?
[Sampler](https://sampler.dev/) is a monitoring tool to visualize, alert and execute shell commands. It is configured with a simple yaml file, is serverless and easy to set up, contrary to more advanced systems like Prometheus with Grafana. Plus, configuration yaml files can easily be reused between projects and templates might be created.
It is a tool adapted for the development phase.

<br>
<img src="assets/demo.jpg" width="80%">

## What you will learn
- Monitoring Docker containers
- Configuring a basic dashboard with sampler

## Prerequisites
- Vim
- Basic knowledge of docker
