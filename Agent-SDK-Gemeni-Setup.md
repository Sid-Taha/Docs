# Agent SDK Gemini Setup Using `uv`

## Step 1: Initialize Project with `uv`

Start by initializing your project environment using the `uv` tool. This sets up the basic structure needed to manage dependencies.

```sh
uv init
```

- **Purpose**: Initializes the project and prepares it for dependency management.
- **Explanation**: Running `uv init` creates the necessary configurations for your project, ensuring that all dependencies can be tracked and managed effectively.

## Step 2: Add `ruff` for Code Linting

Integrate `ruff`, a Python linter, to maintain code quality and adhere to best practices for code style.

```sh
uv add ruff
```

- **Purpose**: Adds `ruff` to the project to ensure code quality.
- **Explanation**: `ruff` is a fast and efficient linter that checks your code for potential issues such as style violations and errors, improving the readability and maintainability of your code.

## Step 3: Activate the Virtual Environment

Activate your virtual environment to isolate dependencies for this project.

```sh
.venv\Scripts\activate
```

- **Purpose**: Activates the virtual environment on a Windows system.
- **Explanation**: Virtual environments allow you to manage project-specific dependencies separately from the global Python installation. This ensures no version conflicts between packages.

## Step 4: Add `python-dotenv` for Environment Variable Management

Install the `python-dotenv` package to manage environment variables like your API key securely.

```sh
uv add python-dotenv
```

- **Purpose**: Adds `python-dotenv` to the project to handle environment variables.
- **Explanation**: The `python-dotenv` library helps load environment variables from a `.env` file into your application, preventing sensitive information (like API keys) from being hardcoded in your codebase.

## Step 5: Add `openai-agents` for OpenAI SDK Integration

Add the `openai-agents` package, which is necessary to interact with OpenAI's APIs and agents.

```sh
uv add openai-agents
```

- **Purpose**: Installs `openai-agents` for using OpenAI’s SDK and building intelligent agents.
- **Explanation**: The `openai-agents` package provides tools for interacting with OpenAI’s models, making it easier to integrate and work with OpenAI’s APIs in your project.

## Step 6: Create the `.env` File for Environment Variables

Create a `.env` file to store your environment variables securely, such as your Gemini API key.

1. Create a file named `.env` in the root directory of your project.
2. Add the following line to store your API key:

```
gemini_api_key=your-api-key-here
```

- **Purpose**: Stores the Gemini API key securely in the `.env` file.
- **Explanation**: The `.env` file helps separate configuration from code and prevents sensitive data from being exposed in version control systems.

## Step 7: Initialize the Python Code

Here’s the starting code to load the environment variables and verify that the API key is correctly set.

```python
    import os
    from dotenv import load_dotenv
    from agents import Agent, Runner, AsyncOpenAI, OpenAIChatCompletionsModel
    from agents import set_default_openai_client
    
    load_dotenv()
    
    gemini_api_key = os.getenv("gemini_api_key")
    
    if not gemini_api_key:
        raise ValueError("API key is not working")
      
    
    
    external_client = AsyncOpenAI(
        api_key=gemini_api_key,
        base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
    )
    
    set_default_openai_client(external_client)
    
    
    model = OpenAIChatCompletionsModel(
        model="gemini-2.0-flash",
        openai_client=external_client
    )
    
    agent: Agent = Agent(
        name="Assistant", 
        instructions="You are a helpful assistant", 
        model=model
      )
    
    
    result = Runner.run_sync(agent, "are you a openai agent or a google agent?")
    
    print(result.final_output)

```

- **Purpose**: Initializes environment variables and checks the API key.
- **Explanation**:
  - `load_dotenv()` loads the environment variables from the `.env` file.
  - `os.getenv("gemini_api_key")` retrieves the OpenAI API key stored in the environment.
  - If the API key is missing, a `ValueError` is raised to alert you of the issue.

---
