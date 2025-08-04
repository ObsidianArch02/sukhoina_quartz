---
title: "How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)"
source: "https://scriv.ai/guides/retrieval-augmented-generation-overview/"
published:
created: 2025-08-04
tags:
  - "clippings"
---

> ## 揭秘基于数据的问答机器人工作原理
> 
> 本文介绍了**检索增强生成（RAG）**技术，该技术通过结合用户输入和从知识库中检索到的相关信息，使大型语言模型（如ChatGPT）能够生成更准确的回答。
> 
> ### 主要内容：
> 1. **RAG概述**：
>    - 检索步骤：从知识库中查找与用户问题最相关的内容。
>    - 生成步骤：将检索到的内容和用户问题一起发送给语言模型生成回答。
> 
> 2. **嵌入（Embeddings）**：
>    - 将文本转换为数字向量，用于语义搜索。
>    - 通过计算向量之间的距离确定文本的相似性。
> 
> 3. **知识库索引**：
>    - **加载**：从原始数据源（如网站、文档）提取内容。
>    - **分割**：将内容分割为适合嵌入搜索的小片段。
> 
> 4. **LangChain示例**：
>    - 使用3行代码即可构建基于特定文档的问答机器人。
> 
> 5. **完整流程**：
>    - 索引知识库 → 检索相关内容 → 生成增强回答。
> 
> 本文适合开发者、非技术人员或对AI技术感兴趣的读者，帮助理解问答机器人的核心原理。

![[images/【译】私域聊天机器人如何工作？检索增强的内容生成（RAG）概述 (2025_8_4 14：42：12).html]]

## Demystifying and deconstructing Q&A bots that work over your data.

Aug. 25, 2023

There’s a popular open-source library called [LangChain](https://python.langchain.com/) that can create chatbots that—among other things—do Q&A over any website/document in 3 lines of code. Here’s [an example of that from the langchain docs](https://python.langchain.com/docs/use_cases/question_answering/).

```python
from langchain.document_loaders import WebBaseLoader
from langchain.indexes import VectorstoreIndexCreator
loader = WebBaseLoader("http://www.paulgraham.com/greatwork.html")
index = VectorstoreIndexCreator().from_loaders([loader])
index.query("What should I work on?")
```

Which outputs an answer specific to [Paul Graham’s essay](http://www.paulgraham.com/greatwork.html):

> The work you choose should ideally have three qualities: it should be something you have a natural aptitude for, something you have a deep interest in, and something that offers scope to do great work. If you're unsure, you could start by working on your own projects that seem excitingly ambitious to you. It's also beneficial to be curious, try lots of things, meet lots of people, read lots of books, and ask lots of questions. When in doubt, optimize for interestingness. It's okay to guess and be wrong sometimes, as this can lead to discovering what you're truly good at or interested in.

*Note: if you’re interested you can try [the chatbot built from Paul Graham’s essay here](https://scriv.ai/a/scriv/bots/b/pg-bot/chat/).*

The first time you run this it feels like pure magic. How the heck does this work?

The answer is a process called **retrieval augmented generation**, or RAG for short. It is a remarkably simple concept, though also has incredible depth in the details of its implementation.

This post will provide a high-level overview of RAG. We’ll start from the big picture workflow of what’s happening, and then zoom in on all the individual pieces. By the end of it, you should have a solid understanding of how those three magic lines of code work, and all the principles involved in creating these Q&A bots.

If you’re a developer trying to build bots like this, you’ll learn which knobs you can tweak and how to tweak them. If you’re a non-developer hoping to use AI tools on your dataset, you'll gain knowledge that will help you get the most out of them. And, if you’re just a curious mind, you’ll hopefully learn a thing or two about some of the technology that's upending our lives.

Let’s dive in.

## What is Retrieval Augmented Generation?

**Retrieval augmented generation is the process of supplementing a user’s input to a large language model (LLM) like ChatGPT with additional information that you have *retrieved* from somewhere else. The LLM can then use that information to *augment* the response that it *generates*.**

The following diagram shows how it works in practice:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/46e8f332a1ef2d5d950f51ac401372c3_MD5.png]]

It starts with a user’s question. For example “How do I do <something>?”

The first thing that happens is the *retrieval* step. This is the process that takes the user’s question and searches for the most relevant content from a knowledge base that might answer it. The retrieval step is by far the most important, and most complex part of the RAG chain. But for now, just imagine a black box that knows how to pull out the best chunks of relevant information related to the user’s query.

Can't we just give the LLM the whole knowledge base?

You might be wondering why we bother with retrieval instead of just sending the whole knowledge base to the LLM. One reason is that models have built-in limits on how much text they can consume at a time (though these are quickly increasing). A second reason is cost—sending huge amounts of text gets quite expensive. Finally, there is [evidence](https://arxiv.org/abs/2307.03172) suggesting that sending small amounts of relevant information results in better answers.

Once we’ve gotten the relevant information out of our knowledge base, we send it, along with the user’s question, to the *large language model* (LLM). The LLM—most commonly ChatGPT—then “reads” the provided information and answers the question. This is the *augmented generation* step.

Pretty simple, right?

## Working backwards: Giving an LLM extra knowledge to answer a question

We’ll start at the last step: *answer generation*. That is, let’s assume we already have the relevant information pulled from our knowledge base that we think answers the question. How do we use that to generate an answer?

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/f7bb248eb5d17b6a9d4ba38bc5b74dee_MD5.png]]

