# Privacy CG telcon - 13 February 2020

* Chair: Erik
* Scribe: Pete Snyder (pes)

## Attendees (sign yourself in)

* Erik Anderson (co-chair, Microsoft)
* Theresa O'Connor (co-chair, Apple)
* Pete Snyder (Brave Software)
* Zach Edwards 
* Mark Xue (Apple)
* Nick Doty (UC Berkeley)
* Chris Needham (BBC)
* John Wilander (Apple)
* Don Marti 
* Pranjal Jumde (Brave)
* Sam Macbeth (Cliqz)
* Mike O'Neill
* Christy Harris (Future of Privacy Forum)
* Bruno Roggeri (Criteo)
* Jack Frankland
* Ehsan Akhgari (Mozilla)
* Brandon Maslen (Microsoft)
* James Hartig (Admiral)
* Jacek Oleksy (Opera)
* Kris Chapman (Salesforce)
* Melanie Richards (Microsoft)
* Scott Low (Microsoft)
* Paras Chetal
* Krzysztof Modras (Cliqz)
* Konrad Dzwinel (DuckDuckGo)
* Dick Hardt
* Tom Lowenthal (Brave)
* Kushal Dave (Scroll)
* Shivan Sahib
* Tanvi Vyas (Mozilla)

## Agenda

