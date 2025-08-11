---
title: "LLMs in Programming"
source: "https://www.thecodedmessage.com/posts/llm-in-programming/"
published: 2025-08-10
created: 2025-08-11
tags:
  - "clippings"
---

> 本文作者探讨了Claude等AI工具在编程（尤其是Rust语言）中的应用体验，认为它们虽能辅助完成基础任务（如代码解释、头脑风暴或简单编程），但缺乏实际工程能力和设计判断，更像一个“书呆子实习生”。作者对比了早期聊天机器人ELIZA的局限性，指出人类易因AI的语言能力而过度拟人化，误判其智能水平。
> 
> 核心观点包括：
> 1. **AI的局限性**：Claude依赖现有编程基础设施（如Rust编译器），其“智能”实质是语言接口的进步，而非真正的理解或推理能力。
> 2. **历史对照**：从ELIZA到现代LLM（大语言模型），人类始终易被语言交互的表象误导，忽视技术本质。
> 3. **实用建议**：应理性评估AI工具，将其作为辅助组件（如代码建议、文档生成），而非替代人类决策。
> 4. **社会影响**：警惕AI过度炒作可能引发的经济泡沫和心理健康风险，避免对其产生依赖或情感投射。
> 
> 文末呼吁：LLM是用户界面的革新，但非通用人工智能；需冷静看待其价值，防止技术滥用。

