---
updated: 26 August 2024
authors:
  - Issac J
---

# LangChain

## Introduction

LangChain is a framework for developing LLM-powered applications. It is a library for building complex chain-based language processing applications. It allows for easy chaining of components like parsers, retrievers and generators, making it ideal for natural language processing tasks.

## [LangChain Expression Language (LCEL)](https://python.langchain.com/v0.2/docs/concepts/#langchain-expression-language-lcel)

We predominantly use LCEL to support quick and easy prototyping of LangChain components.

### [Runnables](https://python.langchain.com/v0.1/docs/expression_language/interface/)

Runnables are standard interfaces that allows us to combine various LangChain components into a chain. We typically use the pipe `|` operator to chain them together. Refer to this [documentation](https://python.langchain.com/v0.2/docs/how_to/sequence/) on how to chain these runnables.

### [Working with Multiple Chains](https://python.langchain.com/v0.1/docs/expression_language/cookbook/multiple_chains/)

There are times when you need to utilise the outputs from a previous chain in the current one. Typically, this requires constructing a `meta-chain` that links all these chains together. Also, you would need to define a dictionary containing a `RunnablePassthrough()` as a value in the chain of interest.
The value can then be fetched via an `itemgetter` in the `meta-chain`.

### [Inspecting Your Runnables](https://python.langchain.com/v0.1/docs/expression_language/how_to/inspect/)

Sometimes, the constructed chain can get very complicated. Hence, it is important to inspect how the chains are arranged so that you can view the data flow along these components for better debugging. This is especially the case when there is parallel execution and/or branches in the LLM chain.

!!! Note

    Use `chain.get_graph().print_ascii()` to obtain the computation graph of the LLM Chain

## Classes

### [AzureChatOpenAI](https://python.langchain.com/v0.2/docs/integrations/chat/azure_chat_openai/)

During the Article Rewriting Process, we are utilising `gpt-4o-mini` from Azure OpenAI Service to evaluate and optimise the articles. We rely on the `AzureChatOpenAI` class to abstract the implementation of making HTTP requests to the service and use them within chains.
