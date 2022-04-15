# 14 April 2022 Privacy CG Telecon
* Chair: Erik Anderson 
* Scribes: Brian Lefler, Johann Hofmann

# Agenda
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [Reject with meaningful exceptions vs undefined. #37](https://github.com/privacycg/storage-access/issues/37)
* [Discuss acceptable use cases for site usage of CNAMEs #17](https://github.com/privacycg/meetings/issues/17)
* [Updates to PCM in Safari](https://webkit.org/blog/12566/private-click-measurement-conversion-fraud-prevention-and-replacement-for-tracking-pixels/)
* Any other business
    * Reminder: our next meeting on April 28th will discuss [Cross-site cookies standardization](https://github.com/privacycg/meetings/issues/16)
    * Are there plans for a face-to-face at TPAC?

## Notes

### [Reject with meaningful exceptions vs undefined. #37](https://github.com/privacycg/storage-access/issues/37)
* [Johann Hofmann] Any objections to changing the type of the exception?
* [John Wilander] Seems reasonable, need to capture differences between the user saying no and the conditions for the API are not met. Have already seen code that attempts to suss out the difference between user acceptance and browser, and punish users if they reject the permission prompts.
* [Brian May] Slippery slope when we tell requesters something other than what happened.  Need to consider impact on developers as well.
* [Johann] This change will give developers the same amount of information. Cannot assume either way.
* [Brian] Concerned about precedent when APIs are fuzzy about what’s going on
* [Aram Zucker-Scharff] Generic type error is fine; whether user declined or whether your code is broken is indistinguishable: It’s not available, the reasons why it’s unavailable do not need to be gone into.  Practically, devs don’t go into why an error happened.
* [Johann] Important for browsers go into better logging of information, such as console logs that storage was unavailable.
* [Brian] If developers can’t figure out why they lack storage, and we make it harder, eventually people simply won’t use the API.  Disagree with making it purposefully harder for developers.
* [Kristen] It’s true if it doesn’t work it doesn’t work, but that doesn’t give the developer the option to give user feedback (to the user) of why the user isn’t experiencing storage.
* [Aram] To John’s point, a user who made this choice does not want feedback, they do not want the site to know that they’ve made this choice.  We see sometimes that users get told to turn on cookies to achieve functionality when sometimes cookies aren’t available and sometimes that’s because code was broken, that use case does not justify the request for more data.  Do not want to open up opportunities to press users to make choices other than the ones they want.
* [John] As a community, getting more and more agreement that after a choice is made there should not be punishment or pressure.  Pattern we should work towards on all these prompts: Inform the user what you want and why you need this privilege and once the user has made the choice it’s over, they made the choice. [John proposed a followup discussion on this as well.]
* [Brian] In agreement with John, think through this context being an agreement and treating it not like just any other API but as a place where a user decision is being made.  If developers don’t trust or reliably use the API they’re going to look for work arounds to do the same things and that leads to an arms race.
* [James] Would welcome discussion on choice and consent; particularly if the browser makes the choice for the user then they may not be meaningful. Example, browsers give broad user terms may not be meaningful because they are captured alongside a wider choice such as setting up a new smartphone. Users should get the choice to get personalized ads from publishers. Debate needs to cover these elements.
* [Kristen] Agree with John after a choice is made, should not be trying to sell user out of that choice.  Would like to be able to tell the user that a choice was made, though, and not an error.  Those responses could allow for a better user experience.
* [Aram] We can look at what’s happening in the wild around browsers ending 3P cookies and the reactions on sites.  It does not matter whether the user made a cookie decision in the moment, whether they chose a browser without 3P cookies, or there’s a bug on the site: The result is consistent.  There’s a site called Admiral maybe that if they find cookies don’t exist they throw up a blocker that says this site requires cookies and you can’t proceed.  That’s likely how developers will respond.  More sophisticated info would be used as a fingerprinting vector.  Generic error seems the easiest way forward.  I understand the desire for better logging, but the end point is that it won’t be used in a way that needs detailed error types.
* [Kaustubha] I wonder if the developer ergonomics questions are about heuristics—sometimes it can be hard for developers to debug when heuristics have flagged something.  I wonder if console logging would help debugging or the reporting API.  Could be a better way to let developers realize there’s a problem than an error code.
* [Cameron] We need ongoing consent, cannot just assume from old consent: At what rate should we re-ask for consent to ensure ongoing meaningful consent without decision fatigue?  Ideally we’d like to know their consent level across the internet, but right now we don’t.
* [Erik] Broader permissions question there, recommend commenting on the issue or having a broader conversation against the meetings repo if you want an adhoc meeting on a topic.


### [Discuss acceptable use cases for site usage of CNAMEs #17](https://github.com/privacycg/meetings/issues/17)
* [Sam Goto] Vittorio made a suggestion about using CNAMEs for preserving certain things we expect to break when 3P cookies are blocked.  I tried to describe it, am looking at this room and want to know from a vendor perspective whether that’s a viable option long term or we expect browsers to intervene specifically.  Want to avoid the merits of whether it’s good idea or not and focus on: whether would IDPs who did this run into a wall in the future or find it viable long term.  Looking for not just Webkit whether it’s viable or not, but whether it would run into their CNAME Cloaking protections, as well as same question to other browser vendors.  Can FedIdCG recommend this to IDPs?
* [Vittorio] Practice already being used, customers run into limitations of 3P cookies and for some cases where the Relying Party (RP) and IDP can be in one single domain they do this.  There are extra constraints, need to be able to use the same certificate because none of these will work outside of SSL, and workaround that applies only to specific scenarios.  Many customers have different domains that go to the same central system, obviously this won’t work because it requires a single domain.  Used in practice now, limited scenarios.
* [John Wilander] We do call out “third party CNAME cloaking” and this is laid out in our documentation on webkit and in the [blogpost](https://webkit.org/blog/11338/cname-cloaking-and-bounce-tracking-defense/).  A cookie set in an HTTP response when 3P CNAME Cloaking occurs it gets a 7 day expiration limit.  This is because operators who would have been restricted to 7 day expirations via setting scripts have used CNAMEs to try and evade that.  Ongoing battle distangling logins and tracking, it’s fundamental.  When something usable for login and easily be used for tracking we’re going to run into this problem.  It’s going to be tough to accommodate both.  We should strive for login specific functionality, things that can only be used for login services.  Part of that we’ve been trying to do with the logged in API.  CNAME Cloaking will continue to be used by cross-site trackers and so there’s a limit to what you can do with a subdomain.
* [Sam] What’s the difference between first party and third party CNAME use?
* [John] Lots of research, we found whole sites which are CNAME cloaked, entire site, that’s what we call first party CNAME cloaking.  If different subdomains match, that’s ok.  The true first party has remapped their domain this way.  When we have a subdomain request that results in a different CNAME than the “first party” site that’s third party.
* [Sam] If subdomain CNAME does not match the main domain CNAME.
* [John] Mismatch is ok if the subresource is not CNAME cloaked at all.
* [Sam] I believe based on this definition a login provider would look like a third party CNAME cloaker.  Vittorio, his customers are pointing CNAMEs to Okta and AuthZero’s servers so you would expect at some point they will be intervened upon. \
[Sam Notes: https://webkit.org/blog/11338/cname-cloaking-and-bounce-tracking-defense/ \
Looking at this table, I’m lead to believe that you’d classify this topology as a third-party CNAME. Ack John?]
* [Vittorio] This cannot be abused by a third party, the owner of the system must be in agreement for this to happen.  I understand the protection but the requirement of adding a certificate for all of these, and only a legitimate owner can get a certificate, seems a strong signal of intent.  The RP does not run their own login infrastructure, they are relying on a service, that is a standard practice.  Site operator is entirely onboard.
* [Johann] Personal thoughts unrelated to Chrome
    * First, there are mechanisms to find out the real DNS name, uBlock and Safari do this, when there are mechanisms to prevent tracking they can defeat CNAME cloaking.  Doesn’t seem like CNAME cloaking is a problem because those solutions are already working.
    * Second thought; CNAME cloaking does have positive implications for privacy because it can act as a form of partitioning.  CNAMEs allow a site to partition their state so they do not need 3P state, gives up 3P state and only has the 1P state of the RP.
    * Third thought; there are security implications.  Some sites offer complete CNAMEing of their entire site, and as an ecosystem maybe we shouldn’t encourage it.
* [Lola Odelola] At Samsung Internet; we are discussing implementing CNAME Cloaking as well.  So to back up John’s concerns. In the future sites with issues due to interventions may see the same thing in Samsung Internet as well.
* [Kristen Chapman] Not first preference but we might be forced to go down the CNAME route and run into these issues.  This is about auth and logins, not tracking.  My concern is that the only browser building something specialized here for login is Google and FedCM.  We need more browsers engaged in our login-specific proposals, we cannot break login in order t prevent tracking, the detriment of that is pretty severe and we need to focus on this now.
* [Jeffrey Yasskin] My understanding is that CNAME Cloaking does not help with cross-site tracking because it is partitioned.  It does allow the service provider to record cross-site tracking information that they received from some other means, though.  I want to know the site operator may not be making this decision entirely freely.  Is that correct?
* [John] Correct.
* [Aram] I’m generally opposed to third party CNAME cloaking for any purposes. CNAME cloaking is worse than pixels, site operators at least understand what pixels do generally.  We (WaPo) get pitches from operators to do CNAME cloaking all the time, we reject this because it’s a bad practice but most publishers don’t have that ability to say no or don’t have the knowledge to understand what they are saying yes to.
* [John] re: Vittorio (agreements) (copied from queue)  ( 1) The site owner also needs to be in agreement to deploy third-party scripts, third-party iframes, and third-party pixels. Such agreement is not sufficient for relaxing tracking prevention policies. 2) There are already multiple tracking companies doing third-party CNAME cloaking for tracking purposes. They even market it as a circumvention of tracking prevention. 3) The user agent acts on behalf of the user, not the site owner or the developer.)
* [Erik] Have folks seen evidence of increased investment in CNAME solutions, e.g. using A records to an IP instead? Does the discussion stop here? There are other things that can be done, reverse proxy etc.
* [Erik] What are the next steps here? Interventions will still apply, should work on FedCM etc. to help with this.
* [Vittorio] Large parts of our infrastructure are based on protocols like SAML (which has no JS at all). If you make interventions that make these things stop we’ll have a problem short-term. Should look at both the intermediate state and long-term horizon. CNAME is critical to this and blocking it categorically would be a problem.


### [Updates to PCM in Safari](https://webkit.org/blog/12566/private-click-measurement-conversion-fraud-prevention-and-replacement-for-tracking-pixels/)
* [John] Wanted to highlight this update, addresses 3 major pieces of feedback
    * 1st: Added unlinkable tokens on the merchant side. Fraud prevention on both sides.
    * 2nd: Same-site pixel API. Were going to go with a JS API to trigger event, but feedback told us site owners might not be able to or want JS, but will accept pixels.
    * 3rd: Measure clicks from cross-site iframes on click-source site. Publishers may want to isolate ads.
* [John] Working on an update on the spec


### Any other business
* Reminder: our next meeting on April 28th will discuss Cross-site cookies standardization
* Are there plans for a face-to-face at TPAC? -> Not sure right now.  September 


# Attendees	 
1. Aditya Desai (Amazon)
2. Alex Cone (IAB Tech Lab)
3. Allan Spiegel (Adobe)
4. Andrew Aikens (TripleLift)
5. Aram Zucker-Scharff (The Washington Post)
6. Ben Kelly (wanderview, Google Chrome)
7. Benjamin VanderSloot (Mozilla)
8. Bill Densmore, ITEGA.org
9. Brian Campbell
10. Brian Lefler (Google Chrome)
11. Brian May (dstillery)
12. Cameron Boozarjomehri (Mozilla)
13. Chris Needham (BBC)
14. Christine Runnegar
15. Daniel LaLiberte (Google Chrome)
16. Don Marti (CafeMedia)
17. Eric Mwobobia (ARTICLE19)
18. Erik Anderson (Microsoft Edge)
19. Erik Taubeneck (Meta)
20. George Fletcher (Capital One)
21. Heather Flanagan (Spherical Cow Consulting)
22. Jack Frankland
23. James Hartig (Admiral)
24. James Rosewell (51Degrees)
25. Jeffrey Yasskin (Google Chrome)
26. Jennifer Schutte
27. Joel Odom (Salesforce)
28. Johann Hofmann (Google Chrome)
29. John Wilander (Apple WebKit)
30. Katharina Koerner (IAPP)
31. Kaustubha Govind (Google Chrome)
32. Kelda Anderson
33. Kris Chapman (Salesforce)
34. Lola Odelola (Samsung Internet)
35. Mark Xue (Apple)
36. Michael Smith
37. Mike O’Neill (Baycloud)
38. Nicolás Peña Moreno (Google)
39. Paul Zühlcke (Mozilla)
40. Peter Snyder (Brave, PING)
41. Ralph W Brown (Brown Wolf Consulting LLC)
42. Russell Stringham (Adobe)
43. Sam Goto (Google Chrome)
44. Sam Weiler (W3C/MIT)
45. Sean Harrison (Google)
46. Stefan Popoveniuc (Google Chrome)
47. Tanvi Vyas (Mozilla, co-chair)
48. Theresa O’Connor (Apple, co-chair)
49. Tim Cappalli (Microsoft Identity)
50. Tim Huang (Mozilla)
51. Vittorio Bertocci (Auth0|OKTA)
52. Wendell Baker (Yahoo)
53. Wendy Seltzer (W3C)
