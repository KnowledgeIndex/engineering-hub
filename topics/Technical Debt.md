> As an envolving program is continually changed, its complexity reflecting deteriorating structure increases unless work is done to maintain or reduce it
>
> Meir M Lehman (1980)

# What Is Technical Debt?

“Technical Debt” is **an emergent condition** present in any digitally-enabled business enterprise that has now discovered its **technology** is **too rigid to accommodate new business objectives** that have come to light. It is too rigid because of past system design and architecture decisions that were made, *usually with the best intentions*, based on the engineer’s knowledge of business objectives that were available at the time.

Good engineers try as a professional point of pride to keep as many options  open as they can in the technology for as long as possible. They know  that requirements will change, and they try mightily to preserve  flexibility, if for no other reason than to save their own sanity when  inevitably requirements change. And change they always do.

But eventually, the system must be optimized for a particular set of  business objectives and at the expense of others. In other words, we  future-proof, to the best of our ability, but we ain’t got a crystal  ball.

# Why Is Technical Debt Important?

The only reason to build technology at all is to automate a manual process  for some human purpose. While we could include non-profit and public  sector examples here, the vast majority of technology is built for  profit-seeking businesses. So those processes have a business purpose.

Automating a business process requires knowing everything about the business that  is relevant to that particular process. But businesses change. Customers want different things. Competitors come along with new capabilities  that we have to copy them in order to keep up (maybe). Partnerships and  mergers arise where we need to replace one system with another slightly  different system that does more or less the same thing. All of this  variation puts pressure on the original design of the system that is  automating the business process in question.

Software can be made to be quite flexible to change. But that flexibility comes  at a cost. It takes time and care to make a flexible system, and that  time is in direct conflict with the pressures from the business to meet  customer needs ASAP.

# Cost of Tech Debt

## What software quality is?

Here lies the first complication - there are many things that can count as quality for software. I can consider the user-interface: does it easily lead me through the tasks I need to do, making me more efficient and removing frustrations? I can consider its reliability: does it contain defects that cause errors and frustration? Another aspect is its architecture: is the source code divided into clear modules, so that programmers can easily find and understand which bit of the code they need to work on this week?"

I thus divide software quality attributes into external (such as the UI and defects) and internal (architecture). The distinction is that users and customers can see what makes a software product have high external quality, but cannot tell the difference between higher or lower internal quality.

A user can judge whether they want to pay more to get a better user interface, since they can assess whether the user interface is sufficiently nicer to be worth the extra money. But a user can't see the internal modular structure of the software, let alone judge that it's better.

So why is it that software developers make an issue out of internal quality? Programmers spend most of their time modifying code. Even in a new system, almost all programming is done in the context of an existing code base. When I want to add a new feature to the software, my first task is to figure out how this feature fits into the flow of the existing application. I then need to change that flow to let my feature fit in. I often need to use data that's already in the application, so I need to understand what the data represents, how it relates to the data around it, and what data I may need to add for my new feature.

## What is cruft?

Its the difference between the current code and how it would ideally be. A common metaphor for cruft is Technical Debt. The extra cost on adding features is like paying interest. Cleaning up the cruft is like paying down the principal. While it's a helpful metaphor, it does encourage many to believe cruft is much easier to measure and control than it is in practice.

**One of the primary features of internal quality is making it easier for me to figure out how the application works so I can see how to add things**. If the software is nicely divided into separate modules, I don't have to read all 500,000 lines of code, I can quickly find a few hundred lines in a couple of modules. If we've put the effort into clear naming, I can quickly understand what the various part of the code does without having to puzzle through the details. If the data sensibly follows the language and structure of the underlying business, I can easily understand how it correlates to the request I'm getting from the customer service reps. Cruft adds to the time it take for me to understand how to make a change, and also increases the chance that I'll make a mistake. If I spot my mistakes, then there's more time lost as I have to understand what the fault is and how to fix it. If I don't spot them, then we get production defects, and more time spend fixing things later.

**Customers do care that new features come quickly**. Here we see a clue of why internal quality does matter to users and customers. Better internal quality makes adding new features easier, therefore quicker and cheaper. 

**Even the best teams create cruft**. Many non-developers tend to think of cruft as something that only occurs when development teams are careless and make errors, but even the finest teams will inevitably create some cruft as they work.

# Personal experiences from developers

## [Mark Dalgleish Tweet](https://mobile.twitter.com/markdalgleish/status/1205352261569806336)

> "Why are my devs always talking about technical debt? My old devs didn't talk, they just shipped."

> The old devs understood the every decision creates technical debt, so they reasoned carefully, documented their assumptions and moved on.

> All assumptions were lost in the third last company mandated consolidation to a single knowledge repository, because users didn’t get write access, and instead had to follow a 10 step process with two management signoffs to authorise a governance delegate to upload each document.

> Oh god you're giving me flashbacks at a bank none of us could edit the wiki, so we had an unofficial windows share to share our knowledge in And I got admonished for putting stuff THERE without team lead approval.

> After that I just gave up and kept my documentation to myself

> It’s really funny that people will pay to service their cars on schedules to avoid catastrophic failure - but turn their nose up at the idea software needs the same thing.

> I wish we’d stop calling it technical debt. Biz think they’re taking on debt to buy something, how about we distinguish technical maintenance (can we avoid high-maint. cars, or depreciating asset/yield?) vs technical misadventure (racing in a demolition derby on our way to work)

# Resources

- [Refactoring to Immutability - Kevlin Henney](../sources/Refactoring to Immutability - Kevlin Henney.md)
-  [How To Explain Technical Debt To Executives. Hint Its Not Technical.md](../sources/How To Explain Technical Debt To Executives. Hint Its Not Technical/How To Explain Technical Debt To Executives. Hint Its Not Technical.md) 
-  [Is High Quality Software Worth the Cost Martin Fowler](../sources/Is High Quality Software Worth the Cost Martin Fowler/Is High Quality Software Worth the Cost Martin Fowler.md) 
