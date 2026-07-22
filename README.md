# FEP-xxxx Endorse Activity

This repository is for brainstorming a Fediverse Enhancement Proposal to add "Endorsements" to the Fediverse. This is an extremely early rough draft that is meant as a starting point for a conversation, not a final document. Please help me to develop this idea by adding issues to this repository.

## The Problem
In our ever-expanding online communities, it's hard to know who to trust.

Blocks (and block-lists) form an important part of this equation by censoring out those identified as most harmful to the community. But there is space for a counterpart, a "thumbs up" endorsement that is the opposite of the block's "thumbs down."

By defining an `Endorse` activity, we would be able to develop things like [web of trust](https://en.wikipedia.org/wiki/Web_of_trust). It could be the foundations for an online reputation system that builds (and improves) on things like LinkedIn recommendations.

With a rich ecosystem of endorsements in place, we could calculate relative trust for unknown people, as in: "You may not know Sam, but four people that you trust also trust her."

A network of `Endorse` activities would also benefit user discovery as well. For example, a band on Bandwagon could endorse other bands that they respect, helping new users to discover content they might otherwise have missed.

## A Proposed Solution

* **Alice** wants to endorse **Bob**.  She publishes an `Endorse` activity to her outbox, addressed to **Bob**.
* **Bob** receives the message, and either `Accept`s or `Reject`s it.  If rejected, then this workflow ends.
* If accepted, then **Alice** adds the endorsement to a new collection in her profile named `endorses`, and **Bob** adds the endorsement to a new collection in his profile named `endorsedBy`.
* The actors may choose to display this endorsement on their home pages as well.

`Endorse` activities may include additional metadata, such as one or more machine-searchable categories (represented in the `hashtag` property) and/or a full text statement (represented in the  `content` property)

## Prior Art

1) This feels very similar to the outstanding [BadgeFed](https://badgefed.vocalcat.com) project, so we really need to work with [Maho Pacheco](https://www.vocalcat.com/) ([GitHub](https://github.com/tryvocalcat)) to find common ways to represent and distribute both badges and endorsements. Currently, badges are verified cryptographically, where this idea for endorsements is only verified by reference.

2) Endorse activities are more explicit than the Follower/Following relationships that we currently have, and even more specific than the two-way Friends/Connections relationships on other social media designs.

3) LinkedIn builds some interesting analytics on top of their endorsements system. While not a part of this protocol specifically, we should learn from this to see how we can improve on it in a distributed ecosystem.

## Outstanding Issues

**Abuse** - Bad people will find ways to abuse any system, and will certainly attack this with all manner of abuses. Sock-puppet accounts, bad-faith endorsements, and all kinds of fraud will happen. Let's list out some ways that we can mitigate this, and make a truly useful system regardless of the trolls.

**Compatibility** - A protocol is no good if nobody uses it, so we'll need to build support for this across many app platforms for it to be valuable.

**Unknowns** - What else are we missing here?