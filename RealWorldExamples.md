# Real-World Examples - AI Agents in Action

## Introduction

Let me show you how AI agents work using **real-world examples** you can relate to.

---

# EXAMPLE 1: Customer Support Center

## The Scenario

Imagine you call **Amazon Customer Support** and need help with a return.

### Without Agents (Old Way):
```
You: Call the number
Human: "Hi, how can I help?"
You: "I want to return my package"
Human: Looks through files manually
Human: "Hold on, let me check... one moment... okay, found it"
(Takes 10-15 minutes)
```

### With Agents (New Way):
```
You: Call the number
Agent 1 (Greeting): "Hi! I'll help you"
Agent 2 (Lookup): Instantly finds your order
Agent 3 (Process): Starts the return immediately
(Takes 30 seconds)
```

## The Code:

### Agent 1: Greeting Agent
```python
greeting_agent = Agent(
    model='gemini-2.5-flash',
    name='greeting_agent',
    instruction="""You are a friendly greeting agent.
    - Welcome the customer warmly
    - Ask how you can help today
    - Route them to the right department"""
)
```

### Agent 2: Lookup Agent (With Tools)
```python
def find_customer_order(customer_id: str):
    """Find customer's recent orders"""
    return {
        "orders": [
            {"id": "ORD123", "item": "Phone", "status": "Delivered"},
            {"id": "ORD124", "item": "Charger", "status": "Delivered"}
        ]
    }

lookup_agent = Agent(
    model='gemini-2.5-flash',
    name='lookup_agent',
    instruction='Find customer information and orders',
    tools=[FunctionTool.create(find_customer_order)]
)
```

### Agent 3: Return Processing Agent
```python
def process_return(order_id: str, reason: str):
    """Process a return request"""
    return {
        "return_id": "RTN001",
        "status": "Return approved",
        "shipping_label": "https://..."
    }

return_agent = Agent(
    model='gemini-2.5-flash',
    name='return_agent',
    instruction='Process returns quickly and efficiently',
    tools=[FunctionTool.create(process_return)]
)
```

### Multi-Agent System:
```python
support_system = SequentialAgent(
    name='amazon_support',
    agents=[greeting_agent, lookup_agent, return_agent]
)

# When customer calls:
# greeting_agent -> handles greeting
# lookup_agent -> finds the order
# return_agent -> processes the return
# Customer gets help in 30 seconds!
```

---

# EXAMPLE 2: Personal Shopping Assistant

## The Scenario

You want to buy a laptop but need help deciding.

### How It Works:

**Step 1: Greeting**
```
Agent: "Hi! I'm your shopping assistant. What are you looking for?"
You: "I need a laptop for programming"
```

**Step 2: Understanding (With Memory)**
```
Agent remembers: 
- User wants: laptop
- Purpose: programming
- This tells agent: need good processor, RAM, SSD
```

**Step 3: Tool Usage (Search & Compare)**
```
Agent uses tools to:
- Search: "Best laptops for programming"
- Compare: Specs, price, reviews
- Filter: By budget, requirements
```

**Step 4: Recommendation**
```
Agent: "For programming, I recommend:
1. MacBook Pro 16" - $2499 (best option)
2. Dell XPS 15 - $1799 (good value)
3. Lenovo ThinkPad - $1299 (budget option)"
```

## The Code:

