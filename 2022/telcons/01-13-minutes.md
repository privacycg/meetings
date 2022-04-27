# Privacy CG Teleconference
* Chair: Tanvi Vyas
* Scribe: Jeffrey Yasskin

# Agenda
* Private Click Measurement
   * Take PCM to PATCG #96
* First-Party Sets 
   * Discuss FPS membership standards #76
* Any other business


# [Take PCM to PATCG #96](https://github.com/privacycg/private-click-measurement/issues/96)
* Martin Thomson: This group has been working on PCM for some time. The PATCG is just starting up its work, and the general question of Attribution has been raised as an important issue to discuss there. This group should consider ceding control of this spec to that group. It makes a better delineation of responsibilities. This group can focus on exclusively things that improve privacy, while that group can focus on making advertising better with the constraint that it remain private.
* Tess: With my chair hat on, I tend to agree with you, Martin. Seems like a clear delineation. Privacy-preserving adtech should be there, while non-adtech should be here.
* Tess: Apple hat on: We don't have a technical reason to oppose that. Practical concern: for slow internal process reasons, we haven't yet joined the PATCG, and John's the active editor. If we decide to do this as a group, would like to wait until Apple can join that CG. Not something I can give an official timeline for. Hopefully sooner than later. Do you have a timeframe in mind? Can you wait a bit?
* Martin: Don't mind all that much. As long as there's hope that it'll happen. I asked because the PATCG is scheduling a meeting in the next 3 weeks, and this'll be a topic there. Having an indication that this group is willing would help. Probably won't make any decisions about outcomes at that point, but I hope we'll decide to take on the problem.
* Tess: PATCG discussing that seems like a possibility. Thinking about the draft can happen before the document officially moves. Don't think we're holding up any discussion. Prefer to wait a bit.
* Martin: Would like John to come to that meeting to talk about the principles. What has he learned from having deployed it?
* John Wilander: As the editor of the spec, I agree with what Tess said. We're working on joining. I'm also excited about the attribution problem being discussed in that group.
* Kris Chapman: Supportive of PATCG taking on the problem. Related via Apple's representation: As the co-chair of the FedID group, we need Apple's participation there too.
* Tess: That's also in the queue. I'm working on it.
* Martin: Thanks for the input here. That's all I wanted.

