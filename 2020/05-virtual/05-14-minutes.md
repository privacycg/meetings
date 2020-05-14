# Privacy Community Group F2F, 14 May

Chairs: Erik, Tanvi, Tess

## [Agenda](https://github.com/privacycg/meetings/blob/master/2020/05-virtual/)

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

## Attendees

* Theresa O'Connor (Apple)
* Kris Chapman (Salesforce)
* Joel Odom (Salesforce)
* Scott Low (Microsoft)
* Chris Needham (BBC)
* Michael Kleber (Google)
* Sam Weiler (w3C/MIT)
* Erik Anderson (Microsoft)
* Mark Xue (Apple)
* Charlie Harrison (Google)
* Kaustubha Govind (Google)
* Shivani Sharma (Google)
* George Fletcher (Verizon Media)
* Shaun Gilmore (Apple)
* Wendell Baker (Verizon Media) 
* Josh Karlin (Google)
* Jack Frankland
* James Hartig (Admiral)
* Anne van Kesteren (he/him; Mozilla)
* Christy Harris (Future of Privacy Forum)
* Jeffrey Yasskin (Google Chrome)
* Tom Lowenthal (Brave)
* Tanvi Vyas (Mozilla, co-chair)
* Allan Spiegel
* Andrea Marchesini (Mozilla)
* Russ Hamilton (Google)
* Arthur Edelstein (Mozilla)
* Brandon Maslen (Microsoft)
* Shivan Sahib (Salesforce)
* Carlos Bracho (Axel Springer)
* Christine Desrosiers
* Paul Jensen (Google)
* Naïma Conton (Sirdata)
* Marshall Vale (Google)
* Melanie Richards (Microsoft)
* Steven Englehardt (Mozilla)
* Devin Rousso (Apple)
* Laszlo Gombos (Samsung)
* Lisa LeVasseur (Me2B Alliance)
* Joey Salazar (ARTICLE 19)
* David Senecal (Akamai)
* Charlie Belmer
* Daniel Appelquist (Samsung)
* Francois Marier
* Frederik Wordenskjold
* Hugo Duthil
* Steven Valdez (Google)
* John Wilander (Apple WebKit)
* Jullian Bellino (Criteo)
* Shuran Huang (Google)
* Tao Liao (Google)
* Thomas
* Thibault Montanier
* Zubair
* Chelsea G
* Dennis
* George London
* Kate Cheney
* Konrad Dzwinel (DuckDuckGo)
* Andrew Knox (Facebook)
* David Benjamin (Google)
* Maciej Stachowiak (Apple)
* Mark Xue
* Martin Meyer (Akamai)
* Mike Smith
* Przemyslaw Iwanczak
* William Bock
* rbaudisc
* niarci
* Brad Lassey (Google)
* Chris Pedigo (DCN)
* Sam Tingleff (IAB Tech Lab)
* Mike O'Neill
* Pranjal Jumde (Brave)
* Wendy Seltzer (W3C)
* Johann Hofmann (Mozilla)
* Bill Densmore (ITEGA.ORG)
* Chris Wilson (Google)
* Ericka Wright (Intuit)

### Regrets
* Wendy Seltzer (for 
part)

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

## [Storage Access API](https://github.com/privacycg/storage-access/)

Chair: Tanvi

Presenter: Tess

Scribe: Jeffrey Yasskin


Tanvi: Taking 20+ minutes at the end to talk about First-Party Sets, so the next break will be later and shorter.

Tess: I'm editing the Storage Access API. No slides. Talking about what the API is, its current status, and some open issues.

It's often the case that there are 3rd-party widgets on sites that have varous functions. e.g. commenting widgets, social-media like or :heart: buttons. In a world where 3rd-party storage is blocked, when they want to click the :heart: button, it doesn't know who the user is, since it doesn't have its 1st-party storage, so it doesn't know if they're signed in. Storage Access API allows frames like that to request access to their 1st-party storage so they can Do The Right Thing.

