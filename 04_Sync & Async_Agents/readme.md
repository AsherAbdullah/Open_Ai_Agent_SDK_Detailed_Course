# Sync aur Async Agents ko Samajhna

### Sync Agents
- **Definition**: Synchronous agents tasks ko blocking manner mein execute kartay hain, yani program wait karta hai jab tak agent apna task complete na kar le.
- **Method**: `Runner.run_sync()` ka istemal hota hai.
- **Use Case**: Simple scripts, testing, ya scenarios ke liye ideal hai jahan straightforward, sequential execution chahiye without async complexities.
- **How It Works**: SDK internally `Runner.run()` ke async logic ko handle karta hai lekin execution block karta hai jab tak task complete na ho, aur `RunResult` object return karta hai with final output.

### Async Agents
- **Definition**: Asynchronous agents tasks ko non-blocking tareeqay se execute kartay hain, yani program ke doosray hissay chal saktay hain jab agent input process karta hai.
- **Methods**:
  - `Runner.run()`: Agent ko asynchronously chalaata hai aur `RunResult` object return karta hai jab complete ho.
  - `Runner.run_streamed()`: Agent ko asynchronously chalaata hai aur events (masalan, token generation, tool calls) ko real-time mein stream karta hai, `RunResultStreaming` object return karta hai.
- **Use Case**: Scalable applications, user-facing interfaces (masalan, chatbots), ya scenarios ke liye best hai jahan progress dikhana ho ya multiple tasks concurrently handle karne hon.
- **How It Works**: Python ke `asyncio` ka istemal karta hai non-blocking execution ke liye, program ko continue karne deta hai jab agent input process karta hai.

### Key Differences
| Feature                | Sync Agent (`Runner.run_sync`) | Async Agent (`Runner.run` / `Runner.run_streamed`) |
|------------------------|--------------------------------|--------------------------------------------------|
| **Execution Style**    | Blocking (completion ka wait karta hai) | Non-blocking (saath saath chalta hai) |
| **Return Type**        | `RunResult`                   | `RunResult` (run) ya `RunResultStreaming` (streamed) |
| **Use Case**           | Simple scripts, testing        | Scalable apps, real-time feedback               |
| **Performance**        | Multiple tasks ke liye slow    | Concurrent tasks ke liye fast                   |
| **Complexity**         | Implement karna simple         | Async/await knowledge chahiye                   |

---

## Environment Setup Karna

Examples chalane ke liye, aapko Python environment chahiye with OpenAI Agents SDK aur optionally Jupyter Notebook for interactive development. Niche steps hain dono setup karne ke:

### Python aur OpenAI Agents SDK Install Karna
1. **Python Install Karna**:
   - Ensure karein ke Python 3.8+ installed hai. [python.org](https://www.python.org/downloads/) se download karein agar zarurat ho.
   - Verify karein:
     ```bash
     python --version
     ```
2. **OpenAI Agents SDK Install Karna**:
   ```bash
   pip install openai-agents
   ```
3. **OpenAI API Key Set Karna**:
   - API key ko environment variable mein set karein for security:
     ```bash
     export OPENAI_API_KEY="your-api-key"
     ```
   - Ya, Python mein set karein (kam secure):
     ```python
     import os
     os.environ["OPENAI_API_KEY"] = "your-api-key"
     ```

### Jupyter Notebook Install Karna
Jupyter Notebook aik interactive environment hai jo code examples test aur visualize karne ke liye ideal hai. Setup ke steps:

1. **Jupyter Notebook Install Karna**:
   - Jupyter ko pip se install karein:
     ```bash
     pip install jupyter
     ```
   - Agar aap JupyterLab (modern interface) prefer karte hain:
     ```bash
     pip install jupyterlab
     ```
2. **Jupyter Notebook Launch Karna**:
   - Jupyter Notebook server start karein:
     ```bash
     jupyter notebook
     ```
     Ya, JupyterLab ke liye:
     ```bash
     jupyter lab
     ```
   - Yeh browser mein Jupyter interface kholega at `http://localhost:8888`.
3. **Naya Notebook Banana**:
   - Jupyter interface mein, "New" > "Python 3" click karein naya notebook banane ke liye.
   - Niche diye gaye code examples ko notebook cells mein copy-paste kar ke chalayein.
4. **Additional Dependencies Install Karna**:
   - Kuch examples Pydantic ka istemal karte hain structured outputs ke liye. Install karein:
     ```bash
     pip install pydantic
     ```
5. **Jupyter mein Async Code Chalana**:
   - Jupyter natively async code support karta hai. Aap `await` directly cells mein use kar saktay hain without `asyncio.run()`. Masalan:
     ```python
     from agents import Agent, Runner
     agent = Agent(name="TestAgent", instructions="Say hello")
     result = await Runner.run(agent, "Hello")
     print(result.final_output)
     ```
   - Agar async code mein issues aayein, `nest_asyncio` install karein:
     ```bash
     pip install nest_asyncio
     ```
     Phir notebook mein apply karein:
     ```python
     import nest_asyncio
     nest_asyncio.apply()
     ```

### Prerequisites
- Intermediate Python knowledge (functions, classes, async/await).
- OpenAI API aur LLMs ke saath familiarity.
- Jupyter ke liye: Cells chalane aur notebooks manage karne ka basic knowledge.
- Optional: Pydantic knowledge for structured outputs aur async programming for Async Agents.

---

## Example 1: Sync Agent - Basic Task Processing

Yeh example aik synchronous agent banata hai jo recursion in programming par haiku likhta hai. Aap isay Jupyter Notebook cell mein chala saktay hain.

### Code Example
```python
from agents import Agent, Runner

# Agent define karna
agent = Agent(
    name="PoetryAgent",
    instructions="Tum aik shayar ho. Programming mein recursion par haiku likho."
)

# Agent ko synchronously chalana
result = Runner.run_sync(agent, "Programming mein recursion par haiku likho.")
print(result.final_output)
```

### Expected Output
```
Code ke andar code,
Functions khud ko bulate hain,
Infinite loop ka nach.
```

### Explanation
- **Agent Creation**: `Agent` ko name aur instructions ke saath configure kiya gaya hai.
- **Execution**: `Runner.run_sync` input ko synchronously process karta hai, jab tak LLM (default model, masalan gpt-3.5-turbo) final output na de.
- **Use Case**: Jupyter mein quick tests ya simple scripts ke liye suitable hai jahan real-time feedback ki zarurat nahi.

---

## Example 2: Async Agent - Tool Call Handling

Yeh example aik async agent banata hai jo weather information ke liye tool ka istemal karta hai. Jupyter mein, aap isay directly cell mein `await` ke saath chala saktay hain.

### Code Example
```python
from agents import Agent, Runner, function_tool

# Weather fetch karne ka tool define karna
@function_tool
def get_weather(city: str) -> str:
    """Di gaye city ka weather fetch karta hai."""
    return f"{city} ka weather sunny hai."

# Agent ko tool ke saath define karna
agent = Agent(
    name="WeatherAgent",
    instructions="Tum aik madadgar agent ho jo weather information deta hai.",
    tools=[get_weather]
)

# Agent ko asynchronously chalana
result = await Runner.run(agent, input="Tokyo ka weather kya hai?")
print(result.final_output)
```

### Expected Output
```
Tokyo ka weather sunny hai.
```

### Explanation
- **Tool Definition**: `@function_tool` decorator `get_weather` ko tool banata hai jo agent call kar sakta hai.
- **Agent Configuration**: Agent ko `get_weather` tool aur instructions ke saath configure kiya gaya hai.
- **Async Execution**: Jupyter mein, `await Runner.run` agent ko non-blocking tareeqay se chalaata hai. SDK tool calls handle karta hai aur final output deta hai.
- **Use Case**: Applications ke liye ideal hai jahan external actions (masalan, API calls) ki zarurat ho without blocking.

---

## Example 3: Async Agent with Streaming - Real-Time Feedback

Streaming real-time progress dikhane ke liye useful hai, khaas kar Jupyter Notebooks mein jahan output incrementally dikhaya ja sakta hai.

### Code Example
```python
from agents import Agent, Runner

# Agent define karna
agent = Agent(
    name="StreamingAgent",
    instructions="Tum aik madadgar assistant ho. Python ki brief history do."
)

# Agent responses ko stream karna
async for event in Runner.run_streamed(agent, "Python ki history ke bare mein batao."):
    if event.event_type == "token":
        print(event.data, end="", flush=True)
```

### Expected Output
```
Python ko Guido van Rossum ne banaya aur 1991 mein pehli baar release kiya. Iska focus code readability aur simplicity par tha, jo web development, data science, aur zyada mein versatile language bana.
```

### Explanation
- **Streaming Execution**: `Runner.run_streamed` events jaise token generation ko real-time mein stream karta hai.
- **Jupyter Compatibility**: `async for` loop Jupyter cell mein directly kaam karta hai, immediate feedback deta hai.
- **Use Case**: Interactive demos ya chat interfaces ke liye perfect hai.

---

## Example 4: Multi-Agent Workflow with Handoffs

Yeh example multi-agent workflow dikhata hai jahan triage agent tasks ko language-specific agents ke hawalay karta hai, sync aur async dono methods ke saath.

### Code Example
```python
from agents import Agent, Runner

# Specialized agents define karna
spanish_agent = Agent(
    name="SpanishAgent",
    instructions="Tum sirf Spanish bolte ho.",
    model="gpt-3.5-turbo"
)
english_agent = Agent(
    name="EnglishAgent",
    instructions="Tum sirf English bolte ho.",
    model="gpt-4o"
)
triage_agent = Agent(
    name="TriageAgent",
    instructions="Request ki language ke mutabiq suitable agent ko handoff karo.",
    handoffs=[spanish_agent, english_agent],
    model="gpt-3.5-turbo"
)

# Sync execution
result_sync = Runner.run_sync(triage_agent, "Hola, ¿cómo estás?")
print("Sync Result:", result_sync.final_output)

# Async execution
result_async = await Runner.run(triage_agent, "Hello, how are you?")
print("Async Result:", result_async.final_output)
```

### Expected Output
```
Sync Result: ¡Hola! Estoy bien, gracias.
Async Result: Hello! I'm doing well, thank you.
```

### Explanation
- **Agent Setup**: Triage agent Spanish ya English agent ko input language ke mutabiq delegate karta hai.
- **Sync aur Async**: Spanish ke liye `run_sync` aur English ke liye `await run`, dono Jupyter cell mein chalaye ja saktay hain.
- **Use Case**: Customer support systems ya multilingual applications ke liye useful hai.

---

## Example 5: Structured Outputs with Pydantic

Yeh example async agent dikhata hai jo Pydantic ka istemal karke structured calendar events extract karta hai, Jupyter ke interactive environment ke liye ideal.

### Code Example
```python
from agents import Agent, Runner
from pydantic import BaseModel
from typing import List

# Pydantic model define karna
class CalendarEvent(BaseModel):
    name: str
    date: str
    participants: List[str]

# Agent define karna
agent = Agent(
    name="CalendarExtractor",
    instructions="Text se calendar events extract karo.",
    output_type=CalendarEvent
)

# Agent chalana
input_text = "Team meeting on 2025-06-01 with Alice, Bob, and Charlie."
result = await Runner.run(agent, input_text)
event = result.final_output_as(CalendarEvent)
print(f"Event Name: {event.name}")
print(f"Date: {event.date}")
print(f"Participants: {', '.join(event.participants)}")
```

### Expected Output
```
Event Name: Team meeting
Date: 2025-06-01
Participants: Alice, Bob, Charlie
```

### Explanation
- **Structured Output**: `output_type=CalendarEvent` ensure karta hai ke output Pydantic model mein ho.
- **Jupyter Use**: Async code notebook cell mein seamlessly chalta hai, structured output clearly dikhata hai.
- **Use Case**: Data extraction tasks ke liye best hai interactive environments mein.

---

## Best Practices aur Tips

1. **Jupyter Notebook Istemal**:
   - Jupyter ko interactive testing aur visualization ke liye use karein.
   - Notebooks ko frequently save karein (`File > Save` ya `Ctrl+S`).
   - Async issues ke liye `nest_asyncio` use karein.
2. **Execution Method**:
   - Quick tests ke liye `Runner.run_sync` use karein.
   - Production ya real-time apps ke liye `Runner.run` ya `Runner.run_streamed`.
3. **Model Selection**:
   - Speed aur cost ke liye `gpt-3.5-turbo`.
   - Complex reasoning ke liye `gpt-4o`.
4. **Guardrails**: Robust applications ke liye input/output guardrails lagayein.
5. **Tracing**: Debugging ke liye OpenAI Dashboard se tracing enable karein.
6. **Error Handling**: `MaxTurnsExceeded` ya `ModelBehaviorError` jaise exceptions handle karein.

---

## Conclusion

OpenAI Agents SDK aur Jupyter Notebook aik powerful platform dete hain AI agents banane aur test karne ke liye. Sync agents (`Runner.run_sync`) simple, blocking tasks ke liye ideal hain, jabke async agents (`Runner.run` aur `Runner.run_streamed`) scalable, real-time applications ke liye behtareen hain. Jupyter Notebook interactive testing aur visualization ko enhance karta hai. Tools, handoffs, aur structured outputs ke saath, developers sophisticated multi-agent systems bana saktay hain.

Ziyada details ke liye, OpenAI Agents SDK documentation dekhein at [openai.github.io](https://openai.github.io) ya OpenAI Platform at [platform.openai.com](https://platform.openai.com).