### Shopping Agent With Memory:
```python
# User profile (long-term memory)
user_profile = {
    "user_id": "user123",
    "name": "Sarah",
    "interests": ["programming", "gaming"],
    "budget": 2000,
    "previous_purchases": ["Mouse", "Keyboard"]
}

# Session storage (short-term memory)
class ShoppingSession:
    def __init__(self, user_id):
        self.user_id = user_id
        self.conversation = []
        self.cart = []
    
    def add_to_conversation(self, role, message):
        self.conversation.append({
            "role": role,  # "user" or "agent"
            "message": message
        })
    
    def add_to_cart(self, product):
        self.cart.append(product)

# Shopping Agent with tools
def search_products(query: str, max_price: float):
    """Search products by query and price"""
    return [
        {
            "id": "PROD001",
            "name": "MacBook Pro 16",
            "price": 2499,
            "specs": {"cpu": "M3", "ram": "16GB", "storage": "512GB"}
        },
        {
            "id": "PROD002",
            "name": "Dell XPS 15",
            "price": 1799,
            "specs": {"cpu": "Intel i7", "ram": "16GB", "storage": "512GB"}
        }
    ]

def get_product_reviews(product_id: str):
    """Get reviews for a product"""
    return {
        "product_id": product_id,
        "rating": 4.8,
        "reviews": [
            "Great for programming!",
            "Fast and reliable",
            "Worth the price"
        ]
    }

shopping_agent = Agent(
    model='gemini-2.5-flash',
    name='shopping_assistant',
    instruction="""You are a helpful shopping assistant.
    - Remember user preferences
    - Use tools to search and compare products
    - Give personalized recommendations
    - Help add items to cart""",
    tools=[
        FunctionTool.create(search_products),
        FunctionTool.create(get_product_reviews)
    ]
)

# Using the agent:
session = ShoppingSession("user123")

# User message
session.add_to_conversation("user", "I need a laptop for programming")

# Agent thinks and uses tools
# 1. Agent calls: search_products("laptop programming", 2000)
# 2. Agent calls: get_product_reviews("PROD001")
# 3. Agent returns: Personalized recommendation

# Agent response
session.add_to_conversation(
    "agent",
    "For programming, I recommend MacBook Pro. Great for development."
)
```

---

# EXAMPLE 3: Healthcare Appointment System

## The Scenario

You call a hospital to book an appointment.

### Without Agents:
```
You: Call and wait on hold
You: "I need to book an appointment"
Receptionist: Checks calendar (manually!)
Receptionist: "We have slots on Tuesday or Thursday"
You: "Tuesday works"
Receptionist: Books appointment, writes it down
```

### With Agents (Multi-Agent System):
```
You: Call the number
Agent 1: "Hi, what do you need?"
You: "Appointment with Dr. Smith"
Agent 1: Understands request
Agent 2: Checks doctor's schedule (instantly!)
Agent 2: Finds available times
Agent 3: Books appointment and sends confirmation
(All in 1 minute!)
```

## The Code:

### Agent 1: Intake Agent
```python
intake_agent = Agent(
    model='gemini-2.5-flash',
    name='intake_agent',
    instruction="""You are a healthcare intake agent.
    - Understand patient concerns
    - Determine urgency
    - Route to appropriate department
    - Ask about patient symptoms if relevant"""
)
```

### Agent 2: Scheduler (With Tools)
```python
def check_doctor_availability(doctor_name: str, date: str):
    """Check doctor's available time slots"""
    return {
        "doctor": doctor_name,
        "date": date,
        "available_slots": [
            {"time": "09:00 AM", "duration": "30 min"},
            {"time": "10:00 AM", "duration": "30 min"},
            {"time": "02:00 PM", "duration": "30 min"}
        ]
    }

def book_appointment(doctor_name: str, date: str, time: str, patient_id: str):
    """Book the appointment"""
    return {
        "appointment_id": "APT001",
        "confirmation": "Confirmed",
        "details": {
            "doctor": doctor_name,
            "date": date,
            "time": time,
            "location": "Building A, Room 101"
        }
    }

scheduler_agent = Agent(
    model='gemini-2.5-flash',
    name='scheduler_agent',
    instruction='Check availability and book appointments',
    tools=[
        FunctionTool.create(check_doctor_availability),
        FunctionTool.create(book_appointment)
    ]
)
```

