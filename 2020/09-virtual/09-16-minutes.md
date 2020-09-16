# Privacy CG F2F, 16 September 2020

[Agenda link](https://github.com/privacycg/meetings/tree/master/2020/09-virtual)

*   Chair: varies throughout the day


#### Introductions
See self-written intros for each participant at the end of the minutes.


#### Storage Partitioning
*   Scribe: Arthur

*   [Clear-Site-Data for partitioned storage can be used for cross-site tracking](https://github.com/privacycg/storage-partitioning/issues/11)
    *   Anne: On July 23 I put in a comment to bring to the attention to the group, and there hasn’t been any follow-up. It’s a header that allows the website to clear storage data, localstorage, indexeddb, Service Workers. Allows for clearing cookies, clearing caches. Allows for reloading execution envs in theory, though not in practice. Part of the issue here is how clear site data should work for cookies, storage and caches in light of state partitioning.
    *   Anne: There are multiple options discussed in the thread. Where I have come down, within a document, if that doc is partitioned and it has the header set, it would only clear the partition data for that document or partition. If it’s a top-level document that is non-partitioned, it would only clear the partitioned data for that document and it wouldn’t clear data in the sub-tree. Minimum amount of storage that needs to be cleared. Caches are problematic -- and that was already problematic in the original Clear-Site-Data. This would be more granular. Not sure if we really need cache to begin with as a feature. In part my proposal is that we don’t do reloading of execution contexts, we don’t do clearing of the http cache, because it doesn’t really match the architecture of how the cache is allocated and stored. There is a weird mismatch where you can go through a subset of the partition but you get really clumsy operations. So I wonder if you really need the functionality at all. Was this clear enough?
    *   John Wilander (Apple): We have talked on other threads that partitioned storage should or could be ephemeral and it is the case of Webkit’s implementation. Wonder if that plays in somehow -- Clear-Site-Data all the data is ephemeral -- does that make it less important somehow?
    *   Anne: When you say ephemeral it’s about a week?
    *   John: Doesn’t have a timer, it’s in-memory only. Eric Lawrence from the Edge team wrote a blog about it: other browses persist session data or session cookies. Not the case with webkit everything that’s session based or ephemeral is in-memory only. After you close the device, everything goes away.
    *   Anne: If for third parties, storage would always be ephemeral, and guess you could not support Clear-Site Data for third parties. I hope that’s not where we end up for third parties.
    *   John: That’s a conversation to be had. Someone on the Chrome team (Michael K) said ephemeral sounds pretty good for partitioned. We are all envisioning a way for giving third party access to storage.
    *   Erik: On the cache discussion and execution context -- do we have a good discussion on how website authors are depending on clear-site data, e.g. bad service worker push. Would this suggestion impact that?
    *   Anne: SW would not be impacted, but SW are part of storage, including Cache API in SW. Cache API in SW is a storage API. Cache thing that Clear-Site-Data is exclusively the http cache. Maybe some related network state. Nothing at the API level.
    *   Josh: In regards to the cache, I need to follow up with engs, before partitioning it’s not something we can support very well. Once we have the partitioning, it would be a reasonable operation to clear everything in a partition. I agree with Anne that we should do it by partition and not everything else on that page. In terms of ephemeral, I don’t remember anyone on Chrome I don’t remember anyone saying that is what we intended or agreed on. I think it might hurt some use cases where someone wants to store third party info but doesn’t want to share with first party.
    *   Anne: I think what you’re saying is if the top-level does clear-site-data in the cache, then you could clear the entire tree in its partition. Wouldn’t necessarily work of one of the frames did that. If A embeds B and B does Clear-Site Data, you don’t want to clear the cache for A.
    *   Josh: That’s right. Chrome is contemplating partitioning both by the top frame and the current frame, so you would just need to clear that data.
    *   Anne: I’m also concerned about the top-frame being able to influence the caches of its subframes. Sub frame thought it was in one state and then the top frame affects that state, and sub-frame goes down that path.
    *   Josh: It’s a poorly supported API in Chrome.
    *   Anne: It’s also a hack in Firefox .Would rather get rid of it until we have better use cases for clearing the cache and figureI’ll  out what better use cases we have.
    *   Josh: It’s very expensive for us as well. We have to enumerate everything in the cache to delete. So we don’t want headers to do that because performance would be abysmal.
    *   Anne: It’s a DOS attack
    *   Valentino: I want to understand the cross-domain nature of this. Shopify is an example, but you can see in many hosted blog platforms. Automattic and Wordpress do. How would clear-site-data work in the case where there is this kind of relationship?
    *   Anne: You have to get the other domain to also serve the header. One origin cannot make statements for the other origin, except in the case of cookies, which are scope to the site boundary. Everything else is origin bound.
    *   Valentino: Haven’t seen a lot of in-depth docs on clear-site-data.
    *   Anne: There’s an existing standard and we’re trying to figure out how to adapt it to a world with storage partitioning.
    *   Charlie Harrison: If I’m a top-level site (A.com) and I use clear-site-data, then it will clear origins in third-parties?
    *   Anne: Only the storage they would have access to.
    *   Charlie: My one concern is, if I am A.com and I had a user navigate to a page that said I am going to clear all the users data, do we feel the proposal make this difficult because the top-level site doesn’t have a way of easily clearing all the partitioned data. If A.com logs a bunch of data on different sites, then it can’t fulfil the promise of clearing the data for that one particular user. I wonder if there are subtle implications there with respect to data deletion.
    *   Anne: All of the data is hosted on a single origin, but the origin is sometimes partitioned.
    *   Charlie: Yes, A/com is sometimes a 3rd party. But clear across sites could also be a side channel. Damned if you do, damned if you don’t
    *   Anne: I think the side channel is worse. We want a model where the site is in the address bar. Try to model the web along that architecture. I can see there might be some tricky implications.
    *   John W: In our implementation decision to use ephemeral for partitioned storage, this kind of played into it. You’ll have these residues that’s partitioned along a great many websites for popular third parties serving content. Completely out of sync with what they see as a first party. Hey, let’s make sure it goes away periodically when you quit browser or restart the device. I filed issue #11 explaining exactly how it can be leverage for cross site tracking. (This issue under discussion.)
    *   Erik: I agree with Charlie that there may be some other important use case under data management UX piece. Side channel seems worth. Something we should think about more. New primitive? Or user education, you need to do this action through browser UI. Pretty confident that someone will comment and say this is important to them.

*   [Expose the first party to a partitioned third party](https://github.com/privacycg/storage-partitioning/issues/14)
    *   Anne: We discussed this in the telcon a while back. It’s about exposing the first party to partitioned third parties. Or third parties in general. We are wondering if we could explore that a bit more. Tanvi brought up the case where if you have A.com embeds B.com and B.com embeds C.com in turn, and A.com might be fine with sharing its identity with B, but not OK with sharing its identity with C.com. What do folks think about that? In general, if there is more input on this issue and how valuable it is for content producers to have this information, on who their first party is?
    *   Michael Kleber: At a philosophical level, I find it unpleasant to think about the state of being partitioned being inherently impossible to know what partitioned you are inside of? The data is a key value store but you’re not allowed to look up the key where you are looking something up. I was involved in discussion with ITP about the difficulty of partitioning cookies where you couldn’t tell if a cookie was partitioned or what partition it belonged to. I think there’s a lot of benefit of exposing the first party domain to show what partition you are inside of.
    *   Erik: Is this describing a new header to make sure every request carries this context?
    *   Anne: The proposal in tht telcon was maybe a header, and at the API level it should be queryable. If you have the former you can make the latter yourself, but the browser might as well expose it for convenience.
    *   Josh: On some browsers, there’s ancestor origins you can figure out what your top origin is. I worry if a frame is embedded, it might want to know it is being embedded and say I don’t want to be embedded on this site. It might make a lot of sense at the header level.
    *   Anne: I think currently Chrome and Safari expose the first party already through Ancestor Origins. Definitely in pages and maybe in SW. Firefox still exposes neither. You might be able to query some stuff in Firefox by clever use of frame ancestors in CSP, but that would be a more exhaustive search rather than definitive ancestors. We do plan to patch at some point but it’s not been a priority. Also curious about Apple’s perspective because they also have been shipping Ancestor Origins, if they are also thinking of reducing along similar lines.
    *   John: We know about this, it is kind of a legacy thing. Someone filed an issue way back . We have been thinking about it. We don’t know the consequences of deprecating or obsoleting it. Don’t know how quickly or willing we would be to remove it.
    *   Anne: BZ and I came up with a scheme where the referrer policy would dictate what ends up getting exposed in Ancestor Origins, but it didn’t pick up steam at the time. Since Google and Apple weren’t keen on addressing it, we didn’t either. We’re also not too keen on exposing it.
    *   Tanvi: I was going to respond to Josh that the website can still use X-Frame-Options to decide what’s allowed to embed it.
    *   Erik: That allows embedding or not. I don’t allow one origin but another is OK?
    *   Anne: X-Frame-Options is only Deny or Same-Origin. With Frame Ancestors, you’d have to enumerate all the possible places you are embedded in. You might want to make a decision on the server instead.
    *   Erik: Not always allow it -- make sure there’s a clear signal. You may be embedded, but whoever’s embedding you do not want you to know who they are. I’d have to determine whether there are other means anyway.
    *   Anne: With the HTTP header, Michael, are you envisioning it would only be there if the storage is being partitioned? Might be hard to determine up front. You might only be able to determine one you are creating the context. Sharing who your first party is and sharing what your storage key is going to be in this header.
    *   Michael: Really good q. I was thinking of sharing what your storage partitioning key is. I think either one could work. Do you have partitioned or unpartitioned storage is a really useful signal for letting servers work with this maybe surprising state.
    *   Anne: I was confused, because you definitely have to know it up front because of cookies.
    *   John: We’ve been doing partitioning for 8 years. It’s been in our toolbox ever since. More and more it’s becoming stateful down in the network stack. HTTP3 state. DNS Caches, TLS sessions and resumption of those. Is this the right place to talk about those things? Or do we need to spin up an IETF sibling effort?
    *   Anne: I think for now we should talk about them here. I was hoping that over time that it was address these mostly in the Fetch standard, in a similar way that the Fetch standard encapsulates the HTTP cache. We could do similar things for DNS and other things. You get your DNS subsystem from this algorithm, and the algo keys it on various things. We have enumerated all this network state already in the storage partitioning document, but we haven’t turned it into prose yet. Definitely in scope. Don’t see that there is much need for IETF side involvement unless we find a place where it gets really unclear.
    *   John: And the rare cases where things are security related: HSTS and HPKP. There was a recent discussion on the W3C Web-App-Sec where we were talking Artur Janc from Google about something similar to CSP a site could say -- for my whole origin apply these protections. I think it was opting out of legacy tech. Please apply to my whole origin. This becomes state.
    *   Anne: Ideally if we do new technology we take this into account from the start. Maybe you are talking about Origin-Policy?
    *   John: I’m just thinking about cases where it is about security. I’m thinking about HSTS. We set HSTS to deal with TLS situations, but if we partition, then there is this trade off (between sec and privacy). Do we partition all of it?
    *   Anne: For HSTS the thing that seems most promising is if we can upgrade all mixed content. Only media can be mixed content currently, but if all mixed content is upgraded, we don’t have to care about third parties.
    *   John: Except when the first party is HTTP, which means that the third party loaded over HTTP will not be mixed content. So HSTS could have helped them but it won’t. The attacker will deliberately move the user to a non-secure page. Not for us to solve here, but it is an interesting tradeoff. Not just work and compat.
    *   Anne: That’s fair.
    *   Josh: Two things: (1) We (Chrome, Matt Menke) are actively working with Anne in terms of spec and currently writing up an explainer. We have partitioned socket pools, DNS, HSTS, all the things. The V8 code cache. We’re talking about -- favicon cache also needs to be double keeyed. Site Isolation also needs to be double keeyed. We have a list of a hundred things. Want to talk about -- what is the partition? We have been playing with is top-frame “site”. We have also been playing with notion of top-frame “site” and “current frame”. If we just key by the top frame site, all the frames can snoop on each other. If we key frame top-frame and current site, then individual frames should not be able to snoop on each other. Although Anne and others have pointed out there are side-channels leaked. We are launching on cache in the near future. Would make sense with compat to go with the top site. For security purposes, I think it makes sense to isolate individual frames. It should also apply across the board to sockets as well.
    *   Josh will create an issue 
    *   Anne: We are interested in exploring that. For now we have decided to just focus on top-level sites, because that’s a big security win over the status quo. Hard to reason about properties of additional keying. We considering for a while each frame adds a key. Even if you have top-level site, plus your current thing, you can not attack your parent, but you can attack your sibling still. Some attacks were still possible. You couldn’t attack your siblings but you can embed siblings. There’s already a number of attack scenarios on embedded websites. It wasn’t entirely clear if solving the cache problem would move the needle enough. We are definitely open to exploring that and adding more keys in the future. I think it needs some more analysis.
    *   Josh: Getting a position on what you are planning on doing helps to.
    *   Kaustubha: Follow up question about the fact that Firefox was not sure about supporting the Ancestor API. What’s the reasoning behind that. Is there an attack we’re worried about?
    *   Anne: I’m trying to remember the exact scenario. I don’t think it was an attack but it was that we didn’t want to leak that information to third parties, what the user was currently visiting. Suppose there’s a popular third party that embeds video. We wouldn’t want that video embedding website to know all the sites the user is visit. We don’t want to have that necessarily. To some extent this would already leak through referer, but if the top-level site did the extra step of not leaking its referrer, then it should definitely not leak that data by other means.
    *   Kaustubha: I think that definitely makes sense. I think we were talking about this being in a partitioned world. Is that sort of attack. If that embedded site has partitioned storage, do we imagine a way there would be for a third-party site to correlate to a users visits across origins?
    *   Anne: We haven’t solved that, there's still IP address, and we also haven’t solved fingerprinting which is pretty tricky. 
    *   Erik: I have put a link in the slack -- ancestor origins and referrer policy. A refresher there would be interesting to see. Do those concerns change with partitioning?


### Attendees:
1. **Abhinav Sinha (PubMatic)**
1. **Andrew Knox (Facebook)** - Research Scientist - I have a background in fraud prevention and online ad attribution. I’m especially interested in tech/prototypes that create good incentives that benefit individual privacy and the free internet.
1. **Andrew Pascoe (NextRoll)** - Data Science Engineering, Sr. Director. Responsible for ML pipelines and algorithms, and also maintaining user privacy throughout.
1. **Anne van Kesteren (he/him; Mozilla)** — I work on web standards at Mozilla and in this group edit [Storage Partitioning](https://privacycg.github.io/storage-partitioning/).
1. **Aram Zucker-Scharff (The Washington Post, Director of Ad Engineering)**
1. **Arthur Edelstein (Mozilla)** - Product Manager for Firefox Privacy and Security
1. **Basile Leparmentier (Criteo)** Data scientist at Criteo, more involved in the IWABG (with SPARROW), but this group also shapes the future of our discussions on privacy and advertising, so interested to understand point of view and current status.
1. **Ben Savage (Facebook)** - I’m an engineer at Facebook. I work on the intersection of ads and privacy. I care deeply about setting up our ecosystem for long-term health, and helping entrepreneurs grow their businesses. 
1. **Brad Lassey (Google)** Browser developer working on anti-tracking for Chrome
1. **Brandon Maslen (Microsoft)** - Engineer working on Tracking Prevention, SAA, and other networking, storage, and privacy features in Edge. 10. **Brian Campbell (Ping Identity)** - works for a different Ping than PING. Observing today and tomorrow looking to better understand the isLoggedIn stuff and potential impacts 
1. **Charlie Harrison (Google)** — Engineer at Google, working on new privacy preserving APIs, especially focused on measurement. Looking forward to hearing developments of new proposals and to contribute where I can
1. **Chris Pedigo (DCN)** - we’re a trade association representing publishers. Looking to help develop solutions that map to consumer expectations
1. **Chris Wilson (Google)** Web Standards lead, process manager, AC rep. 
1. **Christine Runnegar (PING co-chair)** - Senior Director, Internet Trust at the Internet Society
1. **Christy Harris (Future of Privacy Forum)**
1. **Devin Rousso (Apple)** - WebKit software engineer, was (until recently) the lead engineer on Web Inspector, interested in new web proposals. Also a member of CSS WG, Browser Testing & Tools WG, and TC39.
1. **Erik Anderson (Microsoft)** - Engineering manager on Microsoft Edge working on platform privacy, networking, and storage. Also W3C Privacy CG co-chair. Excited to move work items forward towards potential standardization.
1. **Harshad Mane (PubMatic)** - Senior Principal Software Engineer
1. **Jack Frankland (unaffiliated)** - Head of front-end development for an ad tech company, but with a personal interest in user privacy
1. **James Hartig (Admiral)**. Co-Founder at Admiral, which helps publishers build relationships with their visitors. Previously worked at Grooveshark, a music streaming startup.
1. **James Rosewell (51Degrees)** - Engineer, Entrepreneur - understand the problems seeking solutions and policy / external input being sought.
1. **John Wilander (Apple WebKit)**. PhD in CS focused on SW security and did many years in appsec before moving to privacy. Lead engineer on Safari’s Intelligent Tracking Prevention. Love the web and have always worked on web things. Hope to move Storage Access API, IsLoggedIn, and bounce tracking prevention forward.
1. **Josh Karlin (Google)** - Engineer Partitioning Chrome’s Storage & Creating Private Ads APIs
1. **Kate Cheney (Apple)** WebKit Privacy Engineer, interested in staying aware of new web privacy proposals.
1. **Kaustubha Govind (Google)** - Browser developer for Chrome Privacy Sandbox. Co-editor of First-Party Sets.
1. **Kristen Chapman (Salesforce)** - Principal Architect, Salesforce Marketing Cloud.  I focus on data privacy for SFMC engineering in particular, and for Salesforce as a whole.
1. **Mark Xue (Apple User Privacy)** - I work on improving user privacy across a number of our products, including Safari, WebKit, and privacy-preserving mechanisms for ad attribution.
1. **Marçal Serrate (Hybrid Theory)**: CTO @ Hybrid Theory, building data-driven advertising products for brands and agencies.
1. **Marshall Vale (Google):** Product Manager for Chrome’s Privacy Sandbox
1. **Melanie Richards (Microsoft)**—Program Manager on Microsoft Edge, with a prior background in web design and front-end development. Working on privacy among a couple other things. Interested in tracking privacy features across the board with interest specifically in identity-related bits.
1. **Michael Kleber (Google Chrome)** — Engineer working on "Chrome Privacy Sandbox", new browser APIs to offer private-by-design ways to accomplish goals that rely on tracking capabilities (like 3p cookies) today.
1. **Mike Lerra (Google)** - PM working on Chrome privacy; focusing on First-Party Sets and Trust Tokens.
1. **Paul Bannister (CafeMedia)** - Head of Strategy at CafeMedia. We support small publishers/bloggers and we’re particularly interested in creating privacy-oriented advertising solutions that support the free, ad-supported open web and small publishers in particular.
1. **Paul Marcilhacy (Criteo)**: Product Manager at Criteo. Involved in the IWABG and working on the SPARROW proposal. Interested in the ad tech ecosystem health and the future of the “free-internet” in general.
1. **Peter Saint-Andre (Mozilla)**, unfortunately here only for the first hour - Sr. Dir. for Strategic Partnerships on the Firefox team 
1. **Raj Belur (Amazon)** - Here to learn about what Industry is working on
1. **Rowan Merewood (Google)** - Developer Relations lead, looking to make proposals and contributing to them accessible for a wide range of web developers.
1. **Russ Hamilton (Google)** - Chrome engineer working on anti-tracking feature development.
1. **Russell Stringham (Adobe)**: I am an architect on Adobe’s Analytics product responsible for data privacy, including support for GDPR, CCPA and similar laws, as well as understanding and responding to privacy restrictions being implemented by the browsers. I am also the architect over consent and preference management for all Adobe Experience products
1. **Sam Weiler (W3C/MIT)** - lead security and privacy work at W3C
1. **Scott Low (Microsoft)** - PM working on privacy and trust on the Microsoft Edge HTML Platform team. Focused on tracking prevention, Storage Access API. Excited to discuss issues and move the web forward!
1. **Sean Bedford (Facebook)**: Engineer working primarily with businesses who use Facebook to advertise. Interested in learning more about the proposals and understanding how they may impact how businesses use the internet to monetise.
1. **Shigeki Ohtsu (Yahoo JAPAN)**- Engineer working on CDN and privacy/security
1. **Shivani Sharma (Google)** - Engineer working on Chrome’s HTTP cache partitioning and on the new fenced frames for cases like privacy-preserving interest based advertising 
1. **Steven Englehardt (Mozilla)**
1. **Steven Valdez (Google)** - Chrome Engineer working on new browser APIs in the Privacy Sandbox, particularly Trust Token/PrivacyPass.
1. **Tanvi Vyas** (Mozilla, co-chair) - Principal Engineer, Firefox.  Anti-tracking and privacy features, anti-tracking policy, privacy of Firefox features, formerly web app security. 
1. **Theodore Olsauskas-Warren (Google)** - Chrome engineer working on privacy, specifically performing internal privacy reviews for feature launches with an OWP focus. Looking to get a feel for broader privacy views on features.
1. **Theresa O’Connor (co-chair, Apple)** Standards Lead for the WebKit team & W3C TAG participant. Editing the Storage Access API, hoping to make progress on its open issues!
1. **Valentino Volonghi (NextRoll)**: CTO at NextRoll working on marketing technology and looking forward to understanding more and contributing to the new privacy specs to create a privacy friendly advertising ecosystem, in particular for small advertisers.
1. **Wendy Seltzer (W3C)** - Strategy Lead and Counsel (IAAL), trying to help the web platform incubate new features in a secure and privacy-preserving way.
1. **Jeffrey Yasskin (Google Chrome)** - Privacy Threat Model and Web Packaging
1. **John Sabella (PubMatic)** CTO at PubMatic
1. Hannes
1. Jeddy
1. Jlukas
1. Michael Smith
1. Sam Macbeth
1. Soujanya
