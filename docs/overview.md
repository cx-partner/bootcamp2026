# 2026 WxCC Bootcamp

## Overview

Welcome to the 2026 Webex Contact Center (WxCC) Partner Bootcamp — an immersive, hands-on experience designed to equip partners with the skills needed to design, build, and demonstrate next-generation customer journeys powered by AI. Over the course of two days, you will implement a complete, real-world use case for Webex Bank, a fictitious retail banking organization:

<p align="center"><strong>Proactive Debt Collection and Contention Resolution</strong></p>

This use case showcases how organizations can:

- Proactively engage customers
- Automate routine interactions using AI
- Seamlessly orchestrate human intervention when needed
- Deliver secure, compliant, and frictionless experiences

By the end of the bootcamp, you will have built a fully functional, end-to-end use case in your own Gold Tenant environment.

---
## Bootcamp Description

By leveraging cutting-edge functionality, you will learn to deliver fully connected customer journeys for **Webex Bank**, a fictitious retail banking entity. Our goal is to demonstrate how to boost debt recovery while significantly cutting agent workload through state-of-the-art AI automation.

The bootcamp is structured as a progressive, hands-on implementation, where each lab builds on the previous one to create a complete customer journey.

You will work within your own WxCC tenant (Gold Tenant or NFR demo/lab) to deploy a production-like scenario that combines outbound engagement, AI automation, and human-assisted resolution.

**Day 1 – Proactive Automation & AI Foundations**

- Lab 1 - *Proactive Outbound Reach*
    Configure the Native Campaign Manager to trigger voice calls based on debt maturity dates found in your Airtable database.

- Lab 2 - *Automated Customer Engagement* 
    Build an autonomous AI Agent capable of authenticating customers, retrieving debt balances, offering payment options and generating and delivering payment links via NovaPay integration.

**Day 2: Advanced Orchestration & Human-in-the-Loop AI**

- Lab 3 - *Value of the Human Touch* 
    Implement intelligent escalation to a Human Fraud Specialist, including full context transfer via VA Summary, Real-time agent guidance with RT Assist and automated wrap-up and backend actions.

- Lab 4 - *Cross-Skill and Multi-Agent Orchestration* 
    Desing a multi-agent architecture where a Debt Collection AI Agent hands off to an Investment Advisor AI Agent, using Agent transfer capabilities, and automate scheduled appointments with a human advisor.
 
---
## Learning Objectives

By participating in this bootcamp, you will::

- [x] Understand Proactive Engagement and learn how to design outbound, event-driven customer journeys using Webex Contact Center
- [x] Master AI Capabilities in Webex Contact Center and gain hands-on experience with autonomous AI Agents, AI Assistant and RT Assist, Multi-agent orchestration and Campaign Manager
- [x] Build and implement a complete, business-relevant use case within your own Tenant
- [x] Drive Measurable Business Outcomes by increasing operational efficiency through automation, reducing agent workload and after-call work and improving customer experience with seamless interactions.
---
## Lab Infrastructure & Pre-requisites

To ensure a smooth experience during the bootcamp, the following must be pre-configured in your Gold Tenant before starting Lab 1:

1. Digital Channel for Outbound Notifications
    A digital channel must be enabled to allow the AI Agent to send payment links. Recommended: Email channel via Webex Connect

2. Airtable Base (Customer Data Repository)
    You must configure an Airtable Base that will act as the Customer Debt Database, including Customers table, Transactions table and Investments table. 

    This database will be used by AI Agents for customer authentication, debt retrieval, transaction validation and investment advisory scenarios

Additionally, you will leverage the **NovaPay mock-payment service** built for the purpose of this Bootcamp. The NovaPay service provides a mockpayment frontend web site that mimics a typical payment user interface and a web service that exposes an API to create payment sessions, accept payments and check the payment status. Should you want to replicate the Novapay services for your demos, the [Extra Lab](lab_novapay.md) in this Bootcamp provides you a **step-by-step guide** to do so.

Detailed setup instructions are provided in the [Pre-requisites](pre_req_wxcc.md) section of the lab guides.

---
## Disclaimers

- Limited AI Scope: AI Agents in this bootcamp are intentionally restricted to low-risk, reversible, and controlled actions.

- Human Oversight: All high-sensitivity and regulatory processes (e.g., fraud confirmation, financial liability decisions) are handled by human agents.

- Fictitious Data: *Webex Bank* and *NovaPay* are simulated entities and all data and scenarios related to *Webex Bank* and *NovaPay* is for training purposes only.

- Non-Production Setup: This environment is designed for demo and learning purposes, not for production deployment.


---
## Useful Links & Tools
During the bootcamp, you will actively use the following platforms and tools:

- Cisco Platforms: 
    - Control Hub 
    - Webex Connect portal
    - AI Agent Studio
    - Campaign Manager portal

- External tools: 
    - Airtable: Your primary data repository for customer and financial records.
    - NovaPay Mock Service: for payment sessions and confirmation service.
---
## Proctors and Support
Our proctors are available throughout the bootcamp to support you during the labs. Please reach out at any time if you need assistance.



