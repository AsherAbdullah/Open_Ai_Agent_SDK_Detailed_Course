# Lecture: Context Management in OpenAI Agents SDK (Roman English)

## Introduction

OpenAI Agents SDK ek powerful framework hai jo developers ko agentic AI applications banane ki ijazat deta hai. Yeh agents large language models (LLMs) ke sath mil kar kaam karte hain aur tools, handoffs, aur guardrails ke zariye complex workflows ko handle karte hain. Lekin in sab ke markaz mein ek ahem tasawwur hai: **Context Management**.

**Context Management** se murad woh amal hai jis ke zariye agents apne workflow ke dauraan maloomat ko barqarar rakhte hain aur share karte hain. Yeh agents ke darmiyan data aur state ko manage karne ke liye istemaal hota hai taake woh zyada intelligent aur relevant faislay kar saken. Is lecture mein hum dekhenge ke OpenAI Agents SDK mein context management kaise kaam karta hai, is ki ahmiyat kya hai, aur isay code ke sath kaise naafiz kiya jata hai.

---

## Context Management Kya Hai? (What is Context Management?)

Context Management ek agent ke workflow ke dauraan istemaal hone wali maloomat ka intezam karne ka amal hai. Yeh maloomat agents ke darmiyan share ki ja sakti hai, jaise ke user ka input, pichhli guftagu, ya koi khaas data jo agent ke faislon ke liye zaroori ho. OpenAI Agents SDK mein, context ko **RunContextWrapper** class ke zariye handle kiya jata hai, jo agent ke run ke dauraan data ko manage karti hai.

**Ahem Nuktaat**:
- **Context** ek data structure hai jo agent ke run ke dauraan istemaal hone wali maloomat ko store karta hai.
- Yeh agents, tools, aur handoffs ke darmiyan data ke bahao ko yaqeeni banata hai.
- Context ko customize kiya ja sakta hai taake application ki makhsoos zarooriyaat ko poora kiya ja sake.

Misal ke taur par, agar aap ek aisi application bana rahe hain jo user ke pichhle sawalat ya jawabat ko yaad rakhe aur un ke mutabiq jawab de, to context management is kaam ko asaan karta hai.

---

## Context Management Ki Ahmiyat (Importance of Context Management)

Context Management OpenAI Agents SDK mein is liye zaroori hai kyun ke:

1. **State Persistence**: Yeh agents ko apne state (halat) ko barqarar rakhne ki ijazat deta hai, taake woh pichhle interactions ko yaad rakh saken.
2. **Data Sharing**: Multiple agents ya tools ke darmiyan data ko share karne ka ek standardized tariqa faraham karta hai.
3. **Workflow Efficiency**: Context ke zariye workflows ko zyada streamlined aur efficient banaya ja sakta hai.
4. **User Experience**: User ke liye personalized aur relevant responses generate karne mein madad karta hai.

Bina context management ke, agents har interaction ko naye siray se shuru karenge, jo ke user experience ko kharab kar sakta hai.

---

## OpenAI Agents SDK Mein Context Kaise Kaam Karta Hai?

OpenAI Agents SDK mein context ko **RunContextWrapper** class ke zariye manage kiya jata hai. Yeh class ek run ke dauraan tamam zaroori maloomat ko encapsulate karti hai, jaise ke:

- **User Input**: User ka diya hua input ya query.
- **Session Data**: Pichhle interactions ka data, jaise ke conversation history.
- **Agent State**: Agent ka current state ya configuration.
- **Tool Outputs**: Tools ke outputs jo workflow ke dauraan generate hue.

Yeh class ek flexible aur extensible tariqe se context ko store aur retrieve karne ki ijazat deti hai.

### Context Management Ke Components
1. **RunContextWrapper**: Yeh core class hai jo context ko manage karti hai.
2. **Context Store**: Ek dictionary ya object jo key-value pairs mein data store karta hai.
3. **Handoffs**: Jab ek agent doosre agent ko control deta hai, context bhi transfer hota hai.
4. **Guardrails**: Context ke zariye guardrails ko enforce kiya ja sakta hai taake agent specific boundaries ke andar kaam kare.

---

## Code Example: Context Management in Action

Ab hum ek practical code example dekhenge jo dikhata hai ke OpenAI Agents SDK mein context management kaise implement kiya jata hai. Is example mein, hum ek simple chatbot banayenge jo user ke pichhle inputs ko yaad rakhta hai aur un ke mutabiq jawab deta hai.

### Prerequisites
- OpenAI Agents SDK installed hona chahiye (`pip install openai-agents-sdk`).
- OpenAI API key set karni hogi environment variable mein (`OPENAI_API_KEY`).

### Code

<xaiArtifact artifact_id="597a7666-027e-4e64-8155-569bdc53fdc3" artifact_version_id="c2987134-4e4d-4bcd-b006-4c34bc0605f7" title="context_management_chatbot.py" contentType="text/python">
import os
from openai_agents_sdk import RunContextWrapper, Agent

# API Key Setup
os.environ["OPENAI_API_KEY"] = "your-api-key-here"

# Initialize Agent
agent = Agent(model="gpt-4", api_key=os.environ["OPENAI_API_KEY"])

# Function to handle user interaction with context
def chatbot_with_context(user_input, context_wrapper=None):
    if context_wrapper is None:
        # Create a new context if none exists
        context_wrapper = RunContextWrapper()
    
    # Add user input to context
    context_wrapper.add_to_context("user_input", user_input)
    
    # Retrieve previous conversation history (if any)
    conversation_history = context_wrapper.get_from_context("conversation_history", default=[])
    
    # Append current input to history
    conversation_history.append({"role": "user", "content": user_input})
    
    # Generate response using agent
    response = agent.generate_response(
        prompt=f"Answer the user based on this history: {conversation_history}",
        context=context_wrapper
    )
    
    # Append agent response to history
    conversation_history.append({"role": "assistant", "content": response})
    
    # Update context with new conversation history
    context_wrapper.update_context("conversation_history", conversation_history)
    
    return response, context_wrapper

# Example Usage
def main():
    context = None  # Initialize context as None
    print("Chatbot started. Type 'exit' to quit.")
    
    while True:
        user_input = input("You: ")
        if user_input.lower() == "exit":
            break
        
        response, context = chatbot_with_context(user_input, context)
        print(f"Bot: {response}")
    
    # Save context for future use (optional)
    context.save_to_file("chat_context.json")

if __name__ == "__main__":
    main()
