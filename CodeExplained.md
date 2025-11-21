# AI Agents Code Explained Simply - For Beginners

## Before You Start - What Is Code?

Code is like writing **instructions** for a computer, similar to a recipe:
- A recipe says: "Mix flour, add eggs, bake at 350°F"
- Code says: "Take input, process it, return output"

---

# DAY 1: BUILDING YOUR FIRST AGENT

## What Is An Agent?

Think of an agent like a **helpful robot assistant**:
- You give it a task: "Help me find a customer"
- It thinks about what to do
- It performs the task
- It gives you an answer

The robot uses an AI model (like Gemini) as its "brain" to think.

---

## Code Example 1: Creating Your First Agent

### The Code:
```python
from google.adk.agents import Agent

agent = Agent(
    model='gemini-2.5-flash',
    name='support_agent',
    description='Customer support',
    instruction='Help customers effectively'
)
```

### Breaking It Down Line by Line:

**Line 1:** `from google.adk.agents import Agent`
- This says: "Get the `Agent` tool from Google's library"
- Think of it like: "Get a hammer from the toolbox"
- You can't build a house without tools, so you get them first

**Line 3:** `agent = Agent(`
- This says: "Create a new agent and give it the name `agent`"
- Think of it like: "Hire a new employee for your company"
- The variable `agent` is the name you'll use to talk to this agent later

**Line 4:** `model='gemini-2.5-flash',`
- This says: "Use Gemini 2.5 Flash as the AI brain"
- Think of it like: "Hire someone smart" 
- Gemini is Google's AI model - it's the thinking part
- 'gemini-2.5-flash' is a specific version that's fast and good

**Line 5:** `name='support_agent',`
- This says: "Give this agent the ID name 'support_agent'"
- Think of it like: "Name your employee John"
- This is just a label so you remember what this agent is for

**Line 6:** `description='Customer support',`
- This says: "This agent's job is customer support"
- Think of it like: "John's job is to help customers"
- This helps you remember what the agent does

**Line 7:** `instruction='Help customers effectively'`
- This says: "Tell the agent HOW to do its job"
- Think of it like: "John, your instructions are: be polite, solve problems quickly"
- This is like the agent's rulebook or guidelines

### What Happens After?

After this code runs, you have a working agent! But it's not active yet. Think of it like:
- A car is built but not running
- An employee is hired but hasn't started work yet

---

## Code Example 2: Multi-Agent System (Teams)

### The Code:
```python
from google.adk.agents import SequentialAgent

system = SequentialAgent(
    name='support_system',
    agents=[greeting_agent, support_agent, escalation_agent]
)
```

### What Does This Do?

Imagine you call a customer service center:
1. **Greeting Agent** answers: "Hello, how can I help?"
2. **Support Agent** solves your problem
3. **Escalation Agent** handles complicated issues

These three agents work in **sequence** (one after another), like an assembly line:

```
User Input
    ↓
[Greeting Agent] - Says hello and understands the problem
    ↓
[Support Agent] - Tries to solve it
    ↓
[Escalation Agent] - Takes over if problem is complex
    ↓
Final Answer to User
```

### Breaking It Down:

**Line 1:** `from google.adk.agents import SequentialAgent`
- Get the tool for creating multi-agent systems that work in order

**Line 3:** `name='support_system',`
- Name your team of agents

**Line 4:** `agents=[greeting_agent, support_agent, escalation_agent]`
- List the agents that will work together
- The square brackets `[ ]` mean "this is a list"
- They work in the order listed: greeting first, then support, then escalation

---

## Code Example 3: Agent With Google Search

### The Code:
```python
agent = Agent(
    model='gemini-2.5-flash',
    name='research_agent',
    instruction='Use Google Search to find info',
    tools=['google_search']
)
```

### What Does This Do?

Imagine you ask a librarian: "What's the weather in New York?"

**Without tools:**
- Librarian thinks: "I read books until 2020, so I don't know today's weather"
- Librarian can't help

**With tools:**
- Librarian picks up a phone and calls someone
- Gets current information
- Tells you the answer

In this code:
- `tools=['google_search']` gives the agent a "phone"
- The agent can now search the internet
- The agent can give you up-to-date information

