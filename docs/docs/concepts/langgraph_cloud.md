# Cloud SaaS

!!! info "Prerequisites" 
    - [LangGraph Platform](./langgraph_platform.md) 
    - [LangGraph Server](./langgraph_server.md)

## Overview

LangGraph's Cloud SaaS is a managed service for deploying LangGraph APIs, regardless of its definition or dependencies. The service offers managed implementations of checkpointers and stores, allowing you to focus on building the right cognitive architecture for your use case. By handling scalable & secure infrastructure, LangGraph Cloud offers the fastest path to getting your LangGraph API deployed to production.

## Deployment

A **deployment** is an instance of a LangGraph API. A single deployment can have many [revisions](#revision). When a deployment is created, all the necessary infrastructure (e.g. database, containers, secrets store) are automatically provisioned. See the [architecture diagram](#architecture) below for more details.

See the [how-to guide](../cloud/deployment/cloud.md#create-new-deployment) for creating a new deployment.

## Resource Allocation

| **Deployment Type** | **CPU** | **Memory** | **Scaling**         |
|---------------------|---------|------------|---------------------|
| Development         | 1 CPU   | 1 GB       | Up to 1 container   |
| Production          | 1 CPU   | 2 GB       | Up to 10 containers |

## Revision

A revision is an iteration of a [deployment](#deployment). When a new deployment is created, an initial revision is automatically created. To deploy new code changes or update environment variable configurations for a deployment, a new revision must be created. When a revision is created, a new container image is built automatically.

See the [how-to guide](../cloud/deployment/cloud.md#create-new-revision) for creating a new revision.

## Asynchronous Deployment

Infrastructure for [deployments](#deployment) and [revisions](#revision) are provisioned and deployed asynchronously. They are not deployed immediately after submission. Currently, deployment can take up to several minutes.

## Architecture

!!! warning "Subject to Change"
The Cloud SaaS deployment architecture may change in the future.

A high-level diagram of a Cloud SaaS deployment.

![diagram](img/langgraph_cloud_architecture.png)


## Related

- [Deployment Options](./deployment_options.md)
