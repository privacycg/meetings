# Privacy Community Group F2F, 14 May

Chairs: Erik, Tanvi, Tess

## Agenda

* [Client-Side Storage Partitioning](https://github.com/privacycg/storage-partitioning/)
    * [What state should be blocked?](https://github.com/privacycg/storage-partitioning/issues/8)
    * [What state should be able to have its keying relaxed?](https://github.com/privacycg/storage-partitioning/issues/7)
    * [Fix Web-Platform-Tests](https://github.com/privacycg/storage-partitioning/issues/2/)
* [Private Click Measurement](https://github.com/privacycg/private-click-measurement/)
    * [Select a fraud prevention mechanism](https://github.com/privacycg/private-click-measurement/issues/27)
    * [Consider defining a modern JS API in addition to the tracking pixel mechanism](https://github.com/privacycg/private-click-measurement/issues/31)
    * [Layering and the Click Through Conversion Measurement Event-Level API](https://github.com/privacycg/private-click-measurement/labels/layering)
* [Storage Access API](https://github.com/privacycg/storage-access/)
    * [Active or passive storage access after explicit user opt-in](https://github.com/privacycg/storage-access/issues/2)
    * [Storage Types Covered](https://github.com/privacycg/storage-access/issues/4)
    * [Consumption of User Gesture](https://github.com/privacycg/storage-access/issues/25)
* Conclusions

## Attendees TBD

## Welcome

Tess: three work items today.

## [Client-Side Storage Partitioning](https://github.com/privacycg/storage-partitioning/)

Chair: Tess

Presenter: Anne

Scribe: Sam

Anne: I call this state partitioning.  idea is to isolate trackers.  3rd parties embedded on many domains can't have shared state.  want top level site to be unable to detect visiting another site, e.g. by cache attacks or state attacks.  e.g. if A embeds B, A could figure out that there's another top level B eleswhere.  Published as "[XS (Cross-Site) leaks](https://github.com/xsleaks/xsleaks)".  Could figure out credit card numbers, what user is doing in gmail, what kinds of pages they like, what tages are on google photos.  Addresses both privacy and security issues with same soln, which is partitioning.

Huge effort; crosses many standards.  This repo links to all of these efforts; goal is a wholistic view.  Will need Fetch, HTML, storage spec changes.

Some issues on repo itself; some external.

Tanvi: would be nice to head from browsers if/how they're implementing this, before issues.

Anne: doc section "user agent state".  Firefox working on "dynamic first part isolation".  covers cookies and all storage; depends on Storage Access API.  Ambition is to partition all of network state unconditionally, nevermind Storage Access API.  Remainder of items may require specific solns.  e.g. BlockURL - we proposed a way to isolate it, meeting same goals but different mechanism.

Josh Karlin: Google well on way to splitting the disk cache; launching in next few months.  After that, split network cache stuff.  e.g. DoH.
Network Isolation Key (NIK)
Partition cache and network not just by @@ site but also by Schemeful Site.  Triple key, not just double key, since also need to partition by the schemeful site of the iframe you're in.  Questions are if things need special handling.
Open question is 3rd party storage.  We're leaning toward double/triple keying that.  Want to provide specific APIs for use cases, e.g. for login/auth/payments.  We hope these APIs cover the user cases and we don't need an escape hatch like Storage Access.  Our concern is that once you grant Storage Access, you can't take it away until you clear state.

Anne: When you said disk cache, did you mean HTTP cache or more?

Josh: HTTP cache.

Maicej: WebKit did this when Evercookies were popular.  when they were being used to resurrect each other.  We had old 3rd party cookie policy.  We had double-keyed cache - keyed by eTLD+1 + URL to be cached.  Also partition local storage.  BLocking IndexedDB in 3rd party contect; no web compat issues.  Open to changing that.  Cookies: recently changed to blocking both reading and writing in 3rd party contexts.  We're finding that it's more compatible than partitioning.  For some kinds of storage, more obscure forms of state, had to find unique solns.  e.g. for HSTS there are both security and privacy issues.  Bad to block it entirely in 3rd party contexts - it's designed to e.g. prevent attacks from a hostile local network.  This might require IETF work to fix.  We want to get this into standards; we'll write them up - though e.g. the HSTS thing is in a blog post.  Happy to answer questions.

Tom: Brave has the benefit of inheriting everything Chrome does.  We're most excited about farbling, as Pete discussed yesterday. 

Kris Chapman: any plans for future for how orgs with multiple domains could get keying relaxed?  e.g. sports reporting sites have different domains per sport.  Salesforce does it.

Anne: as user visiting baseball site, I might not want that linked to the football site

Kris: User might not want to interact every time.  Domains make sense re: architectural easiness, but not from user perspective

Anne: I disagree. (Thanks to Maciej for explaining why in detail. :-))

Maciej Stachowiak: issue of orgs having multiple domains - there have been a number of proposals.  First Party Sets from Mike West.  Idea is some trustworthy record of domains being affiliated.  One challenge is bad-faith claims - e.g. claims for purposes of ad network.  This could bring back tracking.

2nd issue: how do you get users to understand?  e.g. google.de v. google.com.  But do people know youtube.com is Google?  But then's there's OATH (Verizon Media), with "500" domains; user won't know they're linked.  Users would not think of that as single first party.  Difficult conceptual problem.  Difficult to design one that does not lead to circumvention or violation of user expectations.  First Party Sets is a little different - it doesn't directly bear of the partitioning discussion.  This comes up as a compat issue, esp. re: cookies.  I think we should go back to talking re: partitioning and move this to another discussion.  Also comment in issue in repo.

https://github.com/privacycg/proposals/issues/11

John Wilander: chairs/Anne?

Tanvi: concur

[Kaustubha Govind, Sam Weiler dropped from queue]

Erik: current doc says origin; Kris said eTLD+1.  May need to think on that more?

Anne: starts with origin or site.  UA's seem to converge on "top level site".  More subtle.  Google uses site.  Firefox ignores scheme.  Which leaves you with registerable domain.  Or IP w/ registerable domain of nill.  Including scheme would be good; keep secure and insecure separate.

Jeffrey Yasskin: I suggest we use "site" in talking about this, not implementation detail of eTLD+1, which postpones discussion of First Party Sets, which changes definition of site.

Josh Karlin: Google is partitioning by "schemeful sites" = scheme + eTLD+1

David Benjamin: origin might be too strong.  Subdomains are important for things like XSS. We need a definition of "site" than is broader than "domain".

Anne: ["site"](https://html.spec.whatwg.org/multipage/origin.html#sites) is defined.

Tess: queue closed so we can dig into these issues.

Josh Karlin: would prefer specific APIs for use cases; push for later.

### [What state should be able to have its keying relaxed?](https://github.com/privacycg/storage-partitioning/issues/7)

Steve Englehardt: Firefox would love to do partitioning, but we know it breaks things.  We've had first party isolation for a while, coming over from Tor.  Not shippable now.  We have heuristics to automatically relax blocking in some scenarios.  Can we bring that to partititioning?  Two questions: do we want to do this at all, for web compat reasons, to ship partitioning earlier?  If we wait for fixes, it will be a long road; partition most storage sooner.  For web compat, we're more willing to do it.  Want to chat more re: Storage Access API.

Tom: Brave is willing to nudge sites to make changes.

Anne: Firefox can relax keying for most of storage APIs and cookies; I think safari only does it for cookies, which is weird given the name.  add'l issues with storage, since APis have communications channels - if you're currently partitioned and open channel in future could end up in a weird situation where you can communicate with both first party site and partitioned site.  If you allow relaxing of keying, maybe some should be blocked, like Service Workers.  For web developers, ends up being weird.

Andrea M: No problems implementation-wise.

Josh Karlin: allowing service workers to be merge... inclinded to disallow or keep them partitioned.  curious as to others' thoughts.

Complicated for publisher, in frame, to switch storage, so it's easy for dev to know what they're referring to at a given time.

John: we made localstorage both partitioned and _ephemeral_.  Could serve as the middle way for e.g. ServiceWorkers, if we don't want to switch them.  

Michael Kleber: I like the ephemeral thing.  Would be good to figure out scope of that, e.g. "until window from that domain is no longer open", like incognito, or maybe 24hr max?  Could help w/o allowing long term bridging.

### [Fix Web-Platform-Tests](https://github.com/privacycg/storage-partitioning/issues/2/)

Anne: not that many failures.  Other browsers?

John W: not sure.  We've had issues with various priv decisions.

Maciej Stachowiak: sicne we did partititioning before WPT existed, we didn't notice.  We haven't hit many failures.

Anne: most failures we noticed were around COOP.  Many tests are a single top-level browsing context, and that does not break.

### [What state should be blocked?](https://github.com/privacycg/storage-partitioning/issues/8)

Anne: is list complete?  Are we agreed on all?  Anything controversial?

John W: web compat - will things start to break?  performance?  I think no significant impact on caches.

Maciej Stachowiak: our tests shows benefit of caching.  we did not find any diff from cache partitioning, but that was many years ago (2012-13?).  Not sure that result holds.  Would be good to test and share mitigation techniques.

Josh Karlin: we're seeing 1-2% hit, and costs for lots of extra requests for some widespread 3rd-party embedded things e.g. fonts.

Anne: please [file issues](https://github.com/privacycg/storage-partitioning/issues)
