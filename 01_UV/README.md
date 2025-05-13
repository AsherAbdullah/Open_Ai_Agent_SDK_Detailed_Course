## 🎓 Lecture Title: UV (User Variables) in OpenAI Agent SDK — Full Guide

---

### 🔰 1. Introduction: UV kya hota hai?

**UV ka matlab hota hai "User Variables".**  
Ye variables woh info store kartay hain jo aik user ne agent ko di hoti hai, taake agent personalize response de sakay.

**Example:**
User bole: `"Mujhe Ali bulao"`  
Agent: `session.remember("name", "Ali")`  
Agle message mein user bole: `"Mera naam kya hai?"`  
Agent: `"Aapka naam Ali hai."`

---

## 🛣️ 2. Roadmap: UV Seekhnay ka Tareeqa (Scratch to Advanced)

| Step | Topic | Detail |
|------|-------|--------|
| 1 | Requirements | Python, OpenAI Key |
| 2 | SDK Install karna | `pip install openai-agents` |
| 3 | Project Initialize karna | `openai agents init` |
| 4 | Basic UV Store/Recall | `.remember()` & `.recall()` |
| 5 | Use in Conversation | Personalized logic |
| 6 | Persist UVs | Redis ya DB mein |
| 7 | Deploy | Local ya Cloud deployment |

---

## ⚙️ 3. Installation

### Requirements:
- Python 3.9+
- OpenAI API Key

```bash
pip install openai-agents
```

Project start karne ke liye:
```bash
openai agents init my-agent
cd my-agent
```

`agent.yaml` file mein likho:
```yaml
name: UVAgent
instructions: |
  Tum aik smart AI ho jo user ki info yaad rakhta hai.
```

---

## 💡 4. Basic Example: UV kaise kaam karta hai

### ✅ Naam Save Karna (Remember)

```python
@agent.on_message
async def handle_message(message, session):
    if "mera naam" in message.content:
        name = message.content.split()[-1]
        await session.remember("name", name)
        return f"Theek hai, ab se mein aapko {name} bulaoonga!"
```

### ✅ Naam Recall Karna

```python
@agent.on_message
async def handle_message(message, session):
    if "mera naam kya hai" in message.content:
        name = await session.recall("name")
        if name:
            return f"Aapka naam {name} hai."
        else:
            return "Mujhe aapka naam nahi yaad."
```

---

## 🧠 5. Multiple User Variables ka Use

Aap ek se zyada cheezein yaad rakh saktay ho:

```python
await session.remember("city", "Lahore")
await session.remember("age", "25")
```

Recall karne ke liye:

```python
city = await session.recall("city")
```

**Use Case:**  
User bole: `"Mujhe Lahore ka weather batao"`  
Agent: pehle city recall karega aur us hisaab se weather dikhayega.

---

## 📦 6. Persistent Storage (UV yaad rakhna sessions ke baad bhi)

Agar aap chahte ho ke UVs session close hone ke baad bhi save rahein, to Redis use karo.

### Redis install:

```bash
pip install redis
```

Then config update in `agent.yaml`:

```yaml
memory:
  provider: redis
  url: redis://localhost:6379
```

Ab agent user ki info ko Redis mein store karega — yani **long-term yaad rakhne ke liye**.

---

## 🤖 7. Advanced: Logic with UVs

```python
language = await session.recall("preferred_language")

if language == "Urdu":
    return "Mein Urdu mein jawab doon ga."
else:
    return "Replying in English!"
```

---

## 🚀 8. Deploy karna (optional)

Test locally:

```bash
openai agents dev
```

Deploy options:
- Cloud (Vercel, Render)
- FastAPI backend
- Frontend with React, Next.js

---

## 📚 Final Summary Table

| Feature | Description |
|---------|-------------|
| UV kya hai? | User ki info store karne wala system |
| Store UV | `session.remember("key", "value")` |
| Recall UV | `session.recall("key")` |
| Persistent? | Redis ya DB se possible |
| Use Cases | Personalized bots, Memory agents |

---

## 📘 Final Code (Simple Working Bot)

```python
@agent.on_message
async def handle_message(message, session):
    if "mera naam" in message.content:
        name = message.content.split()[-1]
        await session.remember("name", name)
        return f"Shukriya, ab se mein aapko {name} bulaoonga."

    elif "mera naam kya hai" in message.content:
        name = await session.recall("name")
        if name:
            return f"Aapka naam {name} hai."
        else:
            return "Mujhe abhi tak aapka naam nahi pata."

    else:
        return "Aap kya poochna chahte hain?"
```

---

Agar aap chahte hain:
- Mein is guide ko **PDF mein export** kar doon?
- Ya aapko **GitHub par sample code upload** kar ke doon?

Just bata dein!
