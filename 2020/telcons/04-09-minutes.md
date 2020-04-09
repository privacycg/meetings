
# Privacy CG telcon - 9 April 2020
* Chair: Tanvi
* Scribe: Peter Saint-Andre

## Attendees
* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* Peter Saint-Andre (Mozilla)
* Pete Snyder (Brave)
* Christine Runnegar (PING co-chair)
* Kris Chapman (Salesforce)
* Martin Meyer (Akamai)
* David Senecal (Akamai)
* John Wilander (Apple)
* Jack Frankland
* Sam Weiler (W3C/MIT)
* Wendell Baker (Verizon Media)
* Pranjal Jund
* Scott Low (Microsoft)
* Sam Tingleff (IAB Tech Lab)
* Ben Savage (Facebook)
* Andrew Knox (Facebook)
* Max Gendler (NY Times)
* Mark Xue (Apple)
* James Hartig (Admiral)
* Wendy Seltzer (W3C)
* Tiago Ferreira (Metomic)
* Melanie Richards (Microsoft)
* Brandon Maslen (Microsoft)
* Aram Zucker-Scharff (Washington Post) 
* Steven Englehardt (Mozilla)
* Johann Hofmann (Mozilla)
* Allan Spiegel (Adobe Digital Experience Platform)
* Baudisch Rene
* Ketan Deshpande (Amazon)
* Carlos Bracho
* Chris Pedigo (Digital Content Next)
* Michael Smith
* Shaun Gilmore (Apple)
* Sebastian Zimmeck (Wesleyan University)
* Przemyslaw Iwanczak
* Thomas Bergemann
39 participants on call