### Breaking It Down:

**Line 6:** `tools=['google_search']`
- This says: "Give this agent access to Google Search"
- `tools` = the agent's abilities/powers
- `['google_search']` = the list of tools available
- Now the agent can search the web!

---

# DAY 2: GIVING AGENTS TOOLS (SUPERPOWERS)

## What Are Tools?

Tools are like **superpowers for agents**. Without tools, agents are limited. With tools, they're powerful.

### Example: Customer Support Agent

**Without tools:**
```
User: "What's my account status?"
Agent: "I don't know, I can't check your account"
```

**With tools:**
```
User: "What's my account status?"
Agent: Uses `get_account_status` tool
Agent: "Your account is active with $500 balance"
```

---

## Code Example 1: Creating a Custom Tool

### The Code:
```python
from google.adk.agents import FunctionTool

def get_weather(location: str) -> dict:
    """Get weather for a location"""
    return {"temp": 22, "condition": "sunny"}

tool = FunctionTool.create(get_weather)

agent = Agent(
    model='gemini-2.5-flash',
    name='weather_agent',
    tools=[tool]
)
```

### What Is This?

You're creating a **custom tool** that the agent can use. Think of it like:
- You create a "phone number" the agent can call
- When the agent calls that number, it gets weather information

### Breaking It Down:

**Line 1:** `from google.adk.agents import FunctionTool`
- Get the tool for turning functions into agent tools

**Line 3-5:** 
```python
def get_weather(location: str) -> dict:
    """Get weather for a location"""
    return {"temp": 22, "condition": "sunny"}
```

This is a **function** - think of it as a mini-program:

- `def` = "Define a new function"
- `get_weather` = the name of the function
- `(location: str)` = this function needs one input: the location (as text)
- `-> dict` = this function will output a dictionary (data structure)
- `"""..."""` = description of what it does
- `return {...}` = what the function outputs

**Simple Example:**
```
Input: "New York"
Function runs
Output: {"temp": 22, "condition": "sunny"}
```

**Line 7:** `tool = FunctionTool.create(get_weather)`
- This says: "Turn the `get_weather` function into a tool"
- Think of it like: "Convert your knowledge into a tool the agent can use"

**Line 9-13:**
```python
agent = Agent(
    model='gemini-2.5-flash',
    name='weather_agent',
    tools=[tool]
)
```
- Create an agent with this tool
- Now the agent CAN use the weather tool

---

## Code Example 2: Multiple Tools

### The Code:
```python
def get_customer(customer_id: str) -> dict:
    return {"name": "John", "status": "active"}

def create_ticket(customer_id: str, issue: str) -> dict:
    return {"ticket_id": "TKT001", "status": "created"}

tools = [
    FunctionTool.create(get_customer),
    FunctionTool.create(create_ticket)
]

agent = Agent(
    model='gemini-2.5-flash',
    name='support_agent',
    tools=tools
)
```

### What Is This?

You're giving the agent **two tools**. Imagine a handyman:
- Tool 1: Hammer (for nails)
- Tool 2: Saw (for cutting)

Here:
- Tool 1: `get_customer` - fetch customer info
- Tool 2: `create_ticket` - create a support ticket

### Breaking It Down:

**First function:**
```python
def get_customer(customer_id: str) -> dict:
    return {"name": "John", "status": "active"}
```
- Input: `customer_id` (like "CUST123")
- Output: Dictionary with name and status
- When called: Returns customer information

**Second function:**
```python
def create_ticket(customer_id: str, issue: str) -> dict:
    return {"ticket_id": "TKT001", "status": "created"}
```
- Input: `customer_id` and `issue` (the problem)
- Output: Dictionary with ticket info
- When called: Creates a support ticket

**The Tools List:**
```python
tools = [
    FunctionTool.create(get_customer),
    FunctionTool.create(create_ticket)
]
```
- Square brackets `[ ]` = this is a list
- Contains two tools
- The agent can now choose which tool to use

**Usage Example:**
```
User: "I need help with my account"
Agent thinks: "I should use get_customer first to get their info"
Agent calls: get_customer("CUST123")
Agent receives: {"name": "John", "status": "active"}
Agent thinks: "Now I should create_ticket to help them"
Agent calls: create_ticket("CUST123", "Account issue")
Agent receives: {"ticket_id": "TKT001", "status": "created"}
Agent responds: "Created ticket TKT001 for you John"
```

