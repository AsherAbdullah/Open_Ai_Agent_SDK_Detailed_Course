# Agent as Tools kyun Use Kiya Jata Hai OpenAI Agents SDK mein?

Agent as Tools ka istemal OpenAI Agents SDK mein is liye kiya jata hai kyun ke yeh multi-agent systems ko zyada organized, efficient, aur powerful banata hai. Niche kuch key wajuhaat hain:

### 1. Modularity aur Specialization
- Har agent ko specific expertise ya role ke liye design kiya ja sakta hai (masalan, weather expert, math solver, translator). Agent as Tools ke concept ke saath, yeh specialized agents doosray agents ke liye tools ke tor par kaam kar saktay hain, jisse system modular ho jata hai.
- Masalan, aik triage agent complex query ko analyze kar sakta hai aur usay specialized agent (tool ke roop mein) ke hawalay kar sakta hai.

### 2. Scalability aur Reusability
- Agents ko tools ke tor par use karne se woh reusable ho jate hain. Aik specialized agent multiple workflows ya doosray agents ke liye bar bar use ho sakta hai, jisse development time aur complexity kam hoti hai.
- Yeh large-scale applications ke liye ideal hai jahan bohat se agents collaborate karte hain.

### 3. Simplified Collaboration
- Agent as Tools ke zariye, agents apas mein seamlessly collaborate kar saktay hain. Aik agent doosray agent ko call karta hai jaise woh koi function call karta hai, jisse complex tasks ko smaller, manageable parts mein tohra jata hai.
- Yeh handoffs ke concept ko extend karta hai, jahan agents tasks delegate karte hain without manual intervention.

### 4. Enhanced Problem-Solving
- Specialized agents (tools ke roop mein) complex problems ke specific parts ko solve kar saktay hain, jo overall problem-solving capability ko barhata hai. Masalan, aik agent data fetch kar sakta hai, doosra usay analyze kar sakta hai, aur teesra uske base par recommendation de sakta hai.
- Yeh LLMs ke limitations (jaise context window ya general knowledge) ko overcome karta hai.

### 5. Flexibility aur Customization
- Developers apne needs ke mutabiq agents ko tools ke roop mein design kar saktay hain, jo specific industries ya applications ke liye tailored hote hain (masalan, customer support, financial analysis).
- Yeh flexibility SDK ko versatile banati hai, kyun ke agents ko tools ke tor par mix-and-match kiya ja sakta hai.

### 6. Streamlined Development aur Debugging
- Agent as Tools ke saath, developers ko har task ke liye naye functions ya APIs banane ki zarurat nahi. Specialized agents khud tools ke tor par kaam karte hain, jo development ko streamline karta hai.
- Tracing aur observability features ke saath, agent interactions ko debug karna asan ho jata hai.

---

## Agent as Tools ke Key Benefits OpenAI Agents SDK mein

| Feature | Benefit |
|---------|---------|
| **Modularity** | Specialized agents tasks ko smaller, manageable units mein divide karte hain. |
| **Scalability** | Reusable agents large-scale systems ke liye efficient hote hain. |
| **Collaboration** | Agents seamlessly tasks delegate aur collaborate karte hain. |
| **Problem-Solving** | Complex problems ko specialized agents ke saath solve kiya ja sakta hai. |
| **Customization** | Industry-specific agents tools ke tor par design kiye ja saktay hain. |
| **Streamlined Development** | Development aur debugging asan aur teez hota hai. |

---

## Setting Up the Environment

Agent as Tools ke examples chalane ke liye, environment setup karna zaroori hai:

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
   - Agent interactions ko UI mein dikhane ke liye Chainlit useful hai:
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

## Example 1: Basic Agent as Tool for Language Translation

Yeh example dikhata hai ke kaise aik agent (translator) doosray agent ke liye tool ke tor par kaam karta hai. Triage agent user input ko analyze karta hai aur translation ke liye translator agent ko call karta hai.

