+++
title = "Implementing Redis with Rust"
date = 2024-03-24
+++
**Table of Contents:**
- [My adventure of implementing Redis in Rust](#my-adventure-of-implementing-redis-in-rust)
  - [Where is the idea comes from?](#where-is-the-idea-comes-from)
  - [Searching for resources](#searching-for-resources)
  - [Found the resource, lets start!](#found-the-resource-lets-start)
  - [End of the entrance post...](#end-of-the-entrance-post)
- [Skill issues with event loop](#skill-issues-with-event-loop)
  - [Move closure](#move-closure)
  - [I want a different way of handling concurrent clients](#i-want-a-different-way-of-handling-concurrent-clients)

## My adventure of implementing Redis in Rust
### Where is the idea comes from?
Lately I got an email from a big sized company HR providing some open positions, and one caught my eyes. A position in a team that builds **a global low-latency key-value data store**.

That's the story where is the idea of trying to implement redis from start came in. Of course there was a background that why this topic interests me (Like following Phil Eaton, seeing his database related contents) but I won't dig deeper.

### Searching for resources
So I started looking around for resources to get an idea about how to do it. Luckily after spending some time staring [redis codebase](https://github.com/redis/redis) I found CodeCrafters, and their redis course.

Good to mention for time being I'm not sponsored by CodeCrafters, I was using their campaign while writing this. I didn't even paid for it :D

### Found the resource, lets start!
CodeCrafters supports multiple programming languages, but in between them I was interested in Rust and C#. But this time I'm choosing Rust. I flipped a coin to decide in between btw.

Following the course is easy, it doesn't even require any git knowledge at all. You complete the task, push it to the remote and they test and mark the step as passed.

In past I was using Exercism which followed a similar route, I think they were using a cli for this but git way of doing this is not bad as well.

### End of the entrance post...
This one was the beginning of the journey, I am not planning to write for every small detail but while I go further in the course I will take notes here about the interesting and time consuming topics. 

My goal is leaving notes for myself but you're always welcome to read and suggest better way of doing things!

## Skill issues with event loop

### Move closure
Everything was going cool until the task "Handle concurrent clients" come along. My first intention was just spawning a thread for every request and calling it a day. But while writing I realised I was using this move closure but didn't dig deeper what it **exactly** means.
```rust
std::thread::spawn(move || handle_connection(&mut _stream));
```

[What I did was going directly to Holy Rust Book and searching for the usages of move closure and found it!](https://doc.rust-lang.org/book/ch16-01-threads.html?highlight=move#using-move-closures-with-threads)

(Yes it was my skill issue to realise we are passing the ownership into spawned thread)

### I want a different way of handling concurrent clients
Actually it was a different suggestion in the task, it was saying the original redis implementation uses "event loop" so maybe I can try that out too. Also spamming a new thread per request was not satisfying enough for my curiosity on the topic after hearing there is another way to do it.

I started searching videos, blogs and talks about the topic. Here is two that helped me understanding the concept.
- [What the heck is the event loop anyway? | Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [https://rohitpaulk.com/articles/redis-3](https://rohitpaulk.com/articles/redis-3)

The problem was the ruby equivalent of `IO.Select` was not exists in the Rust std library. But luckily I found it under tokio!

[https://github.com/tokio-rs/mio](https://github.com/tokio-rs/mio)

It even had the simple example I needed! But wait a second, it's not working...

(after digging it for 30m I gave up and moved on)

(but one day in future I will get back and learn how to use mio's event loop implementation)