### Agent 3: Confirmation Agent
```python
def send_confirmation_email(patient_email: str, appointment_details: dict):
    """Send confirmation to patient"""
    return {
        "status": "Email sent",
        "recipient": patient_email
    }

def send_reminder_sms(phone: str, appointment_details: dict):
    """Send reminder SMS"""
    return {
        "status": "SMS sent",
        "message": "Reminder: You have an appointment tomorrow at..."
    }

confirmation_agent = Agent(
    model='gemini-2.5-flash',
    name='confirmation_agent',
    instruction='Send confirmations and reminders to patients',
    tools=[
        FunctionTool.create(send_confirmation_email),
        FunctionTool.create(send_reminder_sms)
    ]
)
```

### Complete Hospital System:
```python
hospital_system = SequentialAgent(
    name='hospital_appointment_system',
    agents=[intake_agent, scheduler_agent, confirmation_agent]
)

# Flow:
# intake_agent -> Understands patient needs
# scheduler_agent -> Books the appointment
# confirmation_agent -> Sends confirmation & reminder
```

---

# EXAMPLE 4: A2A Protocol - Agents Talking to Agents

## The Scenario

You have a **sales company with two departments**: Sales Team and Fulfillment Team.

**Old Way (Manual):**
```
Sales Agent: "We sold a laptop!"
(Calls fulfillment team manually)
Fulfillment Agent: "Okay, noted"
(Writes it down manually)
(Slow, error-prone)
```

**New Way (A2A Protocol):**
```
Sales Agent: Automatically sends order to Fulfillment Agent
Fulfillment Agent: Automatically receives and processes
(Fast, automated, no errors!)
```

## The Code:

### Sales Agent (Server)
```python
# File: sales_agent.py

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Agent Card - tells other agents what we can do
AGENT_CARD = {
    "name": "sales_agent",
    "version": "1.0.0",
    "capabilities": ["process_order", "get_customer_info"]
}

class OrderRequest(BaseModel):
    customer_name: str
    product: str
    quantity: int
    price: float

@app.get("/.well-known/agent.json")
async def get_agent_card():
    """Other agents ask: What can you do?"""
    return AGENT_CARD

@app.post("/process_order")
async def process_order(request: OrderRequest):
    """Process an order"""
    order = {
        "order_id": "ORD001",
        "customer": request.customer_name,
        "product": request.product,
        "quantity": request.quantity,
        "total": request.quantity * request.price,
        "status": "Order created"
    }
    return order

@app.get("/health")
async def health():
    """Check if agent is healthy/working"""
    return {"status": "healthy"}
```

### Fulfillment Agent (Client)
```python
# File: fulfillment_agent.py

import httpx

class FulfillmentAgent:
    def __init__(self):
        self.sales_agent_url = "http://sales-agent:8000"
    
    async def get_order_from_sales(self, order_details):
        """Call Sales Agent to process order"""
        client = A2AClient(self.sales_agent_url)
        
        # Call Sales Agent
        order = await client.call_agent_endpoint(
            "/process_order",
            order_details
        )
        
        # Now fulfillment_agent has the order!
        return order
    
    def fulfill_order(self, order):
        """Fulfill the order"""
        fulfillment = {
            "fulfillment_id": "FULF001",
            "order_id": order["order_id"],
            "status": "Ready to ship",
            "tracking_number": "TRK123456"
        }
        return fulfillment

# Usage:
fulfillment = FulfillmentAgent()
order = await fulfillment.get_order_from_sales({
    "customer_name": "John",
    "product": "Laptop",
    "quantity": 1,
    "price": 1000
})
result = fulfillment.fulfill_order(order)
# Result: {"fulfillment_id": "FULF001", "status": "Ready to ship"}
```

### How It Works:

```
1. Customer buys laptop on website
   
2. Sales Agent creates order
   Order: {customer: "John", product: "Laptop", qty: 1}

3. Sales Agent calls Fulfillment Agent via A2A Protocol
   "Hey Fulfillment Agent! Process this order!"

4. Fulfillment Agent receives order
   "Thanks! I'll prepare it for shipping"

5. Fulfillment Agent ships the product
   Result: {status: "Ready to ship", tracking: "TRK123456"}

6. Fulfillment Agent sends confirmation back
   "Done! Shipping to John"

All automated! No manual work!
```