### Code Example
```python
from agents import Agent, Runner

# Translator agent define karna
translator_agent = Agent(
    name="TranslatorAgent",
    instructions="Tum aik translator ho. Di gaye text ko Spanish mein translate karo.",
    model="gpt-3.5-turbo"
)

# Triage agent define karna
triage_agent = Agent(
    name="TriageAgent",
    instructions="Agar user translation mangta hai, to TranslatorAgent ko handoff karo. Warna general jawab do.",
    handoffs=[translator_agent],
    model="gpt-3.5-turbo"
)

# Async function
async def main():
    result = await Runner.run(triage_agent, input="Translate 'Hello, how are you?' to Spanish.")
    print(result.final_output)

# Run karna
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

### Expected Output
```
Hola, ¿cómo estás?
```

### Explanation
- **Agent as Tool**: `TranslatorAgent` aik specialized agent hai jo translation ke liye tool ke tor par kaam karta hai.
- **Triage Agent**: `TriageAgent` input ko analyze karta hai aur translation task ke liye `TranslatorAgent` ko handoff karta hai.
- **Use Case**: Simple agent collaboration ke liye jahan specialized tasks delegate kiye jate hain.

---

## Example 2: Agent as Tool for Mathematical Reasoning

Yeh example dikhata hai ke kaise aik math agent doosray agent ke liye tool ke tor par kaam karta hai, complex calculations handle karne ke liye.

### Code Example
```python
from agents import Agent, Runner

# Math agent define karna
math_agent = Agent(
    name="MathAgent",
    instructions="Tum aik mathematician ho. Di gaye numbers ka sum calculate karo aur jawab do.",
    model="gpt-4o"
)

# Triage agent define karna
triage_agent = Agent(
    name="TriageAgent",
    instructions="Agar user math calculation mangta hai, to MathAgent ko handoff karo. Warna general jawab do.",
    handoffs=[math_agent],
    model="gpt-3.5-turbo"
)

# Async function
async def main():
    result = await Runner.run(triage_agent, input="1, 2, 3, 4, 5 ka sum kya hai?")
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
- **Agent as Tool**: `MathAgent` aik specialized agent hai jo precise calculations ke liye tool ke tor par kaam karta hai.
- **Triage Agent**: `TriageAgent` math-related queries ko `MathAgent` ko delegate karta hai.
- **Use Case**: Analytical tasks ke liye jahan LLMs ke math limitations ko overcome karna ho.

---

## Example 3: Agent as Tool with Chainlit UI

Yeh example dikhata hai ke kaise aik agent (weather expert) doosray agent ke liye tool ke tor par kaam karta hai, aur Chainlit UI ke saath integrate hota hai.

### Code Example
```python
import chainlit as cl
from agents import Agent, Runner

# Weather agent define karna
weather_agent = Agent(
    name="WeatherAgent",
    instructions="Tum aik weather expert ho. Di gaye city ka weather batao (simulated data).",
    model="gpt-3.5-turbo"
)

# Triage agent define karna
triage_agent = Agent(
    name="TriageAgent",
    instructions="Agar user weather ke bare mein poochta hai, to WeatherAgent ko handoff karo. Warna general jawab do.",
    handoffs=[weather_agent],
    model="gpt-3.5-turbo"
)

@cl.on_message
async def main(message: cl.Message):
    result = await Runner.run(triage_agent, message.content)
    await cl.Message(content=result.final_output).send()
```

### Kaise Chalayein
1. File ko `app.py` ke naam se save karein.
2. Terminal mein run karein:
   ```bash
   chainlit run app.py -w
   ```
3. Browser mein `http://localhost:8000` par input dein: "Tokyo ka weather kya hai?" ya "Python kya hai?"

### Expected Output
```
Tokyo ka weather kya hai? -> Tokyo ka weather sunny hai.
Python kya hai? -> Python aik programming language hai...
```

### Explanation
- **Agent as Tool**: `WeatherAgent` aik specialized agent hai jo weather queries ke liye tool ke tor par kaam karta hai.
- **Chainlit Integration**: `@cl.on_message` user input ko capture karta hai aur triage agent ke responses ko UI mein dikhata hai.
- **Use Case**: Web-based applications ke liye jahan specialized agents UI ke saath collaborate karte hain.

---

## Example 4: Multi-Agent Workflow with Streaming and Agent as Tool

