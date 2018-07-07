---
path: "/use-cases-in-agile-dev"
date: "2018-07-07T17:16:33.000Z"
title: Use Cases in Agile Development
---

Use case analysis probably reached its pinnacle in the 90s. Since then this analysis technique has seen an inexorable decline, going the way of many other traditional analysis techniques when agile development methodologies took over. Today, user stories are the norm for writing requirements for agile development.

However, I believe use cases are actually still very relevant in today's agile development projects, but in a different way than originally envisaged. Here explain what a use case is,and why it has not worked as originally envisaged. I make the further case that use cases are still a great way to document, test or otherwise explain how a system works even in agile projects.

## The Actor-Goal Model of Use Cases

Use cases were first invented by Ivar Jacobson in 1967, and formalized in a paper in OOPSLA in 1987. Alistair Cockburn brought in the all-important "Actors and Goals" concept in 1994 - the idea that use cases should be organized around actors who may want to achieve a specific goal from the system under discussion.

A use case is composed of a series of scenarios - these can be split into success and failure scenarios. A success scenario is one where an actor achieves their goal, and failure scenario is one where the goal is not achieved, or achieved only partially. The use case essentially describes the interaction between the system and the user for these various scenarios. Here is an example of an **ATM Withdrawal** use case. I have kept this short and informal.

```text
Actor : Customer
Goal : Withdraw money

Main Success Scenario :

1. Customer enters card
2. System asks for pin
3. Customer enters pin
4. System validates pin and asks for withdrawal amount
5. Customer enters amount
6. System validates amount exists in primary account and dispenses money
7. Customer collects money and card
8. System shows login screen

Insufficient funds scenario :

6a. System validates amount exists in primary account - and shows an error message
7. Customer collects card
8. System shows login screen

Wrong Pin Scenario :

4a. System validations pin, which fails
4b. If no of retries have exceed three, system shows login screen (failure scenario)
4c. System shows pin reentry screen - and the steps 3 onwards are repeated
```

The example above is a "task-level use case" which is a goal an actor may achieve in a single interaction with the system, versus "summary-level use cases" which are akin to business processes and may be larger goals achieved by multiple actors interacting with the system at different times. 

It is also an example of a "black-box" use case, where we look at the system as a black box. A "white-box" use case may refer to interactions between internal system components. For example 

```
4. ATM System sends pin to Core Banking System for validation
5. Core Banking System responds with confirmation of valid PIN
6. ATM System asks customer for withdrawal amount
```

## Use Cases and Agile Development

Why did use cases fall out of favour?

Use Cases were introduced when the waterfall model of development was the norm. Waterfall development is (typically) characterized by a large up-front analysis, followed by a long development cycle, a long test cycle, and a big bang release. Over time, there has been a realization that this model leads to very late feedback from users. It is very difficult to know all requirements upfront. And finally, requirements change when users actually see and interact with the system. And it makes releases high-risk both in terms of the late feedback and the fact that so many things are changed at the same time.

For these reasons, agile development advocates a different model of development. Start with a small set of "user stories" and build an initial version of the system quickly. Get feedback, identify new stories and iterate. Release early and often. To reduce release overhead, write automated regression tests which give a safety net that we have not broken anything and allow us to release without a long test cycle.

Thus, the agile way of development thus encourages a brief and short initial specification to get started, then a series of changes specified as stories for changing the system. Where then is the place for large, upfront use cases in agile development? And should software engineers spend time learning this tool? I believe the answer is yes, as outlined below.

First of all, use cases are still a great way to think of **organizing automated functional tests**. In many projects, teams write functional tests for stories - but stories are a change specification, and are usually a terrible way to organize functional tests around. Instead, organize functional tests around use cases - actors and their goals. In each group of tests, go through the success and failure scenarios of the use cases, when the goal is either achieved or it fails.  

In fact, BDD (Behaviour Driven Development) is a popular style of writing tests which double as documentation of how a system actually works. A marriage of BDD style tests organized around use cases would be a great way to organize both tests and the associated documentation.

Secondly, user stories can be provided a **better structure by linking them to use cases** identified in BDD style functional tests. The typical use story can be enhanced by a clear linkage - here's a highly simplified example  :   