* Introductions
* Charter updates
  * [New Work Item: Storage Access API](https://github.com/privacycg/admin/issues/5)
  * [Other updates](https://github.com/privacycg/admin/issues/7)
* Proposal: [full storage partitioning/double-keying](https://github.com/privacycg/proposals/issues/4)
* [Storage Access API](https://github.com/privacycg/admin/issues/5)
  * [Per-frame or per-page storage access](https://github.com/privacycg/storage-access/issues/3)
  * [Storage types covered](https://github.com/privacycg/storage-access/issues/4)
* Any other business

## Introductions

- Erik A: Welcomes and introductions.  Folks should attend PING too we've adopted Storage Access API as a "work item" (there is interest in the API). storage access api is shipping in Firefox, and MS is considering


## Charter updates

- Tess: Group is ~1m old, lots of enthusiasm, ~80 members now. lots of feedback on the charter over that month. admin issue 7 has description of changes we've made based on this feedback.  Some termnology changes (e.g. deliverable -> work item) biggest change is that charter now documents how proposals are made, and how proposals become work items other feedback is welcome on the charter too

## Proposal: [full storage partitioning/double-keying](https://github.com/privacycg/proposals/issues/4)

- John W: Proposal is from Maciej at Apple, ships in webkit.  So far called "double keying" tl;dr is any storage gets "siloed" per eTLD+1 (in the Safari implementation at least) currently applied HTTP cache, localStorage and service workers, could be applied else where cookies are not currently partitioned, for web compat reasons. This has been discussed a while, but not proper standardized / documented.  Thats the goal of this proposal.

- Erik A: Apple is supportive, Moz is supportive

- Mike O: (drops)

- pes: whats the dependency between features?  What blocks what?

- John W: Paritionining is necessary but not sufficent for Storage Access API, but not the other way around. cookies are simpler, but other more recent systems (e.g. indexDB) are more difficult to deal with. Storage Access API in Safari *does not* go through paritioning, but Firefox may be different.

- Erik A: (queue please!)

- Mike O: Could you double key cookies if you had some opt-in prefix?

- John W: That has not been considered, but might be useful. we'd be happy to share data about breakage [pes: yes, please!] problems are related to servers not expecting to see the same cookie name w/ diff values

- Kris C: Current this targets eTLD+1. What about trusted domain groups (e.g. first party sets)

- John W: Apple pitched a related proposal to W3C WebAppSec, did not receive much interest. post Safari ITP there was more interest.  There is a related explainaer called ["first party sets"](https://github.com/krgovind/first-party-sets) for a current proposal.  Difficult questions involve how big the sets could be, dynamic vs static, etc.

- Kris C: Does it make sense to have this before double key'ed storage?

- John W: WebKit has it currently enabled w/o 1p sets, but could make sense from a standardization point of view

- Erik A: This has been disucssed in IETF, WebAppSec; seems like the dependencies might not be blocking

- pes: please share data about breakage!

- John W: not crawl-like data, but bug reports.

- Mike O: D-Bound ("Domain Boundaries") in IETF is a DNS like approach to "first party sets"
  (npdoty: dbound sounds totally interesting and relevant! but also IETF concluded the WG in 2017. is there info on what happened or what they decided on?)
  (There is still activity in the list, I know one of the people I will ask them)
  

- Tom L: There are risks related to binding first parties, not clear that users understand the relationship between them, might introduce privacy leaks, its tricky.

- Nick D: Would storage partitioning affect protocol, or just browser behavior?  Can sites interact w/ different storage containers?

- John W: This is driven by developers and people not sure what Safari is doing. So far, in WebKit, there is not intentional signal to sites that partitioning is happening. Storage Access API is part of that proposal (to allow sites to interact with paritions).

- Sam M: my understanding is that WebKit uses a list of domains to partition, while Firefox does full partitioning. Is this correct?

- John W: This is not the case, WebKit paritioning does not use a domain / rule list.

- Ehsan: We see breakage.  Diff between Safari and Firefox is that there are special cases for Safari in websites. predictable standards would reduce breakage.

## [Storage Access API](https://github.com/privacycg/admin/issues/5)

- Erik A: Next, Storage Access API (SAA)

- John W: SAA is now a work item for this community group.  There are differences between Mozilla and WebKit's implementation of SAA. Some of these are likely the result of documentation ambiguities. There are two differences to describe in standards: (i) per frame vs per page (e.g. does SAA relax paritioning per frame or for everything in the document). (ii) cookies vs other storage types.

### [Per-frame or per-page storage access](https://github.com/privacycg/storage-access/issues/3)
- John W: In WebKit, SAA relaxes paritioning per (top level doc, iframe); In Mozilla's implementation, SAA relaxes per (top level doc, origin), so that multiple iframes from the same origin would all be effected SAA proposal is not desigend to enable "the old world" / what exists today; its designed to make web behavior more closely match user expectations / understanding. Each frame that wants storage access needs to ask permission. Moz and MS report less breakage / more compatability with the Moz approach. WebKit / Apple approach is that SAA should be what we want the web to be, not to maximize compat with current web.

- Erik A: Is this issue (per frame vs per origin) about user expectations vs site expectations?  E.g. are sites working with reasonable expectations the API breaks?

- John W: More the latter; we should agree on how SAA would work so that devs can work against predictable behavior. WebKit also measures "legacy mode" / "temp compat fix", where there are some apps that expect 3p storage access across multiple frames / popups.  In these cases (popups), SAA applies to the page; this is temp and will go away.

- Mike O: There should be some way for the user to understand which companies own which domains.

- John W: please file issue, good goal. WebKit implementation requires that the user interact w/ the 3p as a 1p + user gesture to increase chance user will recognize the 3p domain.

- Ehsan: The documentation is unclear regarding popups and temp compat fixing. in FF, storage access is granted to all iframes on the page in both popup compatibility mode and storage access API mode. knowing that Safari doesn't prompt again in storage access API mode and that in popup compat mode access is granted to all iframes makes the idea of changing our behaviour mode palatable, but we still would like to discuss this more internally.

- John W: Some confusion that per-frame resolution was a privacy feature, but that was not the intent

- Kris C: WebKit gives feedback to developers about what SAA does; the documentation is straight forward but doesn't seem comprehensive / useful for debugging in all cases

- John W: We've heard the same (also re ITP).  We should do more, including failure cases. some of this has been discussed in WHATWG, and some of the current decisions are the result of implementation difficulties / concerns.

### [Storage types covered](https://github.com/privacycg/storage-access/issues/4)

- John W: In WebKit implementation of SAA, only cookies change.  All other storage is partitioned. but, as per SAA name (i.e this puts the storage in SAA), SAA was designed to possibly cover all storage types. I (John W) think FF's implementation covers more than just cookies. FF goes from block -> allow under SAA, in WebKit its partition -> allow, which is tricky for correctness and consistency. Moz and MS implementation experence would be valueable to know about.

- Ehsan: as John W said, in FF, SAA moves storage from blocked -> granted, and all storage (cookies, localStorage, etc).  Its much easier in the FF model b/c there is no consistency problem (e.g. DB transactions under changing access). Doing partioned storage -> 1p storage is difficult and needs got be discussed.

- Erik A: MS experience has been similar (correctness is tricky)

## Any other business

- Pete: Link Decoration. What is Privacy CG's plan for this in the future?

- Tess: for any proposal, regardless of topic, we can spend call time on it if the people contributing would like that-- just ask to have it added to the agenda.

- Pete: what's the current status-- has anyone asked for that?

- Tess: bounce tracking or link decoration? Not that I'm aware of.

- John W: re bounce tracking, the goal is to see if the PrivacyCG would like to discuss / think through as a problem. And if there is a set of relatd problems (link decoration, caches, etc) would be good for f2f meeting
-  [pes: f2f sounds like an excellent idea]
  
- Bruno R: Is this the right place to describe what counts as user consent in browsers re GDPR

- Erik A: Lets discuss in PrivacyCG, and see vendor interest

- Tess: PrivacyCG is good for APIs, but not legal questions.  But what an API does and how it works is perfect for PrivacyCG

- Nick D: re Storage Access, good to think through relationship between SAA and permissions, and other permission related concerns (UX, conveting info to users, standards book keeping).

- Erik A: Good questions here, including what needs to be standardized re site expectations.  And issue would be a good way to move forward

- Nick D: sounds good, i'll create an issue

- John W: sounds good, but there are 3 existing issues (pes: John W, can you fill in links post call) WebKit and FF should also post screenshots about how prompts look.

- Nick D: but, the goal is to consider this a permission state?

- John W: 3p can ask w/o permission, 3p doesn't get to effect presentation / prompt.

- Erik A: There is a permissino prompt, need more discussion

- Scott L: MS used prompts from Moz and Apple for UX research.  Happy to share results-
- [pes: yes please!]
