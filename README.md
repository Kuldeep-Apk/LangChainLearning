# LangChainLearning
AI-Powered SQL Agent using LangChain
An autonomous, AI-powered SQL Agent that translates natural language questions into precise SQL queries, executes them against a structured database, and synthesizes the results into clear, human-readable answers.

This project bypasses the traditional requirement of manual SQL expertise, allowing users to interact with databases conversationally (e.g., asking "How many tracks are in the database?" or "List customers from India").

🚀 Key Features
Autonomous Schema Inspection: The agent dynamically lists available tables and inspects column structures without hardcoded prompts.

Modern Tool-Calling Architecture: Built using LangChain's latest create_sql_agent workflow with tool-calling capabilities (agent_type="openai-tools" style).

Deterministic Querying: Uses internal verification steps (sql_db_query_checker) to validate SQL syntax before running it, minimizing hallucinations and invalid executions.

Production-Ready & Stable: Moving away from older, error-prone ReAct step-by-step reasoning prompts in favor of strict JSON schema tool execution.

Observability Integrated: Configured with LangSmith for deep tracing, debugging, and monitoring of the LLM chain reasoning steps.

🛠️ Architecture & Workflow
The agent acts as an independent data analyst by orchestrating four primary internal tools:

sql_db_list_tables: Discovers available tables in the database.

sql_db_schema: Inspects columns, types, and sample rows for relevant tables.

sql_db_query_checker: Double-checks the generated SQL query for syntactic correctness.

sql_db_query: Safely executes the final SQL statement and pulls the raw data payload.

The Agentic Loop: > User Question ➔ List Tables ➔ Inspect Schema ➔ Generate SQL ➔ Validate Query ➔ Execute ➔ Formulate Natural Language Answer.

💻 Tech Stack
Framework: LangChain, LangChain-Community, LangChain-Groq (or LangChain-OpenAI)

LLM Engine: Models supporting native tool-calling (e.g., llama-3.3-70b-versatile, GPT-4)

Database Wrapper: SQLAlchemy / LangChain SQLDatabase utility

Target Database: SQLite (Sample Chinook Media Store Database)

Monitoring: LangSmith

📋 Quick Start & Usage
1. Initialize the Database Connection
Python
from langchain_community.utilities import SQLDatabase

db = SQLDatabase.from_uri("sqlite:///Chinook.db")
2. Create and Invoke the Agent
Python
from langchain_community.agent_toolkits import create_sql_agent

# Initialize your tool-calling LLM (e.g., ChatGroq or ChatOpenAI)
agent = create_sql_agent(
    llm=model,
    db=db,
    verbose=True
)

# Ask questions directly
response = agent.invoke({"input": "List 5 customers from India."})
print(response["output"])
🔮 Future Enhancements
Connect to production-grade remote instances (MySQL, PostgreSQL).

Build an interactive, conversational user interface using Streamlit.

Implement structured Multi-Agent state management using LangGraph for handling complex joint analytics.
