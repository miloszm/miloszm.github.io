---
layout: post
title: "Understanding CAP Theorem"
date: 2026-08-18
categories: blockchain
author: Milosz Muszynski
---

# CAP Theorem

CAP Theorem (also known as Brewer's theorem) states that in a distributed system,
we cannot have consistency (C), availability (A), and partition tolerance (P) all
at the same time.

The theorem is sometimes accompanied by a Venn diagram, consisting of three overlapping
circles, representing C, A, and P. The diagram shows that there is no overlap between all three
circles, yet there is an overlap between all pairs of the circles.

When thinking about this theorem, I tried to picture three gauges.
When we slowly increase any two of them, the third gets automatically decreased.
And then I tried to think concretely - how do you slowly decrease availability?
Availability is a binary switch, not the sliding scale like the other two.

Are Venn diagrams implying sliding scales on all three of the C, A, P incorrect?

# CAP Definition of Availability

In the CAP theorem, availability means: every request received by a non-failing node must return a response.
You cannot "slightly decrease" availability in CAP. The system is either available or not
available. Failing nodes are ignored by the theorem, it deals only with non-failing nodes.
Being not available means that a healthy node is not responding for some reason, as a conscious choice,
it does not mean that the node is failing. Failing nodes are not part of the theorem, it is not a theorem
about fault tolerance.

So what "low availability" looks like in practice? Let's say you choose C + P (Consistency and 
Partition Tolerance), and you neglect A. You tolerate low A, meaning:

- you tolerate telling the users "Service Unavailable" until network heals
- you tolerate nodes not serving users until they catch up with other nodes

Consistency is a spectrum - strong, sequential, causal, eventual.
Partition tolerance can be thought of as a spectrum in practice — e.g., how long you wait before assuming a 
partition has occurred (timeouts), or how many messages can be lost before the system gives up.
Low availability means - you are ok with your system timing out users rather than serving stale data.

In real life, we can turn 20 out of 100 nodes off and say the system is less available.
In CAP theorem, availability means 100% of non-failing nodes must process 100% of requests, 100% of the time.
If we turn off 20 nodes, these 20 nodes are now failing nodes, and CAP theorem ignores them. The theorem
only cares about remaining 80% of non-failing nodes.
In CAP, there is no 80% available, there is only pass or fail:
- pass when every request to a working node gets a response
- fail if at least one (working) node rejects request

CAP is an instantaneous test. If a network partition happens, and a system maximizing C and P rejects requests
for 10 seconds, the system is 100% unavailable for these 10 seconds. It does not matter that it is
available 99.99% over the whole year. So in a way, CAP theorem versus MTBF (Mean Time Between Failures)
is a bit like power vs. work in physics.

# CAP Binary Switches
In the CAP theorem, and in its proof, the C, A, P are treated as binary switches.
So not only availability is binary, but consistency and partition tolerance as well.
This makes it even harder to think in terms of CAP theorem for engineers, because in practice
these parameters are not binary.

# PACELC Theorem
Fortunately, we have much more practical PACELC theorem, also from Eric Brewer.

PACELC states:
- If there is a partition P, you must choose between Availability and Consistency
- Else (E), when there is no partition, you choose between Latency and Consistency

The "if" part contributes to "PAC", while the "else" part contributes to "ELC" part, hence the name "PACELC".

This theorem is much easier to reason about for engineers.

CAP theorem Venn diagram makes it look like you can pick any two, implying that the third one is your sacrifice.
But in reality, Partition Tolerance (P) is not a choice.
Networks do drop packets. You cannot opt-out of P.
The actual decision is - since P is forced upon you, which one are you sacrificing - C or A?

# Practical Formulation of the CAP Trilemma 

Is there a network partition right now?
- Yes -> choose consistency (reject requests) or availability (serve stale data)
- No -> choose strong consistency (higher latency) or eventual consistency (lower latency)

In case of "Yes" we have a harder choice, in case of "No" we have a more comfortable choice.
That sounds like a more practical mind-frame for engineers than the CAP theorem.
