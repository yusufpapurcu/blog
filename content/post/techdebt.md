+++
title = "TechDebt Paper from Google"
date = 2023-11-05
+++
**Table of Contents:**
- [Disclaimer](#disclaimer)
- [Introduction](#introduction)
- [Categories of Tech Debt](#categories-of-tech-debt)
- [How to Handle Tech Debts? (my ideas)](#how-to-handle-tech-debts-my-ideas)
- [How to Handle Tech Debts? (what paper says)](#how-to-handle-tech-debts-what-paper-says)

## Disclaimer
This writing heavily involves my views, if you want to see the actual paper(5 pages), here is the sauce -> [https://www.computer.org/csdl/magazine/so/2023/03/10109339/1MESXKyAYNO](https://www.computer.org/csdl/magazine/so/2023/03/10109339/1MESXKyAYNO)

## Introduction
We kickstart the paper with the history of **Technical debt** definition. [Ward Cunningham](https://en.wikipedia.org/wiki/Ward_Cunningham) gives the definition in this sentence:

> Shipping first time code is like going into debt. A little debt speeds development so long as it is paid back promptly with a rewrite. 

He also mentions that engineering organizations can went stand-still mode under highly unpaid TechDebt.

We can say the TechDebt as a term means the negative side in tradeoff between Delivery Speed and Quality (not always shows up in product level). It's important to **not** use this term as an excuse to engineers writing bad code.

## Categories of Tech Debt
Then we are moving into the research itself, Google collects data about the internal teams view on TechDebt through engineering satisfaction surveys.

And from these surveys, they refined 10 categories of TechDebt.
- Migration needed/in progress.
    - I think this references things in project level, as an example migrating a temporary solution into proper place. It's quite common to create quick (and non-properly placed) solutions to unblock product.
    - Let's imagine a example, you need to send settlement report emails into your clients. Proper way of doing this is exporting settlement data from settlement processing system and handling the report creation outside of that domain right? But in reality it's quite common for settlement system to own this responsibility for some time until proper solution is ready.
    - In the example it might be a big techdebt for engineers in settlement processing, but in the other hand it's better to do this fast and unblock legal issues.
- Documentation on projects and APIs.
    - I can relate to this easily. API documentation is a thing you could automate, but project documentation requires pure effort most of the time. And missing documentation slows down the team in the long run.
- Testing
    - Not testing experimental features/modules in your system might help to speedup development (not always). But once that experimental thingy start used seriously testing is a must.
    - For long run having trustable tests helps changes to be applied much easily and without fear. Also (for me) tests could be considered a good part of documentation. Since they give a good idea about the behavior.
- Code Quality
    - No one likes working on poorly designed code bases.
    - Worth to mention code quality could decrease by time even you don't change it.
- Cleanup of the Dead code. 
    - Features removed/replaced in higher level but not cleaned up properly.
- Code degradation
    - This is the part I mentioned in code quality. tbh they relate a lot and I'm having issues to differentiate them.
    - Maybe we can say code quality is covers a higher scope. Degradation is just a sub-scope.
- Team expertise on Project 
    - This happens after handovers and starting of the projects. 
    - If the team doesn't grab the essence of the project they might fail to maintain, or even properly design it.
- Unstable Dependencies
    - It explains itself quite a bit. Having a changing dependency might be exhausting. 
    - If it's on a legacy/on-maintenance project, exhausting effect gets buffed xd
- Migrations Poorly Executed
    - You might end up having this state between migrations or abandonment of a migration. Bad side is having to maintain different code bases doing similar things.
- Release of a Project
    - As an example, rolling up a new feature without observability creates this type of techdebt. It's a visible trade-off to take.

All code has techdebts and the type of a technical debt could change between projects.

Techdebt definition is includes the current and possible better version of the codebase.

## How to Handle Tech Debts? (my ideas)
In the part of measuring, paper goes and talks about their failing experiments. I will just write the doable way in my view.

Create a TechDebt section in your work management tool, let engineers create, classify and rate techdebts they faced. This is a good and applicable way to measure tech debt from my understanding.

## How to Handle Tech Debts? (what paper says)
To manage technical debt paper gives these three ways:
- TechDebt management framework.
- TechDebt management maturity model.
    - Reactive 
        - They don't have any process against the techdebts.
        - Doesn't mean they don't work on them, everyone works on techdebts. Reactive teams just doesn't have a process in place to manage their techdebts.
    - Proactive 
        - They define and track techdebts. Also they rate their urgency and how important are they for relevant work.
    - Strategic
        - As a plus to proactive maturity, they identify/address route causes and do proper decision making on them.
    - Structural
        - As a plus to strategic maturity, they standardize the techdebt management.
- Evangelism on best practices to manage TechDebt.
- Proper tooling for identification and management of TechDebt.

So that's all from the paper. I'm on my way to try out classification and management procedures. Hope you read until here.
