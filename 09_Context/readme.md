# Lecture: OpenAI Agents SDK mein Context Management samajhna

## Shuruaat
OpenAI Agents SDK ek aisa tool hai jo aapko AI programs banane mein madad deta hai, jo ek team ki tarah kaam karte hain. Is mein ek zaroori feature hai **context management**, jo aapko apne program ke mukhtalif hisson ke beech information share karne deta hai, jaise user ka naam ya ID. Yeh lecture batayega ke context kya hai, iska faida kya hai, aur isko code ke saath kaise use karte hain.

Is lecture ke baad aap:
- Jaan jayenge ke context kya hota hai OpenAI Agents SDK mein.
- Seekhenge ke context object kaise banate aur use karte hain.
- Code ke examples dekhenge jo context ko dikhate hain.
- Samjhenge ke context ko theek se kaise istemal karna hai.

Yeh lecture OpenAI Agents SDK ke context management ke guide par mabni hai.

---

## Context Kya Hai?

OpenAI Agents SDK mein **context** ek aisi cheez hai jo aap banate hain aur apne program ke hisson, jaise tools ya agents, ke saath share karte hain. Isay sochiye ek note ki tarah jo aap apne program mein pass karte hain taake sab kuch organized rahe, masalan user ka naam ya ID.

### Context Ke Baare Mein Zaroori Baatein
1. **AI Model Ke Liye Nahi**: Context aapke program ke code ke liye hai, na ke AI ke dimagh (jisko large language model ya LLM kehte hain) ke liye. AI context ko nahi dekhta jab tak aap usay baat mein shamil na karen.
2. **Flexible Hai**: Aap koi bhi Python object context ke liye use kar sakte hain, jaise dictionary ya custom data.
3. **Ek Hi Type**: Aapke program ke sab hisse ek hi type ka context use karenge taake koi galti na ho.
4. **Istemal**: Context kaam aata hai:
   - User ki details, jaise naam ya ID, store karne ke liye.
   - Tools share karne ke liye, jaise logger ya database connection.
   - Information ko program ke mukhtalif hisson tak pohnchane ke liye.

### Context Aur AI Ki Baat Cheet
AI sirf woh messages dekhta hai jo aap usay dete hain, jaise instructions ya sawal. Agar aap chahte hain ke AI ko koi cheez pata ho (jaise user ka naam), toh aapko woh messages mein daalna hoga. Context sirf aapke code ke liye hai, jaise data nikalna ya cheezon ko track karna.

---

## Context Kaise Kaam Karta Hai

Context ko **RunContextWrapper** naam ke ek wrapper ke zariye share kiya jata hai. Aap apna context `Runner.run` method mein dete hain, aur yeh usay aapke tools aur doosre hisson tak pohnchata hai.

### Zaroori Hissay
1. **RunContextWrapper**: Yeh aapka context rakhta hai aur tools ko us tak pohnchne deta hai.
2. **Runner.run**: Yeh woh method hai jahan aap context dete hain taake program start ho.
3. **Type Safety**: SDK check karta hai ke sab hisse ek hi context type use karen taake galti na ho.

### Kaise Chalta Hai
1. Aap ek context object banate hain (jaise user ka naam aur ID).
2. Isay `Runner.run` mein dete hain.
3. Aapke tools aur doosre hisse context ko `RunContextWrapper` ke zariye lete hain aur use karte hain.
4. Aap context ko parh sakte hain ya badal sakte hain jab zaroorat ho.

---

## Misal: Context Se User Ki Jaankari Share Karna

Chaliye ek simple program banate hain jahan ek AI assistant context se user ka naam le aur uski umar bataye. Hum Python code se dikhayenge ke yeh kaise kaam karta hai.

### Step 1: Context Object Banayein
Hum ek simple data structure banayenge jo user ka naam aur ID rakhega, Python ke `dataclass` se.

```python
from dataclasses import dataclass

@dataclass
class UserInfo:
    name: str  # User ka naam, jaise "John"
    uid: int   # User ka ID, jaise 123
```

### Step 2: Ek Tool Banayein Jo Context Use Kare
Hum ek tool banayenge jisko `fetch_user_age` kehte hain, jo context se user ka naam le aur uski umar bataye. Is misal mein, hum ek fixed umar return karenge.

```python
from agents import RunContextWrapper, function_tool

@function_tool
async def fetch_user_age(wrapper: RunContextWrapper[UserInfo]) -> str:
    # Context se user ka naam lein
    user_name = wrapper.context.name
    # User ki umar ke saath message return karein
    return f"{user_name} ki umar 47 saal hai"
```

- `@function_tool` is function ko AI ke liye tool banata hai.
- Tool context ko `wrapper` se leta hai aur user ka naam use karta hai.

### Step 3: AI Agent Set Karein
Hum ek AI agent banayenge jo is tool ko use karega aur janta hai ke isay `UserInfo` context milega.

```python
from agents import Agent

agent = Agent[UserInfo](
    name="Assistant",
    instructions="Tum ek madadgar assistant ho jo user ki jaankari nikal sakta hai.",
    tools=[fetch_user_age],
)
```

- `[UserInfo]` batata hai ke agent ko `UserInfo` context chahiye.
- `tools` mein humara `fetch_user_age` tool hai.