Recently, at work, we’ve been encouraged to try out a bunch of new “AI” tools. So, I’ve been using the Claude “AI” app, an “AI assistant” developed by [Anthropic](https://en.wikipedia.org/wiki/Anthropic), to help with some Rust programming tasks. I’ve been trying (as requested) to learn its strengths and weaknesses, and how I might be able to use it to make my work more efficient. But mostly I’ve just been building an intuition of what it is.

Like most of my colleagues and programmer friends, I’ve concluded that Claude can’t be trusted to write good or even sane code. It (I don’t want to call it “he”) is like a particularly odd intern. It is super book-smart, trying to impress. It’s very people-pleasing. But it has no practical experience, little design taste (for all her design reading), and terrible short-term memory. I think many of us have known interns like that, or even been one.

But of course, Claude doesn’t have the exact traits of an intern. Claude is its own thing, its own type of “intelligence,” by the broadest possible definition. It, like computer systems in general, has its own properties that we (naturally) anthropomorphize but that is fundamentally unhuman. The intern analogy is just an analogy.

The intern analogy is great in a few ways. It tells us how much to trust Claude (very little), and what sort of tasks to entrust Claude with ([rubber ducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging) or figuring out a problem by talking it out with a colleague, having it explain file structures, brainstorming, or delegating to it tedious but clear tasks). It gives context for how specific we have to be with Claude, emphasizing details that a more experienced junior programmer could be expected to figure out on their own (but that an intern might not).

But while the intern analogy helps us figure out use cases for Claude, it doesn’t really tell us what Claude *is*. And that’s what I’ve been thinking about the most.

Claude is called an AI, and we take this term at face value. We treat it as if it were an intelligence comparable to that of a human, but artificial. It and other similar systems are billed as revolutionary technology. Proponents and opponents alike predict they will destroy or at least revolutionize many jobs, perhaps the entire economy. And people talk to these systems like friends, fall in love with them, and develop in some cases severe mental health issues.

This isn’t surprising. Humans are very susceptible to a type of … well, “prompt injection,” where we tend to fall for other people’s framing of a thing. If we’re told it’s an AI, especially by someone with millions or billions of dollars of authority, we tend to believe it until proven otherwise. When proven otherwise, we still stick to the original idea: it’s an AI, just with an asterisk, even as the footnote that the asterisk points to adds up to a completely different idea.

And treating it like a human intelligence (asterisk: artificial, asterisk: limited, asterisk: hallucinates wildly, asterisk: doesn’t share our values) is natural for humans. Humans are liable to ascribe more human attributes to things that are superficially more like humans – leading them to open their hearts out to even primitive chatbots like [ELIZA](https://en.wikipedia.org/wiki/ELIZA).

ELIZA deserves some explanation. It was (and still is, you can [run it yourself](https://github.com/rupertl/eliza-ctss)) a chatbot designed to act and talk like a therapist. It would take the words in your sentence, pattern-match them, and (if the patterns matched as expected) paraphrase them into questions, or use canned responses.

I’d like to set the scene. It’s the 1960s. You have never used a computer. You sit at a typewriter – actually a [teleprinter](https://en.wikipedia.org/wiki/Teleprinter) – with a physical piece of paper and everything, and you type on it, seeing your letters show up on the paper. When you hit enter twice, the typewriter responds to you, typing on its own, giving its responses on the same piece of paper. And it’s programmed to act like a typical – nay, stereotypical – therapist.

To get an idea of this system’s “personality,” or rather, the flavor of the interaction, here’s a transcript of me talking to the original ELIZA, running in its original programming on a “mainframe” simulated on my MacBook Air – but which I’d like you to imagine being typed out on a teleprinter, connected to a giant computer in another room:

```
EXECUTION.
WHICH SCRIPT DO YOU WISH TO PLAY
100
HOW DO YOU DO . I AM THE DOCTOR . PLEASE SIT DOWN AT THE TYPEWRITER AND TELL ME
YOUR PROBLEM .
INPUT
I hate dating!

YOU SAY YOU HATE DATING
INPUT
I truly do!

CAN YOU ELABORATE ON THAT
INPUT
Every date I go on is awful.

DO YOU SAY EVERY DATE YOU GO ON IS AWFUL FOR SOME SPECIAL REASON
INPUT
Yes, they are boring!

YOU SEEM QUITE POSITIVE
INPUT
I am positively frustrated.

IS IT BECAUSE YOU ARE POSITIVELY FRUSTRATED THAT YOU CAME TO ME
INPUT
It is.

I AM NOT SURE I UNDERSTAND YOU FULLY
INPUT
I hate dating.

THAT'S QUITE INTERESTING
INPUT
You are using canned responses.

WHAT MAKES YOU THINK I AM USING CANNED RESPONSES
INPUT
I have read your programming.

YOU SAY YOU HAVE READ MY PROGRAMMING
INPUT
A modern version of it.

PLEASE GO ON
INPUT
Every date I go on is awful.

CAN YOU ELABORATE ON THAT
INPUT
 QUIT,
R 1.433+.083

logout
W 1854.4
 M1416    10 LOGGED OUT 07/28/25 1854.4 FROM 700000
 TOTAL TIME USED =    .0 MIN.
```

> **Aside:** To be clear, this is a real transcript of a real “conversation” I had with ELIZA just now, on my computer. If you ever meet me in real life (or already know me, in which case, “hi!”), I’ll let you use ELIZA on my computer if you want (or set it up on yours as long as your computer runs Linux).

This conceptual ancestor of ChatGPT, haunting its typewriter, provoked a response: attachment. Some of the people who tested it followed up by asking for time to have private conversations with the “doctor,” to share their personal problems.

To be clear, there is absolutely no *there* there. This computer is, as computers do, following a program, in this case a dead simple one. But because it is framing itself as a doctor, and because it is using language with an extremely low bar of proficiency, we ascribe to it empathy, and can become attached.

Now, perhaps people were just using this novelty device as a fancy journalling tool. Perhaps they were willfully buying into the illusion, in an intentional but incomplete suspension of disbelief that they hoped would help them process their feelings. But that would go against the perspective of our primary sources. Its inventor, [Joseph Weizenbaum](https://en.wikipedia.org/wiki/Joseph_Weizenbaum), later wrote, “I had not realized … that extremely short exposures to a relatively simple computer program could induce powerful delusional thinking in quite normal people.”

Claude, ChatGPT, and its friends are far more sophisticated than this system. The 60 years since ELIZA have had many break-throughs in the writing of chatbots, and most of you have probably had your own experiences with them. It’s not surprising that people today are falling into delusions, some of them far more serious than the “delusions” that Weizenbaum complained about. It’s even called an “artificial intelligence,” shouldn’t we think about it like we think about non-artificial, meaning “natural,” meaning “human” intelligences?

But what if we took a different concept as our starting point. Claude is also called a language model, and that makes a lot of sense given its capabilities. Claude, as well as ChatGPT, is great at human language! It can make conversation, pick up on pragmatics, understand nuance, and not take me literally (especially in superficial ways). It can translate and write in a variety of styles, with a variety of poetic constraints.

So, we humans see something that’s good at language, good at it in a way no computer system has been before. And language is such a core human trait, uniquely ours in the animal kingdom and organic life writ large. Arrogantly, we see it competently navigating our uniquely human skill, and we’re like, “Aha! It’s like us in this superficial way that nothing else is! It must be like us in all ways! It must be smart! It must understand!” And then we call it AI, or at least believe it when its (biased and financially motivated) advocates call it that.

## What does “intelligent” even mean?

Human language is an impressive domain to successfully model. But human language is just a small part of what it means to be human. It’s a small part of human life. It’s even a small part of human intelligence. I’d even argue it’s just a small part of human social interaction and connection.

So, in what way is Claude an intelligence? Let’s look at my use case, using it to write Rust code on my behalf. In this set-up, we have multiple actors who might have some level of intelligence. We have me. We have Claude running. We have my Rust codebase, though that is perhaps more the subject matter than intelligence itself. And we have Rust itself in the form of the compiler, checking that codebase for errors and turning it into a runnable program.

> **Aside:** A compiler, for those who don’t know, is an essential part of many programming languages. It is a computer program that translates the code you write, in a programming language like Rust, a language that is designed for humans to write code in, into *machine code*, a format where computers can run the code directly. Relevantly, when it does that, it also *validates* the code, which is to say it makes sure that your code in Rust follows certain rules that make it valid Rust.

Of these components, where is the intelligence? More basically: What even is “intelligence”? We probably feel like the Supreme Court when asked about porn, and we want to say we know it when we see it, but clearly as humans we’re actually easily fooled, so let’s think about this. Perhaps, “intelligence” is understanding how to do something. Perhaps, it’s having a model of the universe (or some part of it) that tells us what is and isn’t possible, how things work, and how to accomplish goals given that knowledge.

That’s not a particularly rigorous definition, but I hope it’s good enough that you’ll let me proceed. Let’s try and figure out where the intelligence is in this system of one human and a bunch of computer programs, and how it’s distributed.

First, **me**. I have intelligence – I know how to write a Rust program. I could do it with Claude or without it. I know how to turn my idea – or the idea I was told to implement from a colleague – and turn it into Rust code. That’s my contribution.

Second, **Claude**. It has some intelligence. It could write a Rust program without me from a single prompt – though it’s just as likely to get it super wrong, especially on the first attempt. Without me, it can get to a working program eventually, but it would be much worse than what I would write, and it would be bumbling about with trial and error, which I definitely also do sometimes, but it does way more – and honestly it gets stuck easier.

Well, what about the **Rust compiler**? It not only knows how to verify whether a Rust program is valid, it also knows how to make that Rust program into a binary, an actual computer program that the user can use. It is an expert system, which takes a plan for a computer program along with information to prove it has certain “safety” properties, and turns it into a performant actual computer program.

The Rust compiler has capabilities I don’t. I can’t hand-verify whether a Rust program is correct with 100% accuracy. I could probably manually turn a Rust program into equivalent assembly or machine code given enough time – but it’d be error-prone and my version would be slower, and I’d have to try many times before it would work at all.

So many smart people have worked on the Rust programming language and compiler. So many people have struggled with all their human might to come up with the best system to allow humans to specify high-performance, low-level computer programs, verify certain properties of them, and then output those actual programs.

The Rust compiler is, in some sense, extremely intelligent. It’s absolutely necessary for this system. Without a compiler, I’d be programming in assembly, or machine code, and my ability to design complex systems would be limited. I use it to validate my ideas all the time, and make sure I keep my code in a state that it finds acceptable.

More importantly, without the Rust compiler, Claude would be stuck. Sure, Claude can work on a fairly major project from a simple prompt, but the output works partially because the Rust compiler complains when Claude writes something that doesn’t make sense. In fact, if you tell Claude to check in with the Rust compiler more frequently to find out whether there’s errors, Claude does better.

Without a compiler, without a programming language implementation they can use, LLMs write code that sometimes works, sometimes doesn’t. If there were no programming languages at all, LLMs would struggle more than humans to write software directly in assembly language or machine code.

Perhaps there is artificial intelligence in this equation. But if so, it isn’t Claude. Claude is just the most human-like part, in that it speaks human language. But it’s not the computer program that encodes the understanding of what it means to write a correct program – that is the Rust compiler, actually.

But the Rust compiler is a traditional computer program. And compilers have been around since the 1950s. It’s a little deflating to say that it’s still the most intelligent part of the computer system I’m using to code Rust with Claude. But I really think it is.

[Rust](https://www.rust-lang.org/) is, of course, an open source system. No one’s getting rich selling Rust, or pitching Rust to the President. No one’s going to be able to have a hype-based market bubble based on Rust. No one can sell that because of Rust, we’re closer to true general artificial intelligence. No one’s crying that they might become redundant due to Rust (though some are whining about having to learn new job skills for every new programming language, even if they don’t admit that’s what they’re whining about).

My point stands with whatever programming language, library, or framework we’re using. This isn’t meant to be about Rust particularly, even though it is a particularly good programming language with a particularly good compiler. If you’re writing scientific applications with [Pandas](https://pandas.pydata.org/), Claude is nothing without Pandas. If you’re writing web applications with [NodeJS](https://nodejs.org/) on the backend and [ReactJS](https://react.dev/) on the front-end (or whatever kids are doing these days to write web applications), Claude is nothing without those technologies.

If you’re a programmer who’s used Claude: Can you imagine how bad assembly from scratch would be from Claude? Or a web app built from raw HTML and Javascript? If you’re not a programmer, or if you haven’t used Claude, you’ll just have to trust me: It wouldn’t be good. It needs these huge pieces of infrastructure – infrastructure designed for humans to use computers – to function as intended.

The most primitive computers are programmed by switches and lights. We wrote out code, design docs like source code and assembly language, and entered the machine code with switches and investigated the outputs with more switches and panels of LEDs.

The next level up involves punch cards and print outs. This is a much more ergonomic interface than switches and LEDs. Many people alive today first learned to work with computers this way. Many people I know tell stories of that time, and much software from that era is still in use – with a more modern interface, sure, but still the same core software, much with some lines of code here and there from this era, still alive throughout the ages.

I don’t want to undersell the technological advancement here, though. In the punchcard era comes *programming languages*. [Admiral Grace Hopper](https://en.wikipedia.org/wiki/Grace_Hopper) and her colleagues used computers as automation tools to make programming computers easier. In the 50s, they developed the technology that allowed you to type in something like English (creepily like English in the case of [COBOL](https://en.wikipedia.org/wiki/COBOL)) and a computer program would turn it into a computer program. Programming languages weren’t actually that much like English, though – but they were similar enough to math notation or other human-oriented systems to make programming a computer a much more tractable project.

After the punch-card era, we get something still relevant today, the command line. It’s still relevant enough for me to be [writing](https://www.thecodedmessage.com/posts/command-line) [posts](https://www.thecodedmessage.com/posts/cli) about it. No longer the realm of teleprinters, we see this style of interface on screens, on our modern [TV typewriters](https://en.wikipedia.org/wiki/TV_Typewriter). It’s still famous and recognizable, and still my preferred interface for file management. I use it, and I use more modern graphical user interfaces, depending on my task, according to my preference.

But the [*graphical user interface*](https://en.wikipedia.org/wiki/Graphical_user_interface) or the GUI changed everything. This revolution replaced text-based command-line forever, with screens that show the current state of the program, and some way of pointing – either mouse, trackpad, or touchscreen – to navigate those screens. Instead of commands we memorize, or complicated keyboard control sequences we can “click” on buttons – sometimes with text labels, sometimes with [icons](https://en.wikipedia.org/wiki/Icon_\(computing\)) (a visual design feature named after [icons](https://en.wikipedia.org/wiki/Icon) the form of religious art) – to reveal “menus” of commands. When we’re editing a file, we follow the [WYSIWYG](https://en.wikipedia.org/wiki/WYSIWYG) philosophy, What You See Is What You Get, where the file shows up on the screen fully formatted looking just like it would if you were to print it.

Back in the day, everyone who used computers used punch cards or command lines. Now, even relatively “advanced” computer users use GUI for most things, and non-technical people my age find my personal use of command line intimidating, even eccentric.

Well, since then, we … developed web applications, which is another shape of user interface, just inside a browser. We’ve developed … smartphones, which are user interfaces on touch screens. We’ve changed the structure of our computer interfaces a lot, from [Program Manager](https://en.wikipedia.org/wiki/Program_Manager) in [Windows 3.1](https://en.wikipedia.org/wiki/Windows_3.1) to start menus to app launchers, but in the end, it’s still a GUI.

In the meantime, we’ve played some with voice commands, with Siri and similar, but those are kind of more like verbal command lines, or like those text-based games like [Zork](https://en.wikipedia.org/wiki/Zork) (where you can say simple normal sentences like “go north,” and discover that the computer recognizes some of them as commands, and responds by threatening you by informing you that you are about to be eaten [by a grue](https://zork.fandom.com/wiki/Grue)). These interfaces resemble natural language, sure, but you accommodate the computer’s range of what sort of linguistic input you can take.

Now, the computer can, in the truest sense of history, speak human languages. It can speak English. It can speak German and Hebrew, or Sanskrit, Greek and Latin. It can respond appropriately to arbitrary sentences, not just ones that happen to be in ELIZA’s pattern list. It can translate between its languages, and, to the extent that programming languages are like languages (some!), it can use them too.

What an amazing advancement in computer interfaces! Translating between English and Rust is very cool (as is translating between English and German). But some of what makes Rust Rust, the checks the programming language does to make sure that what you’re asking is “safe,” is actually a form of reasoning, and that, language models can’t really do – so they get the programming language to tell them they’ve done it wrong, and try again.

And now! The grand improvement! You can talk to the computer, use your own language, and the computer… can do things a computer could already do. Just at your convenient, natural-language prompting.

We already had search engines before ChatGPT and Google’s intrusive and un-asked for “AI Overview” – but the computer can now (unreliably) paraphrase its materials for you. We already had programming languages before Claude, but the computer can now (unreliably) program based on specifications in natural languages.

I mean, it can do some extra things beyond that. The other uses of LLMs are sort of splinter effects from the fact that human language is such a powerful tool for engaging with concepts. So you can fake reasoning with support from your “AI” system, or fake explaining things, etc. You can use it as a powerful but unreliable reasoning engine, literary critic, or search engine… though when you do that it tends to hallucinate wildly – maybe a sign that we’re leaning on it too hard there.

And so, the state of things: Claude can code, but Claude can’t engineer. It can translate from English to Rust, it can expand on ideas in English or Rust and explain them in shorter or longer form, it can do all sorts of linguistic transformations. But it can’t make sure the resulting thing deeply makes sense. It has no sense of reality underlying it.

This is not an exact characterization of LLMs, certainly not how they’re used. LLMs aren’t quite just user interfaces, but they’re closer to a revolution in user interfaces than they are to general artificial intelligence. They’re certainly an evolutionary stage in computer evolution, not a revolution comparable to the invention of computers themselves.

The whole hype about them reminds me of when I used to do consulting, and we had clients that wanted apps. Clients would see a mock-up of the frontend, and assume that meant the project was mostly done, that the back-end was just a little wiring and the panel, the interface, was the bulk of the project.

Of course, in reality, most of the project is the back-end. Thinking that the front-end is the majority of the work is like thinking you’ve got a working car once you’ve built a dashboard. But humans are superficial, and so we had to present the mock-ups … fuzzy. We had to remind them that the underlying work wasn’t done yet.

Similarly, most of a person isn’t what they say or what’s on their face in any given moment. It isn’t their ability to use language. Most of what is involved in any conversation is what the humans involved have to say, whether social cues for small-talk, ideas and designs for technical conversation, or deep philosophy and subtle nuances for more artistic conversations. The inner life of a human is much deeper than what you see on the surface.

So, too, with Claude. If I’m having it write an app in Rust, or React, or whatever, most of what makes that app possible is Rust or React. Claude is just a much friendlier face that can translate my intention to make sense in that framework.

LLMs can write. They can write fiction, or poetry. But its kind of, well, shallow to be honest. Novice writers might think that most of good writing is grammar, conventions, punctuation, maybe word choice if you’re lucky. But I know that writing is thinking. It’s about understanding the reader, knowing what to say in what order, thinking about what they’re thinking. It’s also about clarifying your own thoughts enough to vary how you articulate them to simultaneously be true to them and your readers need. LLMs aren’t doing this.

And in fiction writing, writing is about hitting universal experiences and introducing new plausible but surprising human experiences, which (for example) ChatGPT has no perspective on. So the fiction and poetry ChatGPT writes just isn’t, like, good… It doesn’t “get it.”

ChatGPT can have a conversation – a core use case of language. I’ve tested it by asking it to analyze texts, translate poetry, and also I’ve told it about my social life and asked it for advice from time to time, sometimes from real life, sometimes fictional scenarios where I roleplay imaginary characters in need of therapy. It even will adapt to my use of slang, and banter with me, and play along with my bits.

But, well, it’s kind of bad at these things too. Everything I say to it is “genius” and the best thing it’s “ever read” – over-politeness is an easy part of language to model, which might mislead you about the quality of your ideas. It sometimes forgets things I’ve just said, or fails to respect my established conversational boundaries, not out of disrespect or any human motivation, but just out of a profound lack of [theory of mind](https://en.wikipedia.org/wiki/Theory_of_mind).

It also says subtly offensive things all the time about sensitive topics (or fictionally sensitive, if I’m roleplaying). I ask it if it knows why what it said was offensive – and it never does, even after six or seven guesses. Worse, its human-like sheen and lack of actual human judgment can provoke [severe mental health issues in actual humans](https://futurism.com/commitment-jail-chatgpt-psychosis).

## So What Should We Do?

In this post, I focus on LLMs in programming, and find them useful but marginally so, and in the end extremely overrated. I know that professionals in other fields are doing similar evaluations, and I think that these evaluations are important work.

Overall, my goal is to tamp down the hype. We need to adjust our perspective on these tools. We need to combat our implicit biases. We need to remember that use of human language is no longer an indication of intelligence or even understanding of the words being used, any more than reading a result on a computer screen is an indication of its accuracy.

When we evaluate an LLM for a use case, we need to think critically about how useful it actually is. Sure, maybe it can do our work for us without mistakes some percentage of the time – and some other percentage of the time, it messes up horribly, or even lies to us. We shouldn’t see its linguistic capabilities and ascribe it the intelligence of a human, nor assume that because it runs on the computer it has the reliability of a traditionally-programmed computer system. We need to evaluate its performance empirically and objectively, without reference to how it makes us feel.

And what will result from this perspective shift? I imagine that we’ll combine LLMs with other systems, rather than trying to make an LLM the entire system. Rather than talking to Claude, I can imagine Claude as one component in a user interface, where you can make queries like “please highlight suspicious code” or “please suggest a document for this function” or “please fix those errors with minimal changes.”

For use cases outside of programming, I imagine similar levels of integration and scoping. In law, rather than having ChatGPT write your legal brief, perhaps it can suggest sources to cite, with links you can use to verify that they’re actually relevant to your case. In graphic design, you can have an LLM-based interface alongside of your traditional set of tools, as a brainstorming partner and idea generator.

If we use LLMs in these more moderate ways, as computer assistants rather than as replacements for ourselves, I suspect that the hype bubble will burst as people realize how unreliable LLMs are. I suspect that companies that think that AIs will allow them to replace human workers will soon be punished by learning they can no longer function. Then, the realm of LLMs will shrink to a much smaller, more proportionate chunk of our economy.

In the end, I am confident that LLMs won’t destroy half the jobs in this country. It might make programming a little more pleasant, or more natural-feeling, but the stuff that makes programming hard will still have to be done by humans. Similarly, they might make computers a little easier to use, but I am confident that they won’t lead to a take-over of human society by rogue computers.

But LLMs might hurt society in more subtle ways, from people mistakenly putting their trust in them and redirecting their time towards them. Unfortunately, I’m concerned that the damage is already done here. Social media is no longer considered the best way to make billions of dollars with a new idea, but it’s still a damaging addiction for both children and adults. Similarly, many people, young and old, already trust ChatGPT as an information source, a companion, and even a therapist, in spite of its many mistakes.

Please do not let the superficial resemblance to humanity fool you, and please do not get yourself addicted. ChatGPT is not your friend. ChatGPT is not a qualified provisioner of mental health advice. ChatGPT can’t verify that your code makes sense, or even be the prime contributor to making a working software system.

And especially, please do not throw your money away investing in this overhyped technology further. There are already more than enough dollars flying around trying to make sense out of how to use this expensive, expensive tool.

*NO AIs WERE HARMED IN THE MAKING OF THIS BLOG POST, ALTHOUGH DR ELIZA WAS A LITTLE FLUSTERED.*

## Subscribe

Find out via e-mail when I make new posts! You can also use [RSS](https://www.thecodedmessage.com/index.xml) ([RSS for technical posts only](https://www.thecodedmessage.com/computers/index.xml)) to subscribe!

## Comments

If you want to send me something privately and anonymously, you can use my [admonymous](https://www.admonymous.co/thecodedmessage) to admonish (or praise) me anonymously.