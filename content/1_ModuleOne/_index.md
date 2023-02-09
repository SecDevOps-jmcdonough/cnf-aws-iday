---
title: "Introduction"
chapter: true
weight: 1
---

# Introduction

## Learning Objectives

At the end of this workshop, you will complete the following objectives:
  
  * Understand AWS Networking Concepts *(10 minutes)*
  * Understand AWS Common Architecture Patterns *(10 minutes)*
  * Understand FortiGate CNF terminology *(5 minutes)*
  * Subscribe to FortiGate CNF *(5 minutes)*
  * Onboard an AWS account *(5 minutes)*
  * Deploy a FortiGate CNF Instance & GWLBe endpoints *(20 minutes)*
  * Create a policy set and apply it to a FortiGate CNF Instance *(10 minutes)*
  * Test traffic flows (distributed in + egress and centralized e-w + egress) *(20 minutes)*

## Workshop Components

These are the AWS and Fortinet components that will be used during this workshop:

  * AWS Marketplace
  * FortiCloud 
  * FortiGate CNF Portal
  * AWS CloudFormation Templates (Infrastructure as Code, IaC)
  * AWS SDN (AWS intrinsic router and route tables in a VPC)
  * AWS Gateway Load Balancer (GWLB)
  * AWS Transit Gateway (TGW)
  * AWS EC2 Instances (Amazon Linux OS)

## AWS Reference Architecture Diagram

This is the architecture and environment that will be used in the workshop.

  * With AWS networking there are several different ways to organize your AWS architecture to take advantage of FortiGate CNF traffic inspection. The important point to know is that as long as the traffic flow has a symmetrical routing path (for forward and reverse flows), the architecture will work.

  * This diagram will highlight two main designs that are common architecture patterns for securing traffic flows.
  * **Distributed Ingress + Egress**
  * **Centralized Egress + East-West**

![](images/image-ref-diag1.png)
