# Lecture: Understanding Context Management in the OpenAI Agents SDK

## Introduction
The OpenAI Agents SDK is a lightweight, powerful framework designed to build multi-agent AI workflows. One of its key features is **context management**, which allows developers to pass and manage data across agents, tools, and lifecycle hooks during an agent’s execution. This lecture dives into the concept of context management, its importance, how it works, and practical examples of implementing it using Python.

By the end of this lecture, you will:
- Understand what context means in the OpenAI Agents SDK.
- Learn how to define and pass context objects to agents and tools.
- Explore practical code examples demonstrating context management.
- Understand best practices and limitations of context management.

This lecture is based on the official documentation from the OpenAI Agents SDK, specifically the context management section.

---

## What is Context in the OpenAI Agents SDK?

In the OpenAI Agents SDK, **context** refers to a user-defined Python object that is passed to the `Runner.run` method and made available to agents, tools, lifecycle hooks, and other components during an agent’s execution. It serves as a mechanism for **dependency injection** and **state management**, allowing you to share data such as user information, configuration settings, or dependencies (e.g., loggers, data fetchers) across your application.

### Key Points About Context
1. **Not Sent to the LLM**: Context is a local object used within your code. It is not passed to the underlying large language model (LLM). Instead, it’s accessible to your tools, hooks, and callbacks.
2. **Flexible Data Structure**: You can use any Python object as a context, such as a dictionary, dataclass, or Pydantic model.
3. **Type Consistency**: All components (agents, tools, hooks) in a single agent run must use the same context type to ensure type safety and consistency.
4. **Purpose**: Context is used to:
   - Store user-specific data (e.g., username, user ID).
   - Manage dependencies (e.g., database connections, loggers).
   - Pass runtime information to tools and lifecycle hooks.

### Context vs. LLM Conversation History
The LLM only sees data from the **conversation history** (i.e., messages passed to it). If you want the LLM to access specific information, you must include it in:
- **Agent instructions** (system prompt).
- **Input messages** passed to `Runner.run`.

Context, on the other hand, is for **your code’s use**, not the LLM’s. For example, you might store a user’s ID in the context to fetch data from a database in a tool, but the LLM doesn’t see this ID unless you explicitly include it in the conversation history.

---

## How Context Management Works

Context is managed through the `RunContextWrapper` class, which wraps the context object you provide when calling `Runner.run`. This wrapper is passed to tools, lifecycle hooks, and other components, allowing them to access the context data.

### Key Components
1. **RunContextWrapper[T]**: A generic wrapper that holds your context object of type `T`. You access the context via `wrapper.context`.
2. **Runner.run**: The method where you pass the context object using the `context` parameter.
3. **Type Safety**: The SDK uses Python’s type hints to ensure that all components in a run use the same context type.

### Workflow
1. You define a context object (e.g., a dataclass or Pydantic model).
2. You pass this object to `Runner.run` or related methods.
3. Tools, hooks, and other components receive a `RunContextWrapper[T]` instance, from which they can access the context.
4. The context can be read, modified, or used to call methods during the agent’s execution.

---

## Practical Example: Using Context to Manage User Information

Let’s walk through a practical example to demonstrate context management. In this example, we’ll create an agent that uses a context object to store user information (name and user ID) and a tool that fetches the user’s age based on the context.

### Step 1: Define the Context Object
We’ll use a Python `dataclass` to define a `UserInfo` context object that stores a user’s name and ID.

```python
from dataclasses import dataclass

@dataclass
class UserInfo:
    name: str
    uid: int
```

### Step 2: Create a Tool That Uses the Context
We’ll define a tool called `fetch_user_age` that uses the context to retrieve the user’s age. In a real application, this might query a database, but for simplicity, we’ll return a hardcoded value.

```python
from agents import RunContextWrapper, function_tool

@function_tool
async def fetch_user_age(wrapper: RunContextWrapper[UserInfo]) -> str:
    # Access the context to get user information
    user_name = wrapper.context.name
    return f"User {user_name} is 47 years old"
```

- The `@function_tool` decorator turns the Python function into a tool that the agent can call.
- The tool receives a `RunContextWrapper[UserInfo]`, from which it accesses the `UserInfo` context via `wrapper.context`.

### Step 3: Create an Agent
We’ll create an agent that uses the `fetch_user_age` tool and is typed to work with the `UserInfo` context.

```python
from agents import Agent

agent = Agent[UserInfo](
    name="Assistant",
    instructions="You are a helpful assistant that can fetch user information.",
    tools=[fetch_user_age],
)
```

- The `[UserInfo]` specifies that this agent expects a `UserInfo` context object.
- The `tools` parameter includes our `fetch_user_age` tool.

