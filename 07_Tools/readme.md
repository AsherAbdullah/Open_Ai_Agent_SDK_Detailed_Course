# OpenAI Agents SDK ke Tools ka Istemal Agents Banane ke Liye kyun Kiya Jata Hai

## Introduction

OpenAI Agents SDK aik Python-based, open-source framework hai jo developers ko AI agents banane mein madad deta hai jo complex tasks khud ba khud perform kar saktay hain. Yeh large language models (LLMs) ko instructions, handoffs, guardrails, aur **tools** ke saath integrate karta hai. Tools is SDK ka aik ahem hissa hain kyun ke yeh agents ki capabilities ko barhatay hain, unhein external data ya functionalities tak pohanch dete hain jo sirf LLM ke paas nahi hoti.

**Tools** woh functions ya services hain jo agents ko external tasks perform karne dete hain, jaise API calls, calculations, database queries, ya custom logic. OpenAI Agents SDK mein tools ka istemal `@function_tool` decorator ke zariye kiya jata hai, jo Python functions ko agent-accessible tools mein convert karta hai. Tools ke saath, agents sirf text generate karne se zyada kuch kar saktay hain, jaise real-world data fetch karna ya specific actions perform karna.

Is lecture mein, hum discuss karenge ke OpenAI Agents SDK mein tools kyun use kiye jatay hain, inke types, key benefits, aur practical code examples ke saath yeh batayeinge ke yeh kaise kaam kartay hain. Yeh lecture Urdu-English mein hai taake Urdu-speaking developers asani se samajh sakein.

---

## Tools kyun Use Kiye Jatay Hain OpenAI Agents SDK mein?

Tools ka istemal OpenAI Agents SDK mein is liye kiya jata hai kyun ke yeh agents ko zyada powerful, flexible, aur practical banatay hain. Niche kuch key wajuhaat hain:

### 1. External Data aur Functionality Access
- LLMs apne training data ke base par jawab dete hain, lekin unke paas real-time data (jaise weather, stock prices) ya specific system access nahi hota. Tools agents ko external APIs, databases, ya custom functions se data fetch karne dete hain.
- Masalan, aik agent weather API se current weather fetch kar sakta hai ya database se user information retrieve kar sakta hai.

### 2. Complex Task Automation
- Tools agents ko complex tasks automate karne dete hain jo sirf text generation se nahi ho saktay. Yeh tasks jaise calculations, file operations, ya system commands shamil ho saktay hain.
- Yeh multi-step workflows ke liye ideal hai jahan agent ko multiple actions perform karne hotay hain.

### 3. Enhanced Reasoning aur Decision-Making
- Tools agents ko structured data ya precise results ke saath kaam karne dete hain, jo unki reasoning capabilities ko improve karta hai. Masalan, aik tool calculation perform kar sakta hai, aur agent us result ke base par decision le sakta hai.
- Yeh LLMs ke limitations (jaise math errors) ko overcome karta hai.

### 4. Customization aur Flexibility
- Developers apne specific needs ke mutabiq custom tools bana saktay hain. Yeh flexibility agents ko industry-specific applications ke liye adaptable banati hai, jaise healthcare, finance, ya customer support.
- Tools ko `@function_tool` decorator ke saath asani se define kiya ja sakta hai, jo integration ko simple banata hai.

### 5. Scalability aur Reusability
- Tools reusable hote hain, yani aik tool multiple agents ya workflows mein use ho sakta hai. Yeh development ko teez karta hai aur code maintenance ko asan banata hai.
- Scalable applications ke liye, tools external services ke saath integrate ho kar heavy lifting handle karte hain.

### 6. Real-World Interaction
- Tools agents ko real-world systems ke saath interact karne dete hain, jaise sending emails, updating databases, ya controlling IoT devices. Yeh agents ko practical applications ke liye viable banata hai.

---

## Types of Tools in OpenAI Agents SDK

OpenAI Agents SDK mein tools ko unke functionality aur use case ke base par mukhtalif categories mein divide kiya ja sakta hai. Niche main types hain jo developers commonly use karte hain:

