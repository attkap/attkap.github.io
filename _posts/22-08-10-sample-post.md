---
layout: default
title: CustomerSupportGPT - a demo of using LLM-APIs to automate Customer Service
---

# The Art of Customer Support Automation: Using Decision-Trees to Overcome Context Limitations.

*{{ page.date | date: "%B %d, %Y" }}*

## Introduction

Introductory text goes here...

## Main Content

I have created an MVP of a customer support bot using the OpenAI API. The idea was inspired by Andrew Ngen's excellent [Building Systems with the ChatGPT API](https://www.deeplearning.ai/short-courses/building-systems-with-chatgpt/). 
To me, this is one of the most obvious applications of LLMs as they are formidable at repetitive corporate writing and never get frustrated — even with the most irate and uncomfortable customers. I estimate that such applications can easily automate >20% of what is a mostly boring and uncomfortable job. 

## The Challenge of Context Windows

A significant challenge when creating a customer support chatbot using LLMs is that their context window is typically too small to contain all relevant information for handling a wide variety of customer requests. In such cases, it's often suggested to store the relevant information in a [vector store](https://python.langchain.com/docs/modules/data_connection/vectorstores/) and to retrieve any relevant context to a particular question using algorithms like [vector similarity search](https://python.langchain.com/docs/modules/data_connection/vectorstores/). However, in my experience, the results of such searches are too poor in quality and too inconsistent to be used for customer service. Of course there are also LLMs like Claude with very large context windows out there. But they suffer from similar quality issues. In fact, [langchain compared an earlier version of Claude and found that similarity search performed better](https://blog.langchain.dev/auto-evaluation-of-anthropic-100k-context-window/). So what else can we try? 

## An Alternate Solution: Decision-Trees and Work-Packages

The fundamental insight to crack this issue is that new customer service representatives don't have all the relevant knowledge in their memory either. Instead, they just look up the answers. Customer service departments usually have an internal wiki or handbook which contain answers to the most common customer service requests in a structured manner. Using the same type of structure, we can create a decision tree that splits and orders answers to certain chapters or categories. These categories can be used by an LLM to quickly select the relevant "context" for a given question. That context is then small enough to fit into the context window of the LLM. 

By splitting up each task into a work-package that is self-contained, we can map the process using individual LLM-calls with different prompt messages, without having to carry over the context of previous work-packages. In my MVP the following LLM-API calls are made:

1. **Translate the customer request into clear English.** Current LLMs work best in English, and by making the English clear we can also translate poorly formulated English requests for better performance.
2. **Assign a primary and a secondary category to the question**. As Input I send the LLM a description of all available categories together with the translated customer request. As Output, the LLM returns the primary and the secondary category that fit best to the question (These are equivalent to chapters and sub-chapters in a handbook).
3. **Answer the question based on category context**. Using the categories I navigate to a page containing the relevant information to answer the given question. As input I send the LLM the context from the page, the translated customer request and a prompt to answer the request based on the context provided. As output, the LLM creates a customer service response.
4. **Check for harmfulness**. Using OpenAI's [moderation endpoint](https://platform.openai.com/docs/guides/moderation) I check the response for harmfulness, so that customers don't receive inappropriate responses.

Of course this system could be significantly enhanced and enlarged, by for instance adding sanity checks to the workflow. 

## Conclusion

My MVP demonstrates a practical approach to overcome the challenges of context limitations in LLMs for customer service applications. By splitting workflows into self-contained work-packages and leveraging existing structures like decision trees, we can pave the way for AI to tackle larger tasks. What are your thoughts on this approach? Feel free to check out the code on [GitHub](https://github.com/attkap/CustomerServiceGPT) and share your insights!



```language
// Code snippet example