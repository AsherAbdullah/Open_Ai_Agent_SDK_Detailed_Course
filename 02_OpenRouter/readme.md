# Lecture: OpenRouter aur OpenAI Agents SDK ka Istemaal AI Agent Development ke Liye (Urdu-English)

## Mukadma (Introduction)
AI ke tezi se badalte duniya mein, developers ko aise tools ki zarurat hai jo multiple large language models (LLMs) tak asaan rasai dein aur intelligent, kaam-karney waley agents banayein. **OpenRouter** ek unified API hai jo OpenAI, Anthropic, Google, aur Meta jaise providers ke models tak ek hi endpoint se rasai deta hai, sath hi sasti pricing aur seamless integration bhi. **OpenAI Agents SDK** ek Python-based framework hai jo agentic AI systems banata hai jo tasks perform kar sakte hain, tools use karte hain, aur collaboration ke liye handoffs karte hain. Is lecture mein hum dekhenge ke OpenRouter ke model access ko OpenAI Agents SDK ke sath kaise combine kiya jaye taake powerful AI applications banaye ja sakain.

**Lecture ke Maqsad (Goals)**:
- OpenRouter ke API aur iski OpenAI SDK ke sath compatibility samajhna.
- OpenAI Agents SDK ke core components (Agents, Tools, Handoffs, Guardrails, Tracing) seekhna.
- Python mein practical coding examples banakar hands-on implementation karna.
- Real-world use cases aur best practices explore karna.

---

## Hissa 1: OpenRouter ko Samajhna (Understanding OpenRouter)
OpenRouter ek unified API gateway hai jo developers ko ek hi API key ke zariye kai LLMs tak rasai deta hai. Ye requests aur responses ko providers ke beech normalize karta hai, OpenAI-compatible APIs support karta hai, aur features jaise automatic fallbacks, cost optimization, aur streaming bhi deta hai. Iske faide shamil hain:
- **Universal Model Access**: Ek API se OpenAI, Anthropic, Google, Meta aur dosre models use karein.
- **Sasti Pricing**: Pay-as-you-go model ke sath per-token costs transparent hain.
- **High Availability**: Automatic failover se reliability barh jati hai.
- **Asaan Integration**: OpenAI SDK aur dosre frameworks jaise LangChain aur Vercel AI SDK ke sath kaam karta hai.