---

# DAY 3: MEMORY (AGENTS THAT REMEMBER)

## What Is Memory?

Imagine two conversations:

**Without Memory:**
```
Conversation 1:
You: "My name is John"
Agent: "Hi John!"

Conversation 2 (next day):
You: "Hello again!"
Agent: "Hi! Who are you?"
(Agent forgot you!)
```

**With Memory:**
```
Conversation 1:
You: "My name is John"
Agent: "Hi John! I'll remember you"

Conversation 2 (next day):
You: "Hello again!"
Agent: "Hi John! Good to see you again!"
(Agent remembered!)
```

---

## Code Example 1: Session (Short-Term Memory)

### The Code:
```python
class SessionManager:
    def __init__(self):
        self.sessions = {}
    
    def create_session(self, user_id: str) -> str:
        session_id = f"session_{user_id}_{time.time()}"
        self.sessions[session_id] = {
            "messages": [],
            "context": {}
        }
        return session_id
    
    def add_message(self, session_id: str, role: str, content: str):
        self.sessions[session_id]["messages"].append({
            "role": role,
            "content": content
        })
    
    def get_history(self, session_id: str) -> list:
        return self.sessions[session_id]["messages"]
```

### What Does This Do?

This creates a **conversation history tracker**. Think of it like:
- A notebook where you write down what the customer said
- You can look back at previous messages
- All within the same conversation (session)

### Breaking It Down:

**Line 1:** `class SessionManager:`
- `class` = create a template for a new type of object
- Think of it like: "Define what a SessionManager does"

**Line 2:** `def __init__(self):`
- This is the "setup" code
- `__init__` = when you create a SessionManager, run this
- Think of it like: "When you turn on the coffee machine, brew coffee"

**Line 3:** `self.sessions = {}`
- Create empty storage for sessions
- `{}` = empty dictionary/storage
- Think of it like: "Open an empty filing cabinet"

**Lines 5-12: Create a Session**
```python
def create_session(self, user_id: str) -> str:
    session_id = f"session_{user_id}_{time.time()}"
    self.sessions[session_id] = {
        "messages": [],
        "context": {}
    }
    return session_id
```

What happens:
1. Create a unique session ID (like a file folder name)
2. Create an empty storage: `"messages": []` and `"context": {}`
3. Return the session ID so you can use it later

**Example:**
```
Input: user_id = "john123"
Creates: session_id = "session_john123_1234567890"
Returns: "session_john123_1234567890"
Stored: {"messages": [], "context": {}}
```

**Lines 14-18: Add a Message**
```python
def add_message(self, session_id: str, role: str, content: str):
    self.sessions[session_id]["messages"].append({
        "role": role,
        "content": content
    })
```

What happens:
1. Get the session by its ID
2. Add a new message to the messages list
3. Include who said it (`role`: user or agent) and what they said (`content`)

**Example:**
```
Input: 
  - session_id: "session_john123_1234567890"
  - role: "user"
  - content: "What's my account balance?"

Result:
messages = [
    {"role": "user", "content": "What's my account balance?"}
]
```

**Lines 20-21: Get History**
```python
def get_history(self, session_id: str) -> list:
    return self.sessions[session_id]["messages"]
```

What happens:
- Retrieve all messages from a session
- Return the whole conversation history

**Example Usage:**
```python
# Create a session
session_id = manager.create_session("john123")

# Add messages
manager.add_message(session_id, "user", "Hello!")
manager.add_message(session_id, "agent", "Hi John!")
manager.add_message(session_id, "user", "What's my balance?")

# Get history
history = manager.get_history(session_id)
# Returns: [
#   {"role": "user", "content": "Hello!"},
#   {"role": "agent", "content": "Hi John!"},
#   {"role": "user", "content": "What's my balance?"}
# ]
```

---

## Code Example 2: Long-Term Memory

