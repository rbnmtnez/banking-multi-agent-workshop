# Multi Agent Workshop

Welcome to our multi-agent sample and workshop for a retail banking scenario. Implemented in both C# using Semantic Kernel Agents and Python using LangGraph.

## Build a Multi-Agent AI application using Semantic Kernel Agents or LangGraph

This sample application and workshop shows how to build a multi-tenant, multi-agent, banking application with containerized applications built using two multi-agent frameworks

- Semantic Kernel Agents in C#
- LangGraph in Python

Both are hosted on Azure Container Apps, with Azure Cosmos DB for NoSQL as the transactional database and vector store with Azure OpenAI Service for embeddings and completions. This complete sample and workshop provides practical guidance on many concepts you will need to design and build these types of applications.

## Architecture Diagram

Here’s the deployment architecture and components of the workshop!

<img src="media/Multi-agent.png" alt="Multi-Agent Image">

## User Experience

https://github.com/user-attachments/assets/0e943130-13c5-4bb5-a40b-51b6c85dd58c

## Complete the Workshop Exercises

There are two completely separate implementations for this sample multi-agent application with different instructions on how to deploy and configure for use.

The workshop for this sample is on the [HOL branch](https://github.com/AzureCosmosDB/banking-multi-agent-workshop/blob/hol) in this repository. To navigate and complete this workshop select one of the following:

- Navigate to the [LangGraph Python Workshop](https://github.com/AzureCosmosDB/banking-multi-agent-workshop/blob/hol/python/workshop/Module-0.md)
- Navigate to the [Semantic Kernel Csharp Workshop](https://github.com/AzureCosmosDB/banking-multi-agent-workshop/blob/hol/csharp/workshop/Module-0.md)
## Important Security Notice

This template, the application code and configuration it contains, has been built to showcase Microsoft Azure specific services and tools. We strongly advise our customers not to make this code part of their production environments without implementing or enabling additional security features.

## Guidance

### Region Availability

This template uses gpt-4o and text-embedding-3-large models which may not be available in all Azure regions. Check for [up-to-date region availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) and select a region during deployment accordingly.

### Costs

You can estimate the cost of this project's architecture with [Azure's pricing calculator](https://azure.microsoft.com/pricing/calculator/)

As an example in US dollars, here's how the sample is currently built:

Average Monthly Cost:

- Azure Cosmos DB Serverless ($0.25 USD per 1M RU/s): $0.25
- Azure Container Apps (1 CPU, 2 Gi memory): $8.00
- Azure Container Registry(Standard): $5:50
- Azure App Service (B3 Plan): $1.20
- Azure OpenAI (GPT-4o 1M input/output tokens): $20 (Sample uses 10K tokens)
- Azure OpenAI (text-3-embedding-large): < $0.01 (Sample uses 5K tokens)
- Log Analytics (Pay as you go): < $0.12

## Resources


## Application Flow: Frontend, Backend, and Agents

1. **Frontend Request**: The user interacts with the web app and sends a chat message. The frontend makes an HTTP request to the backend, targeting endpoints like:
	- `POST /tenant/{tenantId}/user/{userId}/sessions/{sessionId}/completion` (send a chat message)
	- `GET /tenant/{tenantId}/user/{userId}/sessions/{sessionId}/messages` (get chat history)
	These endpoints are defined in `ChatEndpoints.cs`.

2. **Backend Routing**: The backend receives the request and routes it to the appropriate method in `ChatService` via the mapped endpoint in `ChatEndpoints.cs`. For chat completion, it calls a method that interacts with the multi-agent system.

3. **Agent Workflow**: The backend creates an AgentGroupChat using the AgentFactory. Each agent (Sales, Transactions, etc.) is initialized with its own plugin and prompt. The message history is loaded so agents have context. The user’s new message is added to the chat. The system enters a loop, invoking agents asynchronously:
	- Each agent processes the message, possibly calling banking APIs, searching for offers, or handling transactions.
	- Agents generate responses, which are collected as Message objects.
	- Debug logs are captured for traceability.
	The loop continues until the conversation is marked complete by the agents.

4. **Backend Response**: The backend returns the list of agent-generated messages and debug logs as JSON to the frontend.

5. **Frontend Display**: The frontend receives the response and updates the chat UI with the new messages.

**Summary:**
The frontend sends user input to the backend. The backend routes the request, loads the session, and invokes a group of specialized agents. Each agent processes the message according to its domain (sales, transactions, support), and their responses are returned to the frontend for display. This enables rich, multi-agent conversational AI for banking tasks.

To learn more about the services and features demonstrated in this sample, see the following:

- [Azure Cosmos DB for NoSQL Vector Search announcement](https://aka.ms/CosmosDBDiskANNBlog/)
- [Azure OpenAI Service documentation](https://learn.microsoft.com/azure/cognitive-services/openai/)
- [Semantic Kernel](https://learn.microsoft.com/semantic-kernel/overview)
- [Azure App Service documentation](https://learn.microsoft.com/azure/app-service/)
- [ASP.NET Core Blazor documentation](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
