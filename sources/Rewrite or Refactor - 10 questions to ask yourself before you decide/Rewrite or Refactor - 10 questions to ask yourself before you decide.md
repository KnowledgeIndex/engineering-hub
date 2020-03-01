# Source

[Rewrite 🖋 or Refactor 🧹? 10 questions to ask yourself before you decide.](https://dev.to/ericathedev/rewrite-or-refactor-10-questions-to-ask-yourself-before-you-decide-2b6m)

> *The code is horrible. Looking at it makes you want to tear your hair out. Something has to be done! The question is, what are you going to  do?*

You have two options:

- You either *rewrite* and replace it, building a whole new sub-system which will eventually result in the old version of the code being deleted.
- Or else you *refactor* it, i.e. *applying a series of  small behavior-preserving transformations, each of which "too small to  be worth doing". However the cumulative effect of each of these  transformations is quite significant.* ([Refactoring - Improving the Design of Existing Code by Martin Fowler, with Kent Beck](https://martinfowler.com/books/refactoring.html)) 

The question is, which do you pick? Your heart says *"Rewrite it, this code is beyond hope!"*. The internet says *"Refactor it, rewriting is evil!"*. What should you do? The answer is, as it often is, *"it depends"*. 

Here are my 10 questions to ask yourself to decide whether to *rewrite*, or to *refactor*:

| №    | Question                                                     | Rewrite                           | Refactor                                         |
| ---- | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------ |
| 1.   | How much time ⏰ do you have? If you start rewriting, are you committed to finishing it? | Lots of time                      | Not a lot of time                                |
| 2.   | Do you have the right people 👩🏻‍💻👨🏽‍💻👩🏿‍💻👨🏼‍💻 available  to make this change happen? (People with backend/frontend/tech ops etc  skills as applicable) | Yes we do                         | No we don't                                      |
| 3.   | How much of the system as a whole 🌐 are you rewriting?       | A small part or a specific domain | The whole system or most of it                   |
| 4.   | What is it about the code that is bad 🗑?                     | The whole design                  | The way it's written                             |
| 5.   | How often do you predict 🔮 (hopefully know) this code will need to change in the future? | Very often                        | Not often                                        |
| 6.   | How mission critical and/or valuable 💸 is this feature or domain? | Very valuable                     | Not very valuable                                |
| 7.   | Do you understand 🧠 the existing code? Do you know in detail what it does? Are the requirements clear? | Yes                               | No                                               |
| 8.   | Is it just the code that is bad, or do you also *need* (not just want!) to make changes to the technology stack 🤖 being used? | Technology                        | Just the code                                    |
| 9.   | If you had to rewrite it, how long will it take you to ship 🚢 the first part of your rewrite to production, with *actual people* using it? | A few days, a couple of weeks max | Months, or even years                            |
| 10.  | Why are you doing this? No really: Dig deep and be honest! I'll wait 💅🏻. | I believe this is the best choice | I hate this code & my ego thinks I can do better |

Here's the catch: If at least *one* of the questions above  resulted in you picking an answer in the "refactor" column, hit the  brakes on that rewrite! The risk of refactor is much smaller than a  rewrite. The cost of a rewrite gone bad are immeasurable. 

Does this mean you should *never* rewrite code? No! What it  does mean is that if the conditions are not right you should go back to  the drawing board and make do for now until the conditions *are*  right. Sometimes a rewrite is truly the better option, but avoid doing  it just because it sounds like the easiest or most appealing.

