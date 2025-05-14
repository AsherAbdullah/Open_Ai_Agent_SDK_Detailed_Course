# Lecture: LiteLLM Agent SDK with Google Gemini (Urdu-English)

## 1. LiteLLM aur Google Gemini ka Taaruf

### 1.1 LiteLLM Kya Hai?
LiteLLM ek free Python tool hai jo 100 se zyada Large Language Models (LLMs) jaise Google Gemini, OpenAI, aur dosre ke saath kaam ko asaan karta hai. Yeh ek hi tarah ka API deta hai sab models ke liye. Important baatein:

- **Ek Jaisa Tareeqa**: Sab models ke liye same code kaam karta hai.
- **Bohat Models**: Gemini, GPT-4, Claude, aur dosre ke saath kaam karta hai.
- **Tools**: Google Search ya code chalane ke tools use kar sakta hai.
- **Cost aur Errors**: Cost track karta hai aur errors ko achha handle karta hai.
- **Server Option**: Models ke liye middleman ban sakta hai.

LiteLLM AI agents banane mein madad karta hai by making model ka use simple.

### 1.2 Google Gemini Kya Hai?
Google Gemini Google ka ek smart models ka group hai. Yeh text, images, aur shayad dosre data ko samajh sakta hai aur bana sakta hai. Aap isse Google AI Studio ya Vertex AI ke through use kar sakte hain. Important baatein:

- **Multi-Talented**: Text, images, aur shayad audio/video ke saath kaam karta hai.
- **Smart Soch**: Mushkil problems solve karne mein acha hai.
- **Tools**: Google Search ya custom tools ke saath kaam karta hai.
- **Bade Projects**: Google Cloud ke saath bade systems ke liye perfect hai.

Gemini smart AI agents banane ke liye bohat acha hai.

### 1.3 LiteLLM aur Gemini Saath Kyun?
LiteLLM aur Gemini ek saath use karna faida deta hai kyunki:
- **Asaan Hai**: LiteLLM Gemini ke API ko simple karta hai.
- **Model Badlo**: Dosre models jaise Claude par switch karna asaan hai.
- **Tool Support**: Gemini ke tools ko OpenAI jaisa banata hai.
- **Paisa Bachao**: Gemini sasta hai, aur LiteLLM cost track karta hai.
- **Flexible**: Google AI Studio ya Vertex AI dono ke saath kaam karta hai.

Yeh smart aur sasta AI agents banane ke liye perfect hai.

## 2. LiteLLM Agent SDK ke Main Points

### 2.1 LiteLLM mein Agents
AI agent ek program hai jo LLM use karta hai to think, plan, aur user ke sawalon par act karta hai. LiteLLM ka koi special “Agent SDK” nahi, lekin yeh `completion` function aur tools ke saath agents banane ka tareeqa deta hai. Yeh Google ke Agent Development Kit (ADK) ke saath bhi kaam karta hai.

- **Agent ka Kaam**: User ka sawal leta hai, LLM use karta hai, aur tools call kar sakta hai.
- **LiteLLM ka Role**: Sawal LLM ko bhejta hai aur jawab lata hai.
- **Gemini ka Role**: Sochta hai aur sawal ko samajhta hai.

### 2.2 Main Cheezein
- **Completion Function**: `litellm.completion` sawal Gemini ko bhejta hai.
- **Tools**: Gemini ke tools jaise Google Search ya custom functions support karta hai.
- **Settings**: `GEMINI_API_KEY` jaisi environment variables use karta hai.
- **Safety**: Content ke liye safe rules set kar sakta hai.
- **Output Style**: Jawab ko JSON mein arrange kar sakta hai.

### 2.3 Google ADK ke Saath
Google ka ADK bohat agents ek saath banane mein madad karta hai. LiteLLM ADK ke saath `LiteLlm` wrapper use karta hai. Yeh lecture LiteLLM ke main tools par focus karta hai, lekin example mein ADK bhi dikhayenge.

## 3. LiteLLM aur Gemini Setup Karna