API consists of 2 new methods on Document: 1 allows frame to detect whether it already has storage access, and the second, `requestStorageAccess()` allows frame to request ... storage access. API is implemented and shipping in Safari and Firefox. Work happening to implement in Blink, driven by Microsoft. So it's important to look at the differences in those implementations and drive them toward interoperability. There are a number of differences that are worth looking at to find compromises or common positions. The work to do that will help ongoing implementation in Blink since the spec will be clearer.

Have 3 open issues to talk about specifically, but let's first get some general questions about the API and its purpose.

Jeffrey: can you say something about the UIs currently used when an iframe calls request storage access

Tess: At least in Safari, user might be prompted with trusted browser UI saying something like "Would you like Y frame to have access to its storage on X top level site." So user's aware that the requesting site might be able to join info about the user. Don't know about details on Firefox

Steve: Dialog is pretty similar in wording, but a difference is that we don't always prompt the user. We allow a 3rd-party origin as long as it gets user interaction. 5 sites or might scale based on total number of sites user's visiting. When same 3rd party requests on more than 5 sites, user will see a prompt.
 
John Wilander: Safari never opens up storage access without a prompt, but if the user opts in, then we don't re-prompt (on the same pair of top-level and 3rd-party origin)

Tess: Variety of differences, and want to reduce those.

e.g. Is this per-frame or per-page? Conclusion is that it should be per-page?

John: Webkit folks said "we like per-frame", but Firefox & MS said it should be per-page, so we're changing to per-page.

### [Active or passive storage access after explicit user opt-in](https://github.com/privacycg/storage-access/issues/2)

John Wilander: Will let Steve describe what Firefox does. Safari/WebKit keep the granted storage access for the life of the requesting frame. If the top frame goes away, of course all the subframes go away, so access also goes away then. And also, if the requesting iframe navigates cross-site, we also remove storage access. An app that isn't a single-page-app will destroy its iframes, so the new frames won't have access after a navigation.

Tess: How does that behave w.r.t. the bfcache? You're on a page, request access, grant, navigate top level to another document, then hit back, which revivifies the old page. What happens?

John: Would have to test. I believe Storage Access gets revoked in that case. So it's active: the navigation is detected and storage is cleared. Not cerain about this. I don't think this is a privacy boundary that has to be enforced; we could re-allow if the user goes back to a palce they'd already granted it.

Steve: If we don't prompt the user, the grants are ephemeral, and they get revoked at the end of the session or 24 hours. If we do prompt, it's more permanent. We provide access to the current frame and all the other frames on the tab and maybe the agent cluster. If there are other same-site images or other same-site frames on the same page, they'll also get access.

John: Do you persist across top-level navigations? We'll open up for all other frames within the same page. We haven't yet agreed about persistent across top-level navigations.

steve: We grant a sort of permission for the pair of top-level frame + 3rd-party, and then if the permission is there, access is granted. 30-day permission when user is prompted.

Tess: Edge?

Brandon Maslen: Following mostly Mozilla's path right now. First N grants get an ephemeral grant. Past that, you get a similar prompt, and that gives a more permanent grant.

Michael Kleber: Worried by discussions of ephemerality: thinking about the grant as something that can be ephemeral at all. Once you grant storage access to a 3rd party's user identity, it's easy to join with the 1st party identity on the embedder. Withdrawing the ability to get to the cookies at particular times doesn't actually change what is knowable about the person if there's cooperation between the 1st and 3rd party. The Safari notion of deleting data once you don't visit a 1st party is an attempt to deal with this, but none of it helps if the 1st and 3rd party collaborate.

James Hartig: Brought up in an issue: if you're a 3rd party, how do you know if you should request storage access on click? If they logged in yesterday, you could just ask again, but if they've never logged in, it'd be jarring to pop up a dialog.

Tess: Currently `hasStorageAccess()` tells you if you already have access, but doesn't tell you if there's meaningful storage at all. In Firefox, I think you auto-grant the first several times you see a third party which might help with this?

