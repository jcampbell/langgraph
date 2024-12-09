# Quick Start: Launch Local LangGraph Server

This is a quick start guide to help you get a LangGraph app up and running locally.

!!! info "Requirements"

    - Python >= 3.11
    - [LangGraph CLI](https://langchain-ai.github.io/langgraph/cloud/reference/cli/): Requires langchain-cli[inmem] >= 0.1.58

## Install the LangGraph CLI

```bash
pip install -U "langgraph-cli[inmem]" python-dotenv
```

## 🌱 Create a LangGraph App

Create a new app from the `react-agent` template. This template is a simple agent that can be flexibly extended to many tools.

=== "Python Server"

    ```shell
    langgraph new path/to/your/app --template react-agent-python 
    ```

=== "Node Server"

    ```shell
    langgraph new path/to/your/app --template react-agent-js
    ```

!!! tip "Additional Templates"

    If you use `langgraph new` without specifying a template, you will be presented with an interactive menu that will allow you to choose from a list of available templates.

## Install Dependencies

In the root of your new LangGraph app, install the dependencies in `edit` mode so your local changes are used by the server:

```shell
pip install -e .
```

## Create a `.env` file

You will find a `.env.example` in the root of your new LangGraph app. Create
a `.env` file in the root of your new LangGraph app and copy the contents of the `.env.example` file into it, filling in the necessary API keys:

```bash
LANGSMITH_API_KEY=lsv2...
TAVILY_API_KEY=tvly-...
ANTHROPIC_API_KEY=sk-
OPENAI_API_KEY=sk-...
```

<details><summary>Get API Keys</summary>
    <ul>
        <li> <b>LANGSMITH_API_KEY</b>: Go to the <a href="https://smith.langchain.com/settings">LangSmith Settings page</a>. Then clck <b>Create API Key</b>.
        </li>
        <li>
            <b>ANTHROPIC_API_KEY</b>: Get an API key from <a href="https://console.anthropic.com/">Anthropic</a>.
        </li>
        <li>
            <b>OPENAI_API_KEY</b>: Get an API key from <a href="https://openai.com/">OpenAI</a>.
        </li>
        <li>
            <b>TAVILY_API_KEY</b>: Get an API key on the <a href="https://app.tavily.com/">Tavily website</a>.
        </li>
    </ul>
</details>

## 🚀 Launch LangGraph Server

```shell
langgraph dev
```

This will start up the LangGraph API server locally. If this runs successfully, you should see something like:

>    Ready!
> 
>    - API: [http://localhost:8123](http://localhost:8123/)
>     
>    - Docs: http://localhost:8123/docs
>     
>    - LangGraph Studio Web UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:8123


!!! note "In-Memory Mode"

    The `langgraph dev` command starts LangGraph Server in an in-memory mode. This mode is suitable for development and testing purposes. For production use, you should deploy LangGraph Server with access to a persistent storage backend.

    If you want to test your application with a persistent storage backend, you can use the `langgraph up` command instead of `langgraph dev`. You will
    need to have `docker` installed on your machine to use this command.

## LangGraph Studio Web UI

Test your graph in the LangGraph Studio Web UI by visiting the URL provided in the output of the `langgraph up` command.

>    - LangGraph Studio Web UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:8123

!!! warning "Safari Compatibility"
    
    Currently, LangGraph Studio Web does not support Safari when running a server locally.

## Test the API

=== "Python SDK (Async)"

    **Install the LangGraph Python SDK**

    ```shell
    pip install langgraph-sdk
    ```

    **Send a message to the assistant (threadless run)**

    ```python
    from langgraph_sdk import get_client

    client = get_client(url="http://localhost:8123")

    async for chunk in client.runs.stream(
        None,  # Threadless run
        "agent", # Name of assistant. Defined in langgraph.json.
        input={
            "messages": [{
                "role": "human",
                "content": "What is LangGraph?",
            }],
        },
        stream_mode="updates",
    ):
        print(f"Receiving new event of type: {chunk.event}...")
        print(chunk.data)
        print("\n\n")
    ```

=== "Python SDK (Sync)"

    **Install the LangGraph Python SDK**

    ```shell
    pip install langgraph-sdk
    ```

    **Send a message to the assistant (threadless run)**

    ```python
    from langgraph_sdk import get_sync_client

    client = get_sync_client(url="http://localhost:8123")

    for chunk in client.runs.stream(
        None,  # Threadless run
        "agent", # Name of assistant. Defined in langgraph.json.
        input={
            "messages": [{
                "role": "human",
                "content": "What is LangGraph?",
            }],
        },
        stream_mode="updates",
    ):
        print(f"Receiving new event of type: {chunk.event}...")
        print(chunk.data)
        print("\n\n")
    ```

=== "Javascript SDK"

    **Install the LangGraph JS SDK**

    ```shell
    npm install @langchain/langgraph-sdk
    ```

    **Send a message to the assistant (threadless run)**

    ```js
    const { Client } = await import("@langchain/langgraph-sdk");

    // only set the apiUrl if you changed the default port when calling langgraph up
    const client = new Client({ apiUrl: "http://localhost:8123"});

    const streamResponse = client.runs.stream(
        null, // Threadless run
        "agent", // Assistant ID
        {
            input: {
                "messages": [
                    { "role": "user", "content": "What is LangGraph?"}
                ]
            },
            streamMode: "messages",
        }
    );

    for await (const chunk of streamResponse) {
        console.log(`Receiving new event of type: ${chunk.event}...`);
        console.log(JSON.stringify(chunk.data));
        console.log("\n\n");
    }
    ```

=== "Rest API"

    ```bash
    curl -s --request POST \
        --url "http://localhost:8123/runs/stream" \
        --header 'Content-Type: application/json' \
        --data "{
            \"assistant_id\": \"agent\",
            \"input\": {
                \"messages\": [
                    {
                        \"role\": \"human\",
                        \"content\": \"What is LangGraph?\"
                    }
                ]
            },
            \"stream_mode\": \"updates\"
        }" 
    ```

!!! tip "Auth"

    If you're connecting to a remote server, you will need to provide a LangSmith
    API Key for authorization. Please see the API Reference for the clients
    for more information.

## Next Steps

Now that you have a LangGraph app running locally, take your journey further by exploring deployment and advanced features:

### 🌐 Deploy to LangGraph Cloud

- **[LangGraph Cloud QuickStart](../../cloud/quick_start.md)**: Deploy your LangGraph app using LangGraph Cloud.

### 📚 Learn More about LangGraph Platform

Expand your knowledge with these resources:

- **[LangGraph Platform Concepts](../../concepts/index.md#langgraph-platform)**: Understand the foundational concepts of the LangGraph Platform.  
- **[LangGraph Platform How-to Guides](../../how-tos/index.md#langgraph-platform)**: Discover step-by-step guides to build and deploy applications.

### 🛠️ Developer References

Access detailed documentation for development and API usage:

- **[LangGraph Server API Reference](../../cloud/reference/api/api_ref.html)**: Explore the LangGraph Server API documentation.  
- **[Python SDK Reference](../../cloud/reference/sdk/python_sdk_ref.md)**: Explore the Python SDK API Reference.
- **[JS/TS SDK Reference](../../cloud/reference/sdk/js_ts_sdk_ref.md)**: Explore the Python SDK API Reference.
