# 2024-08-08 Privacy CG meeting
* Chair: Erik
* Scribe: Arthur Coleman

# Agenda
* [CHIPS](https://github.com/privacycg/CHIPS)
    * [SameSite attribute handling needs to be clarified #87](https://github.com/privacycg/CHIPS/issues/87)
    * [Keying of "CHIPS" cookies should align with other state #40](https://github.com/privacycg/CHIPS/issues/40)
* [Storage Partitioning](https://github.com/privacycg/storage-partitioning)
    * [Expose partitionedness #32](https://github.com/privacycg/storage-partitioning/issues/32)
* [TPAC 2024 topics](https://github.com/privacycg/meetings/issues/38)
* Any other business


# Notes

### SameSite attribute handling needs to be clarified #87

[Ben] There was … I think we are all in agreement that it should not.  That things like “None” should be required

[Erik] That is my reason the issue as well. If there are no other opinions, then we’ll figure out how to do


### Keying of "CHIPS" cookies should align with other state #40

[Aaron Selya at Google] Wanted to raise awareness that is now implemented in chromium on by default. So far our metrics are not indicating any significant crashes as of this morning.  - at least at it relates to partition cookies.  If you are finding any, please file a bug report.

Small question involving a difference in how storage access API is handled between firefox and Chromium.  Part of the implementation I am working with in the web extensions group is adding an API to the cookies that will allow a service worker to query on a frame and see what a partition cookie --- should be.  Question of whether storage access API implementation would change on what the key is - depending on if it has been granted.

[Ben] So this is a Service Worker --- for a frame partition key to query

[Aaron] A way for a service worker when requesting cookies to know which partition key they should query on to get those cookies specific to that frame.  It used to be easier with the top level site being available on the service worker but since the cross-site ancestor bit has been added that is not as easily exposed.  Covering that gap is something we agreed was a good idea. 

But the question on whether the --- tema change is good - is worth a discussion in this group.  Don’t need to solve it today but something to think about.

[Ben] Someone at Mozilla in this space has not reached out to me on API - I imagine 

[Aaron] That is the way Rob and I will end up hammering it out.  But not sure if there is any instant view on this is a risk.

[Ben] I think we are aligned.

[Erik] Any issues or artifacts anywhere else discussing this?

[Aaron] Included in extensions proposal including this feature but buried in the more recent comments. A lot of back and forth between Rob and me. [(Link to discussion referenced)](https://github.com/w3c/webextensions/pull/581#discussion_r1707852207)

I know you had some interest in learning from our experience in implementing the ancestor thing for partitioned cookies.  If you want to fine time, happy to chat.  

[Erik] Question on terminology - when you are describing crashes - you mean in context of sites not longer working

[Aaron] Yes - ancestor only comes into play when a comparable site is embedded with a cross-site embed.  That was about 0.002% of pageloads.  And the major sites we reached out to to do some manual testing to see if things break with Paypal it is not going to be an issue for them.  I should have been more specific about that.


### Expose partitionedness #32

[Erik] Looks like a similar discussion around ancestor change and such.  Anyone have anything to share?

[K] I think having this would be useful but might be introducing complexity.  We have a number of situations where sites do have access to one partition versus another.  We also have enterprise policies where enterprise could allow specific sites to have access to third party cookies.  

So there are a few different situations.  And as talking to partners, the adds for ability to query - having access to partitioned cookies - I think there was this older issue that Anne had opened in storage partitioning  - something other developers asked for in the past - I think the storage access API has givestorageaccess = that give you access if you are in a partitioned state and if not you don’t.  So there is an equivalent HTTP header proposal that is now part of that proposal.  Trying to solve that issue as part of that proposal.  That in combination with the fact you have access to unpartitioned cookies - if I have access to --

There is a separate conversation happening in the fetch ancestors discussion - I think Chris was proposing a recipe to capture those APIs that would allow developers to determine whether they have access to partitioned cookies or not.

I think the presence of this issue suggests we have been thinking about it.

[Ben] I think having something here sounds good.  It was a little bit in case we wanted to change how -- works and it seems more like we need to settle this in ---- context.  I know there is a counter proposal for sec-fetch-partitioned.  Why this over that is the question.

[K] - That is a good question.  Chris would be a better person to answer that.

[Ari] The uniqueness of the way chrome would work - storage partitioning is separate from 3PCD - so what we would return in terms of --- is confused.  That doesn’t mean it is  the right way to design the API -= so good feedback.

[Aram] From a developer perspective this would be a good feature to have, but don’t know what the right architectural approach is.  But see a number of ways this would be useful.

[Kaustubha] I think someone with a broader perspective look at the proposal would be good.

[Aram] I’ll take a closer look


### TPAC 2024 topics

[Erik] TPAC towards the end of September.  We can discuss topics here or issue in repo.  So lots of ways to propose.  With that in mind, any topics you want to gauge interest in?

[Kaustubha] Identity use cases a - they were going to bring a couple of topics - will talk to Johann about that.  I think Ari was considering partitioned pop-ins adoption?

[Ari] - Pushing for more adoption for people in Chrome -= but my plan is to have that in TPAC

 Also don't know if the -- storage access - the partitioned data for cookies - don’t know if talking a bit about that if there is feedback about that might be good.  Help to understand how things need to work.

[Ben] Elephant in room topic - a lot of this is around third-party cookie deprecation and it being a user choice/not always the default.

[Kaustubha] For at least for API development, a lot of what my team does ---.  Maybe some timelines will shift - you will see that that will change.  Instead of that being a force default - it will be a user choice.  The implication will be a non-trivial faction of users that will be in the end state that would use this.  I think they are definitely thinking about ergonomics for the user - the last topic was kind of tied to this.  If you think about a person’s ability to test and build applications for those spaces.  People have been asking for that a lot.

OI think we are think in in terms of principals in terms of actual work - we are going to keep API button there - looking at where do we have gaps.

[Erik] Yes, more combinatorial configurations that developers will need to think about. Good conversation for TPAC.

[Lee] That was a big one to talk about.  Happy Chrome is backing off on it.  Curious if providers are rethinking their stance about embedding, header usage, etc. so existing flows around SSO can continue working.  That in combination with partitioned cookies and the normal embedded content - it should all be secure.  I’d be interested to see if being discusses and hear more about that.

[Kaustubha] Can you clarify the first part?

[Lee] A lot of embedded use cases- we try to figure out who you are and then redirect you to the right identity provider you belong to.  But ID providers don’t want to show us inside the frames because they are worried about 3PCD.  Thinking about only be embedded but only in places that I trust.  With Google’s change, are we going to re-address these existing players - these guys are fine - ID providers you should use ---- now.  You should ensure you cookies are set up to allow cross-site access.  And then everyone is happy and the end user has not extra clicks - and everything keeps working.

[Kaustubha] I don’t think it is going to be true for all users.  I don’t know if our guidance is going to change for identity providers 

[Lee] Why?  With frame ancestors and with users ability to opt-in -.  As a user I am willing to say “track me” - then why aren’t ID providers using frame ancestors and partitioned --- to do authentication.

[Kaustubha] Maybe this is where a separate conversation is needed.  I don’t think the cookie thing will be a per-site cookie choice.  That is true even with work going on in FedCM and other groups.  A per site choice it would have to be one of the APIs

[Lee] I think that is ok for ID providers to use API - they are a small set of ID providers.  They can make the API call and make the cookie available. Third-parties can add cookie support and be safe.  Why won’t the identity providers provide support for frame ancestors and change their documentation?

[Campbell] There are thousands and thousands of ID providers out there.

[Lee] Didn’t mean to suggest otherwise.  But a small number control the majority of the market.  (note by SRoddy:  Which market?  Consumer?  Enterprise?  Research & Education?)

[Erik] This is worthy of discussion 

[Arthur] From a product perspective– I’m not in the deep-end here, but I’m dealing as a customer with this. The conversation/question that was asked before was “why should we continue with the development of CHIPS when there’s a change of position in the sandbox.” I want you to know what I’m telling my customers, e.g. publishers– just because PAPI or ARA are problematic (ad-tech specific for me), CHIPS, SAA, etc. offer you a separate ability that present you a way to do your use cases with better fraud detection, better use of retargeting, better results for frequency capping on the front-end (on backend with aggregated reporting it could still be problematic). As a representative as an end-customer, I really don’t think that the change in the position of the sandbox for the CMA and issues there should delay the important improvements in privacy in the sandbox more broadly for other use cases.

[Aram] I’ll add that our assumption here is that the privacy stuff that needs to happen (whether 3p cookies exist or not) still needs to happen. I don’t think anyone in this group is _not _interested in moving forward with those things. Lots of other browsers have already proceeded with 3pcd.

[Erik] Worth noting Google’s blog has implied users will still have a choice to not have 3p cookies. How many is still unclear based on their announcements.

[Arthur] No one is assuming even if cookies stay, there will be more loss still.

[Lee] Cookies have many purposes of which tracking is only one of them. The idea of grouping all 3p cookies together and wiping off the map, most of us work for companies that use them. If I log into Google… it sets a cookie. Would love if we could describe 3p tracking cookies and then separately things like CHIPS for 3p cookies that do not track you. Login sessions which are not meant to track you but needed to embed.

[Ben] Firefox has partitioned cookies by default. Adding CHIPS is something we plan to do as well. And being able to get rid of the non-CHIPS 3p partitioned cookies is our vision. As a final takeaway, it’s something promising in the long term to be compatible on 3p cookies, 3p tracking cookies, and contexts that are partitioned. I desperately want it and think it is still possible for those Chrome users that opt in.

[Kaustubha] Following up on Lee’s point. Perceptive point. There was a discussion with TAG talking about removing 3p cookies. The general #privacy channel in the W3C Slack… I’ve been on this project ~5 years. I had a more definitive “we need to remove 3p cookies” but in practice there are a lot of security and anti-fraud properties. Cookies can be signed is a nice property. They’re general purpose which is great for developer innovation. But this group, I think, is concerned about all of the ways they could be used to track; hard to disambiguate between legitimate and non-legitimate use cases. CHIPS is a perhaps safer version of 3p setting cookies. Also have SAA which has different opinions from folks re: if it belongs in the platform longer term. Chrome team also has a Shared Storage proposal which is cross-site state w/ unlimited writes but reads controlled in a way that has some privacy characteristics. Interesting philosophical conversation that is worth having in this group– do we believe cross-site state as it exists should go away or exist in a controlled fashion.

[Aram] Generally I’m of the opinion that 3p cookies are not fit for the purpose. TAG doc went into this (which I was not involved in) but the problem is lots of things that are extremely useful on the web but cookies not optimized for. SAA an example of how we can maintain important things while removing 3p cookies that were not originally intended to be used for. I want to double check– CHIPS as a concept– when Chrome team originally proposed it, I recalled (and could be wrong) it was a temporary measure until APIs could be created that solved for the other things. Is that still the case? I imagine a continuation of 3p cookies in Chrome, but putting that aside I still wonder if CHIPS is still a temporary measure) until we develop more fit-for-purpose APIs.

[Kaustubha] Never intended for CHIPS to be temporary. Confident about privacy properties. Equivalent to 1p cookies. Like asking if we’d ever get rid of 1p cookies– not the plan. SAA, though, we’ve talked about. Since it gives unpartitioned state which could be used for tracking, requiring users to understand implications of a prompt, so we’ve had conversations about if it’s temporary or long-term. Identity/payments use cases. But CHIPS was never intended as temporary.

[Aram] Got it, was confusing it with something else like 1p sets.

[Brian] Re: CHIPS. When Ben indicated interest to implement CHIPS. Understand desire for consistency. First time I’ve heard anyone other than Google express the need for CHIPS. Even in the context of partitioned cookies. Many browsers have already partitioned cookies without CHIPS. I’ve asked in a number of forums why CHIPS is needed from a developer perspective. It’s just another hurdle to try to navigate in order to get things working with no perceived benefit. Ben– are there perceived benefits beyond conforming to what is being done elsewhere?

[Ben] The concerns with just partitioning all cookies coming from these contexts is coming from both Chrome and Safari. Firefox is happily partitioning all cookies, however it is more of a compat story from our perspective.

Concerns from other browsers were about increased state in the browser. Was once before, now once on every site. CHIPS is a way to cap that as an opt-in. Other ways to do that– maybe– but haven’t seen proposals.

[Arthur] Wanted to talk re: cross-site targeting that Kaustubha mentioned. I’ve talked to Michael Kleber about this before. Cross-site targeting is almost religion to the point of forcing the product into places it’s not easy to go right now. You asked, should we relax some of the requirements. As a product owner, take 80/20 rule– would love to see Google get us there faster by finding easy use cases for cross-site retargeting, implement those, leave the others for later. Get a baseline and get APIs working at that level. Microsoft’s approach is relaxing some of those restrictions. Regulatory questions I don’t know about. Fenced frames/others harder to think through and don’t block on them.

[Erik] Getting into some of the PATCG topics here. They’re at TPAC too, more to discuss there.

[Aram] Adding on– please attend PATCG if you’re at TPAC. Big chunk of time on Friday. We’re a CG, open to everyone, please come in. Talking about this kind of thing.

[Lee] I know what Ben is saying about compat. But something to keep in mind is cookies are not always owned by service developers. Sitting behind an ELB setting cookies, need to ask those providers owning those network layers between us and the browser to make sure they set their cookies correctly. Talk of other magical headers, seems unlikely those layers will have a clue what to do with those things. Food for thought, other layers setting cookies.


# Attendees
* Erik Anderson (Microsoft Edge)
* Laszlo Gombos (Samsung)
* Benjamin VanderSloot (Mozilla)
* Kaustubha Govind (Google Chrome)
* Christian Biesinger (Google Chrome)
* Russell Stringham (Adobe)
* Shannon Roddy (LBL)
* Lee Graber (Salesforce / Tableau)
* Matthew Atkinson (Samsung)
* David Dabbs (Epsilon)
* Paul Zuehlcke (Mozilla)
* Christine Runnegar (PING co-chair)
* Tim Huang (Mozilla)
* Ari Chivukula (Google Chrome)
* Laura Morinigo (Samsung)
* Aaron Selya (Google Chrome)
* Joey Stanford (PING invited expert) - Tardy 16:08 UTC
* Sam Goto (Google Chrome)
* Brian Campbell (Ping Identity)
* Arthur Coleman (IDPrivacy)
* Chris Needham (BBC)
* Maxime Guerreiro (Cloudflare, not CG member)
* Joshua Ssengonzi (Article 19/DefendDefenders)
* Aram Zucker-Scharff (The Washington Post)