### Step 4: Program Ko Context Ke Saath Chalayein
Hum agent ko context ke saath chalayenge aur user ki umar poochenge.

```python
from agents import Runner
import asyncio

async def main():
    # User ki details ke saath context banayein
    user_info = UserInfo(name="John", uid=123)
    
    # Agent ko context aur sawal ke saath chalayein
    result = await Runner.run(
        starting_agent=agent,
        input="User ki umar kya hai?",
        context=user_info,
    )
    
    # Jawab dikhayein
    print(result.final_output)

# Program chalayein
if __name__ == "__main__":
    asyncio.run(main())
```

### Expected Output
```
John ki umar 47 saal hai.
```

### Yeh Kaise Kaam Karta Hai
1. **Context**: `UserInfo` object mein user ka naam ("John") aur ID (123) hai.
2. **Tool**: `fetch_user_age` tool context se naam le aur umar batata hai.
3. **Agent**: Agent janta hai ke tool aur `UserInfo` context use karna hai.
4. **Runner**: `Runner.run` context ko tool tak pohnchata hai.

---

## Advanced Misal: Context Ke Saath Dynamic Instructions

Ab hum ek aur misal dekhenge jahan hum context se dynamic instructions banayenge, taake AI ke system prompt mein user ka naam shamil ho.

### Step 1: Context Aur Dynamic Instructions Banayein
Hum wohi `UserInfo` dataclass use karenge aur ek function banayenge jo context se instructions banaye.

```python
from dataclasses import dataclass
from agents import Agent, RunContextWrapper

@dataclass
class UserInfo:
    name: str
    uid: int

def dynamic_instructions(context: RunContextWrapper[UserInfo], agent: Agent[UserInfo]) -> str:
    return f"Tum {context.context.name} ke liye madadgar assistant ho. Unke sawalon ka jawab shistagi se do."
```

### Step 2: Dynamic Instructions Ke Saath Agent Banayein
Hum ek agent banayenge jo dynamic instructions aur ek simple tool use karega.

```python
@function_tool
async def greet_user(wrapper: RunContextWrapper[UserInfo]) -> str:
    return f"Salaam, {wrapper.context.name}! Mein aapki kya madad kar sakta hoon?"

agent = Agent[UserInfo](
    name="Assistant",
    instructions=dynamic_instructions,
    tools=[greet_user],
)
```

### Step 3: Agent Chalayein
Hum agent ko context ke saath chalayenge aur user se salaam kehne ko bolenge.

```python
async def main():
    user_info = UserInfo(name="Ayesha", uid=456)
    
    result = await Runner.run(
        starting_agent=agent,
        input="Mujhe salaam kaho.",
        context=user_info,
    )
    
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

### Expected Output
```
Salaam, Ayesha! Mein aapki kya madad kar sakta hoon?
```

### Yeh Kaise Kaam Karta Hai
- **Dynamic Instructions**: `dynamic_instructions` function context se user ka naam le aur AI ke liye personalized prompt banata hai.
- **Tool**: `greet_user` tool bhi context se naam le aur personalized salaam karta hai.
- **Context**: `user_info` context instructions aur tool dono ke liye use hota hai.

---

## Context Ke Liye Behtareen Tareeke

1. **Type Batayein**: Hamesha context ka type (jaise `Agent[UserInfo]`) batayein taake galti na ho.
2. **Context Halka Rakhein**: Zyada bhaari ya complex cheezon ko context mein na rakhein, warna program slow ho sakta hai.
3. **Dataclasses Ya Pydantic Use Karein**: Yeh context ko organized aur safe rakhte hain.
4. **AI Se Alag Rakhein**: Yaad rakhein ke context code ke liye hai, AI ke liye nahi. AI ko data dene ke liye instructions ya messages use karein.
5. **Sensitive Data Sambhalein**: Agar context mein user ka ID ya sensitive cheez hai, toh usay secure rakhein.

---

## Limitations Aur Dhyan Dene Wali Baatein

1. **Ek Type**: Ek program mein sab hisse ek hi context type use karenge, warna error hoga.
2. **AI Ko Nahi Jata**: Context AI ko nahi dikhta jab tak aap usay messages mein na daalein.
3. **Speed**: Agar context mein bhaari kaam hai (jaise database se data lena), toh usay fast karne ki koshish karein.
4. **Tracing**: Agar aap SDK ke tracing feature use karte hain, toh context ka data trace mein dikh sakta hai. Sensitive data ko chupane ke liye settings dekhein.

---

## Khatma

OpenAI Agents SDK mein context management ek zabardast feature hai jo aapko data share karne mein madad deta hai, jaise user ka naam ya ID, bina AI ke conversation ko complex kiye. Upar di gayi misalon se aapne dekha ke context object kaise banate hain, tools mein kaise use karte hain, aur dynamic instructions ke liye kaise istemal hota hai.

Zyada jaankari ke liye, OpenAI Agents SDK ke official guide dekhein (https://openai.github.io/openai-agents-python/). Mukhtalif context types (jaise dictionaries ya Pydantic models) try karein aur doosre features jaise guardrails ya tracing explore karein taake behtar AI programs bana sakein.