```text
As a customer,   
I want to be shown my frequent withdrawal amounts during ATM Withdrawal   
So that I can have a faster withdrawal experience
```

Or,

```text
As a customer,   
I want my mobile number to be captured during Account Setup,   
and an SMS sent after a successful or failed ATM Withdrawal   
So that I can be made aware quickly of any unauthorized withdrawal
```

When a user story is developed, identify which use cases are impacted and change those functional tests (or create new ones if the story results in a new use case being created). 

## Use Cases vs User Stories

Based on the discussion above, we can see both the differences and relationships between the actor-and-goal style use cases and user stories.

Use Cases describe how a system works (or should work). User Stories describe how a system should be changed. This is like the difference in a bank account between seeing your balance, or seeing the credit or debit entries - the two are related, but very different.

Both use cases and user stories refer to goals, but these goals are very different. The user story explains the goals for why a change to the system is needed. The use case goal refers to the goal the user wants to achieve at the end of the use case. For example, in the second story above, the goal of the user story is to change the system to reduce instances of fraud during withdrawal, while the use case itself still is based on the customer's goal of withdrawing money.

User stories are a great unit of development. Use cases are a great unit of testing and documentation of the system.

Use Cases have a permanent existence in the system - user stories are transient because they describe a change to the system. Once the change is complete, their primary significance is historical. It is hard to understand a system just by reading the stories over time - that would be like trying to understand a code base by reading patches in sequence - useful to understand the evolution, but painful for understanding the current state. But you could understand a system, in theory, by reading the BDD specs of a system organized as use cases.

Thus, though different, use cases and stories are related. Lets take the examples above and see the relationships:


| Use Case | Goal | Original ATM Withdrawal Story | Show Most Recent Withdrawals Story | Send SMS on Withdrawal Story |
| ------------- | ------------- | ------------- | ------------- | ------------- |
|  |  | Goal : Withdraw Money | Goal : Faster Withdrawals | Goal : Detect Fraud Early |
| Account Setup | Create a new account | | | Change - Capture mobile number on setup |
| ATM Withdrawal | Withdraw Money | New use case. This story could be written as a use case. Note the goals are the same. | Change to show recent withdrawals after pin entry | Change to send SMS after withdrawal  |

The table shows that when a use case is first created, the user story is often just a use case. The difference is that it may not be exhaustive, as we may not handle every scenario of the actual use case. Instead we build the minimal use case which provides some value. 

We then evolve the use case by adding further stories. The "Show Most Recent Withdrawals" story changes the "ATM Withdrawal" use case to improve the user experience. The "Send SMS on Withdrawal Story" adds the sending of SMS to notify the customer for early fraud detection. But note that this story also impacts the "Account Setup" use case - a story, thus, could impact multiple use cases.

## Adopting Use Cases in Agile Projects

If you are a software engineer (a developer, a QA, a BA or an architect), and you have not added use cases to your engineering toolkit, I would suggest you do the following :

- Do read "Writing Effective Use Cases". Ignore the upfront analysis aspect, but pick up the concepts of task and summary level use cases, of white and black box use cases, the key idea of actors, stakeholders and goals and the main, exception, success and failure scenarios. I promise you it will make you a better software engineer. You can leave the more elaborate aspects for a situation where you actually need them and just focus on the bread and butter concepts.  Also ignore use case diagrams - they are largely useless and have caused no end of confusion. Use cases are a text-based technique for analysis.

- In any project you are working, write tests in BDD style organized as actor-and-goal based use cases. When a new story comes, don't directly create a test for the story - instead consider which use case it impacts, and change the test cases for that use case. Your commits can link the change to the story. 

- Similarly, write stories that explicitly references the BDD use cases that they impact. This will improve the quality of impact analysis and improve communication in the whole team. If you identify a completely new use case (this means there is a new goal for the actor which did not exist before), then try and explicitly identify that as well.

## References

* [Writing Effective Use Cases](https://www.amazon.com/Writing-Effective-Cases-Alistair-Cockburn/dp/0201702258) by Alistair Cockburn is a great book - but read it with a critical eye for how it would work in agile projects.