Steve: We do have passive access. Is this also tied to a desire for a way to ask "do I have storage at all"?

James: Yeah, related to that.

Steve: It helps that we have the automatic grant, but it doesn't solve the problem.

Tanvi: Why we persist across different tabs: If you go to a website and log in with a login provider, and get access, you might be shopping across multiple tabs. Then go to purchase. On the original tab, hyou had third-party access, but on the purchase tab you don't. User will wonder why they need to log in again on the purchase tab.

John Wilander: On Michael Kleber's concerns: I don't think it's ephemeral in the technical sense. But the storage access is ephemeral. We assume there's not cooperation between 1st and 3rd parties in all places. It happens, but it's not everywhere. If everyone's bad, we should persist this, but we see lots of 1st parties refusing to cooperate wiht 3rd parties, and this helps them.
On James: We preserve the user gesture: if you get denied storage access because there's nothing to request access to, you can do a popup to log the user in. Only when the user explicitly says "don't allow", we consume the gesture. This all assumes the iframe needs login state or cookies to function. We assume it's not speculative. You have to make a decision of whether you can function without cookies.
On Steve: the question of a boolean of "do I have state", we don't think the existence of cookies is a good enough signal. There are lots of cookies, and very few are about login. We want `isLoggedIn()` to distinguish cookies from login state. Then maybe we could let 3rd parties query their `isLoggedIn()` state.

Maciej Stachowiak: Any questions about "Do you have storage" that can be asked w/o a user gesture and without an explicit grant, can be used to build a tracking ID, with N of them.
Google thoughts on 'the widget problem'? The storage access API wants to solve the "embedded widget problem". The linking problem, where once it's linked it can associate with the embedder's identity. Similar to the link decoration problem. Do Google folks have thoughts on alternate ways to solve the widget problem. If the user interacts with the embedded widget, it makes sense to give them identity.

Josh Karlin: We want to avoid showing prompts to users when they'd have trouble making an informed decision. One possible idea: new types of iframes that might have constrained communication ability. e.g. think of an iframe that can't talk to the embedder. Maybe it's like a navigation: the user clicks, the constrained frame gets storage. Could work for a like button. Could work for a video that identifies that the user has paid and so they shouldn't see ads. Wouldn't work in cases where the embedding page needs to talk to the iframe. Still trying to investigate use cases, and we'll fall back to Storage Access if necessary.

Michael: In Tanvi's use case, with a single-sign-on, the right behavior is different enough in different cases that we'd like dedicated APIs. Should have an API where we can ask the user 'do you want to use this identity to sign into this other site' or a payment API where we can ask about sharing purchase history. Even if an outlet exists in the platform for strange things, it'd be good to only have to use it where we don't have a specialized API for that use case.

John: Josh's goal of more purpose-built APIs is interesting. `requestStorageAccess()` could include a marker of the site's goal, for example, or a request that we 

Are you thinking the 1st, 3rd, or both help decide whether the 3rd party can get storage access without involving the user?

Josh: We've been talking about having the 1st party embed it in a way that disclaims communication, which allows the 3rd party to have access. Could also imagine the 3rd party messaging it in response headers.

John: HAve you thought about adding a parameter to `requestStorageAccess()`

Josh: Haven't thought along those lines. Interesting idea.

Maciej: Idea of purpose-built APIs is interesting. Some of the initial cases we thought about were like buttons, commenting sections, videos, and captchas, which have plausible needs for user identity. We weren't thinking of sign-on or payment. Coudl imagine forbidding `requestStorageAccess()` if you've never `postMessage()`d. Don't want to lose those original use cases.
A lot of sign-in works fine because it navigates at the top level. Similarly "tweet this" works fine because it doesn't need identity while embedded.

Michael: All 4 particular use cases are ones we think about, and ones we could build specific APIs for. You mentioned that single-signin doesn't need anything at all, but that's only because Safari has an exception that gives away user identity without any prompt at all.


### [Storage Types Covered](https://github.com/privacycg/storage-access/issues/4)