### OpenRouter Setup Karna
1. **Account Banayein**: [openrouter.ai](https://openrouter.ai) par sign up karein aur dashboard se API key hasil karein.
2. **Environment Variables Set Karein**: API key ko securely `.env` file mein store karein.
3. **Dependencies Install Karein**: OpenAI Python SDK install karein, jo OpenRouter ke sath compatible hai.

**Misaal: Environment Setup**
```bash
# Packages install karein
pip install openai python-dotenv
```

**`.env` file banayein**:
```plaintext
OPENROUTER_API_KEY=your-openrouter-api-key
```

**Environment Variables Load Karein**:
```python
import os
from dotenv import load_dotenv

load_dotenv()
OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY")
if not OPENROUTER_API_KEY:
    raise ValueError("OPENROUTER_API_KEY .env file mein nahi mila")
```

---

## Hissa 2: OpenAI Agents SDK ko Samajhna (Understanding OpenAI Agents SDK)
OpenAI Agents SDK ek Python library hai jo AI agents banane ke liye design ki gayi hai. Ye agents tasks khud perform kar sakte hain, external tools use karte hain, tasks delegate karte hain, aur safe operation ensure karte hain. Ye OpenAI ke Swarm framework ka upgraded version hai, jo production ke liye optimized hai. Iske core components hain:
- **Agents**: LLMs jo specific instructions aur roles ke sath configure kiye jate hain.
- **Tools**: External functions (maslan, web search, calculations) jo agents call kar sakte hain.
- **Handoffs**: Agents ke liye mechanism jo tasks dosre specialized agents ko delegate karte hain.
- **Guardrails**: Inputs aur outputs ke liye validation layers.
- **Tracing**: Debugging aur performance analysis ke liye logs.

### OpenAI Agents SDK Setup Karna
1. **SDK Install Karein**:
```bash
pip install openai-agents python-dotenv
```
2. **OpenAI API Key Set Karein**: Halanke hum OpenRouter use karenge, SDK ko compatibility ke liye `OPENAI_API_KEY` environment variable chahiye.

**Note**: OpenRouter ka API key use hoga, lekin hum SDK ko OpenRouter ke endpoint (`https://openrouter.ai/api/v1`) par route karenge.

---

## Hissa 3: OpenRouter ko OpenAI Agents SDK ke Sath Integrate Karna
OpenRouter ke models ko OpenAI Agents SDK ke sath use karne ke liye, hum OpenAI client ko OpenRouter ke base URL aur API key ke sath configure karte hain. Isse hum OpenRouter ke model diversity ka faida utha sakte hain aur agentic workflows bana sakte hain.

### Misaal 1: Ek Sada Agent Banayein OpenRouter ke Sath
Hum ek agent banayenge jo math ke sawalon ka jawab deta hai, aur iske liye Anthropic ka Claude model OpenRouter ke zariye use karenge.

**Code Misaal**:
```python
import os
from dotenv import load_dotenv
from openai import OpenAI
from agents import Agent, Runner

# Environment variables load karein
load_dotenv()
OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY")
if not OPENROUTER_API_KEY:
    raise ValueError("OPENROUTER_API_KEY nahi mila")

# OpenAI client ko OpenRouter ke liye configure karein
client = OpenAI(
    api_key=OPENROUTER_API_KEY,
    base_url="https://openrouter.ai/api/v1",
    default_headers={
        "HTTP-Referer": "https://your-site.com",
        "X-Title": "Math Agent Demo"
    }
)

# Math tutor agent define karein
math_agent = Agent(
    name="Math Tutor",
    instructions="Aap ek math tutor hain. Problems ko step-by-step solve karein aur wazahat karein.",
    model="anthropic/claude-3.5-sonnet"  # Claude model OpenRouter se
)

# Agent ko run karein
result = Runner.run_sync(
    agent=math_agent,
    prompt="2x + 3 = 11 ko x ke liye solve karein."
)

# Result print karein
print(result.final_output)
```

**Tawaqo ka Output**:
```
Equation 2x + 3 = 11 ko x ke liye solve karne ke steps:

1. Dono taraf se 3 subtract karein taake x ka term alag ho:
   2x + 3 - 3 = 11 - 3
   2x = 8

2. Dono taraf ko 2 se divide karein taake x mile:
   2x / 2 = 8 / 2
   x = 4

Isliye, jawab hai x = 4.
```

**Wazahat (Explanation)**:
- OpenAI client ko OpenRouter ke API endpoint aur key ke sath configure kiya gaya.
- `Agent` ko clear instructions ke sath setup kiya gaya aur Claude 3.5 Sonnet model use kiya.
- `Runner.run_sync` method user ka prompt process karta hai aur agent ka jawab deta hai.
- OpenRouter ke headers (`HTTP-Referer`, `X-Title`) optional hain lekin leaderboard visibility ke liye madadgar hain.

---

### Misaal 2: Tools ke Sath Agent ka Istemaal
Ab hum agent ko extend karenge taake wo complex calculations ke liye calculator tool use kare. OpenAI Agents SDK agents ko external functions dynamically call karne deta hai.

**Code Misaal**:
```python
import os
from dotenv import load_dotenv
from openai import OpenAI
from agents import Agent, Runner, Tool

# Environment variables load karein
load_dotenv()
OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY")
if not OPENROUTER_API_KEYmiss_value = os.getenv("OPENROUTER_API_KEY")
if not OPENROUTER_API_KEY:
    raise ValueError("OPENROUTER_API_KEY nahi mila")

# OpenAI client ko OpenRouter ke liye configure karein
client = OpenAI(
    api_key=OPENROUTER_API_KEY,
    base_url="https://openrouter.ai/api/v1",
    default_headers={
        "HTTP-Referer": "https://your-site.com",
        "X-Title": "Math Agent with Tools"
    }
)

# Calculator tool define karein
def calculate(expression: str) -> float:
    """Mathematical expressions ko evaluate karta hai jaise '2 + 3 * 4'."""
    try:
        return eval(expression, {"__builtins__": {}}, {})
    except Exception as e:
        return f"Error: {str(e)}"

calculator_tool = Tool(
    name="Calculator",
    description="Mathematical expressions ko evaluate karta hai jaise '2 + 3 * 4'.",
    function=calculate
)

# Math tutor agent ko calculator tool ke sath define karein
math_agent = Agent(
    name="Math Tutor",
    instructions="Aap ek math tutor hain. Complex calculations ke liye calculator tool use karein aur steps clearly wazeh karein.",
    model="openai/gpt-4o",  # GPT-4o model OpenRouter se
    tools=[calculator_tool]
)

# Agent ko run karein
result = Runner.run_sync(
    agent=math_agent,
    prompt="5 + 3 * 4^2 ki value kya hai?"
)

# Result print karein
print(result.final_output)
```

**Tawaqo ka Output**:
```
5 + 3 * 4^2 ko solve karne ke liye ye steps follow karein:

1. Exponent solve karein: 4^2 = 16.
2. Multiply karein: 3 * 16 = 48.
3. Add karein: 5 + 48 = 53.

Calculator tool se confirm karein:
Expression: 5 + 3 * 16
Result: 53

Value hai 53.
```

**Wazahat (Explanation)**:
- `Tool` class ek calculator function define karta hai jo expressions ko safely evaluate karta hai.
- Agent ko GPT-4o (OpenRouter ke zariye) aur calculator tool ke sath configure kiya gaya.
- Agent tool ko expression compute karne ke liye use karta hai aur steps wazeh karta hai.
- OpenRouter request ko OpenAI ke GPT-4o model tak seamlessly route karta hai.

---

### Misaal 3: Multi-Agent System Handoffs ke Sath
Ab hum ek multi-agent system banayenge jahan math agent historical math sawalon ko history agent ko handoff karta hai. Ye SDK ke handoff feature ko dikhata hai.

**Code Misaal**:
```python
import os
from dotenv import load_dotenv
from openai import OpenAI
from agents import Agent, Runner

# Environment variables load karein
load_dotenv()
OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY")
if not OPENROUTER_API_KEY:
    raise ValueError("OPENROUTER_API_KEY nahi mila")

# OpenAI client ko OpenRouter ke liye configure karein
client = OpenAI(
    api_key=OPENROUTER_API_KEY,
    base_url="https://openrouter.ai/api/v1",
    default_headers={
        "HTTP-Referer": "https://your-site.com",
        "X-Title": "Multi-Agent Demo"
    }
)

# Math tutor agent define karein
math_agent = Agent(
    name="Math Tutor",
    instructions="Aap ek math tutor hain. Math problems ko step-by-step solve karein. Historical math sawalon ke liye History Tutor ko handoff karein.",
    model="openai/gpt-4o",
    handoff_descriptions=["Historical math sawalon ke liye specialist agent"]
)

# History tutor agent define karein
history_agent = Agent(
    name="History Tutor",
    instructions="Aap ek history tutor hain. Mathematics ki tareekh ke sawalon ka jawab context aur misaalon ke sath dein.",
    model="anthropic/claude-3.5-sonnet",
    handoff_description="Historical math sawalon ke liye specialist agent"
)

# Multi-agent system ko run karein
result = Runner.run_sync(
    agent=math_agent,
    prompt="Calculus kisne invent kiya aur ye 2x + 3 = 11 solve karne se kaise relate karta hai?"
)

# Result print karein
print(result.final_output)
```

**Tawaqo ka Output**:
```
Ye sawal mathematics ki tareekh se hai. History Tutor ko handoff kar raha hoon.

**History Tutor ka Jawab**:
Calculus ko Sir Isaac Newton aur Gottfried Wilhelm Leibniz ne 17th century ke akhir mein independently develop kiya. Ye mathematics ka wo hissa hai jo rates of change (differentiation) aur quantities ke accumulation (integration) se deal karta hai.

Equation 2x + 3 = 11 ke liye, calculus directly zaroori nahi, lekin linear equations solve karne ka concept un mathematical developments se hai jo calculus ke liye rasta banaye. Solve karne ke liye:
1. 3 subtract karein: 2x = 8
2. 2 se divide karein: x = 4

Calculus is algebraic techniques ko dynamic systems tak le jata hai, jaise curves ke slopes ya curves ke neeche areas nikalna.
```

**Wazahat (Explanation)**:
- Math agent historical sawal ko pehchanta hai aur history agent ko handoff karta hai.
- History agent Claude 3.5 Sonnet (OpenRouter ke zariye) use karta hai taake detailed jawab de.
- SDK ka tracing feature (OpenAI Dashboard mein dekha ja sakta hai) handoff ko debug karne ke liye log karta hai.
- OpenRouter har agent ke liye munasib provider ko route karta hai.

---

## Hissa 4: Best Practices aur Real-World Use Cases
### Best Practices
1. **API Keys Secure Karein**: Hamesha API keys ko environment variables mein store karein. OpenRouter exposed keys ko scan karta hai aur users ko notify karta hai agar compromised ho.
2. **Model Selection**: Task complexity ke mutabiq models chunein (maslan, GPT-4o reasoning ke liye, Claude Haiku sasti pricing ke liye). OpenRouter ke model list check karein.
3. **Error Handling**: API errors ya rate limits ke liye try-catch blocks implement karein.
4. **Tracing**: OpenAI Dashboard mein agent workflows monitor aur handoffs debug karein.
5. **Cost Management**: OpenRouter ke dashboard mein usage monitor karein taake costs optimize ho.
6. **Guardrails**: Inputs aur outputs ko validate karein taake safety aur compliance ensure ho, khas tor par user-facing apps ke liye.

### Real-World Use Cases
1. **Taleemi Chatbots**: Math, history, ya science ke tutors banayein, OpenRouter ke saste models aur Agents SDK ke task delegation ka faida uthate hue.
2. **Customer Support**: FAQs handle karne wale agents banayein, complex queries ko specialized agents ko escalate karein, aur CRM APIs jaise tools integrate karein.
3. **Content Generation**: Blog posts, scripts, ya code generate karne wali apps banayein, quality vs. cost ke liye models switch karein.
4. **Research Assistants**: Papers summarize karne, tools se data fetch karne, aur analysis par collaborate karne wale agents banayein, OpenRouter ke model diversity ka faida uthate hue.

---

## Hissa 5: Nateeja (Conclusion)
OpenRouter aur OpenAI Agents SDK AI applications banane ke liye ek zabardast combination hain. OpenRouter diverse LLMs tak asaan rasai deta hai, jabke Agents SDK intelligent, collaborative agents banane deta hai. In tools ko integrate karke, developers scalable, cost-effective, aur robust AI systems bana sakte hain.

**Agla Qadam (Next Steps)**:
- OpenRouter ke model list explore karein aur mukhtalif providers ke sath experiment karein.
- OpenAI Agents SDK documentation mein advanced features jaise custom guardrails seekhein.
- Multiple agents, tools, aur OpenRouter models ke sath ek project banayein.
- Performance optimize karne ke liye OpenAI Dashboard mein traces monitor karein.

**Resources**:
- OpenRouter Documentation: [openrouter.ai](https://openrouter.ai)
- OpenAI Agents SDK: [openai.github.io](https://openai.github.io)
- GitHub Examples: [OpenRouterTeam/openrouter-examples](https://github.com/OpenRouterTeam/openrouter-examples)

---
