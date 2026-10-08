---
type: posts
tag: blag
layout: post
title: Interviewing engineers
---

*This post was originally published on the [SADA Engineering Blog](https://engineering.sada.com/interviewing-engineers-at-sada-b5b3c67d396f).*

In my [numerous job hops](/resume.pdf) over the last few years I've experienced all ranges and types of interviewing. At SADA, we've been able to build out interview scaffolding that has proven to be effective for both parties—the interviewers and the candidate. This post is a rough outline of how we move engineering candidates from recruiting to hiring. There shouldn't be any big secrets here. This guide will help you know what to expect, but you certainly want to be prepared with the right mix of skills and experience, as well.

Please note that interviews for leadership candidates are out of scope for this post.

<!-- toc -->

- [General approach](#general-approach)
- [Rounds](#rounds)
- [Ending a round early](#ending-a-round-early)
- [Take-home tests](#take-home-tests)
- [The interview rounds](#the-interview-rounds)
    * [Technical round](#technical-round)
    * [Behavioural round](#behavioural-round)
    * [Leadership round](#leadership-round)
- [Wrapping it up](#wrapping-it-up)
- [Personal horror stories](#personal-horror-stories)
    * [Reading obscure bash](#reading-obscure-bash)

<!-- tocstop -->

## General approach

Some of the primary goals of interviewing are to determine whether:

* The leveling is correct.
* Current employees would like to work with this engineer.

Keeping these assumptions in mind will help you to answer the question "what should I ask this engineer to make the interview effective?"

## Rounds

After a short recruiter call, we use two to three interview rounds, consisting of about one hour each. These rounds focus on three areas:

* Technical
* Behavioral
* Leadership

These should be fairly self-explanatory, but we’ll look at each of these rounds in more detail later on. The leadership round is used only for higher-level positions.

In each round:

* An internal Slack group is set up to coordinate the interviewer’s efforts.
* The last ten-to-fifteen minutes are reserved for the candidate to ask their own questions.
* After the round, we congregate internally to align on our thoughts and compare notes.

It's important to keep in mind that structuring the interview process into these rounds helps to avoid the overlap of question themes. We try to keep the technical round technical, and so on. Additional commentary is always welcome in the scorecard, of course.

## Ending a round early

We're all busy, but we all have feelings. If the basic knowledge or expertise is quickly ruled out, the panel may coordinate an early ending to the interview. This usually cuts the meeting time in half. The candidate is still prompted to ask their questions of the panel and isn't directly notified of the outcome of the interview.

On the other hand, if the candidate excels and is an obvious fit, we may also end the meeting early. This is rare, as instead of ending early, we generally try to ask harder questions to see if the candidate is suitable for a higher-level position.

## Take-home tests

We’ve tried them, but we don't do them. I have mixed feelings about their efficacy but did build out [some concepts](https://github.com/slavaaaaaaaaaa/devops-homework).

## The interview rounds

Next, let’s take a look at the interview rounds. Usually they follow this same sequence, although sometimes scheduling demands might require them to happen in a different order than shown here.

### Technical round

In this meeting, the panel includes two engineers and one manager. The manager is there to direct the flow and provide general oversight, while the engineers take turns asking technical questions. The interviewing engineers generally don't require any special preparation to take part in this panel, aside from a list of generic areas of expertise we seek. We don't do whiteboarding, but we may present a diagram to facilitate specific queries. For example, a networking diagram might be used for asking CIDR math and routing questions in a given network.

The questions asked of the candidate are never the same, but the interviewing engineers will go in-depth on several technologies and ask questions or prompt for conversation lines that could only be answered by someone experienced in the topic at hand. For example, if a candidate says they know Terraform, they may be asked about locals, variables, count, looping techniques, state, and state locking. It's easy to determine the level of a candidate’s expertise (basic, intermediate, or expert) by their depth of knowledge of technologies that we use on a regular basis. On a more general level, we may use a specific client scenario and ask how the candidate would proceed within that context.

### Behavioural round

The key of this round is to query the candidate on their soft skills. Specifically, we aim to verify these abilities:

* Engagement
* Communication
* Interest, direction, and motivation
* Working in a team
* Culture fit
* Relocation fit (if applicable)

I like to summarize this in three sentences:

* Can you work with other engineers?
* Are you motivated to grow and learn over time?
* Are you friendly, overall?

### Leadership round

Lastly, a mix of Directors, Department Heads, and sometimes the COO will congregate to vet the decision to hire a higher-level engineer. This round is generally focused around self-selling and soft skills. The necessity of this round is driven by the role for which we are hiring and it may not occur for all positions.

## Wrapping it up

That about covers it. The process described here is generalized and there might be fewer (or more) interview rounds. Naturally, the details will vary depending on the position. Hopefully, this overview provides some insight into our interviewing process and what you can expect. If you would like to see this process in action, do consider our open positions—we’re hiring!

## Personal horror stories

As I remember them, I'll add my own experiences of interviewing at other companies.

### Reading obscure bash

**Scenario:** I was presented three five-line bash scripts with various relatively obscure bash uses and was questioned of their output.

**Issue:** This isn't a real-world scenario: you always have access to Google, `man` pages and most importantly the ability to test things out in a local environment. Testing for remembering obscure syntax is closer to a high school history exam than an tech interview.

**Remediation:** Ask a problem and how you'd solve it in `bash`. If it doesn't suffice, ask how you could improve it. Talk about tools available, both native to `bash` and external ones. Brainstorm with the candidate, assess how they think: not how they remember syntax.
