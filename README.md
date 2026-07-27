---
slug: "3447"
authors: Ben Pate \<@benpate@mastodon.social\>
status: DRAFT
dateReceived: 2024-04-19
relatedFeps: 
trackingIssue:
discussionsTo:
---

# FEP-3447 Endorse Activity

This repository is for brainstorming a Fediverse Enhancement Proposal to add "Endorsements" to the Fediverse. 

This is a temporary, extremely rough, early draft that is meant as a starting point for a conversation, not a final document. Please help me to develop this idea by adding issues to this repository. Once we have more than _12% of a plan_ we'll move this to where it belongs: on Codeberg as a proper FEP

TO-DO NOTES:
* Add definitions for different kinds of endorsements: 
	* In-Person (I have interacted positively with this person in meatspace)
	* Online (I have interacted positively with this person online)
	* Professional (I admire their work, but don't know them personally)
	* ???
* Finish the "UX" section
* Finish the "Abuse" section

## Summary
`Endorse` activities give us a positive counterpoint to blocks and mutes -- a way to send a positive signal about other actors online that is more definitive than a `Follow` or a `Like`.

These records create the opportunity for a privacy-preserving, opt-in network similar to webs of trust, enabling third-parties to know who they're dealing with before beginning an online interaction.

## 1. Requirements
The key words "MUST", "SHOULD", and "MAY" are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119.html).

## 2. History
In our ever-expanding online communities, individual behavior is often disconnected from the social norms that often work on in-person relationships - factors like reputation and trust. This lack of accountability and long-term thinking often powers the worst abuses online spaces.

Blocks (and block-lists) form an important part of this equation by censoring out those identified as most harmful to the community. But there is space for a counterpart, a "yes" endorsement that is the opposite of the block's "no."

This document defines an `Endorse` activity,  which allows people to make positive statements about others on the Fediverse. This data facilities a web of trust, and builds the foundations for an online reputation system that helps reinforce social bonds.

## 3. Vocabulary Definitions
This document adds three vocabulary definitions and the workflows required to establish a new endorsement.  In broad terms, actor A sends an `Offer(Endorse)` to B which is `Accept`ed or `Reject`ed by B.  If accepted, both actors publish the activity in their respective `endorses` or `endorsedBy` collections. 

### 3.1 The `Endorse` Activity

#### 3.1.1 URI
The `Endorse` activity is defined with the URI:
 `http://w3id.org/fep/3447#Endorse`

#### 3.1.1 Description
The `Endorse` activity indicates that the `actor` endorses the `object`, which MUST be another JSON-LD actor. Endorsing is defined as making a positive statement about another actor, and establishing some level of trust as defined by the additional properties included within this activity.

`Endorse` activities

#### 3.1.2 Properties
Optional properties MAY be included with any `Endorse` activity.