### 3.1 Kya Chahiye
- **Python**: Version 3.8 ya usse zyada.
- **LiteLLM**: `pip install litellm` se install karo.
- **Gemini API Key**: Google AI Studio (https://ai.google.dev/) se lo.
- **Setup**: `GEMINI_API_KEY` environment mein daalo.

### 3.2 LiteLLM Install Karo
```bash
pip install litellm
```

### 3.3 API Key Daalo
System mein key set karo:
```bash
export GEMINI_API_KEY="your-api-key-here"
```
Ya code mein key daal do (safe nahi hai).

## 4. LiteLLM aur Gemini se Simple Agent Banana

### 4.1 Basic Idea
Hum ek weather agent banayenge jo:
1. User ka sawal leta hai (maslan, “Boston ka mausam kaisa hai?”).
2. Gemini ke through LiteLLM se sawal samajhta hai.
3. Ek fake weather tool se data lata hai.
4. Clear jawab deta hai.

### 4.2 Steps
- **LiteLLM Start Karo**: `completion` ko Gemini model ke saath set karo.
- **Tools Banao**: Fake weather data ka function banao.
- **Sawal Handle Karo**: Sawal Gemini ko bhejo aur tools use karo.
- **Jawab Do**: Tool ke thirteenth ko user ke liye arrange karo.

## 5. Example Code: Weather Agent Concept

Yeh ek simple Python code hai jo weather agent dikhata hai LiteLLM aur Gemini ke saath. Yeh basic concept ke liye hai.

```python
import os
import json
from litellm import completion

# Gemini API key set karo
os.environ["GEMINI_API_KEY"] = "your-api-key-here"

# Fake weather tool (mausam ka data pretend karta hai)
def get_current_weather(location, unit="celsius"):
    # Example ke liye fake data
    weather_data = {
        "Boston, MA": {"temperature": 15, "condition": "Cloudy"},
        "San Francisco, CA": {"temperature": 20, "condition": "Sunny"}
    }
    location = location.strip()
    if location in weather_data:
        return {
            "location": location,
            "temperature": weather_data[location]["temperature"],
            "condition": weather_data[location]["condition"],
            "unit": unit
        }
    return {"error": f"{location} ka mausam data nahi hai"}

# Agent function banao
def weather_agent(query):
    # Gemini ko tool ke bare mein batao
    tools = [
        {
            "type": "function",
            "function": {
                "name": "get_current_weather",
                "description": "Kisi jagah ka current mausam pata karo",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "City aur state, jaise Boston, MA"
                        },
                        "unit": {
                            "type": "string",
                            "enum": ["celsius", "fahrenheit"],
                            "description": "Temperature ka unit"
                        }
                    },
                    "required": ["location"]
                }
            }
        }
    ]

    # Sawal Gemini ko bhejo
    response = completion(
        model="gemini/gemini-1.5-flash",
        messages=[{"role": "user", "content": query}],
        tools=tools
    )

    # Jawab check karo
    choice = response.choices[0]
    if hasattr(choice.message, "tool_calls") and choice.message.tool_calls:
        tool_call = choice.message.tool_calls[0]
        if tool_call.function.name == "get_current_weather":
            # Tool ke details lo
            args = json.loads(tool_call.function.arguments)
            # Weather tool use karo
            weather_data = get_current_weather(
                location=args["location"],
                unit=args.get("unit", "celsius")
            )
            # Jawab tayyar karo
            if "error" in weather_data:
                return weather_data["error"]
            return (f"{weather_data['location']} ka mausam "
                    f"{weather_data['condition']} hai aur temperature "
                    f"{weather_data['temperature']}°{weather_data['unit'][0].upper()} hai.")
    else:
        # Agar tool nahi, to Gemini ka jawab do
        return choice.message.content

# Agent try karo
if __name__ == "__main__":
    query = "Boston, MA ka mausam kaisa hai aaj?"
    print(f"Sawal: {query}")
    response = weather_agent(query)
    print(f"Jawab: {response}")
```

### 5.1 Code Ki Samajh
- **Setup**: `GEMINI_API_KEY` Gemini ke liye lagata hai.
- **Fake Tool**: `get_current_weather` kuch cities ke liye fake mausam data deta hai.
- **Tool Info**: Gemini ko batata hai weather tool kaise kaam karta hai.
- **Agent Ka Kaam**:
  - Sawal `litellm.completion` se Gemini ko bhejta hai.
  - Dekhta hai agar Gemini tool use karna chahta hai.
  - Agar haan, to `get_current_weather` ko sahi details ke saath chalata hai.
  - Tool ke data se clear jawab banata hai.
- **Backup**: Agar tool nahi chahiye, to Gemini ka seedha jawab deta hai.

### 5.2 Sample Output
Sawal “Boston, MA ka mausam kaisa hai aaj?” ke liye, output yeh ho sakta hai:
```
Sawal: Boston, MA ka mausam kaisa hai aaj?
Jawab: Boston, MA ka mausam Cloudy hai aur temperature 15°C hai.
```

## 6. Zyada Features aur Uses

### 6.1 Images wale Agents
Gemini images aur videos ke saath kaam kar sakta hai. LiteLLM aapko image URLs (jaise Google Cloud se) bhejne deta hai in tasks ke liye.

### 6.2 ADK ke Saath
Is agent ko Google ADK ke saath use karne ke liye, model ko is tarah wrap karo:
```python
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm

agent = Agent(
    model=LiteLlm(model="gemini/gemini-1.5-flash"),
    name="weather_agent",
    instruction="Mausam ki info do using get_current_weather tool."
)
```

### 6.3 Asli Duniya ke Uses
- **Customer Help**: Live data ke saath sawalon ke jawab do.
- **Travel Plan**: Mausam, flights, aur activities check karo.
- **Data Dekho**: Databases ya APIs se data ke liye tools use karo.

## 7. Behtar Tareeqe

- **Errors Handle Karo**: API problems ke liye try-catch use karo.
- **Cost Dekho**: LiteLLM se Gemini ka cost track karo.
- **Safe Raho**: API keys ko environment variables mein rakho.
- **Test Karo**: Tool outputs aur jawab formats check karo.
- **Bada Karo**: Bade projects ke liye Docker ya Google Cloud use karo.

## 8. Khatam

LiteLLM aur Google Gemini ek zabardast aur asaan tareeqa hai AI agents banane ka. LiteLLM model ka use simple karta hai, aur Gemini smart soch aur tool support deta hai. Upar ka example ek basic weather agent dikhata hai, lekin aap isse bade kaam ke liye use kar sakte hain. Yeh dono ek saath bohat se kaamon ke liye smart agents banane mein madad karte hain.

## 9. References
- LiteLLM Guide: https://docs.litellm.ai/
- Google Gemini API: https://ai.google.dev/
- Google ADK: https://google.github.io/adk/
