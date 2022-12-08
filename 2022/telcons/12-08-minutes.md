# 8 December Privacy CG Meeting
* Chair: Erik Anderson
* Scribe: Kaustubha Govind (first 30 minutes), Brian Lefler (second half)

# Agenda
* [requestStorageAccessFor: Page-level cross-site cookie grant API #35](https://github.com/privacycg/proposals/issues/35)
* [SAA Developer Use Cases: Google Workspace #25](https://github.com/privacycg/meetings/issues/25)
* Any other business

## Notes
* [requestStorageAccessFor: Page-level cross-site cookie grant API #35](https://github.com/privacycg/proposals/issues/35)
    * Johann/Matt to provide context
    * [Matt presenting slides]
    * This is a discussion about requestStorageAccessForOrigin, hoping for incubation.
    * SAA was introduced ~5 years ago by Safari. Allows for an embed b.example to request storage access on top-level site a.example. When granted, it has access to cross-site cookies. As of today, the access applies across iframes and subresources, although that is likely to be changed to be frame-only (another issue on SAA repo).
    * This proposal is to allow a.example to make that request at the top-level. Allows different browsers to make different decisions on granting. Embedded opt-in is also key. Subresources would also be granted access. Allows for easier integration for devs, especially where user interaction on iframes is not possible. Regarding utility, both Safari and Firefox have internal APIs that provide similar capability. Want to cover potential for abuse. 
        * User interaction: Still required on top-level
        * Embeddee opt-in required via CORS
        * Privacy concern: Not allowed wide access, need to limit via other mechanisms.
        * Prompt spam: Prevent user annoyance since top-level sites get much more interaction. Permissions infrastructure, wording of prompt (who’s requesting what). Again, numeric limits/FPS could come into play.
        * Goals for incubation: SAA has a lot of impl-defined behavior - that is also likely to be true for this API as well. However, would like to minimize that, and be as interoperable as possible. Separating from SAA to allow that API to graduate independently.
    * JohnW: Thank you for bringing up one of the risks that I am raising. Original SAA did look into this use case, and the risk of sites being more or less gated on cross-site tracking meant that we needed engagement with 3p content as a requirement (authenticated embeds being the key use case). User understands why they should opt-in to this. Issue is that many sites have tracker scripts embedded in 1p context. Now those scripts can start requesting storage access for their sites. Probably won’t be an issue for large sites since they can push back. However, it is a possibility for smaller sites, which is a regression for UX and privacy.
        * Matt: Want to clarify that the goal for incubation isn’t to agree on abuse mitigation, but to start that discussion. FPS is one option, but permissions policy could be another. Would like to discuss those options in this CG.
        * JohnW: Have heard discussion of FPS, that’s an interesting observation. Would be interesting to know if it would be possible for browsers to block access to this API for known trackers. But not sure if there is a standard that relies on blocklists.
        * Matt: Some of the implementation-defined behavior (in SAA) veers in this direction.
    * Johann: Want to echo Matt’s point. Interested in moving this API forward because in a way it is an alternative to SameParty cookies that was suggested in this CG. Moving to a SAA-like model. Would love to figure out a path forward in this CG. Maybe simple heuristics (e.g. only do this on a max of 2-3 sites might help deter massive tracking usage, blocklist based on abuse detection). I realize we don’t have a perfect solution yet, but may not be such a big problem in practice.
    * BenV: Have a question - seemed to have missed FPS discussion in my brief absence. Anne had an interesting point that he raised about the ability to delegate authority to another party is where FPS comes in, and seems pernicious. Maybe hard to disentangle. Have a limit of 5 top-level domains might be reasonable, but then it does allow some leaks. Some weird hiccups show up there which Anne alluded to.
        * Matt: In terms of embedded opt-in, requiring the embedee to invoke a call to SAA is one option (but not require user interaction). CORS requirement.
        * Matt: This would not address privacy concerns, so that’s where having anti-abuse heuristics / numeric limits / etc. might be options. We think these could be answered during incubation. We think there are ways forward.
        * BenV: Sounds reasonable. Another question - does Chrome/your team see this as a permanent solution for the web platform? (Recognize that’s a big question)
        * Johann: Can't give a 100% correct answer, and this may end up differently. We’ve building high-level APIs (like FedCM), and we are committed to that idea. At the same time, we are seeing things we can’t deny for short- or mid-term where there are complicated web apps which we can’t build web APIs for, or where flexibility is needed for access to 3p cookies. This is also what we’ve heard from other vendors as being a common scenario. We think there’s a place to allow for such complex cookie access in the mid-term, at the very least until we figure out all the high-level APIs that are needed. Also another big open question is how to allow such access. FPS is one solution, but thinking about cases beyond that.
    * Aram: I agree with John. Very high likelihood that this API could be abused in some way. Many pubs run uncontrolled scripts in 1p context. Beyond that, ads are uncontrolled packages of code that could make a mess of this. I do see utility for more controlled use-cases. Sounds like this proposal doesn’t rely on FPS. I don’t think we should advance a proposal that relies on FPS, since we are not working on FPS in this group. For cases where top-level page that knows that certain embeds need access to 3pc, and the downstream embeds agree to this as well. (Wordpress example is a good one). Top-level should have the capacity. If the browser says we also need affirmation from the user, that’s fine. But shouldn’t allow embedded contexts to request uncontrolled access. If the scale for the embedded context 3rd party is high, like in WordPress, I can see why it might not want to state who it will be asking for permission for and that’s reasonable, but it seems like the use cases here would all have at least the top level 1st party context knowing who will be asking for permission. If the goal is unlimited access to 3pc, then that doesn’t align with our goals.
        * Matt: Want to clarify, re-enabling 3pc is not the goal. Preventing widespread tracking is important, and that’s recognized.
    * JohnW: Ben raised a valid point re: incubation and longer perspective on the web platform. In the context of SAA, we have mentioned that we would like long-term APIs instead of compatibility fixes. Once such things are part of the web platform. It’s hard to remove them. Needs to be address in the spec. Having said that, these use cases have been identified in the context SAA, so it sounds reasonable to incubate this.
    * Johann: Agree w/ John. When we discussed with devs, we have heard these use cases for 3pc. Good task for this CG to figure out whether/how long we want to preserve support for 3p cookies.
    * BenV: Our use of rSAFor in Firefox are all auth use cases, and are hopefully cases where FedCM/other identity API is a reasonable replacement.
    * JohnW: Same for Safari.
    * Erik: Queue closed. For further discussion on whether this should be a work item, chairs to get together to decide whether there is sufficient multi-implementer interest to do that.

&lt;scribe switch>

* [SAA Developer Use Cases: Google Workspace #25](https://github.com/privacycg/meetings/issues/25)
    * Presentation:
        * Lindsay Hall and Philipp Weis presenting (software engineers from the Google Workspace team, had been working with Johann and had experiences with SAA which Johann thought was useful for the PrivacyCG to hear)
        * Part of a workgroup looking at impact of Privacy Sandbox in Chrome, and also looking at how products work in other browsers and how to improve behavior with 3PCs disabled.
        * Goal: Share out current behavior and feature requests for how to improve that behavior.
        * Two use cases for SAA, one using SAA now and one potential:
            * 1) Authenticated embeds; embedded calendar appointment booking page which allows a user to sign up for an appointment using their Google account, or an embedded Google Doc with restricted access.
                * Works seamlessly on Chrome.
                * On Safari and FF, some embeds fail to load, others use Storage Access API.
            * 2) DOM storage access from an embed
                * Meet.com embed loads from a 3p context (mero.com) which needs DOM storage access for reading from indexed db and access to service workers.
        * Authenticated embed complications:
            * Can’t reliably know if content is missing cookies because it comes from an embed or if it’s because the user has no cookies.  So they have some javascript which does a client-side check for SAA, then redirects to login if SAA is granted, this has a latency penalty.
            * Same origin frames not notified; SAA was granted but they don’t know, user needs to interact with each in order to have them load properly.
            * &lt;notetaker missed 3rd case>
        * Ideal user journey would be a browser-level UI if SAA is needed, and to learn on other origin frames if SAA access was granted.  Feature requests:
            * FR #1: Way for a page to indicate in initial server request that storage access is required.
            * FR #2: Know if a request is coming from a frame without storage access.
            * Other same-site frames should be notified with storage access is granted, so they can request and reload
            * SAA should persist across same-site redirects without additional calls to rSA
        * Is it possible to update the Storage Access API to grant access to DOM Storage for embeds as well?
        * Erik: Clarification Q about using SAA for same-domain usage to get other kinds of storage.
    * JohnW: Happy to hear its being used.  Concern: Knowing that a user has cookies for your site introduces the risk of login fingerprinting.  Could be used to stitch up a fingerprint, has been noted as a risk in the login status API work, and various mitigations discussed there, mentioning it here.  That’s a main reason the SAA Api does not reveal if you can get storage access or not.
        * If user does get SAA but the user is not logged in, we decided if persist the user gesture, so that the frame can continue by having a pop-up for the user to login. Not sure if Firefox does the same thing. [Ben VanderSloot shows a thumbs up on video indicating they do]
        * Same site redirect for the frame should be working, at least in Safari’s implementation, because we do want frames to move onto the actual content once they get access.
        * On HTML storage or DOM Storage, consensus is that we should only cover cookies.  It’s unknown how to deal with concurrent access to unpartitioned and partitioned storage, for IndexedDB and Service Worker specifically.  It’s an unknown of how to deal with that currently.
    * Nick Doty: Thanks for presenting specific needs, very helpful.  Browser UI request, I wanted to understand that more.  It seemed to Nick that what was presented was already good, where Google Docs was explaining what it needed and why effectively to the user, better than the browser.  Example, services can do this sometimes better, showing partial content or logged out content.
        * Phillipp: Agree the browser can’t do degraded experiences.  In our case, we have this implementation on our doc service for embedded docs content, but there is this standard situation here where there’s nothing that can be done and we have to do it across all of our products, repeating this over and over again.  For those standard cases where you can’t show any content without SAA, to us, it made sense to have that be handled by the browser.  The default treatment in the browser might just show the domain name and that this content cannot be accessed without SAA.
        * Lindsay: What it looks like, something on top of the frame itself, so it’s clear that you’re authenticating to see the content on that frame.
    * Johann: Quick note– issues are filed for each of these.  And for login fingerprinting, Johann didn’t see the risk for what they’re proposing.  The team is suggesting a way for sites to avoid calling SAA when a grant would be automatically be there.  Johann believes this would take away some developer pain, and Chris Fredrickson from Chrome has an idea for making that possible via header.  This would make it easier for sites to opt into this per end point, and in doing so reduce the risk of attack from embedding these sites.
    * BenVS: We’ve been looking at header sec-fetch-metadata proposal, a sec header that says this is partitioned or this is not, seems useful and not a particular risk, but open to more careful conversations there.  Johann touched on same origin vs same site navigation from an iframe; having permission on an iframe and navigating to a new origin, even same site, crosses a security boundary and raises interesting security questions.  Should accounts.google.com have a say in whether you can SAA or not?  Allowing a navigation takes that control away from them possibly, need to dig into that in the security considerations bug.
    * JohnW: I understood request as a cross-site iframe wanting to know if it is useful to request or not.
        * Phillipp: No, wanted to know whether we had it.
        * JohnW: Is the existing hasStorageAccess not good enough?
        * Philipp: No, would like that on server to avoid the javascript latency.
        * JohnW: Ah, agree, no risk of login fingerprinting in that case.
* Any other business
    * No call on the 22nd, happy holidays and new years.  Next Meeting January 12th.


## Attendees
1. Providence Baraka 
2. Erik Anderson (Microsoft Edge)
3. Eric Mwobobia (ARTICLE19)
4. Benjamin VanderSloot (Mozilla Firefox)
5. Matt Reichhoff (Google Chrome)
6. Ari Chivukula (Google Chrome)
7. Chris Fredrickson (Google Chrome)
8. Allan Spiegel (Adobe)
9. Don Marti (CafeMedia)
10. John Wilander (Apple WebKit)
11. Kaustubha Govind (Google Chrome)
12. Paul Zuehlcke (Mozilla)
13. Andrew Aikens (TripleLift)
14. Aram Zucker-Scharff (The Washington Post)
15. Johann Hofmann (Google Chrome)
16. Brian Lefler (Google Chrome)
17. Daniel LaLiberte (Google Chrome)
18. Lindsay Hall (Google Workspace)
19. Helen Cho (Google Chrome)
20. Tom Van Goethem (Google Chrome)
21. Philipp Weis (Google Workspace)
22. Cameron Boozarjomehri (Mozilla)
23. Tim Huang (Mozilla)
24. Tim Cappalli (Microsoft Identity)
25. Chris Needham (BBC)
26. Darryl Green Jr (The Washington Post)
27. Sam Weiler (W3C/MIT)
28. Ben Kelly (Google)
29. Brian May
30. Emily Lauber (Microsoft Identity)
31. George Fletcher (Capital One)
32. Nick Doty (Center for Democracy & Technology)
33. Sam Goto
34. Stefan Popoveniuc (Google Chrome)
35. Vinod Panicker
36. Brian May (dstillery)
37. Yi Gu (Google Chrome)
38. Alexandre Nderagakura (IAB Europe)