### The Code:
```python
class MemoryStore:
    def __init__(self):
        self.profiles = {}
        self.preferences = {}
    
    def store_profile(self, user_id: str, data: dict):
        self.profiles[user_id] = data
    
    def get_profile(self, user_id: str) -> dict:
        return self.profiles.get(user_id, {})
    
    def save_preference(self, user_id: str, key: str, value):
        if user_id not in self.preferences:
            self.preferences[user_id] = {}
        self.preferences[user_id][key] = value
```

### What Does This Do?

This stores **permanent information about users**. Think of it like:
- Customer database
- Customer preferences file
- Information that persists across conversations

### Breaking It Down:

**Lines 1-4: Setup**
```python
class MemoryStore:
    def __init__(self):
        self.profiles = {}
        self.preferences = {}
```
- `self.profiles` = storage for user profiles (like a customer database)
- `self.preferences` = storage for user preferences

**Lines 6-7: Store Profile**
```python
def store_profile(self, user_id: str, data: dict):
    self.profiles[user_id] = data
```
- Input: user_id (like "john123") and data (like {"name": "John", "email": "john@example.com"})
- Action: Save this data under that user's ID

**Example:**
```python
memory = MemoryStore()
memory.store_profile("john123", {"name": "John", "email": "john@example.com"})

# Result: profiles = {"john123": {"name": "John", "email": "john@example.com"}}
```

**Lines 9-10: Get Profile**
```python
def get_profile(self, user_id: str) -> dict:
    return self.profiles.get(user_id, {})
```
- Input: user_id
- Output: The user's data, or empty dictionary if not found
- `.get(user_id, {})` = "Get this user's data, or return {} if not found"

**Example:**
```python
data = memory.get_profile("john123")
# Returns: {"name": "John", "email": "john@example.com"}

data = memory.get_profile("unknown_user")
# Returns: {} (empty, because user not found)
```

**Lines 12-15: Save Preference**
```python
def save_preference(self, user_id: str, key: str, value):
    if user_id not in self.preferences:
        self.preferences[user_id] = {}
    self.preferences[user_id][key] = value
```

What happens:
1. Check if user exists in preferences
2. If not, create empty storage for them
3. Save the preference

**Example:**
```python
memory.save_preference("john123", "theme", "dark")
# First time: preferences = {"john123": {"theme": "dark"}}

memory.save_preference("john123", "language", "english")
# Second time: preferences = {"john123": {"theme": "dark", "language": "english"}}
```

---

# DAY 4: LOGGING & OBSERVABILITY (WATCHING AGENTS)

## What Is Logging?

Logging means **writing down everything the agent does**. Think of it like:
- A flight recorder in a plane (writes everything that happens)
- A diary (you write down daily events)
- Security cameras (record what happens)

When something goes wrong, you can look at the logs to see what happened.

---

## Code Example: Basic Logging

### The Code:
```python
import logging
import json
from datetime import datetime

logger = logging.getLogger("agent")
logger.setLevel(logging.DEBUG)

def log_tool_call(tool_name: str, params: dict, result: any):
    log_entry = {
        "timestamp": datetime.now().isoformat(),
        "event": "tool_call",
        "tool": tool_name,
        "params": params,
        "result": result
    }
    logger.info(json.dumps(log_entry))

# Usage
log_tool_call("get_customer", {"id": "123"}, {"name": "John"})
```

### What Does This Do?

This logs every time the agent uses a tool. It records:
- When it happened (timestamp)
- What tool was used
- What input was given
- What output was received

### Breaking It Down:

**Line 1-2:**
```python
import logging
import json
from datetime import datetime
```
- `import` = load tools from Python
- `logging` = tool for recording events
- `json` = tool for organizing data
- `datetime` = tool for timestamps

**Lines 5-6:**
```python
logger = logging.getLogger("agent")
logger.setLevel(logging.DEBUG)
```
- Create a logger for recording
- `logger` = your event recorder
- `.setLevel(logging.DEBUG)` = "Record everything, including detailed info"

**Lines 8-16:**
```python
def log_tool_call(tool_name: str, params: dict, result: any):
    log_entry = {
        "timestamp": datetime.now().isoformat(),
        "event": "tool_call",
        "tool": tool_name,
        "params": params,
        "result": result
    }
    logger.info(json.dumps(log_entry))
```

