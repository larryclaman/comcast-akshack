# Challenge 5 - Resiliency

[< Previous Challenge](04-k8sdeployment.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Next Challenge>](05b-scaling.md)

## Introduction

Resiliency is the ability to recover quickly from issues.  For Cloud Native applications, we want to rely upon automation when possible.  In this challenge, we will add Readiness and Liveness Probes to our existing Kubernetes deployments.

## Description

Many microservice applications are developed with support for health probes.  By unofficial convention, the uri `/readyz` is used for readiness probes, and `/healthz` is used for liveness probes.  Sadly, our legacy app does not support this convention, so use the `/` for the probes on content-web, and use `/stats` for the probes on content-api.

- Create a Readiness probe for content-web and content-api
- Create a Liveness probe for content-web and content-api
- Force the Readiness Probe to fail for content-api

## Success Criteria

- Demonstrate that your Readiness & Liveness probes are working
- Demonstrate 'breaking' the Readiness probe for content-api updating it to probe an invalid uri

## Optional challenge

- Get all pods back to a ready state
- After getting all pods in the ready state, use `kubectl` to verify all pods are ready

## Hints

1. [Kubernetes probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)