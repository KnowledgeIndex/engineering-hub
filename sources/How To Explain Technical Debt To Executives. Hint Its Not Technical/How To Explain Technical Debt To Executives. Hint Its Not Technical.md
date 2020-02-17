# Source

[[Medium] How To Explain Technical Debt To Executives. Hint: It’s Not Technical.](https://medium.com/startup-patterns/how-to-explain-technical-debt-to-executives-hint-its-not-technical-c581563375dc)

# What Is Technical Debt?

“Technical Debt” is **an emergent condition** present in any digitally-enabled business enterprise that has now discovered its **technology** is **too rigid to accommodate new business objectives** that have come to light. It is too rigid because of past system design and architecture decisions that were made, *usually with the best intentions*, based on the engineer’s knowledge of business objectives that were available at the time.

Good engineers try as a professional point of pride to keep as many options  open as they can in the technology for as long as possible. They know  that requirements will change, and they try mightily to preserve  flexibility, if for no other reason than to save their own sanity when  inevitably requirements change. And change they always do.

But eventually, the system must be optimized for a particular set of  business objectives and at the expense of others. In other words, we  future-proof, to the best of our ability, but we ain’t got a crystal  ball.

# Why Is Technical Debt Important?

The only reason to build technology at all is to automate a manual process  for some human purpose. While we could include non-profit and public  sector examples here, the vast majority of technology is built for  profit-seeking businesses. So those processes have a business purpose.

Automating a business process requires knowing everything about the business that  is relevant to that particular process. But businesses change. Customers want different things. Competitors come along with new capabilities  that we have to copy them in order to keep up (maybe). Partnerships and  mergers arise where we need to replace one system with another slightly  different system that does more or less the same thing. All of this  variation puts pressure on the original design of the system that is  automating the business process in question.

Software can be made to be quite flexible to change. But that flexibility comes  at a cost. It takes time and care to make a flexible system, and that  time is in direct conflict with the pressures from the business to meet  customer needs ASAP.

# Where Does Technical Debt Come From?

Technical Debt is not actually a technical problem. It manifests as rigidity in a technology system, sure. But it is actually a byproduct of a process  and communication problem, which themselves are rooted in culture  problems within the organization.

Organizations that are highly siloed, where the developers, product managers,  designers, and other functions tend to work in isolation, are much more  likely to suffer from technical debt. This is because decisions made by  engineers in the code — decisions that are making explicit trade-offs in favor of one direction over another — are so many layers removed from  the actual needs of the customer. The customer need is translated  through these layers from one hand to another, and in the process the  richness and fidelity of the customer need is lost. The engineer then  does their best to interpret the needs of the customer, but sometimes  gets it wrong.

In addition, work is often removed from its business and strategic  context. Leadership makes demands on the product organization based on  some mystified business strategy, usually with little in the way of  measurable goals. The product managers then have to interpret that  strategy into technical requirements, and instruct designers to build  experiences that meet those goals. Finally, at the end of the telephone  game, engineers get some design mocks and poorly written user stories  and have to make sense of them in order to make the product real. Much  misinterpretation can occur in this kind of arrangement.

In other words, technical debt occurs more often in businesses where there is a lack of trust and a lack of teamwork and collaboration.

# How To Bring Engineering And Business Together.

Given that technical debt arises from human communication problems, and not  in fact any technical problem, the first component of the solution is to stop focusing on the software altogether and instead focus on the  culture of the organization. Everyone is happier when they have the  trust of their peers. And happier teams produce better business  outcomes.

1. We should focus on closing the gap between the engineering and business  cultures within the organization. Engineers who have a clear sense of  empathy with the customer tend to make better decisions about the  technical design of the systems they are building. Business leaders who  trust their engineers to make more decisions don’t have to focus on  micro-managing the development of features that go into the product.  Micromanagement doesn’t serve anyone, and yet it is very hard for  management to let go of control if they don’t trust that customers needs will be adequately addressed by engineering teams.
2. We should put systems in place that enable the engineering teams to have  direct contact with customers, and place the different functions  (engineering, design, and product management) into dedicated  cross-functional teams. Business leaders should specify the business  objectives required, but leave the implementation specifics to the team.
3. Engineering teams should learn to articulate system design decisions in terms of  concrete and measurable business impact. If their choice of design  pattern or technology doesn’t have a verifiable business outcome tied to it, it should be classified as waste.
4. Technology leaders should be trained to leverage their bilingual skills between  the business leadership culture and the technical engineering culture,  and work to create more opportunities for trust-building collaboration  and information sharing between the clusters and layers of the  organization.