What happens:
1. Create a `log_entry` (a dictionary with information)
2. `timestamp` = current date and time
3. `event` = what happened ("tool_call")
4. `tool` = which tool was called
5. `params` = what inputs were given
6. `result` = what output was received
7. `logger.info()` = record this in the log
8. `json.dumps()` = convert dictionary to text format

**Example Log Entry:**
```json
{
  "timestamp": "2025-11-19T21:19:00",
  "event": "tool_call",
  "tool": "get_customer",
  "params": {"id": "123"},
  "result": {"name": "John"}
}
```

**Line 19:**
```python
log_tool_call("get_customer", {"id": "123"}, {"name": "John"})
```
- Call the logging function
- Record: Tool "get_customer" was called with id "123" and returned John's info

---

## Code Example 2: Execution Tracing

### The Code:
```python
class ExecutionTrace:
    def __init__(self, trace_id: str):
        self.trace_id = trace_id
        self.steps = []
    
    def add_step(self, name: str, input_data, output_data, duration_ms: float):
        self.steps.append({
            "name": name,
            "input": input_data,
            "output": output_data,
            "duration_ms": duration_ms
        })
    
    def print_narrative(self):
        for i, step in enumerate(self.steps, 1):
            print(f"Step {i}: {step['name']}")
            print(f"  Input: {step['input']}")
            print(f"  Output: {step['output']}")
            print(f"  Duration: {step['duration_ms']}ms\n")
```

### What Does This Do?

This traces the **entire flow** of agent execution. Think of it like:
- Watching a movie scene by scene
- Following a recipe step by step
- Tracing a detective's investigation

### Breaking It Down:

**Lines 1-3:**
```python
class ExecutionTrace:
    def __init__(self, trace_id: str):
        self.trace_id = trace_id
        self.steps = []
```
- Create a new trace with an ID
- `self.steps = []` = empty list to store steps

**Lines 5-12:**
```python
def add_step(self, name: str, input_data, output_data, duration_ms: float):
    self.steps.append({
        "name": name,
        "input": input_data,
        "output": output_data,
        "duration_ms": duration_ms
    })
```

What happens:
- Record one step in the execution
- Store: What it's called, what went in, what came out, how long it took

**Example:**
```python
trace = ExecutionTrace("trace_001")

# Agent looks up customer
trace.add_step(
    name="lookup_customer",
    input_data={"customer_id": "123"},
    output_data={"name": "John", "balance": 500},
    duration_ms=150
)

# Agent creates ticket
trace.add_step(
    name="create_ticket",
    input_data={"customer_id": "123", "issue": "Account problem"},
    output_data={"ticket_id": "TKT001"},
    duration_ms=200
)
```

**Lines 14-19:**
```python
def print_narrative(self):
    for i, step in enumerate(self.steps, 1):
        print(f"Step {i}: {step['name']}")
        print(f"  Input: {step['input']}")
        print(f"  Output: {step['output']}")
        print(f"  Duration: {step['duration_ms']}ms\n")
```

What happens:
- Print out the entire execution story
- `for` = go through each step
- `enumerate(..., 1)` = number them starting from 1
- Print each step's information

**Output:**
```
Step 1: lookup_customer
  Input: {'customer_id': '123'}
  Output: {'name': 'John', 'balance': 500}
  Duration: 150ms

Step 2: create_ticket
  Input: {'customer_id': '123', 'issue': 'Account problem'}
  Output: {'ticket_id': 'TKT001'}
  Duration: 200ms
```

---

# DAY 5: MULTI-AGENT SYSTEMS (AGENTS TALKING TO AGENTS)

## What Is A2A Protocol?

A2A = **Agent to Agent**

Think of it like:
- Person A calls Person B on the phone
- Person B calls Person C on the phone
- They exchange information
- Agents do this automatically!

---

## Code Example 1: A2A Server (An Agent That Other Agents Call)

### The Code:
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

AGENT_CARD = {
    "name": "my_agent",
    "version": "1.0.0",
    "capabilities": ["task_a", "task_b"]
}

class Request(BaseModel):
    input: str
    context: dict = None

class Response(BaseModel):
    output: str
    status: str

@app.get("/.well-known/agent.json")
async def get_agent_card():
    return AGENT_CARD