Tess: The name is very generic and sounds like it's agrant for all storage. The first implementation was Safari, but in Safari it only grants cookie access. In Firefox, it grants access to many more types.

John: For us, the only remaining cross-site tracking vector was cookies. We tried to address that with ITP, and then we needed an affordance for embeds. The rest of our storage was already partitioned or blocked. But the intention was always to cover all kinds of storage.
It's tied to `document` because cookies live there, and the cookie API changes if you're granted storage.
`IndexedDB` is tough because it has connections: if you have an open connection in a partitioned iframe, and then a sibling iframe requests access, it's trickty to figure out what to do. Safari blocked 3rd party IndexedDB, so we might have a path, but other browsers might not like that.

Steve Englehardt: We haven't decided whether we're comfortable blocking `IndexedDB` in 3p contexts.
Implementation: We started with no partitioned state, so getting storage access is like going to the previous state where you have access to everything. In the Storage Partitioning discussion, we are talking about how to scope it. If we only scope it to cookies, we encourage people to use cookies instead of better storage mechanisms, which we don't want.

Tess: We might be able ot find some set of storage mechanisms that we can agree should be covered. John's willing to do more storage, and Steve hasn't yet made a decision about the quirky IndexedDB case.

John: For other implementers, 3rd party Service Workers are also tricky.

Anne: Non-Safari browsers also have SharedWorkers and BroadcastChannel.

Tess: Brandon, do you have thoughts about Blink?

Brandon: We're following the Mozilla model? What would be blocked by a "block 3rd party cookies" or tracking-prevention tool, would be unblocked. But if we give different answers between, say, cookies vs localstorage, you can cause compat problems.

Tess: and `hasStorageAccess()` doesn't let you distinguish types of storage.

Josh: +1 that it's confusing to think about what did I get from the request. By default we think it's helpful to have partitioned storage, for example for Docs offline. And for that case granted storage access seems like it should get access to more types of storage too.
Have y'all compiled use cases? I know docs and videos do get embedded, and those tools do use lots of storage APIs.

Tess: Seems like we can converge here; please keep discussing on github.

### [Consumption of User Gesture](https://github.com/privacycg/storage-access/issues/25)

Tess: Cut based on time, can discuss over github.

## [First Party Sets](https://github.com/krgovind/first-party-sets/)

Chair: Tanvi

Scribe: Jeffrey

Presenter: Kaustubha

N.B. First Party Sets is not currently being standardized in this or any other standards body, and contributions to the GitHub repo are not governed by any IPR policy.

Kaustubha: We talked earlier about how we really need a site. Lots of use use the pair of a scheme + eTLD+1 as the site. A site can instead be defined as a collection of domains, or a collection of origins. So that's the problem we started with when thinking about first-party sets (FPS). Idea craeted by Mike West, then taken over by David Benjamin and Kaustubha

There's the risk that we'll force sites to move to a single domain. In the real world, sites are acquired and sold. Could lead to the domain name becoming meaningless. Flickr was owned by Yahoo, and now there's a different organization. Then we're training users to look at a subdomain instead of a top-level domain, which is bad for security. Lots of discussion on Slack about the potential for abuse. We've iterated a bunch are are getting toward something we think is acceptable. We've left the browser model open so browsers can use Disconnect or some other model if they want. Site authors should have some agency in defining which other entities they're in an FPS with. Sites might assert a set, and browsers would approve it.

Questions?

Tess: Currently in a personal github repo w/o IPR policy. What's the route toward standardization? Venue?

Kaustubha: I don't have lots of standards experience, so defer to someone else with opinions.

Chris: We've been trying to establish a better policy of moving things into CGs where we have a better IPR policy. I expect the easiest place is the WICG, but it's definitely not the only place. We can always discuss it here, and if multiple parties are interested in pulling it in, we can discuss that.

