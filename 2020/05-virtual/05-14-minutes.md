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

## [Private Click Measurement](https://github.com/privacycg/private-click-measurement/)

Chair: Erik

Presenter: John

Scribe: Scott Low

- John: Quick recap of what it is, and then presenting the three issues we'd like to discuss.
  - There are three steps to PCM. First, an ad click is stored. Ad click contains metadata to define campaign ID and reporting domain
  - Second, conversions are matched in the browser against stored ad clicks
    - Traditionally, this would be achieved using tracking pixels. This uses 3P cookies that immediately reveal who the user is so that the previous ad click could be linked back to the user who converted. In browsers that block cookies, this linking is no longer possiblee
    - In PCM, the conversion domain can perform an HTTP redirect that will ask the browser whether it has any stored ad clicks that should be attributed
   - Third, if there is a match, there will a delayed, ephemeral HTTP request that tells the ad publisher about the conversion

### [Select a fraud prevention mechanism](https://github.com/privacycg/private-click-measurement/issues/27)

- John: Motivation - Once that ephemeral report works the way that it's supposed to work, there's no way for the search engine/publisher to know whether the conversion report is a real conversion. We need to come up with a way to determine that the report came from a legitimate ad click.
- We're hoping that blinded signatures can help prevent fraud prevention
  - We add a nonce to the ad link that is stored in the browser
  - The browser then creates a secret that is matched to the ad click and blinded
  - It then makes an out of band request back to the advertiser with this nonce and this blinded secret
  - If the advertiser agrees that this is a legit click, the advertiser provides the browser with a blind signature and the signing certificate that was used to sign the signature
  - Browser can then store this information
  - Time passes, tracking pixel redirects to advertiser which goes into the PCM database
  - 24-48 hours later (if there's a match), the PCM in the browser, before sending out the report, asks for the signing certificate again.
    - It's worth noting here that search.example could send a personalized signing certificate that could be used as a fingerprint to track the user
  - PCM checks the certificate and sends the secret and the signature which allows search.example to verify that the conversion originated from a real ad click
  
- Charlie: I have a lot of comments. First, I have some clarifying questions
  - The request that gets sent back from the PCM to the browser to search.example (the first one), are we saying that this is happening out of band without cookies so that the only thing associated with the original context is the click nonce?
    - John: I don't think it needs to be sent without cookies. First, I was wondering if we could do this synchronously. But didn't want to overload the navigation. But in this case, the click just happened. So I'll immediately fire off this request. I'm envisioning that cookies would be sent here, but I just added the nonce so that search.example had something to link up properly.
  - I noticed there's only a signature on the click event. Do you envision the same thing happening on the conversion?
    - John: Yes absolutely. We believe that if these signatures and secrets get us the fraud prevention we want, then we can deploy this in other spots in the flow as well
  - This is similar to Trust Tokens. In our original design for Trust Tokens, we had this flow where we asked for the signing certificate to ensure that you can't use the signing certificate as a tracking vector. What we've initially implemented is an out of band for pre-downloading static certificates and holding onto them for a long time (perhaps as long as key rotation). We download these from a central endpoint that Chrome controls. That ensures that all browsers are seeing the exact same certificate. I understand that this adds another trust component, but it prevents continuous pulling down of the same certificates. 
    - John: This could likely be specified to be flexible. We looked at Trust Tokens, but since that was written as an API, we stripped out the API bits and dsitilled it down to what we actually needed for this
  - Charlie: Perhaps we distill Trust Tokens down to make them a reusable component
  - Tess: It'd be great if we could separate Trust Token's model and API and reuse the Trust Token model here
- Andrew: Facebook published something along the same lines. Charlie hit some of the highlights.
  - John: We had numerous people comment on the fact that big click sources such as Google and Facebook have one type of relationship with advertisers. This will need to work for any type of click source. It could be a blog doing this. Is your assessment the same that conversions are the more worrisome from a fraud standpoint?
  - Andrew: Hijacking fraud is more lucrative. If the conversions don't need to be signed, you can cook the books in your favor. The conversions won't add up, but they won't know who did it unless they get a conversion signature as well.
  - Have you considered partially blind signatures? In a similar manner, let's say that a website sells lots of things with various conversion values. You could buy lots of cheaper conversions, but if the conversion is not tied to the value, then you could lie about those. In our proposal, we alleviate this by having as many keys as there are conversion types. There is some research on partially blind signatures, but it's fairly under-researched. Might be worth looking into
  - John: I'd first document how this scheme could work on both sides. I'd then discuss with you to see if it would suffice to postpone partially blind signatures to v2 to see if we can get something into developers' hands.
  - Andrew: I think it's all incremental. You're never going to prevent all types of click fraud. Even today with tracking, click fraud still prospers in some ways. I do think it's worth implementing without partially blinding, but if this proposal has fraud holes, you may see folks jump on those unblocked types of fraud.
- Anne: A lot of this hinges on some underlying protocol that's yet to be standardized. It seems that Privacy Pass and blinded signatures are potentially solutions here. When I looked into those, neither was really started up in terms of IETF activity. One of the questions we got from our crypto people was "what exactly are the goals that this protocol needs to accomplish and what properties does it need to have?" Is this in scope to discuss?
  - John: One of the reasons we moved to blinded signatures was to detatch from the specifications you mentioned. Blinded signatures have been established since the 80s. We could at least feel safe about the crypto primitives. Then it would be up to this spec to define how they're used. I'm very much open to feedback on crypto and stating the goals properly so that folks can reason over whether the cryptographic schemes lend themselves well
- Kris: Sites like Amazon don't allow for tracking pixels to send signals out. Is there anything in here that will allow advertisers to get this data if the landing page doesn't support tracking pixels?
  - John: It does not have to be a tracking pixel per se. The HTTP request has to go out from shop.example. I don't think Amazon is worried about images in particular. The next level of that is what we're talking about next, which is creating a JS API for recording conversions rather than just requiring a pixel. Perhaps Amazon would be more comfortable with that, but we'd like this to be V2. In the case of Amazon or big e-commerce, however, the advertiser is not in charge of the site. I have not seen a great solution for this use case yet since it introduces a new party that detatches the advertiser from the site where the conversion happens.
- Charlie: I had a related question which was that I think it's important to look through this flow to determine who the signing/verifying entities are. Especially if we're in a scenario where both the click and the conversion are both getting signed. To what extent are you willing to support delegated signing responsibilities to another entity shared by publisher/advertiser so that we can simplify this flow? This is also related to third-party reporting as well
  - John: Three flavors of this brought up. Can the ad be served by a 3P? Can the report go back to a third party? And could signing be handled by a third-party? Our general stance is that we want this to be 1P. Users don't understand 3Ps. They understand 1Ps. One thing we are okay with is ad serving. Conversion will still be tracked as a conversion against the 1P. This gets more complicated if we introduce signing
    - Charlie: If all this flow is happening with 1Ps (for both ad click and converion) are you envisoning the publisher being one signer and the advertiser being the other signer? Or are you imagining shared keys?
      - John: My thinking is that the two first parties would be responsible for signing. They are free to have business deals so that things running in their cloud could be delivered by 3P services. I think it's important that 1Ps become empowered. Assume that publishers are leading a lot of ad clicks that lead to conversions. I think they should be able to look at that data and get those reports and choose who to partner with for analytics. This is good for the user as it helps them understand where these reports are going and good for 1Ps to get ahead of this. 
      - Charlie: If we want publishers to be signing and advertisers to be signing, are you imaginging that we need to do some server-to-server call to ensure that we can verify all pieces of it?
        - John: Maybe they need to do that. Maybe the report goes to two places. Shop.example could get the signing cert from search.example, but that's again server to server communication.
- Maciej: Very briefly, I think the intention here is to not rely on an unspecified protocol. We want to specify how blinded signatures are used or reference some shared document. We do not want to reference unspecified documents
- Tom: I'm confused about why we should have events firing from landing pages that don't cooperate
  - Kris: What I'm saying is how they're tracking click tracking today happens before they hit the landing page. The landing page provider doesn't have to provide conversion data, but the advertisers do understand that the click happened. If we link click info to the fact that a conversion happened, then advertisers lose that signal
  - John: Is this redirect tracking?
  - Kris: Exactly.
- Michael: On the subject of 1P vs. 3P, I'm surprised by the diretion that the discussion went. If you feel that 1Ps are going to have 3Ps that control processing, I admire the desire to have delegation be explicit to give 1Ps control. But the notion that somebody acting as a signer should have a million subdomains for a million blogs that they're handling signing for, it seems like there's a better way to enable the 1P to delegate signing to a trusted 3P
  - John: I was not thinking about CNAME cloaking. I was thinking okay so this publisher is running NGINX. They set up a module. 3Ps can be software providers rather than CNAMEs.
    - Michael: I know you don't like 3Ps running code on 1Ps. Having 3Ps run server-side code seems like the wrong way to move.

### [Consider defining a modern JS API in addition to the tracking pixel mechanism](https://github.com/privacycg/private-click-measurement/issues/31)

- John: Modern way of replacing tracking pixel. Strawman so far is an API off the navigator object that takes in a conversion value. Interesting thing is that rather than having 5 tracking pixels firing to the five places where ad campaigns are run, you could batch calls in this API. This could be a real enhancements. No network request is needed here, which is why we'd like to get to this point. I propose that we try to do this in a level 2 of PCM. I'm very interested in this, but would like to get something shipping and to figure out layering with Google's proposals.
- Wendell Baker (Verizon): We'd love to test this. We're looking for a stable enough implementation of something to be able to do some trials. Pilot has some trade characteristics where there is discretionary but live money at stake. We've had a team working on this and some of the other proposals to see if we can make this work in an economic sense so that we can sell this to folks and convince them to try these new solutions
- Charlie: I think in general, I'm okay with this approach of having a JS API to register conversions. One thing I want to mention is that from our perspective, it seems good to have pixel registration. Reason for that is because in practice, it will still be the case that most of these pixels or JS APIs will be set up via some JS that runs in the 1P context but is from a 3P. All else being equal, it's better to avoid that when possible. If the JS API becomes to the only way to do PCM, we could end up in a world where we have more 3P JS running in the main document. If we're doing this to make sure that we establish 1:1 correspondances between the JS and the pixel, then that's good. 
  - John: I share this concern. I don't see an aggressive timeline for deprecating pixels. At some point we'd like to get away from it. 

### [Layering and the Click Through Conversion Measurement Event-Level API](https://github.com/privacycg/private-click-measurement/issues?q=is%3Aissue+is%3Aopen+label%3Alayering)

- John: This has it's own label. This is layering Click Through Conversion Measurement Event-Level API on top of PCM. Five issues:
  - We've covered the first two
  - Third one, we've agreed to move to a JSON structure
  - Issue 29 - last vs. multiple conversions
    - Say you have multiple ad clicks and some time later, you have one conversion. You could have more conversions happening within seconds (such as a funnel). The way that PCM works today is it has a priority bit. And it will schedule the highest priority ad click. Once the report is sent out, the ad click is consumed. This must happen within 24-48 hours. If the conversion happens two days later, then the ad click will be consumed. Google has come up with a way to send out three reports to meet additional use cases. Open question is can the same conversion measurement solution work for both of our proposals so that developers don't need to do UA sniffing?
      - Kleber: We remain interested in 3P endpoints. For last vs. multiple conversions, we're happy to align on something. But the ability to enable 3Ps to come in is an area where we have deep opinions

- John: Bits of entropy (Issue 28). PCM has 6 bits of entropy for impression data campaign ID. What we're saying here is that to layer these approaches, we move to an 8 bit model where we can choose to spend our budget on the impression metadata side rather than the conversion side. This would give static 3 bits of conversion data. Comments? [Out of time. Moving to GitHub]