This process may feel like black magic, but behind the scenes it is *just a language model*. So in broad strokes, the answer is “just ask the LLM”. How do we get an LLM to do something like this?

We'll use ChatGPT as an example. And just like regular ChatGPT, it all comes down to prompts and messages.

### Giving the LLM custom instructions with the system prompt

The first component is the *system prompt*. The system prompt gives the language model its overall guidance. For ChatGPT, the system prompt is something like “You are a helpful assistant.”

In this case we want it to do something more specific. And, since it’s a language model, we can just *tell it what we want it to do*. Here’s an example short system prompt that gives the LLM more detailed instructions:

> You are a Knowledge Bot. You will be given the extracted parts of a knowledge base (labeled with DOCUMENT) and a question. Answer the question using information from the knowledge base.

We’re basically saying, *“Hey AI, we’re gonna give you some stuff to read. Read it and then answer our question, k? Thx.”* And, because AIs are great at following our instructions, it kind of just... *works*.

### Giving the LLM our specific knowledge sources

Next we need to give the AI its reading material. And again—the latest AIs are really good at just *figuring stuff out*. But, we can help it a bit with a bit of structure and formatting.

Here’s an example format you can use to pass documents to the LLM:

```python
------------ DOCUMENT 1 -------------

This document describes the blah blah blah...

------------ DOCUMENT 2 -------------

This document is another example of using x, y and z...

------------ DOCUMENT 3 -------------

[more documents here...]
```

Do you need all this formatting? Probably not, but it’s nice to make things as explicit as possible. You can also use a machine-readable format like JSON or YAML. Or, if you're feeling frisky, you can just dump everything in a giant blob of text. But, having some consistent formatting becomes important in more advanced use-cases, for example, if you want the LLM to cite its sources.

Once we’ve formatted the documents we just send it as a normal chat message to the LLM. Remember, in the system prompt we told it we were gonna give it some documents, and that’s all we’re doing here.

### Putting everything together and asking the question