# [Discuss FPS membership standards #76](https://github.com/privacycg/first-party-sets/issues/76)
* Kaustubha: Scott and Ralph reached out to the editors showing us a specification, hoping we can leverage the trust.txt specification. Exciting to see the privacycg thread.
* Ralph Brown: Scott and I are new to the Privacy CG. Scott co-chairs the Credibility CG. Last month we noticed that the Privacy CG was working on this issue, and it overlaps with work we're doing with the Journal Lists trust.txt spec expressing this kind of relationship. Might be some value contributing to the work here. There's a lot of overlap between privacy and the work JournalList is trying to do around credibility. Publicly disclose in both human and machine-readable form the relationship between sites. We came at it from the perspective of mis- and dis-information, and providing trust signals. "You're known by the company you keep.", so this expresses business relationships. The interest there is to share what we've learned and how we're approaching the problem. You're solving a problem that we're also seeing, so what can we learn from you?
* Scott Yates: The goal of trust.txt is showing relationships in a way that's easy to machine-read. Builds on the robots.txt and ads.txt specs. Adoption has been positive. I was hoping when I started trust.txt that there would be things that layered on top of it, and this seems like the first tangible example.
* Aram Zucker-Scharff: I'm familiar with trust.txt, and I appreciate the work you've done on it. I've spoken to this on the email thread. The core question is about how well these things overlap. I don't think that first-party-ness and shared trustworthiness are principles that map. Doesn't benefit what either group is trying to do. Without judging any of the organizations, e.g. Wall St. Journal & Fox news could be a FPS, since they're owned by the same group, have an interest in sharing users for targeting purposes. But they probably don't want to be considered a shared trusted party in the way trust.txt would imply. More examples when we move out of journalism, already in the email. If we build one on the other, we end up with an unsteady foundation. Interested in talking more about trust.txt outside of this context, but these seem to mismatch.
* Ralph: We think this *might* be a solution, but we're trying to learn w.r.t. FPS there's a question of control, the relationship; that's a problem we're trying to solve irrespective of FPS in the privacy group. JournalList is working on this too. Issues about how to verify that. There's a proposal for having an independent entity that's the authority to verify whether a group is living up to that attestation. Our vision was to start small and simple, but we knew we'd face the same problems. Want to see what thoughts you have for addressing the problem. Wasn't suggesting that you just adopt trust.txt. Want to find flaws in our thinking and see if it's useful.
* Kaustubha: Echoing what Ralph said. When I first read trust.txt my initial reaction was that they seem like different things. When we started working on FPS, there's a lot of prior art, where people create associations between domains. They're cheap, indicate brands. On other platforms, they've recognized this by identifying a vendor. They indicate that several apps are related. Many in the group think the domain name is the only way users understand domain, but we've seen evidence that this isn't entirely true. My local bank uses a different website for login. The work Ralph and Scott are doing highlights that there's a need. Need to bridge domain names and how users perceive it. Trust or privacy. Not exactly the same, as Aram said. We can collaborate on whether there's a shared sense of controllership. On the thread there was a distinction between content control and data control. Perhaps trust.txt would like to express both relationships. Whether it's the Independent Enforcement Entity (IEE), or how we express the controllership relationship .Don't see FPS directly depending on trust.txt, but let's share learnings.
* Scott: What about the bank?
* Kaustubha: If you look at smaller banks and local mom&pop stores. It's not mom&pop.com. Lots is covered by doing a search on the web. From a browser vendor perspective, some of these real first-party entities have played fast and loose, and have real apps spread across multiple domains. If the only solution is to make everyone move, we're going to have to postpone things. Mozilla and Microsoft have acknowledged this, in which disconnect.me has an entity list. FPS creates an allow list for the whole web. Maybe in 10-15 years maybe we can say that's not enough, but we're not in that state now.
* Scott: Like with the small bank, trust.txt wants to support small news publishers. Some pretty small publishers operate on multiple domains. e.g. a lifestyle magazine that's clearly associated but on a different domain. Goal is to support local journalism.
* * Brian May: I like the mutual validation aspect. Like to empower browsers to look at the files under two entities and say they're mutually supportive. Want to pursue something like that in FPS. User should be able to look at browser and see that what the site claims is what it's actually doing. How do you folks deal with changes over time? If you have a small publisher that's part of a group and then mutates out of the group, what happens to work published while the publisher was part of an old group?
* Ralph: The principle is that the entity that controls the domain is responsible for their trust.txt file. They're responsible for all content on the domain. As that changes over time, they need to update their trust.txt. If a small publisher was part of Gannett and then was sold, both have to update their trust.txt.
* Brian: Situation where an independent publisher publishes something, and then is acquired with a group with different sensibilities, what happens to prior publications.
* Scott: trust.txt is an attestation of what's true at the time it's read, so if something is bought by someone nefarious, it's up to them to change it. Things published in the past aren't affected. There’s no claim about content in the trust.txt file, only the trusted relationships.
* Don Marti: Aram mentioned that perhaps Fox News and Wall St. Journal might be FPS members because they have common ownership. In the current version of the UA policy, the proposal says the members have a group identity that's easily discoverable by a user. So don't think sites can go in or out of an FPS just based on business transactions that most users don't notice. Has to be obvious to the user. At some point in the future, they could change their brand identity so that it's clear to an ordinary visitor that they're the same brand, but that wouldn't be the case today. Looking forward to discussion of processes on how the IEE can test user perception.
* Kris: I'm excited by trust.txt and FPS. They're not exactly the same thing, but they're related, and I think they should be used together. From a SAAS perspective, the FPS salesforce would put out would have a lot of domains for all of our products, but a publisher would only be using some products. In the trust.txt, they might say they're using Salesforce's marketing cloud and CRM. Somewhat dynamically it could drop domains from the FPS that Salesforce puts out to say that only some domains should be associated. We need it to be more dynamic so that it matches what our clients are actually buying. Otherwise it exposes the client to more domains than they want.
* Aram: Clarification: is this saying the publisher would be in an FPS with a product?
* Kris: The domains salesforce has would … the publisher might have one domain with the tags, but there's a network of other domains that need a relationship for the product to work.
* Ralph: trust.txt does have this vendor-customer relationship. Issue of having the IEE is something we talked about early on. Tried to understand who validates, not necessarily the control structure, but more whether the org is living up to its claimed journalistic standards.  Envisioned that press associations might be able to do some form of audit. Didn't think there would be a single enforcement entity. Multiple entities would be evaluating the validity of trust.txt files. Which could lead to certification or other impartial attestations of their trustworthiness. Haven't gone that far yet. Gets at the scale issue.
* John: There's no hard link between FPS and relaxing cookie blocking. It's one way FPS can be used, and Chrome has said they want to use FPS like that. Safari has said we won't use FPS in that way. It's just a way to express that some domains are part of the same party, but each browser decides its policy independently.
* Aram: Per John, part of the issue with creating the IEE is that it's verifying the sense of first-party-ness against what we want to accomplish with FPS. We'd establish the IEE to check that it's appropriate for the party to handle data in the way Chrome needs it to. The IEE would be checking things on behalf of just Chrome. that doesn't bode well for its stability. Says to me that the first thing plugin and extension developers will do is to block FPS actions in Chrome. 
* Aram: Question of whether FPS is a temporary measure. There are a lot of people relying on cross-context communication in their current development. They'll have to do a lot of development to get away from this. FPS gives them more space. But nobody takes action until the last minute anyway. It's there until we decide to get rid of it. Even if we encode into the standard that it'll be gone in 10 years, nobody will start migrating for 9 years and 10 months. The more we spin our wheels on FPS, the more we'll narrow the window for developers to respond to the change that's happening. The change will eventually be that you need to get all your stuff on the same domain. Don't think there's a long-term solution other than that. I like that Ralph and Scott want to do more verification, but it's hard to do and contentious. I'm worried: if the IEE depends on users, someone's going to propose a DAO. (Which to be clear, I think is a terrible idea, but inevitable that it will be suggested) Lots of reasons users disagree with each other, even just with trust, when it's not affecting their web interactions. A friend did an experiment on ad trust. First group kicked out of the trust network was the NYT because their reporting on the Iraq War was incorrect.
* Scott: I agree on just about everything. I think trust.txt … a group shouldn't say something's trustworthy or not trustworthy, but a group of publishers can self-define that they're a group. That should be discoverable. Just that. Larger goals are fraught with peril. 
* Martin: Only thing I disagree with Aram on is the 9 years out of 10. People treat deadlines as guidelines, and most websites move *after* the deadline. This conversation is too abstract for me to be happy with it. Scott said many interesting things, but we have to ask what we're going to do with that information. Machine responses will depend on the details of the information. I'm looking for answers to the questions John raised. If Google attaches a different cookie policy, but Apple doesn't, and I think Mozilla would not either. That's not doing the Web any service. The point of standards is so that all of the things in the space operate the same way. Not good to say "they'll have different policies." We need to talk about those concrete things. If you say "this is about cookies", you come to different conclusions. I'm opposed to letting FPS share state across diverse entities. Don's interpretation and my interpretation of the proposal wording, lead to very different conclusions. An org is using user identities across multiple very different entities, including ones that i had no idea were under the same umbrella.
* Kaustubha: Responding to Aram's points: How do browsers use FPS? For Safari, you've shipped 3p cookies blocking, and the solution for developers for cross-site state is Storage Access. As browsers iterate on other privacy mitigations, it's clear there's a need for something like FPS. Are we going to prompt on every navigation? Regardless of how browsers use it, whether for cookies or other privacy protections, we need to agree on what a first party is. If we're using it to allow an auth token to be exchanged across domains, whether via cookies or URL parameters, we need to agree on the policy. 
* Kaustubha: Thinking about extension developers turning this off. Chrome's going to offer a toggle so users who want to disable all cross-site state sharing can do that. That's a fine thing for extension developers or users to do. We're finding a balance between usefulness and looking out for the users' best interests.
* John: Agree with Martin. We should discuss the cross-site cookie blocking and whether FPS should be used for that. People who approach the FPS proposal do make that link, and they make a bunch of assumptions and think about how their websites work. that's going to cause us a bunch of pain, and we should address it straight on.
* Brian May: We're taking a half step in the direction of a real solution. What we want to end up with is that a user is clear what tradeoffs they're making. Pulling things into a FPS as if it's an uber-domain doesn't help a user understand how their interactions will affect them down the road.
* Aram: I hear there are other use cases beyond breaking the 3p cookie wall. Not sure FPS is the best solution for those. FedID solves many problems around passing tokens via URLs. Our work would be better used for addressing these things more directly. Especially not setting up a governance entity.
* Kaustubha: FedCM doesn't target 1p single sign on. They target things like Google/Apple sign-in. Playstation.com's login uses sony.com. FedCM targets a particular protocol. There's a section in the FPS explainer, but if folks aren't clear on that, I'm happy to talk over it again.
* Kris: Backing up that FedCM isn't going into this particular space. It's very specifically looking at the cookie situation, but not what's happening to information in URLs. Agree with Aram that there are lots of ways we could solve these problems, and we don't have a good hierarchy of CHIPS vs FPS vs other use cases, and what we think the answer is for each situation. Proposals are answering half of a use case, and it's very confusing.
* Ralph: Thank you; this has been very illuminating in understanding where the Privacy CG is in its process. Feel free to ask us more about JournalList and trust.txt.
* Kaustubha: Controllership seems like the most promising area to learn from each other.