Yeh example multi-agent workflow dikhata hai jahan aik triage agent multiple specialized agents ko tools ke tor par use karta hai, aur streaming ke saath real-time responses deta hai.

### Code Example
```python
import chainlit as cl
from agents import Agent, Runner

# Specialized agents define karna
translator_agent = Agent(
    name="TranslatorAgent",
    instructions="Tum aik translator ho. Di gaye text ko Spanish mein translate karo.",
    model="gpt-3.5-turbo"
)
math_agent = Agent(
    name="MathAgent",
    instructions="Tum aik mathematician ho. Di gaye numbers ka sum calculate karo.",
    model="gpt-4o"
)
triage_agent = Agent(
    name="TriageAgent",
    instructions="Request ke mutabiq suitable agent ko handoff karo: translation ke liye TranslatorAgent, math ke liye MathAgent. Warna general jawab do.",
    handoffs=[translator_agent, math_agent],
    model="gpt-3.5-turbo"
)

@cl.on_message
async def main(message: cl.Message):
    msg = cl.Message(content="")
    async for event in Runner.run_streamed(triage_agent, message.content):
        if event.event_type == "token":
            await msg.stream_token(event.data)
        elif event.event_type == "handoff":
            await msg.stream_token(f"[Handoff to {event.data}]")
    await msg.update()
```

### Kaise Chalayein
1. File ko `app.py` ke naam se save karein.
2. Run karein:
   ```bash
   chainlit run app.py -w
   ```
3. Browser mein `http://localhost:8000` par inputs dein: "Translate 'Hello' to Spanish" ya "1, 2, 3 ka sum kya hai?"

### Expected Output
```
Translate 'Hello' to Spanish -> [Handoff to TranslatorAgent] Hola
1, 2, 3 ka sum kya hai? -> [Handoff to MathAgent] 1, 2, 3 ka sum 6 hai.
```

### Explanation
- **Agent as Tool**: `TranslatorAgent` aur `MathAgent` specialized agents hain jo tools ke tor par kaam karte hain.
- **Streaming**: `Runner.run_streamed` real-time mein tokens aur handoff events stream karta hai.
- **Chainlit UI**: Responses aur handoff events UI mein engaging tareeqay se dikhai dete hain.
- **Use Case**: Complex multi-agent applications ke liye jahan real-time collaboration aur feedback zaroori ho.

---

## Best Practices aur Tips

1. **Agent Design**:
   - Har agent ko specific role ke liye design karein taake woh tool ke tor par effective ho.
   - Clear instructions dein taake agent apna kaam theek se samajh sake.
2. **Handoff Logic**:
   - Triage agent ke instructions mein clear handoff rules define karein taake tasks sahi agent ko jayein.
3. **Streaming Integration**:
   - Real-time feedback ke liye `Runner.run_streamed` use karein jab Agent as Tools ke saath UI banayein.
4. **Error Handling**:
   - Handoff errors ya agent failures ko handle karein robust systems ke liye.
   - Exceptions jaise `MaxTurnsExceeded` ko monitor karein.
5. **Chainlit UI**:
   - Chainlit ke saath agent interactions ko user-friendly banayein, khaas kar handoff events ke liye.
6. **Testing**:
   - Jupyter Notebooks mein Agent as Tools workflows test karein quick prototyping ke liye.
   - Production ke liye Chainlit ya server-based setup use karein.

---

## Conclusion

OpenAI Agents SDK mein Agent as Tools ka istemal is liye kiya jata hai kyun ke yeh multi-agent systems ko modular, scalable, aur collaborative banata hai. Specialized agents ko tools ke tor par use karne se complex tasks ko smaller, manageable parts mein tohra jata hai, aur LLMs ke limitations ko overcome kiya jata hai. Code examples se wazeh hai ke Agent as Tools simple translations se lekar complex multi-agent workflows tak sab mein kaam aata hai. Chainlit jaisi frameworks ke saath, yeh feature user-friendly aur real-time applications ke liye aur bhi powerful ho jata hai.

Ziyada details ke liye, OpenAI Agents SDK documentation dekhein at [openai.github.io](https://openai.github.io) aur Chainlit documentation at [chainlit.io](https://chainlit.io).
