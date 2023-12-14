# 2023-12-14 Privacy CG
* Chair: Erik
* Scribe: Nick Doty

# [Agenda](https://github.com/privacycg/meetings/blob/main/2023/telcons/12-14-agenda.md)
* [Storage Access Headers #45](https://github.com/privacycg/proposals/issues/45)
* [Add explicit behavior requirement for "Partitioned" attribute on First Party cookies #51](https://github.com/privacycg/CHIPS/issues/51)
* Any other business
    * Slack instance

# Notes

### [Storage Access Headers #45](https://github.com/privacycg/proposals/issues/45)

Chris Fredrickson: continuation of idea presented at TPAC. Storage Access API – iframes need to explicitly call a Javascript API in order to opt-in. Nothing besides an iframe can do that. And need to load/execute before you can do anything with unpartitioned cookies. This proposal is for a server to opt-in with a response header to _unpartitioned cookies_. And a request header to inform the server that this context could opt-in if it wants to.

John Wilander: confirm understanding, an HTTP response header could opt-in to partitioned or unpartitioned cookies?

Chris: unpartitioned cookies. only affects browser behavior if the user has already been prompted and granted permission.

John: in Safari, always a user activation requirement, meaningful. access immediately on page load doesn’t provide unpartitioned access, even without a prompt. could that requirement still be put in place with this proposal?

Chris: yes. if a browser wants to ensure that any opt-in also has a user interaction, that browser could choose not to honor this response header or to send a request header.

John: but the difference between browsers could force adoption of that functionality, because sites may expect it to work with the header and not test otherwise. not sure we could uphold the idea of browser vendors having different policies about what will happen after user opt-in.

Chris: if a browser does not honor this header but other browsers do, then to the server it will just look like the user has not given permission yet. so it shouldn’t introduce the compatibility concern.

John: sites could use state (recording on the server that they got permission from this user) and then expect it will work header-wise on subsequent navigation. could force browsers to abandon their user activation requirement. I’ve been happy that we have a way for browsers to vary in how strict/relaxed they are about that. going to another page on the same site a day later shouldn’t automatically have access.

Chris: would be interested in specific example of that situation. open an issue to discuss?

John: commenting widgets would be a good example; cross-site commenting iframe on a page may need to authenticate you, but that doesn’t mean that from this point on the social media company can identify you when you read the news.

Chris: could be different interpretations. if user doesn’t want the permission to persist, they can remove the permission.

John: WebKit has implemented and documented and intends to provide that behavior in our browser.

Ben VanderSloot: seems like this integrates more logically with CORS, which is a server opt-in. tighter integration with CORS Access-Control-Allow-Credentials might make sense. not sure the design is what we want here, but Mozilla thinks this a good problem to try to address.

Chris: agree about using existing things where we can, and avoiding round-trip where we can.

Brian M: +1 John. I think the permission grant is an interaction-level thing, not a session-level thing.

Chris: this doesn’t remove the opportunity, browser can still ignore the headers. if a browser doesn’t support it, will be like the state where the user hasn’t granted permission yet, like user interaction and meet-and-greet presentation.

Brian M: concern that users may be persuaded to choose browser with fewer permissions.

Kaustubha: to clarify, purely an ergonomic improvement, only if the site has been granted permission and can reduce number of round trips. if the browser policy doesn’t persist the permission, then it would be as if a user was interacting with it for the first time. don’t think this changes the dynamic. browsers might persist the permission for different amounts of time, for example, so servers would still handle the situation where the permission expired.

BenV: to JohnW, very sensitive about not disincentivizing sites from handling browsers that provide more privacy protections; so will think more about it. thinking about it as a per-page permission, though, doesn’t make sense because the page can store a paired identifier and re-connect to the third-party identifier anyway.

JohnW: I will write up an issue with the flow that I’m envisioning. per-page, if there’s collusion between the first party and third party, then user identifiers can be copied into the first-party storage space. but I don’t think we should assume that this collusion will always happen; many sites won’t do that and will recognize that there are different relationships between the top-level page and the embedded page.

Lee Graber: we’ve done a bunch of work with partitioned cookies and it sort of works. the biggest problem is … doing clickjack protection with an iframe using CSP frame ancestors, I need to return that in the response on page load. if my service is multi-tenant, then I likely need some information flowed to me on page load, doesn’t work for just subsequent requests. in trying to implement around it, but if you are an iframe that doesn’t have cookies on initial page load, your ability to use frame ancestors is difficult; possible, but requires a lot of building and hoping. could create an issue with written detail.

Erik: +1, issue with more detail would help.


### [Add explicit behavior requirement for "Partitioned" attribute on First Party cookies #51](https://github.com/privacycg/CHIPS/issues/51)

BenV: what the partitioned attribute should do for first party cookies. because Chrome is shipping the ancestor bit as a requirement for CHIPS. because much of the disagreement was about this A-B-A embedding case, I suggested that we re-open that question now.

Dylan Cutler: not shipping that change yet, but planning to in the near future.

Dylan: when the browser receives the partitioned attribute in the first party context:



1. set the partition to be the same site as the cookie site
2. reject the cookie altogether
3. ignore the attribute and just treat it as an unpartitioned cookie (but it would only be available in a third-party context)

Kaustubha: why not the status quo?

BenV: having partitioned and unpartitioned cookie jars in the top-level seems confusing. same-site partitioned cookies is a pretty unintuitive concept. prefer avoiding Case 1.

Brian Campbell: applications that don’t necessarily know how they’ll be used or embedded, but want to set cookies and continue to work in cases where they’ll be partitioned. without knowing for sure, it would be nice if those cookies would be recognized as regular first party cookies. please don’t start breaking that for non-embedded contexts, as we start adding the Partitioned attribute on our cookies.

Lee: +1. don’t know reliably whether we’re embedded or not. SameSite=None was annoying for variation between different browsers. We are just setting Partitioned everywhere. If browsers throw those cookies away, I don’t know how we’ll be able to set the Partitioned flag.

JohnW: my gut feeling was to drop/ignore the cookie altogether, because quirky behavior can have downstream problems because the developer doesn’t know how it works. but I can see that the easier path is that the developer should just always set Partitioned if they will sometimes be partitioned. What about renaming the attribute to signal “Only Partition me if you have to, but if you have to, I’m fine with that”

Kaustubha: for cases where you’re setting the Partitioned attribute all the time, would you expect that cookie to become available when you call Storage Access API and get permission in some embedded context?

Brian C: we aren’t even thinking about that. we are just trying to get the cookies when they’re set in a third-party context. doesn’t want it to break in a first party context.

Lee: we don’t use Storage Access API at the moment. I would expect that if we did, we would think of it as a first-party cookie and get access to it. but we aren’t trying to share our cookies across sites. in fact, we have problems that will be solved by partitioning them – some customers think it’s working by logging in to a site directly, which isn’t actually the intended use case. for identity purposes, we want to know if you’re logged in to Okta.

Kaustubha: thanks. do any of these cookies involve authentication? for authenticated embeds, identity/login cookies are a prominent use case. if we treat it as an unpartitioned cookie, then we need to make it available elsewhere. otherwise, we need to treat it as SameSite=Strict, – that seems like better behavior.

Dylan: if rejecting the cookie is bad… would it help if you sent the Set-Cookie response header twice? you’d be creating a SameSite Lax cookie by using a different mechanism.

Lee: it’s possible to modify the cookie code we call. but no standard way to know if you’re embedded or not, and a lot of old code. I would love to not have to do it. things have been added over years, so I would like to avoid asking developers to do weird things.

JohnW: a way for the developer/server to express that they want this cookie to never be available in a cross-site context. But that still doesn’t help Lee/Brian who are fine either way but don’t especially want it to be isolated. Server can use SameSite=strict and SameSite=lax to tell your server about the context that you’re in. Still a bootstrapping challenge, how to know your context when you have no cookies at all.

BenV: should have informational headers, overdue to indicate whether or not you’re partitioned. I think we should settle on something there. semantics should guide the name. what Lee is proposing is the semantic “if I set this, I’m willing to be partitioned in a third-party context” – it’s a reasonable semantic and one that makes sense to me.

Lee: yes, that was my original intention. if I set that as a first-party, then I’d just be setting the context to “/” root.

Kaustubha: Partitioned is a privacy-oriented semantic, not the security semantic.

Kaustubha: re informational headers, one option is to reject the cookie but we make it clear what context you’re in so that you can decide. deploying support for new request headers increases the size of requests, servers and middleboxen may not like it. or opt-in would require another roundtrip. because it’s complex, I’m trying to figure out if it would even be useful to indicate what kind of context it is.

Lee: maybe. our intent/preference would just be to express our intent regardless of the context.

Lee: one trick we use is to call an endpoint (CORS-enabled) from the first-party context to convert a JWT we were given into a cookie that we mark as Partitioned and then load the page with that cookie, in order to get the identity that we can use as ancestor to avoid clickjacking attack. right now it does work. I hope that this won’t break our workaround.

Aram: it would be preferable to just signal intent and get the cookie sent regardless. but also it would be useful to know what context we are in. I would love both. I could see how both would be useful.


### Any other business

Erik: Ari wanted to note that [Google Chrome is running a trial for the Storage Access API extension](https://developer.chrome.com/blog/saa-non-cookie-storage) discussed in this group at TPAC. Feedback can be left [here](https://github.com/arichiv/saa-non-cookie-storage/issues). (I will probably be gone from the meeting by this point so just wanted to be sure there was a record for people to see in the notes)

Nick Doty updates from PING: Privacy Working Group charter for review; merging Slack instances.

Nick: PING co-chair here, charter was expiring. Proposing Privacy WG charter to take on existing horizontal review activities. Make it clear the group isn’t doing incubation (Privacy CG is where that happens). Privacy CG deliverables that don’t have a natural standardization home for rec track could go here. Privacy Control is an existing one, possibly Bounce Tracking. Just a proposed charter out for review. Give comments or ask your AC to look at it.

Nick: W3C Privacy Slack was created a long time ago when there wasn’t a general W3C Slack. Confusing because there is now a W3C Community Slack which has a bunch of groups. Proposing to merge Slacks.

Tess: best idea ever.

Nick: great, sounds like that’s what we want to do.

Sam: I will be moving on as W3C team role after 1st of year. Been a privilege, look forward to working with you as a member of the broader internet standards community to improve privacy on the web.


## Attendees
* Erik Anderson (Microsoft Edge)
* Chris Fredrickson (Google Chrome)
* Kaustubha Govind (Google Chrome)
* Chris Needham (BBC)
* Brian May (dstillery)
* Nick Doty (CDT)
* Sam Weiler (W3C)
* Aaron Selya (Google Chrome)
* Darryl Green Jr (Washington Post)
* Cameron Boozarjomehri (Mozilla)
* Benjamin VanderSloot (Mozilla)
* Craig Erickson
* Dylan Cutler (Google Chrome)
* Erik Taubeneck (Meta)
* John Wilander (Apple WebKit)
* Ari Chivukula (Google Chrome)
* Aram Zucker-Scharff (The Washington Post)
* Lee Graber (Tableau/Salesforce)
* Arthur Coleman
* Claude Vervoort
* Brian Campbell (Ping)
* Theresa O’Connor (Apple, TAG)
* Susan Stroud
* Tim Cappalli (Microsoft)