---

# EXAMPLE 5: Multi-Agent AI Research Assistant

## The Scenario

You ask an AI: "What are the latest breakthroughs in quantum computing?"

### What Happens Behind the Scenes:

```
1. User Question Agent
   Input: "What are the latest breakthroughs in quantum computing?"
   Output: Structured query

2. Research Agent
   Uses tools to search web
   Gets latest articles, papers, news
   
3. Analysis Agent
   Reads all the sources
   Identifies key breakthroughs
   Summarizes findings

4. Writing Agent
   Writes a clear, comprehensive answer
   Cites sources
   
5. Fact Check Agent
   Verifies all claims
   Ensures accuracy
   
Output: Complete, accurate research summary
```

## The Code:

```python
# Agent 1: Question Parser
question_agent = Agent(
    model='gemini-2.5-flash',
    name='question_parser',
    instruction='Parse user questions and structure them for research'
)

# Agent 2: Web Researcher (With Tools)
def search_web(query: str):
    """Search for recent articles"""
    return [
        {"title": "Quantum breakthrough...", "url": "...", "date": "2025-11-15"},
        {"title": "New quantum chip...", "url": "...", "date": "2025-11-10"}
    ]

researcher_agent = Agent(
    model='gemini-2.5-flash',
    name='researcher_agent',
    instruction='Search and gather research materials',
    tools=[FunctionTool.create(search_web)]
)

# Agent 3: Analyzer
analyzer_agent = Agent(
    model='gemini-2.5-flash',
    name='analyzer_agent',
    instruction='Read sources and extract key findings'
)

# Agent 4: Writer
writer_agent = Agent(
    model='gemini-2.5-flash',
    name='writer_agent',
    instruction='Write clear, well-structured answers with citations'
)

# Agent 5: Fact Checker
fact_checker_agent = Agent(
    model='gemini-2.5-flash',
    name='fact_checker_agent',
    instruction='Verify facts and ensure accuracy'
)

# Multi-Agent System
research_system = SequentialAgent(
    name='research_system',
    agents=[
        question_agent,
        researcher_agent,
        analyzer_agent,
        writer_agent,
        fact_checker_agent
    ]
)

# Usage:
result = research_system.process(
    "What are the latest breakthroughs in quantum computing?"
)

# Output: Comprehensive research summary!
```

---

# SUMMARY: How Agents Work in Real Life

## Real-World Pattern:

```
User Input
    ↓
Agent 1 (Understands) - What does user want?
    ↓
Agent 2 (Gathers Info) - What information do I need?
    ↓
Agent 3 (Analyzes) - What does this information mean?
    ↓
Agent 4 (Decides) - What should I do?
    ↓
Agent 5 (Executes) - Do it!
    ↓
Agent 6 (Verifies) - Did it work correctly?
    ↓
User Output
```

## Key Concepts:

1. **Agents are Specialized**
   - Each agent has one job
   - Works best for that job
   - Hands off to next agent

2. **Agents Have Memory**
   - Remember past conversations
   - Understand context
   - Personalize responses

3. **Agents Use Tools**
   - Search the web
   - Access databases
   - Call APIs
   - Send emails
   - Anything!

4. **Agents Work Together**
   - Call each other via A2A
   - Share information
   - Coordinate tasks
   - Solve complex problems

5. **Agents Are Observable**
   - You can watch what they do
   - See logs of all actions
   - Debug if something goes wrong
   - Measure performance

---

# Your Next Step

Now that you understand:
- ✅ How code works
- ✅ How agents think
- ✅ How agents work together
- ✅ Real-world examples

**You're ready to build your own AI Agent!**

Choose one of these projects:
1. **Customer Support Bot** - Use Example 1
2. **Shopping Assistant** - Use Example 2  
3. **Appointment Scheduler** - Use Example 3
4. **Multi-Company Integration** - Use Example 4
5. **Research Assistant** - Use Example 5

Pick your favorite and start coding!