@app.post("/run")
async def run(request: Request) -> Response:
    result = process_request(request.input)
    return Response(output=result, status="success")

@app.get("/health")
async def health():
    return {"status": "healthy"}

def process_request(input_text: str) -> str:
    return f"Processed: {input_text}"
```

### What Does This Do?

Creates a **server** that other agents can call. Think of it like:
- You open a restaurant
- Customers come and order
- You process their orders
- You give them food

Here:
- You create an "agent restaurant"
- Other agents come and ask for help
- Your agent processes their request
- You send back the result

### Breaking It Down:

**Lines 1-4:**
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()
```
- `FastAPI` = tool for creating web servers
- `BaseModel` = tool for defining data formats
- `app = FastAPI()` = create your web server

**Lines 6-9:**
```python
AGENT_CARD = {
    "name": "my_agent",
    "version": "1.0.0",
    "capabilities": ["task_a", "task_b"]
}
```
- Create a "business card" for your agent
- Other agents see this to understand what you can do
- `capabilities` = list of things your agent can do

**Lines 11-18:**
```python
class Request(BaseModel):
    input: str
    context: dict = None

class Response(BaseModel):
    output: str
    status: str
```
- Define the format of incoming requests
- Define the format of outgoing responses

**Lines 20-22:**
```python
@app.get("/.well-known/agent.json")
async def get_agent_card():
    return AGENT_CARD
```
- `@app.get()` = when someone makes a GET request (asks for info)
- Other agents ask: "What can you do?"
- You respond: Here's my agent card with my capabilities
- Think of it like: Someone calls and asks "What's your menu?"

**Lines 24-27:**
```python
@app.post("/run")
async def run(request: Request) -> Response:
    result = process_request(request.input)
    return Response(output=result, status="success")
```
- `@app.post()` = when someone sends a request to do work
- They send: `input` and optional `context`
- You do: Process the request
- You return: Result and status
- Think of it like: Customer orders food, you cook it, you serve it

**Lines 29-31:**
```python
@app.get("/health")
async def health():
    return {"status": "healthy"}
```
- When asked "Are you working?"
- You respond: "Yes, I'm healthy!"
- Like a health check

**Lines 33-34:**
```python
def process_request(input_text: str) -> str:
    return f"Processed: {input_text}"
```
- This is where the actual work happens
- Takes input, processes it, returns output

---

## Code Example 2: A2A Client (An Agent That Calls Other Agents)

### The Code:
```python
import httpx

class A2AClient:
    def __init__(self, agent_url: str):
        self.agent_url = agent_url
    
    async def call_agent(self, input_text: str) -> str:
        async with httpx.AsyncClient() as client:
            response = await client.post(
                f"{self.agent_url}/run",
                json={"input": input_text}
            )
            return response.json()["output"]
    
    async def get_capabilities(self) -> dict:
        async with httpx.AsyncClient() as client:
            response = await client.get(
                f"{self.agent_url}/.well-known/agent.json"
            )
            return response.json()

# Usage
client = A2AClient("http://other-agent:8000")
# result = await client.call_agent("Hello")
```

### What Does This Do?

Creates a **client** that calls other agents. Think of it like:
- You need help, so you call another business
- You ask them for help
- They process your request
- They send back the result

### Breaking It Down:

**Lines 1-5:**
```python
import httpx

class A2AClient:
    def __init__(self, agent_url: str):
        self.agent_url = agent_url
```
- `httpx` = tool for making web requests (like making a phone call)
- Create a client with the URL of the other agent
- `self.agent_url` = where to find the agent you want to call

**Lines 7-13:**
```python
async def call_agent(self, input_text: str) -> str:
    async with httpx.AsyncClient() as client:
        response = await client.post(
            f"{self.agent_url}/run",
            json={"input": input_text}
        )
        return response.json()["output"]
```

What happens:
1. Create a connection to the other agent
2. Send a POST request to their `/run` endpoint
3. Include: `{"input": input_text}`
4. Wait for response
5. Extract the "output" from the response
6. Return it

**Example:**
```python
client = A2AClient("http://support-agent:8000")
result = await client.call_agent("What's my balance?")
# Makes request to http://support-agent:8000/run
# Sends: {"input": "What's my balance?"}
# Receives: {"output": "Your balance is $500", "status": "success"}
# Returns: "Your balance is $500"
```

