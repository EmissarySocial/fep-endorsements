# FEP-3447 Endorse Activity

This repository is for brainstorming a Fediverse Enhancement Proposal to add "Endorsements" to the Fediverse. 

This is a temporary, extremely rough, early draft that is meant as a starting point for a conversation, not a final document. Please help me to develop this idea by adding issues to this repository. Once we have more than _12% of a plan_ we'll move this to where it belongs: on Codeberg as a proper FEP

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

## How Might this Be Used?
The protocol itself will just cover the publishing and accepting of endorsements. However, once this data is published, we can do lots of interesting things with it.  The FEP should also include some (non-normative?) implementation guides to help people get started.  Some suggestions:

* Similar to how LinkedIn manages 1st- 2nd- and 3rd-level relationships, various "trust levels" might extend to the network of endorsements based on their relative "distance" from me.
* Trust levels could be displayed next to author's names on their posts.
* Trust levels could be used algorithmically to suggest people to follow, or to sort posts based on various trust levels
* Trust levels could be a permission setting on posts, in addition to limiting posts to "Followers Only", you could make posts only for "People I Endorse" or "People within my Trust Network" (with a depth of 1, 2, or 3). This goes a long way to facilitating small communities on the Fediverse.
* Endorsements could become a new way to cross-link between actors. Using Bandwagon.fm as an example, it would allow one Band to `Endorse` another - a signal directly from the band that they recommend their listeners also check out another band they respect.

## How Might This Be Abused?
Bad people will find ways to abuse any system, and will certainly attack this with all manner of abuses. Sock-puppet accounts, bad-faith endorsements, and all kinds of fraud will happen. Let's list out some ways that we can mitigate this, and make a truly useful system regardless of the bad guys. 

Systems that rely on `Endorse` activities should prevent various forms of abuse. We could recommend various limits for relying systems to place on `Endorse` activities, to ensure that they're working with real humans, and not botnets. We can identify "human-scale" behavior, vs. "machine-scale", and decide to believe the endorsements of actors or servers based on their behavior. 

Some ideas (please add more):

* **Maximum Endorsements**: There is some reasonable threshold of endorsements to be trusted. If an actor issues \>N `Endorse` activities, then their endorsements are delisted or ignored.
* **Rate Limiting**: Limit belief based on the number of `Endorse` activities sent or received. If someone receives \> N endorsements per (time span) a bot swarm is at work.. information available to identify and block the bots at work.
* **Hacking**If a person's account is hacked, it could be used to provide false endorsements for others. So perhaps a relying system might wait for  endorsements to aged by a certain amount of time before they are trusted.

## Outstanding Issues

**Compatibility** - A protocol is no good if nobody uses it, so we'll need to build support for this across many app platforms for it to be valuable.

**Unknowns** - What else are we missing here?