Once we’ve got our system prompt and our “documents” message, we just send the user’s question to the LLM alongside them. Here’s how that looks in Python code, using the OpenAI [ChatCompletion API](https://platform.openai.com/docs/guides/gpt/chat-completions-api):

```python
openai_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "system",
            "content": get_system_prompt(),  # the system prompt as per above
        },
        {
            "role": "system",
            "content": get_sources_prompt(),  # the formatted documents as per above
        },
        {
            "role": "user",
            "content": user_question,  # the question we want to answer
        },
    ],
)
```

Python code for doing a retrieval-augmented answer with OpenAI's ChatGPT

That’s it! A custom system prompt, two messages, and you have context-specific answers!

This is a simple use case, and it can be expanded and improved on. One thing we haven't done is told the AI what to do if it can't find an answer in the sources. We can add these instructions to the system prompt—typically either telling it to refuse to answer, or to use its general knowledge, depending on your bot's desired behavior. You can also get the LLM to cite the specific sources it used to answer the question. We’ll talk about those tactics in future posts but for now, that’s the basics of answer generation.

With the easy part out of the way, it’s time to come back to that black box we skipped over...

## The retrieval step: getting the right information out of your knowledge base

Above we assumed we had the right knowledge snippets to send to the LLM. But how do we actually get these from the user’s question? This is the *retrieval step*, and it is the core piece of infrastructure in any “chat with your data” system.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/44a8bf4bc17744fbcc7dfcb60d74bc8d_MD5.png]]

At its core, retrieval is a search operation—we want to look up the most relevant information based on a user’s input. And just like search, there are two main pieces:

1. **Indexing:** Turning your knowledge base into something that can be searched/queried.
2. **Querying:** Pulling out the most relevant bits of knowledge from a search term.

It’s worth noting that *any search process could be used for retrieval*. Anything that takes a user input and returns some results would work. So, for example, you could just try to find text that matches the user's question and send that to the LLM, or you could Google the question and send the top results across—which, incidentally, is approximately how Bing's chatbot works.

That said, *most* RAG systems today rely on something called *semantic search*, which uses another core piece of AI technology: *embeddings*. Here we’ll focus on that use case.

So...what *are* embeddings?

## What are embeddings? And what do they have to do with knowledge retrieval?

LLMs are weird. One of the weirdest things about them is that nobody really knows *how* they understand language. Embeddings are a big piece of that story.

If you ask a person how they turn words into *meaning*, they will likely fumble around and say something vague and self-referential like "because I know what they mean". Somewhere deep in our brains there is a complex structure that knows "child" and "kid" are basically the same, "red" and "green" are both colors, and "pleased," "happy," and "elated" represent the same emotion with varying magnitudes. We can't *explain* how this works, we just *know* it.

Language models have a similarly complex understanding of language, except, since they are *computers* it's not in their brains, but made up of *numbers*. In an LLM's world, any piece of human language can be represented as a vector (list) of numbers. This vector of numbers is an *embedding*.

A critical piece of LLM technology is a *translator* that goes from human *word-language* to AI *number-language*. We'll call this translator an "embedding machine", though under the hood it's just an API call. Human language goes in, AI numbers come out.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/5f2c52d245d0a5ac1c812313d0175691_MD5.png]]

What do these numbers mean? No human knows! They are only “meaningful” to the AI. But, what we do know is that *similar words end up with similar sets of numbers*. Because behind the scenes, the AI uses these numbers to “read” and “speak”. So the numbers have some kind of magic comprehension baked into them in AI-language—even if we don't understand it. The embedding machine is our translator.

Now, since we have these magic AI numbers, we can plot them. A simplified plot of the above examples might look something like this—where the axes are just some abstract representation of human/AI language:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/b80619dcc9ff6e7c1390dee5d9aa6836_MD5.png]]

Once we’ve plotted them, we can see that the closer two points are to each other in this hypothetical language space, the more similar they are. “Hello, how are you?” and “Hey, how’s it going?” are practically on top of each other. “Good morning,” another greeting, is not too far from those. And “I like cupcakes” is on a totally separate island from the rest.

Naturally, you can’t represent the entirety of human language on a two-dimensional plot, but the theory is the same. In practice, embeddings have many more coordinates (1,536 for the current model used by OpenAI). **But you can still do basic math to determine how close two embeddings—and therefore two pieces of text—are to each other**.

These embeddings, and determining “closeness” are the core principle behind *semantic search*, which powers the retrieval step.

Like what you're reading?

