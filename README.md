# 🗄️ Natural Language SQL Agent with n8n  

## 📌 Overview  
This project enables you to query your PostgreSQL database using **plain English**.  
Example:  
> *“Show me sales from Mumbai last month”*  

The system:  
- Converts natural language into SQL queries  
- Executes queries on Postgres  
- Returns results in chat format  
- Handles ambiguity and missing data gracefully  

---

## 🧠 How It Works  
1. **Chat Trigger** – User enters a natural language query via the chat interface.  
2. **AI Agent (Powered by Gemini)** – This is the core intelligence. It uses the **Google Gemini Chat Model** for reasoning and **Simple Memory** for context.  
3. **Schema & Query Tools** – The agent accesses your database using two tools:  
    * **Postgres Tool:** Used to retrieve the database schema (table/column names) for planning the SQL query.  
    * **SQL Query Executor:** A dedicated sub-workflow (see diagram below) used to safely run the final, generated SQL query.  
4. **Response** – The AI Agent processes the query results and formats the final answer, returning it back to the user via chat.  

### Workflow Diagrams  

**Main AI Agent Workflow**
![Main AI Agent Workflow](/screenshots/sql-agent.PNG)
*This shows the primary flow, connecting the chat trigger, the AI Agent, the Gemini LLM (the brain), memory, and the two required database tools.*

**SQL Query Executor Sub-Workflow**
![SQL Query Executor](/screenshots/sql-query-executor.PNG)

*This sub-workflow is called by the AI Agent to execute the generated SQL query against the Postgres database.*

---

## 🚀 Setup Guide  

### Step 1: Install & Configure n8n  
- Sign up for [n8n.cloud](https://app.n8n.cloud/register)  
- Or set up locally using n8n’s hosting docs  

### Step 2: Import Workflows  
- Import both workflow templates into n8n:  
  - `SQL Agent.json` (main workflow)  
  - `SQL Query Executor.json` (sub-workflow)  

### Step 3: Connect Database  
- Configure Postgres credentials in n8n  
- The agent uses this query via the Postgres Tool to read schema information:  
  ```sql
  SELECT table_schema, table_name, column_name, data_type
  FROM information_schema.columns
  WHERE table_schema = 'public'
  ORDER BY table_name, ordinal_position;
  ```

---

## 🧪 Test Query  

**Query:**  
> *“Give me top 3 products by revenue in USA”*  

**Response:**  
The top 3 products by revenue in the USA are:

- **AQ BZ Compact:** 27,448,586.42
- **AQ Smash 1:** 27,232,651.06
- **AQ Gamer 2:** 26,079,308.53

---

## ✅ Final Outcome

- Query databases in plain English
- AI agent generates and executes SQL automatically
- Results are returned instantly, without needing SQL expertise