| Field                                                                    | Description                                                                                                                       |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| [Content](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-content) | HTML formatted text of the endorsement. This SHOULD be a statement from the original actor about the actor being endorsed         |
| [Image](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-image)     | Image to go along with the endorsement. This may be a badge or icon that represents the original actor's endorsement              |
| [Tag](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-tag)         | One or more [hashtags](https://www.w3.org/TR/activitystreams-vocabulary/#h-microsyntaxes) that help to organize this endorsement. |

`Endorse` activities may include additional metadata, such as one or more machine-searchable categories (represented in the `hashtag` property) and/or a full text statement (represented in the  `content` property)

#### 3.1.2 Example JSON-LD
```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "type": "Endorse",
  "id": "https://alice.social/endorsements/1234"
  "actor": "https://alice.social/@alice",
  "object" "https://bob.social/@bob",
  "content": "Bob is swell",
  "to": "Public"
}
```

### 3.2 The `endorses` Collection

#### 3.2.1 URI
The `endorses` collection is defined with the URI:
`https://w3id.org/fep/3447#endorses`

#### 3.2.2 Description
The `endorses` collection is included in the JSON-LD of an actor's profile. This [OrderedCollection](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) contains all of the `Endorse` activities that this actor has published.

Actors MUST NOT publish activities in this collection if they have not been `Accept`-ed by the actor in the `object` property.

### 3.3 The`endorsedBy` Collection

#### 3.3.1 URI
The `endorsedBy` collection is defined by the URI:
`https://w3id.org/fep/3447#endorsedBy`

#### 3.3.2 Description
The `endorsedBy` collection is included in the JSON-LD of an actor's profile. This [OrderedCollection](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) contains all of the `Endorse` activities that this actor has `Accept`-ed from other actors.

## 4 Workflow
This section walks through all of the scenarios in which "Alice" initiates an endorsement of "Bob". In high-level terms, she sends an Offer that he either Accepts or Rejects.

### 4.1 Offer an Endorsement
Alice begins the workflow by sending an `Offer(Endorse)` to Bob.

```json
{
  "context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "actor": "https://alice.social/@alice",
  "to": "https://bob.social/@bob"
  "type": "Offer",
  "object": {
    "type": "Endorse",
    "id": "https://alice.social/endorsements/1234",
    "actor": "https://alice.social/@alice",
    "object": "https://bob.social/@bob",
    "content": "Bob is swell"
  }
}
```

Alice MUST include an id for the `Endorse` activity, which makes it possible for for Bob to `Accept` the offer (§4.2) or `Reject` the offer (§4.3)

Alice MUST wait for a reply from Bob before continuing with the endorsement. 

Alice MUST NOT publish this endorsement before receiving a response from Bob

### 4.2 Accept an Offer
If Bob wants to accept this endorsement, he MUST send an `Accept(Offer)` response to Alice.

`
````json
{
  "context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "to": "https://alice.social/@alice",
  "type": "Accept"
  "id": "https://bob.social/activities/abc123"
  "actor": "https://bob.social/@bob",
  "object": "https://alice.social/endorsements/1234",
}
```

Bob MUST include an `id` property in this message, which makes it possible for him to `Undo` this operation in the future (see §4.5)

After receiving this activity, Alice MUST add the `Endorse` activity into her `endorses` collection. This fulfills the contract she offered to Bob.

After successfully delivering this message, Bob SHOULD add the `Endorse` activity into his `endorsedBy` collection. Although, there are certain situations where a person may accept an endorsement but not publish it.

### 4.3 Reject an Offer
Bob may not want Alice's endorsement, or may not want to be associated with her. In this case, Bob MUST send a `Reject(Offer)` response to Alice.

`{
````json
  "context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "actor": "https://bob.social/@bob",
  "to": "https://alice.social/@alice",
  "type": "Reject"
  "object": "https://alice.social/endorsements/1234",
}
```

After receiving this activity, Alice MUST remove the `Endorse` activity from her profile.

### 4.4 Undo an Endorsement
If Alice decides withdraw her endorsement of Bob, she MUST send a `Undo(Endorse)` activity to Bob.  She may send this activity at any time after sending the `Offer(Endorse)` activity from §4.1.. Specifically, this may occur either _before_ or _after_ Bob accepts her endorsement.

`{
````json
  "context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "actor": "https://bob.social/@bob",
  "to": "https://alice.social/@alice",
  "type": "Undo"
  "object": "https://alice.social/endorsements/1234",
}
```

After receiving this message, if Bob **has accepted** the endorsement, then he MUST remove it activity from his `endorsedBy` collection.  

After receiving this message, if Bob **has not accepted** the endorsement, then his server software SHOULD remove it from his user interface and no longer display it as an option to `Accept` or `Reject`.

After delivering this message, Alice MUST respond to this  `Endorse`  URL with a 404 Not Found error, or a  [Tombstone](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-tombstone) record.

### 4.5 Undo Accepting an Endorsement
If Bob decides that he no longer wants Alice's endorsement, he MUST send an `Undo(Accept)` activity to Alice. Bob MAY send this activity at any time after sending the `Accept(Offer)` activity from §4.2

`{
````json
  "context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/fep/3447"
  ],
  "actor": "https://bob.social/@bob",
  "to": "https://alice.social/@alice",
  "type": "Undo"
  "object": {
    "actor": "https://bob.social/@bob",
    "type": "Accept"
    "object": "https://alice.social/endorsements/1234",
  }
}
```

## 5.0 User Interface Considerations

INCOMPLETE!!!

(this section is non-normative)

Alice has endorsed Bob

Alice MUST provide a human-friendly URL for the endorsement.

Content Negotiation: IF Alice receives a request with `Accept:application/activity+json`, she MUST provide a JSON-LD version of this endorsement .

Bob MAY display this endorsement on his profile.

The protocol itself will just cover the publishing and accepting of endorsements. However, once this data is published, we can do lots of interesting things with it.  The FEP should also include some (non-normative?) implementation guides to help people get started.  Some suggestions:

* Various "trust levels" might extend to the network of endorsements based on their relative "distance" from me. 1st-degree endorsements are people I endorse directly.  2nd-degree endorsements are people endorsed by by 1st-degree endorsements. And 3rd-degree endorsements are people endorsed by my 2nd-degree endorsements.
* Trust levels could be displayed next to author's names on their posts.
* Trust levels could be used algorithmically to suggest people to follow, or to sort posts based on various trust levels
* Trust levels could be a permission setting on posts, in addition to limiting posts to "Followers Only", you could make posts only for "People I Endorse" or "People within my Trust Network" (with a depth of 1, 2, or 3). This goes a long way to facilitating small communities on the Fediverse.
* Servers may also choose to Endorse other actors/servers on the Fediverse.
* Servers might automatically assume an "endorse" relationship from all people on the server up to the server level itself.  This would mean that all endorsements from the server itself would automatically become 2nd-degree endorsements for all users on the server.
* Endorsements could become a new way to cross-link between actors. Using Bandwagon.fm as an example, it would allow one Band to `Endorse` another - a signal directly from the band that they recommend their listeners also check out another band they respect.

With a rich ecosystem of endorsements in place, we could calculate relative trust for unknown people, as in: "You may not know Sam, but four people that you trust also trust her."

A network of `Endorse` activities would also benefit user discovery as well. For example, a band on Bandwagon could endorse other bands that they respect, helping new users to discover content they might otherwise have missed.

## 6.0 Security Considerations

INCOMPLETE!!!

Bad people will find ways to abuse any system, and will certainly attack this with all manner of abuses. Sock-puppet accounts, bad-faith endorsements, and all kinds of fraud will happen. Let's list out some ways that we can mitigate this, and make a truly useful system regardless of the bad guys. 

Systems that rely on `Endorse` activities should prevent various forms of abuse. We could recommend various limits for relying systems to place on `Endorse` activities, to ensure that they're working with real humans, and not botnets. We can identify "human-scale" behavior, vs. "machine-scale", and decide to believe the endorsements of actors or servers based on their behavior. 

Some ideas (please add more):

* **Maximum Endorsements**: There is some reasonable threshold of endorsements to be trusted. If an actor issues \>N `Endorse` activities, then their endorsements are delisted or ignored.
* **Rate Limiting**: Limit belief based on the number of `Endorse` activities sent or received. If someone receives \> N endorsements per (time span) a bot swarm is at work.. information available to identify and block the bots at work.
* **Hacking**If a person's account is hacked, it could be used to provide false endorsements for others. So perhaps a relying system might wait for  endorsements to aged by a certain amount of time before they are trusted.
* **Privacy-Preserving** - discuss how this model allows for anonymity, and preserves privacy
* **Opt-In** - discuss how this model is opt-in, following the Fediverse spirit of consent.