Powered by [EmailOctopus](https://emailoctopus.com/?utm_source=powered_by_form&utm_medium=user_referral)

## Finding the best pieces of knowledge using embeddings

Once we understand how search with embeddings works, we can construct a high-level picture of the retrieval step.

On the indexing side, first we have to break up our knowledge base into chunks of text. This process is an entire optimization problem in and of itself, and we’ll cover it next, but for now just assume we know how to do it.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/a144fd17c72180d95a0b38dbf671f11d_MD5.png]]

Once we’ve done that, we pass each knowledge snippet through the embedding machine (which is actually an OpenAI API or similar) and get back our embedding representation of that text. Then we save the snippet, along with the embedding in a *vector database* —a database that is optimized for working with vectors of numbers.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/d5de065600bd66285455d7a5cfeabc31_MD5.png]]

Now we have a database with the embeddings of all our content in it. Conceptually, you can think of it as a plot of our entire knowledge base on our “language” graph:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/43e77727762f61f2492cfca2eca1b21b_MD5.png]]

Once we have this graph, on the query side, we do a similar process. First we get the embedding for the user’s input:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/7cc3f62e964b59c8a42bc1910eb5436a_MD5.png]]

Then we plot it in the same vector-space and find the closest snippets (in this case 1 and 2):

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/00f53d5cd3a8d1d3703dcae1fa02c475_MD5.png]]

The magic embedding machine thinks these are the most related answers to the question that was asked, so these are the snippets that we pull out to send to the LLM!

In practice, this “what are the closest points” question is done via a query into our vector database. So the actual process looks more like this:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/5e99f23a81125cbe7eb71b5ac93e920f_MD5.png]]

The query itself involves some semi-complicated math—usually using something called a cosine distance, though there are other ways of computing it. The math is a whole space you can get into, but is out of scope for the purposes of this post, and from a practical perspective can largely be offloaded to a library or database.

Back to LangChain

In our LangChain example from the beginning, we have now covered everything done by this single line of code. That little function call is hiding a whole lot of complexity!

`index.query("What should I work on?")`

## Indexing your knowledge base

Alright, we’re almost there. We now understand how we can use embeddings to find the most relevant bits of our knowledge base, pass everything to the LLM, and get our augmented answer back. The final step we’ll cover is creating that initial index from your knowledge base. In other words, the “knowledge splitting machine” from this picture:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/8208131fb97554ce9d24edb94c9ed68f_MD5.png]]

Perhaps surprisingly, indexing your knowledge base is usually the hardest and most important part of the whole thing. And unfortunately, it’s more art than science and involves lots of trial and error.

Big picture, the indexing process comes down to two high-level steps.

1. **Loading**: Getting the contents of your knowledge base out of wherever it is normally stored.
2. **Splitting**: Splitting up the knowledge into snippet-sized chunks that work well with embedding searches.

Technical clarification

Technically, the distinction between "loaders" and "splitters" is somewhat arbitrary. You could imagine a single component that does all the work at the same time, or break the loading stage into multiple sub-components.

That said, "loaders" and "splitters" are how it is done in LangChain, and they provide a useful abstraction on top of the underlying concepts.

Let’s use my own use-case as an example. I wanted to build a chatbot to answer questions about my [saas boilerplate product, SaaS Pegasus](https://www.saaspegasus.com/). The first thing I wanted to add to my knowledge base was [the documentation site](https://docs.saaspegasus.com/). The *loader* is the piece of infrastructure that goes to my docs, figures out what pages are available, and then pulls down each page. When the loader is finished it will output individual *documents* —one for each page on the site.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/cc1774de732eb34bafab6b3310872f2e_MD5.png]]

Inside the loader a lot is happening! We need to crawl all the pages, scrape each one’s content, and then format the HTML into usable text. And loaders for other things—e.g. PDFs or Google Drive—have different pieces. There’s also parallelization, error handling, and so on to figure out. Again—this is a topic of nearly infinite complexity, but one that we’ll mostly offload to a library for the purposes of this write up. So for now, once more, we’ll just assume we have this magic box where a “knowledge base” goes in, and individual “documents” come out.

LangChain Loaders

Built in loaders are one of the most useful pieces of LangChain. They provide [a long list of built-in loaders](https://python.langchain.com/docs/integrations/document_loaders/) that can be used to extract content from anything from a Microsoft Word doc to an entire Notion site.

The interface to LangChain loaders is exactly the same as depicted above. A "knowledge base" goes in, and a list of "documents" comes out.

Coming out of the loader, we’ll have a collection of documents corresponding to each page in the documentation site. Also, ideally at this point the extra markup has been removed and just the underlying structure and text remains.

Now, we *could* just pass these whole webpages to our embedding machine and use those as our knowledge snippets. But, each page might cover a lot of ground! And, the more content in the page, the more “unspecific” the embedding of that page becomes. This means that our “closeness” search algorithm may not work so well.

What’s more likely is that the topic of a user’s question matches some piece of text *inside* the page. This is where splitting enters the picture. With splitting, we take any single document, and split it up into bite-size, embeddable chunks, better-suited for searches.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/b1c5c7c235e3464bb17286cac9384b51_MD5.png]]