### 1. Data Retrieval Tools
- **Purpose**: Yeh tools external data sources se information fetch karte hain, jaise APIs, databases, ya web services.
- **Examples**:
  - Weather APIs se current weather data lena.
  - Stock market APIs se real-time prices fetch karna.
  - Database queries se user ya product information retrieve karna.
- **Use Case**: Jab agent ko real-time ya up-to-date information chahiye jo LLM ke training data mein nahi hoti.

### 2. Computational Tools
- **Purpose**: Yeh tools calculations ya data processing perform karte hain jo LLMs accurately nahi kar saktay, jaise mathematical operations ya data transformations.
- **Examples**:
  - Numbers ka sum, average, ya statistical analysis.
  - Date/time conversions ya formatting.
  - Complex algorithms jaise sorting ya graph traversal.
- **Use Case**: Analytical tasks ke liye jahan precision zaroori ho, masalan financial calculations ya scientific analysis.

### 3. Action Tools
- **Purpose**: Yeh tools real-world actions perform karte hain, jaise sending notifications, updating systems, ya controlling devices.
- **Examples**:
  - Email ya SMS bhejna.
  - Database mein records update karna.
  - IoT devices ko control karna (jaise smart lights on/off).
- **Use Case**: Automation tasks ke liye jahan agent ko external systems ke saath interact karna ho.

### 4. Search Tools
- **Purpose**: Yeh tools internet ya internal knowledge bases se information search karte hain, jo agent ko relevant data provide karte hain.
- **Examples**:
  - Web search APIs (jaise Google Search ya Bing) se results lena.
  - Internal company documents se information retrieve karna.
  - FAQ databases se answers extract karna.
- **Use Case**: Research-oriented tasks ya customer support ke liye jahan agent ko vast information mein se jawab dhoondna ho.

### 5. Custom Logic Tools
- **Purpose**: Yeh tools developer-defined logic implement karte hain jo specific application needs ke mutabiq hotay hain.
- **Examples**:
  - Custom validation rules (jaise input formats check karna).
  - Business-specific workflows (jaise order processing logic).
  - Proprietary algorithms ya company-specific processes.
- **Use Case**: Industry ya application-specific requirements ke liye jahan standard tools kaam na karein.

### Key Points about Tool Types
- **Flexibility**: Developers kisi bhi type ka tool bana saktay hain using `@function_tool`, jab tak woh Python function ke roop mein define ho.
- **Combination**: Aik agent multiple types ke tools use kar sakta hai, jaise data retrieval aur computational tools saath mein.
- **Scalability**: Tools ko reusable banaya ja sakta hai taake woh multiple agents ya projects mein kaam aayein.

---

## Tools ke Key Benefits OpenAI Agents SDK mein

| Feature | Benefit |
|---------|---------|
| **External Access** | Real-time data aur services tak pohanch, jaise APIs ya databases. |
| **Task Automation** | Complex aur multi-step tasks ko automate karta hai. |
| **Improved Reasoning** | Precise results se decision-making behtar hota hai. |
| **Customization** | Industry-specific needs ke liye custom tools banaye ja saktay hain. |
| **Reusability** | Tools multiple agents mein use ho saktay hain, development teez karta hai. |
| **Real-World Use** | Agents ko practical systems ke saath kaam karne deta hai. |

---

## Setting Up the Environment

Tools ke examples chalane ke liye, environment setup karna zaroori hai:

