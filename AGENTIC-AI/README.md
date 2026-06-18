## AI Agent
AI agents is a programs that takes input, think and acts to complete a task in autonomous way using tools, memory and knowledge.
LLM + Tools (APIs) + Knowlegde (Database Access) + Memory (Past interactions)

## Building Effectice Agent
    - (Source: https://www.anthropic.com/engineering/building-effective-agents)
    - The document source categorize these AI applications into two categories
        - Workflow: The above RAG based chatbot is actually a workflow. It is not an agentic.
        - Agents: LLM + Tools (APIs) + Knowlegde (Database Access) + Memory (Past interactions)

## RAG Retrival Augmented Chatbot.
    - Retrieval augmented chatboard which can simply pull the information from the PDF and answer the questions.


### Tool Augmented Chatbot 
        - Lets Advance the above RAG based chatbot a bit more amd lets add a support where you can ask question like below
    "How many leaves do it have left". Also add further capability to take action by given a command to apply for a leave. This is possible since the LLM integrated with HR management system through their APIs where you can't only pull the leaves for a given employee you can also apply for a leave.

### Agentic AI
    - Lets advance tool augmented chatbot further by give it a goal for example. prepare sara maternity leave. Now this require multistep reasoning and multitrun planning.
        ex. Onboard a new intern joining next monday
            - Schedule welcome meeting (Email) LLM
            - Create intern profile in HR Management System (Files) LLM
            - IT helpdesk (WiFi credentials, email, slack access) integration of multiple tool like slack, outlook (LLM)
            - Order laptop, ID card.
    - It requires multistep reasoning and you have to do multistep planning and in each of these steps LLMs will be involved along with APIs, tools, knowledge and memory.

## Characterstic of Agentic AI:
        - Goal oriented planning.
        - Multistep reasoning.
        - Autonomous decision making.
            - It isn't just giving a simple task to agent like how many leaves are left instead it should be a complex task which requires reasoning as well as multistep planning.

## Agentic AI In-General
    - Agentic AI is an AI system that can make decisions and take actions on its own to achieve a goal without being told exactly what to do at everystep.

## Application of Agentic AI
    - AI coding
        It will first identify application features and perform multistep planning and execute writing code.
        - Lovable
        - Replet