Once more, there’s an entire art to splitting up your documents, including how big to make the snippets on average (too big and they don’t match queries well, too small and they don’t have enough useful context to generate answers), how to split things up (usually by headings, if you have them), and so on. But—a few sensible defaults are good enough to start playing with and refining your data.

Splitters in LangChain

In LangChain, splitters fall under a larger category of things called " [document transformers](https://python.langchain.com/docs/modules/data_connection/document_transformers/) ". In addition to providing various strategies for splitting up documents, they also have tools for removing redundant content, translation, adding metadata, and so on. We only focus on splitters here as they represent the overwhelming majority of document transformations.

Once we have the document snippets, we save them into our vector database, as described above, and we’re finally done!

Here’s the complete picture of indexing a knowledge base.

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/8fe2bc616ad5cffc2af2dc39b05c7438_MD5.png]]

Back to LangChain

In LangChain, the entire indexing process is encapsulated in these two lines of code. First we initialize our website loader and tell it what content we want to use:

`loader = WebBaseLoader("http://www.paulgraham.com/greatwork.html")`

Then we build the entire index from the loader and save it to our vector database:

`index = VectorstoreIndexCreator().from_loaders([loader])`

The loading, splitting, embedding, and saving is all happening behind the scenes.

## Recapping the whole process

At last we can fully flesh out the entire RAG pipeline. Here’s how it looks:

![[images/How do domain-specific chatbots work? An Overview of Retrieval Augmented Generation (RAG)/4c46c98c1534bfd11c2ae7886d7d2162_MD5.png]]

First we *index* our knowledge base. We get the knowledge and turn into individual documents using a *loader*, and then use a *splitter* to turn it into bite-size chunks or *snippets*. Once we have those, we pass them to our *embedding machine*, which turns them into vectors that can be used for semantic searching. We save these embeddings, alongside their text snippets in our *vector database*.

Next comes *retrieval*. It starts with the question, which is then sent through the same embedding machine and passed into our vector database to determine the closest matched snippets, which we’ll use to answer the question.

Finally, *augmented answer generation*. We take the snippets of knowledge, format them alongside a custom system prompt and our question, and, finally, get our *context-specific* answer.

Whew!

Hopefully you now have a basic understanding of how retrieval augmented generation works. If you’d like to try it out on a knowledge base of your own, without all the work of setting it up, check out [Scriv.ai](https://scriv.ai/), which lets you build domain-specific chatbots in just a few minutes with no coding skills required.

In future posts we’ll expand on many of these concepts, including all the ways you can improve on the “default” set up outlined here. As I mentioned, there is nearly infinite depth to each of these pieces, and in the future we’ll dig into those one at a time. If you’d like to be notified when those posts come out, sign up to receive updates here. I don’t spam, and you can unsubscribe whenever you want.

Interested in learning about building with LLMs?

Powered by [EmailOctopus](https://emailoctopus.com/?utm_source=powered_by_form&utm_medium=user_referral)

*Thanks to [Will Pride](https://canvasapp.com/blog) and Rowena Luk for reviewing drafts of this.*