1. **Python Install Karna**:
   - Python 3.8+ chahiye. [python.org](https://www.python.org/downloads/) se download karein.
   - Verify karein:
     ```bash
     python --version
     ```

2. **OpenAI Agents SDK Install Karna**:
   ```bash
   pip install openai-agents
   ```

3. **OpenAI API Key Set Karna**:
   - API key ko environment variable mein set karein:
     ```bash
     export OPENAI_API_KEY="your-api-key"
     ```
   - Ya Python mein (kam secure):
     ```python
     import os
     os.environ["OPENAI_API_KEY"] = "your-api-key"
     ```

4. **Chainlit Install Karna (Optional)**:
   - Tools ke output ko UI mein dikhane ke liye Chainlit useful hai:
     ```bash
     pip install chainlit
     ```

5. **Jupyter Notebook (Optional)**:
   - Interactive testing ke liye Jupyter install karein:
     ```bash
     pip install jupyter
     ```
   - Jupyter Notebook launch karein:
     ```bash
     jupyter notebook
     ```
   - Async code ke liye `nest_asyncio` install karein agar zarurat ho:
     ```bash
     pip install nest_asyncio
     ```
     Phir notebook mein:
     ```python
     import nest_asyncio
     nest_asyncio.apply()
     ```

---

## Example 1: Data Retrieval Tool for Weather Information

Yeh example aik **data retrieval tool** dikhata hai jo weather information fetch karta hai. Agent is tool ko call karta hai user input ke mutabiq.

### Code Example
```python
from agents import Agent, Runner, function_tool

# Weather tool define karna
@function_tool
def get_weather(city: str) -> str:
    """Di gaye city ka weather fetch karta hai."""
    return f"{city} ka weather sunny hai."

# Agent define karna
agent = Agent(
    name="WeatherAgent",
    instructions="Tum aik madadgar agent ho jo weather information deta hai.",
    tools=[get_weather]
)

# Async function tool ke saath
async def main():
    result = await Runner.run(agent, input="Tokyo ka weather kya hai?")
    print(result.final_output)

# Run karna
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

### Expected Output
```
Tokyo ka weather sunny hai.
```

### Explanation
- **Tool Type**: Data retrieval tool jo external weather information (simulated) fetch karta hai.
- **Tool Definition**: `@function_tool` decorator `get_weather` ko tool banata hai.
- **Agent Configuration**: Agent ko tool ke saath configure kiya gaya hai.
- **Use Case**: Real-time data fetching ke liye, jaise weather ya news APIs.

---

## Example 2: Computational Tool for Mathematical Calculations

Yeh example aik **computational tool** dikhata hai jo mathematical sum calculate karta hai, LLM ke math limitations ko overcome karte hue.

### Code Example
```python
from agents import Agent, Runner, function_tool

# Calculation tool define karna
@function_tool
def calculate_sum(numbers: list) -> float:
    """Di gaye numbers ka sum calculate karta hai."""
    return sum(numbers)

# Agent define karna
agent = Agent(
    name="MathAgent",
    instructions="Tum aik mathematician ho jo numbers ka sum calculate karta hai.",
    tools=[calculate_sum]
)

# Async function tool ke saath
async def main():
    result = await Runner.run(agent, input="1, 2, 3, 4, 5 ka sum calculate karo.")
    print(result.final_output)

# Run karna
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

### Expected Output
```
1, 2, 3, 4, 5 ka sum 15 hai.
```

### Explanation
- **Tool Type**: Computational tool jo precise calculations perform karta hai.
- **Tool Definition**: `calculate_sum` tool numbers ka sum accurately deta hai.
- **Agent Execution**: Agent input ko parse karta hai aur tool ko call karta hai.
- **Use Case**: Analytical tasks ke liye jahan precision zaroori ho.

---

## Example 3: Action Tool with Chainlit UI

Yeh example aik **action tool** dikhata hai jo simulated email sending action perform karta hai, Chainlit UI ke saath integrate kiya gaya hai.

### Code Example
```python
import chainlit as cl
from agents import Agent, Runner, function_tool

# Email sending tool define karna
@function_tool
def send_email(recipient: str, message: str) -> str:
    """Di gaye recipient ko email bhejta hai."""
    return f"Email bheja gaya {recipient} ko: {message}"

# Agent define karna
agent = Agent(
    name="EmailAgent",
    instructions="Tum aik agent ho jo emails bhejta hai.",
    tools=[send_email]
)

@cl.on_message
async def main(message: cl.Message):
    result = await Runner.run(agent, message.content)
    await cl.Message(content=result.final_output).send()
```

### Kaise Chalayein
1. File ko `app.py` ke naam se save karein.
2. Terminal mein run karein:
   ```bash
   chainlit run app.py -w
   ```
3. Browser mein `http://localhost:8000` par input dein: "Ali ko email bhejo: Meeting kal hai."

### Expected Output
```
Email bheja gaya Ali ko: Meeting kal hai.
```

### Explanation
- **Tool Type**: Action tool jo email sending action simulate karta hai.
- **Chainlit Integration**: `@cl.on_message` user input ko capture karta hai aur tool output ko UI mein dikhata hai.
- **Use Case**: Automation tasks ke liye jahan external actions perform karne hon.

---

## Example 4: Multi-Agent Workflow with Search Tool

Yeh example aik **search tool** aur multi-agent workflow dikhata hai jahan triage agent tasks ko specialized agents ke hawalay karta hai, aur aik agent search tool use karta hai.

### Code Example
```python
import chainlit as cl
from agents import Agent, Runner, function_tool

# Search tool define karna
@function_tool
def web_search(query: str) -> str:
    """Di gaye query ke liye web search karta hai."""
    return f"Search results for '{query}': Simulated web data."

# Specialized agents define karna
search_agent = Agent(
    name="SearchAgent",
    instructions="Tum web search queries handle karta hai.",
    tools=[web_search],
    model="gpt-3.5-turbo"
)
general_agent = Agent(
    name="GeneralAgent",
    instructions="Tum general sawalaat ke jawab dete ho.",
    model="gpt-4o"
)
triage_agent = Agent(
    name="TriageAgent",
    instructions="Request ke mutabiq suitable agent ko handoff karo. Search sawalaat ke liye SearchAgent ko bhejo.",
    handoffs=[search_agent, general_agent],
    model="gpt-3.5-turbo"
)

@cl.on_message
async def main(message: cl.Message):
    result = await Runner.run(triage_agent, message.content)
    await cl.Message(content=result.final_output).send()
```

### Kaise Chalayein
1. File ko `app.py` ke naam se save karein.
2. Run karein:
   ```bash
   chainlit run app.py -w
   ```
3. Browser mein `http://localhost:8000` par inputs dein: "AI kya hai search karo" ya "Python kya hai?"

### Expected Output
```
AI kya hai search karo -> Search results for 'AI kya hai': Simulated web data.
Python kya hai? -> Python aik programming language hai...
```

### Explanation
- **Tool Type**: Search tool jo web search simulate karta hai.
- **Multi-Agent Workflow**: Triage agent search-related sawalaat ko `SearchAgent` ko handoff karta hai jo `web_search` tool use karta hai.
- **Chainlit UI**: Responses UI mein dikhai dete hain.
- **Use Case**: Research ya information retrieval ke liye.

---

## Best Practices aur Tips

1. **Tool Design**:
   - Har tool type ke mutabiq specific aur reusable tools banayein.
   - Clear docstrings likhein taake LLM tool ka purpose samajh sake.
2. **Tool Selection**:
   - Task ke mutabiq correct tool type chunein (masalan, data retrieval for APIs, computational for calculations).
   - Multiple tool types combine karein complex workflows ke liye.
3. **Error Handling**:
   - Tool inputs ko validate karein taake errors se bacha ja sake.
   - Exceptions jaise `ToolExecutionError` ko handle karein.
4. **Performance**:
   - Tools ko lightweight rakhein, khaas kar data retrieval aur action tools.
   - Async tools use karein for external calls.
5. **Chainlit Integration**:
   - Chainlit ke saath tool outputs ko user-friendly UI mein dikhayein.
6. **Testing**:
   - Jupyter Notebooks mein tools test karein quick prototyping ke liye.
   - Real-world scenarios ke liye Chainlit ya production setup use karein.

---

## Conclusion

OpenAI Agents SDK mein tools ka istemal is liye kiya jata hai kyun ke yeh agents ko external data, complex tasks, aur real-world interactions ke qabil banatay hain. Mukhtalif types ke tools—data retrieval, computational, action, search, aur custom logic—agents ko versatile aur industry-specific banatay hain. Tools LLMs ke limitations ko overcome karte hain, customization aur scalability dete hain, aur practical applications ke liye zaroori hain. Code examples se wazeh hai ke har tool type specific use cases ko address karta hai, aur Chainlit jaisi frameworks ke saath yeh aur bhi powerful ho jata hai.

Ziyada details ke liye, OpenAI Agents SDK documentation dekhein at [openai.github.io](https://openai.github.io) aur Chainlit documentation at [chainlit.io](https://chainlit.io).