## Agenda:
* New Introductions
* [Charter updates](https://github.com/privacycg/admin/issues/12)
* [Face-to-face](https://github.com/privacycg/meetings/issues/3) update
* [Agenda+ items](https://github.com/search?q=org%3Aprivacycg+label%3Aagenda%2B&unscoped_q=label%3Aagenda%2B)
  * [Proposal: Implementing Privacy Rights](https://github.com/privacycg/proposals/issues/10)
* Upcoming ad-hc meeting on [Bounce Tracking](https://github.com/privacycg/meetings/issues/5)
* Any other business

## New Introductions

* Chris Pedigo (Digital Content Next)
* Allan Spiegel (Adobe Experience Platform)
* Tiago Ferreira (Metomic)
* Andrew Knox (Facebook)
* Ben Savage (Facebook)
* Max Gendler (NY Times)
* Sebastian Zimmeck (Wesleyan University)
* Shaun Gilmore (Apple)

## [Charter updates](https://github.com/privacycg/admin/issues/12)

* Tess provides an update
* Pushed an update a few days ago
* Charter requires that we announce changes
* This change consists of bug fixes and clarifications
* Summary:
  - We now describe what kind of artifact we expect a proposal to produce
  - Clarified rules about when editors make feature additions and removals to work items
  - Clarified need to sign CLA in order to contribute changes

## [Face-to-face](https://github.com/privacycg/meetings/issues/3) update
* May 13-14 15:00-19:00 UTC
* Wednesday-Thursday
* This is 8am-11am PDT
* Evening in Europe
* Only one respondent from APAC (who works on Pacific Time!)

## [Agenda+ items](https://github.com/search?q=org%3Aprivacycg+label%3Aagenda%2B&unscoped_q=label%3Aagenda%2B)
### [Proposal: Implementing Privacy Rights](https://github.com/privacycg/proposals/issues/10)

* Proposal from Sebastian Zimmeck
* Follows up on CCPA rights and email thread
* Background
  - CCPA is privacy law in California
  - Regulations say that browser extensions and similar technologies that use automatic signals to tell services not to sell data must be honored
  - Some people asked for changes but the California Attorney General has not made changes
  - This might have implications for DNT
* What form could the signal take?
* Could be a simple binary signal
* But it could be a more robust/granular communication mechanism
* Basis for this could include headers, cookies, etc.
* This topic is beyond just web technology, includes policy aspects
* Sebastian's goal is to help form a standard for meaningful privacy selections for users but also allow monetization of data under reasonable terms
* Such a standard would bring together multiple stakeholders
* Output could be a single standard or multiple standards
* Sebastian submitted the proposal to start the discussion
* Sebastian and his students have written code toward this
* Aram Zucker-Scharff
  - Really interested in this issue
  - The signal isn't specified by CCPA laws and regulations
  - Concerned that there could be many signals
  - IAB Consent String process seems like a good starting place
  - IAB has put their spec up on GitHub to gather comments
  - Headers are interesting, but an active request could be a good fallback
  - IAB uses defined interfaces on window objects
  - ??? proposal uses cookies and that's not future-proof
  - IAB proposal requires a per-case active event (not a generic do-not-sell)
  - One challenge is joining user consent with interaction of content
  - We would like consent to travel with the content (but this is hard if that content moves to a third-party site because origins are different and can't necessarily sync in an active way)
  - Need a way to send consent indication to those other sites
  - Audio is hard (podcasts, Spotify, etc.)
  - Possible to present a personalized ad over audio but difficult to sync the consent
  - Hopefully we can address these concerns during standardization
* Ben Savage
  - Text about "global privacy goals" are ambitious and have gotten attention
  - It's difficult to follow all the rules in all jurisdictions
  - This is especially a burden on smaller companies
  - A single control would simplify matters
  - There's a lot of potential to do this well
  - Concerned that platforms vendors could move quickly and complicate standardization
  - We need a way to provide input to their thinking
* Chris Pedigo
  - Agree this is a worthwhile effort
  - We need to avoid a profusion of signals
  - There was a lot of controversy in the DNT effort over policy but not so much over technology
  - Things are different now because CCPA is the policy
  - Just need to figure out what the signal looks like
  - Using Storage Access API would be helpful
* Pete Snyder
  - There have been efforts in PING and other bodies to indicate user desire for stronger privacy in the context of existing standards
* Sebastian Zimmeck
  - One way to go about this is to define something simple (e.g., a do-not-sell indication)
  - Another is to design a larger framework
  - Which direction we take depends on how the conversation goes
  - Perhaps look not at specific devices or applications but a more general platform (e.g., to store advertising IDs) that publishers and others can query
  - Input is welcome on how to approach things
* John Wilander
  - We (Apple) are positive about working in this space
  - Our basic policy on WebKit is privacy by default
  - So we need to look at opt in vs. opt out
  - We need to make sure that things are safe by default
  - For instance, the lack of signal on a shared device shouldn't violate user expectations
  - The signal should not become a tracking vector (as happened with DNT)
* Wendell Baker
  - Verizon is very interested in this as well
  - W3C ran a workshop in 2018 on permissions and user consent (with Brad Kulick)
  - We submitted a position paper to that workshop
  - Posited that such a mechanism should exist
  - Workshop discussion indicated that compliance and notification mechanisms should move to a panel in the browser
  - Concerns were raised that jurisdictional incursions weren't the same across cultures and might move faster than software
  - Mechanism would be a cookie-jar store beyond cookies
  - Various opinions about default-on vs. default-off
  - For instance, some browsers might default users to one state or the other
  - One concern we had about DNT was that content and media were moving from servers to CDNs (same origin concepts are not as straightforward)
  - IAB project around consent management
  - Various proposals to standardize on a particular vendor in the ad world
  - Neutral platform needs to be operated by someone and that can be a blocker
    - (Scott Low): Queue is closed, but Microsoft authored a proposal for an [auditing body](https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/master/WebPrivacyAuditing/explainer.md) a while back. We're supportive of helping unblock this in the industry and fleshing out use cases!
  - This comes up in Google's privacy sandbox proposal - need for a neutral entity
  - When consent is registered, by whom and for what? Need an identity system of some kind to make those associations stick
* James
  - We have implemented both IAB specs for CCPA and GDPR
  - Whatever spec we develop should address both laws
  - GDPR has more cross-origin requirements
  - We've seen a much higher level of confusion about how to implement GDPR
* Aram Zucker-Scharff
  - Defaulting changes in different regulatory regimes
  - In GDPR, default opt-out
  - In CCPA, user must explicitly opt out
  - How to handle defaulting when we can't determine it? If we (WaPo) can't determine user's location, we should assume that the user is in California and has previously opted out
  - If user has selected opt out in California, need to honor opt out
  - If the user is unknown to us, we have to assume that the user is in our system and has opted out
  - User identity is key here
  - I'd like to recommend against a centralized identity bank
  - Not just the problem of finding someone to maintain it and single point of failure, but even optimally institutions won't necessarily remain trusted
  - User agent needs provide explicit signals
  - Transaprency and consent model needs authenticated participants
  - There is a CCPA negative signal, IAB spec covers this
* Johann Hofmann
  - We (Firefox team) are very interested in this too
  - We have looked into it conceptually
  - Very concerned about the different interests that are in play
  - Getting together on a common signal is the least of our worries
  - Designing the UX and getting real consent are extremely difficult
  - The UI can lead the user in one direction or another
  - Don't want to end up where we did with DNT


### Storage Access API

#### [Consider moving API to Navigator](https://github.com/privacycg/storage-access/issues/22)

* Jack Frankland
  - A few comments indicated appetite for expanding the scope
  - Currently the API is bound to iframe
  - If scope were to expand, might not make sense to bind it to the document
  - Not suggesting that we change behavior
  - More about making clear what the scope should be
  - Even though Navigator is global, service worker can register to specific scope
  - Thus the behavior doesn't change
  - Opening up to future flexibility
* Tess
  - Objects: nav, window, doc
  - These are common places on which to hang APIs
  - Sometimes we don't choose the right place to hang things
  - There are two shipping implementations that use Document
  - In the past when we've made similar moves, we've usually regretted it
  - This caused compatibility problems with DNT
  - Future flexibility might not be a good reason to change things
* Jack
  - Don't think this needs to change unless it's a hinge for some browsers to implement

#### [Opt-in mechanism for shared storage access](a)

* John Wilander
  - Hoping for input here
  - This is the main thing we need to solve
  - Scoping is at the heart of this (per frame, nested frames, frames on page, resources on page)
  - How do we handle restrictions in browser?
  - Blocked vs partitioned could be tricky for IndexedDB
   - Blocked easier - just unblock.  But if partitioned, more tricky if you have say open connections to IndexedDB
  - Blocking or partitioning across the board or using a list-based mechanism
  - Harsh behavior for origins on a naughty list minght be more palatable to a browser implementer
  - Whereas across the board policy could lead in a different direction
  - How to handle sibling frames?
* Steve Englehardt
  - We are still leaning toward all frames on the page
  - Signal to other frames?
* John Wilander
  - Would like to hear from more developers (e.g., people who deploy with nested iframes)
  - Will things break if we switch for all frames at once? What kind of signal do they need?
* James Admiral
  - Won't break for us (we have multiple frames but not nested)
  - We're not using IndexedDB
* John Wilander
  - Send us feedback!

## Any other business
### Upcoming ad-hoc meeting on Bounce Tracking
* https://github.com/privacycg/meetings/issues/5
* Proposed time: Thursday, April 23rd 10am PDT, after our next teleconference.