Maciej: Welcome the idea in the Privacy CG.
2 problems: fake claims, where unrelated entities claim to be related. "500 domains" problem, where same entity owns lots of domains that the user doesn't think of as connected.
"change of control" problem. If domain moves from one entity to another, that could link identities. 
Explainer punts on all of this. Says it's up to individual UAs to solve it. It's a good starting point for an explainer, but to get to incubation, we need a security and privacy section that addresses the problems and defines a solution or sketches a set of possible solutions.

Kaustubha: When domains leave and change their FPS, we intend to wipe the storage of the moved domain. Search for Clear Site Data.  (This section of the explainer: https://github.com/krgovind/first-party-sets/#discovering-first-party-sets)
We're looking toward lessons from TLS certificates with the CAB forum. Technically the browsers run their own root stores. So each browser does hav eits own policy, but folks agree about common policy.
As engineers we've come up with a technical framework, and working with folks who know the policy better.

David Benjamin: Any solution to this problem is a policy question. We can't unilaterally define things, which is why we're trying to specify UA flexibility. entities.json isn't in any spec, but browsers are shipping it anyway. Take what we want from entities.json and any other strategy, and then converge. There will be an initial period of figuring out how it works, and then standardize on the things that work.

Maciej: Policy and negotiated policy should be a way to agree on which sites are related to the same entity. Don't think it's an answer of the same entity owning 500 domains.

David: Think we do have the idea of a numeric limit. Also, if you're only allowed to form a set that's allowed by UA policy, UAs will wind up limiting the number too.

Maciej: Would be bad if the limits are different in every browser.

David: Site's interested in whether they can join X and Y, rather than "can I join 1000 sites". Agree that we want to eventually specify any limits.

Tanvi: Policy aspect is the hard part, rather than the engineering work. For Firefox, we use the Disconnect list, and they have a policy. It would be difficult for us as a user agent to take ownership of the policy. Maybe have a separate entity similar to the CAB forum, that owns the policy.

Kaustubha: Agree with that idea. Want to come to some consensus. 

Tanvi: Is there an independent group that could help with this?

Kaustubha: Haven't looked for one yet.

John: We proposed something like this in 2017, called "associated domains", and brought up the fact that organizations register new domains for branding reasons. Can we let browsers allow branding without new domain names, so we're not mixing branding with the security purposes of origins?
We seem to be assuming what being in the same FPS would give you, but we haven't actually agreed on that. For example, different rules for `requestStorageAccess()`, or domains could have purposes which would affect `isLoggedIn()`, or different rules for expiring data, or rules for detecting bounce tracking. e.g. 1st-party bounces wihtin a set might not count. It's a different thing to find a trustworthy source of FPSs from figuring out what we'd use it for.

Kaustubha: We propose it as a privacy boundary, and deifnitly not a security boundary. Origins should be the security boundary. Seen the criticism that this expands today's restrictions, but we're proposing new restrictions, and FPS defines the boundary across which those restrictions happen.
The first example is that cookies might be made available 

Tanvi: Going to :30, and quick break after.

David: Re John, I think we don't want different sets for different restrictions. If you have different rules for different restrictions, the boundary constraining identity sharing is the transitive closure of all those constraints, and the limits wind up much more permissive unless they all match. E.g. the privacy budget also needs to add up across the entire FPS.

Wendell Baker: Verizon noticed this in the W3C workshop at the Ads Business Group. Discussed timing. There's a somewhat firm commitment to deprecate 3rd-party cookies. For a place like Verizon that has lots of sites because brand managers love their brands, if FPS were there, we wouldn't have to worry since we'd identify the stable of brands. It would be good if this were in place ahead of the 3p cookie deprecation. For those of us with sprawling legacy businesses, it takes time to migrate things, so if it's not going to be done, we need to start that migration soon.

Sam: Linking Verizon's brands violates customer expectations. Users don't know that Huffington Post is connected to other Verizon things.  And then there's the transfer problem...

https://tools.ietf.org/html/draft-brotman-rdbd-03
https://tools.ietf.org/html/draft-sullivan-dbound-problem-statement-02#section-4

DBOUND failed arguably because it tried to be everything to everyone, and there was another proposal in 2019 that looked similar, but with unclear use cases.  There are lots of interesting use cases in other parts of the stack for this kind of solution. Be aware that other people will try to re-use this for other uses.


Kaustubha: Trying to scope this to browser-level sites, to avoid that problem.

Kris: Companies like Salesforce and Disney own lots of properties, and users don't understand those relationships, so it's not obvious that a parent company should be able to group all their ownership. But there are some groupings that consumers do understand. E.g. users might udnerstand that a bunch of sports sites are associated with ESPN. Maybe make users interact with the parent site before sub-brands can associate with it. But doing nothing would force companies to move domains, which would also obscure things.

Tom: The concept of a brand and a domain, and that users expect ownership to be reflected, is very scary. Expecting people to know who owns what domain, is terrifying. If you're interacting with one company and one branded domain, the amount of friction needed to migrate domains is the right amount. Want only 3-5 domains to be linked in a FPS. e.g. brave.com and basicattentiontoken.com. Don't think we're going to find an easy time of consensus, since different people are showing strong divergent opinions.
Don't think the CAB forum is a good model for resolving this. Don't want to delegate to an external forum of that type. Better off contemplating making the size of the sets an implementer choice.
Incentives are so varied on this number that we probably won't get to consensus on this. Can probably agree on the protocol itself, but that might not be useful if there's disagreement on a limit from 3 to 1000.

Maciej: Want to file issues, but don't want to file them on a personal repository.

Kaustubha: In terms of users understanding: Thought about putting entity attribution in the URL bar, since very FPS has an owner.

## Conclusion
(Starting at 40 after)

Chairs

Scribe: wseltzer

Tess: Thanks all for joining us! We know 8hours is a lot.  Thanks speakers, questioners, scribes.
The world has changed, so has the way we do standards work. This is our first F2F, and we're learning as we go. Very interested in your feedback on how it's gone. We expect to have at least one more f2f this year. 
Please fill out the survey. 

All of our technical work is happening in github. Issues, Pull Requests. 

Hoping to see new issues and PRs, new proposals, based on these conversations. 

The group has biweekly teleconfs, also able to set up ad hoc meetings, so let us (chairs) know if you want one. 

Questions/comments?

Maciej: Think the format was very effective overall. More people able to participate than might have been able to travel. WHat did I miss from F2F? I didn't miss whiteboard scribbling, since prep'd slides were good. I missed hallway conversations, where people could make progress in groups on the side. Try to think about how we can form side-groups during break.

Tanvi: Thanks! Please fill the survey.

Sam: Does anyone want to have a side conversation now? 

Tess: or thoughts how to incorporate side conversations in virtual meetings?

John Wilander: appreciate the perspectives we've heard from AdTech. How's the level and stakeholder mix? Is this group working for you from Ad Tech? 

Kris: Don't think this was too technical. It has a different PoV from the Ads BG. Different experience of browsers. Would like to see more communication between the groups. 

Wendell: I'm in IAB, Ads BG, PING, here. They all have different flavors. What's interesting from a commercial mindset is predictability. We can work with almost anything. We understand there are different orgs with different viewpoints on how the web will turn out. Trying to figure out how to live within the new world as users. Heartened by the level of understanding of adtech practices. We're trying to learn how to produce another business model that can produce value for everyone on the open web . Like the interaction and the tone. 

Christy: FPF Ad Tech WG, echo what Kris and Wendell said. I often get question from ad tech companies: how can they be involved, prepare for changes. Hard for me to know where to direct people, even at W3C. 

Wendy: From w3c staff perspective: thanks for the feedback on information-finding, and we'll see what we can do it improve navigation for outside folks who want to come in.  (Web site redesign might help.)

Tess: Thanks, we'll see you on github!

Survey - <see #privacycg slack for link, mail coming soon to group members list>
Github https://github.com/privacycg/
Ad-hoc Meeting Request - https://github.com/privacycg/meetings/issues/new
