---
layout: post
title: "FLP Impossibility Theorem: What It Actually Says and How Blockchains Bypass It"
date: 2026-08-18
categories: blockchain
tags: [distributed-systems, flp-impossibility, blockchain-consensus, cap-theorem]
author: Milosz Muszynski
---

# FLP Impossibility Theorem

FLP stands for the last names of Michael J. Fischer, Nancy A. Lynch, and Michael S. Paterson, who
formulated the theorem in 1985.

Theorem states that:
- in an asynchronous distributed system
- where processes can crash

there is no deterministic algorithm that can guarantee consensus in bounded time.

"Asynchronous distributed system" means that message passing may take arbitrarily long.
There is no timeout that would let us state that some messages are lost or some processes are down.
All we can say is that some message or response did not arrive.
If we can say that a response to the message must come within 10 seconds otherwise we consider the
recipient down, then we deal with a synchronous system. That would be out of scope for the FLP theorem.

"Processes can crash" means that crashing processes belong to the FLP theorem's model.
Unlike the CAP theorem, which ignores failed processes, FLP considers them part of its model.

The theorem states that no matter how clever the algorithm is:
- if it does not use randomness
- if it does not assume timeouts (in other words, does not convert an asynchronous system into a synchronous one)

then it cannot guarantee that unanimous decision will ever be reached.

We can imagine an extended family where every member must be considered in reaching
some unanimous decision. Assume the family is distributed, meaning, communication with members
is only possible via message
passing. Message passing is asynchronous - a particular message may take 
arbitrarily long to arrive. In addition, members of the family may disappear or refuse to respond.
With such assumptions, it seems intuitive that reaching a unanimous decision, in which every member of such distributed
family must have a say and agree, will possibly take infinitely long.

We can't conclude from the lack of response that the person is out. We can't even state that there is a lack
of response, as there is no limit for it to arrive. In such case,
we may never reach the conclusion of the decision process.

# How do Blockchains Deal with FLP?
Bitcoin uses a probabilistic leader election to come up with a decision, and then it assumes that eventually everyone will agree with it.
The decision is taken with asymptotically increasing probability of being final.
It is not final right away, yet is final with high probability after six blocks.

Cosmos network uses explicit timeouts for its voting rounds. Validators take turns proposing blocks.
If more than one-third of the validators go offline, blockchain stops moving, choosing safety over liveness.
Note that this works because Cosmos has a known validator set (permissioned or proof-of-stake) 
where validators can be held accountable. A purely permissionless system cannot rely on 
timeouts alone, as an attacker could manipulate them to win elections—this is why permissionless 
systems must lean on randomization, while systems with known validators can use timeouts.

These two examples show us two approaches of bypassing the FLP impossibility:
- Bitcoin focuses on a randomized element, trying not to impose timeouts but rather relies on decreasing probabilities of finality.
- Cosmos focuses on timeouts yet uses some form of elections of validators.

Many blockchains deal with FLP in their unique ways, yet the ideas are always similar - synchronicity and randomness.

We cannot beat FLP. We cheat it by changing the rules of the game:
- we use timeouts
- we use leaders (elections)

# Are Elections Always Randomized?

Can we treat elections as the randomized element?
In permissionless blockchains, elections must be based on a strong, cryptographically secure random process
to beat FLP.
In a permissionless system, you cannot use timeouts alone to beat FLP,
because an attacker can manipulate the timeout to win the election.
Permissionless systems must use Verifiable Random Functions (VRF) or Proof-of-Work to pick the leader.
So in permissionless systems, elections must be defined by their randomness.

In essence, elections are not always randomized.
Yet they need to be in permissionless systems.

For CFT (Crash Fault Tolerance) they do not need to be randomized (unless for a tie breaking).
Yet for BFT (Byzantine Fault Tolerance - meaning - when malicious or unpredictable behavior is possible and considered),
elections must be randomized to beat FLP.

To be exact, elections in permissionless systems must be unpredictable and unbiased, they do not need to
have uniform distribution to deserve the randomized name in a mathematical sense.

# Conclusion
FLP Impossibility Theorem is the mental anchor for thinking about blockchain consensus. 
