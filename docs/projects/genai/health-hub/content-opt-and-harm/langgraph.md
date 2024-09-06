---
updated: 26 August 2024
authors:
  - Issac J
---

# LangGraph

## Introduction

LangGraph is a framework for developing LLM-powered Agentic Workflows. It is a library for building stateful, multi-actor applications with large language models (LLMs). It extends the LangChain framework to enable the creation of complex, stateful workflows involving multiple AI agents.

## [Key Concepts](https://langchain-ai.github.io/langgraph/concepts/)

### [Graphs](https://langchain-ai.github.io/langgraph/concepts/low_level/)

LangGraph agents compose of three key components -

1. State: A shared data structure that represents the current snapshot of the Agentic Workflow. We are using a TypedDict to store data throughout the workflow.
2. Nodes: Python functions that perform transformation on the current state and returns an updated state.
3. Edges: Python functions that directs the execution of the Agentic Workflow along a sequence of nodes based on either conditional or fixed transitions.

The Graph State must consist of Schema and Reducers. The Schema is typically stored in [`states/definitions.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/states/definitions.py) within the `article-harmonisation` project.
The reducers are stored in the [`utils/formatters.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/utils/formatters.py).

There are 3 key functions that we have created to run the LangGraph application. We have stored these functions in [`utils/graphs.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/utils/graphs.py). They are -

1. `create_graph`: Python function that compiles a LangGraph Object based on the provided schema, nodes and edges.
2. `draw_graph`: Python function to generate a visual representation of the Compiled LangGraph object, displaying the possible paths of the workflow
3. `execute_graph`: Python function to execute the LangGraph Application with the provided input and CompiledGraph object.

## Notes

### [Branching](https://langchain-ai.github.io/langgraph/how-tos/branching/)

In our project, we implemented a custom reducer that assist in updating dictionaries during parallel node execution.
Refer to this [link](https://langchain-ai.github.io/langgraph/how-tos/branching/?h=parallel) to understand how to implement branches for parallel execution.