# Attendees    
1. Aram Zucker-Scharff (The Washington Post)
2. Anuvrat Singh (Amazon)
3. Ben Savage (Meta)
4. Ben VanderSloot (Mozilla)
5. Bill Densmore (ITEGA.org)
6. Brad Rodriguez
7. Brian May (dstillery)
8. Chris Needham (BBC)
9. Chris Wood (Cloudflare)
10. Daniel LaLiberte (Google Chrome)
11. Don Marti (CafeMedia)
12. Dongoh Park (Google)
13. Eric Mwobobia (ARTICLE19) 
14. Erik Anderson (Microsoft Edge, co-chair)
15. Erik Taubeneck (Meta)
16. Grant Nelson
17. Harneet Sidhana (Microsoft Edge)
18. Heather Flanagan (Spherical Cow Consulting)
19. Hirsch Singhal (Microsoft Identity)
20. Hitesh Lad (CJ Affiliate)
21. Jason Nutter (Microsoft)
22. Jeffrey Yasskin (Google Chrome)
23. John Wilander (Apple WebKit)
24. Kaustubha Govind (Google Chrome)
25. Kelda Anderson (Microsoft)
26. Kris Chapman (Salesforce)
27. Mark Xue (Apple)
28. Marshall Vale (Google)
29. Martin Thomson (Mozilla)
30. Michael Smith
31. Paul Bannister (CafeMedia)
32. Pete Snyder (Brave Software)
33. Raj Belur (Amazon)
34. Ralph W Brown (Brown Wolf Consulting)
35. Russell Stringham (Adobe)
36. Sam Weiler
37. Sanketh Menda (N/A)
38. Scott Yates (JournalList (trust.txt))
39. Steven Englehardt (DuckDuckGo)
40. Tanvi Vyas (Mozilla, co-chair)
41. Tara Whalen (Cloudflare)
42. Theresa O’Connor (Apple, co-chair)
43. Wendell Baker (Yahoo)
44. Brad Rodriguez (Magnite)