**Lines 15-20:**
```python
async def get_capabilities(self) -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.get(
            f"{self.agent_url}/.well-known/agent.json"
        )
        return response.json()
```

What happens:
1. Ask the other agent: "What can you do?"
2. They send back their agent card
3. You get their capabilities

**Example:**
```python
client = A2AClient("http://support-agent:8000")
capabilities = await client.get_capabilities()
# Returns: {"name": "support_agent", "version": "1.0.0", "capabilities": [...]}
```

---

## Code Example 3: Multi-Agent Orchestrator (Manager)

### The Code:
```python
class Orchestrator:
    def __init__(self):
        self.agents = {}
    
    def register(self, name: str, url: str):
        self.agents[name] = A2AClient(url)
    
    async def execute_workflow(self, task: str, workflow: list) -> str:
        result = task
        for step in workflow:
            agent_name = step["agent"]
            result = await self.agents[agent_name].call_agent(result)
        return result
```

### What Does This Do?

Creates a **manager** that coordinates multiple agents. Think of it like:
- A project manager with multiple team members
- Manager: "Team, here's the task"
- Agent 1: Processes it, passes to Agent 2
- Agent 2: Processes it, passes to Agent 3
- Agent 3: Finishes it
- Manager: Gets final result

### Breaking It Down:

**Lines 1-5:**
```python
class Orchestrator:
    def __init__(self):
        self.agents = {}
    
    def register(self, name: str, url: str):
        self.agents[name] = A2AClient(url)
```
- Create a manager
- `self.agents = {}` = empty list of agents
- `register()` = hire an agent
- Store their name and URL

**Example:**
```python
orchestrator = Orchestrator()
orchestrator.register("analyzer", "http://analyzer:8000")
orchestrator.register("processor", "http://processor:8000")
orchestrator.register("formatter", "http://formatter:8000")
```

**Lines 7-12:**
```python
async def execute_workflow(self, task: str, workflow: list) -> str:
    result = task
    for step in workflow:
        agent_name = step["agent"]
        result = await self.agents[agent_name].call_agent(result)
    return result
```

What happens:
1. Start with the task
2. Go through each step in the workflow
3. Call the agent for that step
4. Pass the result to the next step
5. Return the final result

**Example:**
```python
orchestrator = Orchestrator()
orchestrator.register("analyzer", "http://analyzer:8000")
orchestrator.register("processor", "http://processor:8000")
orchestrator.register("formatter", "http://formatter:8000")

workflow = [
    {"agent": "analyzer"},
    {"agent": "processor"},
    {"agent": "formatter"}
]

result = await orchestrator.execute_workflow("Analyze data", workflow)

# What happens:
# 1. analyzer.call_agent("Analyze data") -> returns analyzed_data
# 2. processor.call_agent(analyzed_data) -> returns processed_data
# 3. formatter.call_agent(processed_data) -> returns formatted_result
# Final result: formatted_result
```

---

# SUMMARY: What Each Day Teaches

## Day 1: Build an Agent
- What: Create one agent with a name and instructions
- Why: Single agent can handle simple tasks
- Code: `Agent(model='...', name='...', instruction='...')`

## Day 2: Give It Power
- What: Add tools so agents can do more
- Why: Tools let agents interact with databases, APIs, external systems
- Code: `FunctionTool.create(function)` and add to agent

## Day 3: Give It Memory
- What: Let agent remember conversations
- Why: Agents can be personal and context-aware
- Code: `SessionManager()` for sessions, `MemoryStore()` for long-term

## Day 4: Watch It Work
- What: Log and trace everything the agent does
- Why: When something goes wrong, you can debug it
- Code: `logger.info()` for logs, `ExecutionTrace` for tracing

## Day 5: Make It Powerful
- What: Let agents talk to other agents
- Why: Multiple agents working together solve complex problems
- Code: Create servers (`@app.get()`, `@app.post()`) and clients (`A2AClient`)

---

**Remember:** Code is just instructions. Like a recipe, you follow steps to get a result. Read it slowly, line by line, and it will make sense!