### Step 4: Run the Agent with Context
We’ll use the `Runner.run` method to execute the agent, passing a `UserInfo` context object.

```python
from agents import Runner
import asyncio

async def main():
    # Create a context object
    user_info = UserInfo(name="John", uid=123)
    
    # Run the agent with the context
    result = await Runner.run(
        starting_agent=agent,
        input="What is the age of the user?",
        context=user_info,
    )
    
    # Print the result
    print(result.final_output)

# Run the async main function
if __name__ == "__main__":
    asyncio.run(main())
```

### Expected Output
```
The user John is 47 years old.
```

### Explanation of the Code
1. **Context Object**: The `UserInfo` dataclass stores the user’s name (`John`) and ID (`123`).
2. **Tool**: The `fetch_user_age` tool accesses the user’s name from the context and returns a string with the user’s age.
3. **Agent**: The agent is configured with the tool and expects a `UserInfo` context.
4. **Runner**: The `Runner.run` method passes the `user_info` context to the agent and tool, and the tool uses it to generate the response.

---

## Advanced Example: Dynamic Instructions with Context

In this example, we’ll use a dynamic instruction function that generates the agent’s system prompt based on the context. This is useful when you want the LLM to have access to context data (e.g., the user’s name) in its instructions.

### Step 1: Define the Context and Dynamic Instructions
We’ll reuse the `UserInfo` dataclass and create a function to generate dynamic instructions.

```python
from dataclasses import dataclass
from agents import Agent, RunContextWrapper

@dataclass
class UserInfo:
    name: str
    uid: int

def dynamic_instructions(context: RunContextWrapper[UserInfo], agent: Agent[UserInfo]) -> str:
    return f"You are a helpful assistant for {context.context.name}. Answer their questions politely."
```

### Step 2: Create the Agent with Dynamic Instructions
We’ll create an agent that uses the dynamic instructions and a simple tool.

```python
@function_tool
async def greet_user(wrapper: RunContextWrapper[UserInfo]) -> str:
    return f"Hello, {wrapper.context.name}! How can I assist you today?"

agent = Agent[UserInfo](
    name="Assistant",
    instructions=dynamic_instructions,
    tools=[greet_user],
)
```

### Step 3: Run the Agent
We’ll run the agent with a context object and a user input.

```python
async def main():
    user_info = UserInfo(name="Alice", uid=456)
    
    result = await Runner.run(
        starting_agent=agent,
        input="Say hello to me.",
        context=user_info,
    )
    
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

### Expected Output
```
Hello, Alice! How can I assist you today?
```

### Explanation
- **Dynamic Instructions**: The `dynamic_instructions` function uses the context to include the user’s name in the system prompt, making the LLM’s responses personalized.
- **Tool**: The `greet_user` tool also accesses the context to generate a personalized greeting.
- **Context Passing**: The `user_info` context is passed to both the instructions and the tool, ensuring consistent access to user data.

---

## Best Practices for Context Management

1. **Use Type Annotations**: Always specify the context type (e.g., `Agent[UserInfo]`) to leverage type checking and prevent errors.
2. **Keep Context Lightweight**: Avoid storing large or complex objects in the context unless necessary, as it’s passed to all components.
3. **Use Pydantic or Dataclasses**: These provide structured, type-safe ways to define context objects.
4. **Separate Context from LLM Data**: Remember that context is for your code, not the LLM. Use agent instructions or input messages to pass data to the LLM.
5. **Handle Sensitive Data Carefully**: Since context is used locally, ensure sensitive data (e.g., user IDs) is handled securely and not accidentally exposed in logs or outputs.

---

## Limitations and Considerations

1. **Type Consistency**: All components in a single agent run must use the same context type. Mixing types will result in type errors.
2. **Not Sent to LLM**: If you need the LLM to access context data, you must explicitly include it in instructions or input messages.
3. **Performance**: If your context involves heavy computations (e.g., fetching data from a database), ensure these are optimized to avoid slowing down the agent’s execution.
4. **Tracing**: Context data may appear in traces if you’re using the SDK’s tracing feature. Disable sensitive data capture if needed (see the tracing documentation).

---

## Conclusion

Context management in the OpenAI Agents SDK is a powerful feature that enables flexible, type-safe data sharing across agents, tools, and lifecycle hooks. By using context objects, you can manage user-specific data, dependencies, and state without cluttering the LLM’s conversation history. The examples provided demonstrate how to define context objects, use them in tools, and incorporate them into dynamic instructions.

To explore more, refer to the official OpenAI Agents SDK documentation at https://openai.github.io/openai-agents-python/. Experiment with different context types (e.g., Pydantic models, dictionaries) and explore advanced features like guardrails, handoffs, and tracing to build robust AI applications.
