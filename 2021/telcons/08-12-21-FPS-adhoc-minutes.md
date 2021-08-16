# Privacy CG Telecon - Ad Hoc Meeting, 12 August 2021
* Chair: Tanvi Vyas
* Scribe: Peter Saint-Andre, Jeffrey Yasskin for the last few minutes
* Thursday, August 12th
* 2pm PT, 5pm ET, 9pm UTC
Call-in details: see https://lists.w3.org/Archives/Member/internal-privacycg/2021Jun/0000.html.

# [First Party Sets Ad Hoc Meeting](https://github.com/privacycg/meetings/issues/14)
* Kaustubha Govind and Jeff Lazarus present - [Presentation Deck](https://docs.google.com/presentation/d/1iNZcuV1Eybhmgiw3V-EjVUiogIOggTIcMNIsqqmFMA0/edit?usp=sharing)
* Kaustubha reviews various anti-tracking policies (DNT, Chrome Privacy Model, Mozilla Anti-Tracking Policy, WebKit Tracking Prevention Policy, Edge policy, etc.)
* How do browsers define "third parties"? different registrable domain from the first party
* Why "site" or "registrable domain" isn't sufficient…
   * Single sign-on across domains
      * e.g., users typically understand that sony.com and playstation.com are served from the "same website" or "same first party"
   * Embedded content such as documents and videos
      * e.g., sharepoint and live.com
   * Improved security via isolation / separation of content
      * e.g., Google Drive loads content from googleusercontent.com
      * e.g., gov.uk sites are treated as separate because gok.uk is on the Public Suffix List (PSL)
   * Analytics and measurement
* First-Party Sets
   * Draw a "box" or "boundary" around a collection of domains
   * Small enough that users understand it, but large enough to enable functionality
* How to use First-Party Sets
   * SameParty cookies
   * Partitioned / double-keyed third-party cookies
   * Bounce / redirect tracking interventions
   * In WebID proposal, directed identifiers are keyed by set
   * Privacy budget could be applied across the entire set
   * Etc.
* Why do we need a policy?
   * Compliance with privacy model for the web
   * Consistency across browsers
   * Prevent abuse
* [Proposed policy](https://github.com/privacycg/first-party-sets/blob/main/ua_policy_proposal.md) (Jeff Lazarus)
   * Domains must have a common owner and controller
   * Domains must share a common group identity that is easily discoverable by users
      * e.g., wirecutter and NY Times (nytimes.com/wirecutter)
      * perhaps via a list of associated sites on a given website
   * Domains must share a common privacy policy that is presented to the user via UI treatment
* Enforcement via independent enforcement entity / verification entity (can be used by all browsers)
   * Maintain public declaration system
   * Verify that requester has control over the domains in the set
   * Perform spot checks for conformance
   * Perform technical check to ensure privacy policy is the same across all domains in the set
   * Perform technical check to ensure all First-Party Sets are mutually exclusive
   * Conduct manual reviews and investigations that have been flagged by civil society groups / research community
* Questions …
* [Sam Weiler] can we distribute the mutually exclusive checking? UA can help or do this on its own?
* [Kaustubha] we need an independent entity to do the other aspects, so having that entity also do technical verification seemed reasonable - but OK for UA to do that too
* [Sam] Perhaps could have both UA and verifier enforce those controls.
* [Sam] checking for common control - can imagine this being gamed via automated methods - how can we make it hard to game?
* [Jeff] we need to better develop the threat model to figure that out, more work needed
* [Kaustubha] we're hoping that because this is a public declaration, this becomes part of the privacy representation that an entity makes; could be enforceable in certain locales
* [Jeff] this could serve as a meaningful deterrent
* [Martin Thomson] I'm somewhat disappointed that this presentation doesn't seem to address TAG feedback yet; for instance, users might want to have different identities at sites like YouTube vs. Google; are we really doing uses a favor by building this mechanism? I'm still completely opposed to this based on TAG feedback and user comprehension
* [Kaustubha] TAG feedback had a few aspects, some of them were misunderstandings (e.g., not intended to violate the Same Origin Policy) and latest explainer should cover that; TAG reviewed latest changes; ???; concern that verification entity wouldn't scale, but it's public so gov/reg could get involved; we talked about having a group identity that is easily discoverable by users
* [Jeff] the challenge we're facing is that user comprehensive falls in a continuum: some users understand that, say, YouTube and Google are the same entity, but some don't; we hope that FPS would encourage sites to perhaps unify their branding to improve user comprehension
* [Martin] you're assuming that this is a problem to be solved, but for many users the current separation is actually a feature; let's look at the use cases like SSO, where other solutions are coming or could be brought to bear[a][b]
    * Comments:  [a]Anonymous: Sandpiper is an example of another proposal in this area : https://github.com/carbondmp/sandpiper.  [b]Martin: there are a few things that have proposed building on FPS.  same-party cookies is another one.  that doesn't make it a good idea,  it makes it more of a hazard
* [Kaustubha] aside from Storage Access API, what do you have in mind?
* [Martin] WebID is a possibility
* [Kaustubha] WebID is for federated auth, not SSO-- for a scenario like bbc.co.uk and bbc.com it would require them to rearchitect to have an RP+IDP setup.
* [Eli] question about coordinating second/third parties such as consent management platforms; right now they're syncing in many ways, but not easy to tease out what's happening there; it'd be good to have a way to separate access from Storage Access API
   * [eli written] My question is in regards to coordinating second parties, and how they might come into play in extension of the current ecosystem of consent management vendors.
You mentioned that every domain in a FPS needs to share the same privacy policy. Have you put thought into allowing good-faith privacy compliance vendors to be a 'coordinating' second party, that could share logic between the FPS members but cannot store data? Think: Privacy center & consent manager vendors wanting to help sync tracking consent data (not PII) cross-domain, but don’t want to see the consent data themselves
   * [Kaustubha] is consent shared across domains that are 1P?
   * [Eli] we actually don't want the data, we want other parties to get what they need
   * [Kaustubha] have heard other CAPTCHA / anti-fraud / SAAS providers that, say, have chat widget / session that needs to work across multiple sites
   * [John Wilander] (1) where are we with the small sets requirement? (2) with associated domains proposal we had a requirement that each domain needed to declare on its own what the complete set is (3) I think by domain you mean registrable domain so that different subdomains can't be in the same set
   * [Kaustubha] we felt that we could have technical limits instead of a particular number but we can think about that and get ecosystem feedback; some people have said they might have 200 domains but they might be service domains
   * [Kaustubha] as to restate requirement, that's part of the proposal and part of the check
   * [Jeff] regarding well-known locations, on the sites themselves?
   * [John] this could prevent an arbitrary site from including, say, Twitter.com
   * [Kris Chapman] we definitely have 400 domains and it would be hard to limit sets to a small number; if we could customize it for particular subsets / scenarios, that would be helpful; we're in favor of FPS and will be using CHIPS as well; would love a way for other entities to say what vendors they're using (e.g., Salesforce), this would help discoverability; better if we have a way that enables consumers to learn what's going on
   * [James] questions around competition and enforcement entities, no audio so we're going 
by written notes below. James Rosewell – not in a position to input via audio (too much background noise):
    * Two questions:
    * 1. The anti-tracking policies described at the start of the presentation impairs competition as they favour parties that can operate multiple services under the same domain and discriminate against parties that rely on suppliers to provide their services. This First-Party-Sets proposal groups together a small number of single controller domains operate by the same party and does not address the discrimination problem. Are Google willing to extend the proposal to become “Party-Sets” which encompasses data privacy laws with joint data controllers and separate processers rather than artificial domain names or a single controller?
    * 2. Why would civil society or the internet society be the enforcement entities? Shouldn’t we define the role of the enforcement entity before deciding who can fill that role? How do these enforcement entities align to elected law makers in Europe, UK or US?
Request response in written form if not able to answer now.
   * [Hirsch] / [Jason Nutter] question about how we mitigate false negatives in the approval process (e.g., the body says that xbox.com is not actually a Microsoft property), how do we handle that?
   * [Jeff] we hope that doesn't happen
   * [Jason] how would a domain move between sets, e.g. if there's a change of ownership
   * [Jeff] we talk more about that in the explainer, but we do envision that this might be necessary; the hope is that there is a public repository where those changes are available, and browsers would update every 30 days, anyone could watch the changes as well
   * [Kaustubha] when domain moves from one set to another, the old set definition needs to be purged; also considering rate limiting (can't change sets more than N times in a given period of time)
   * [Alex Cone] if something like FPS isn't created, there seems to be a boundary that's drawn based on a past decision that would arbitrarily enforce things based on that; end users don't have in their head what companies decided to do what when based on the facts on the ground; if we don't do something like this, we have an under-informed proxy for boundaries
   * [Kaustubha] 100% agree, that's the motivation for this work; another consideration is gTLDs, if we use the quaint notion of registrable domains those might not be supported
   * [Eli] is there anything like CHIPS that doesn't require the site to send network requests? imagine a widget vendor whose default sync mechanism doesn't necessitate seeing the data
   * [Kaustubha] those are partitioned by domain, not First-Party Set; what you'd prefer is local storage partitioning by FPS?
   * [Eli] it could be different but something like that might be fine
   * [John] instead of having each site state the entire set, an owner site could do that; cache policy needs to go into this proposal (how frequently should the cache be cleared for cookie scoping and such; think about PSL) - don't want to do this on every page load; branding is one of the reasons that people have multiple domains, some other browser affordance for branding than domain name might be worth considering
   * [Kaustubha] we are thinking about a designated owner domain, could help with double keying and perhaps also cookie clearing and other forms of user interface
   * [John] you want a policy suggestion that the owner domains needs to be the most recognizable domain
   * At the hour.  But we will go over a few minutes to finish the queue. NEW SCRIBE :-)
   * [Harshad] From PubMatic: Considering everything from the perspective of the domain owner. But as a user, maybe I don't want my information from Amazon.com shared with Amazon.in. Do we want the user's consent?
   * [Kaustubha] We definitely will, but at what granularity? At the browser level, "no FPSes at all"? At the level of the FPS? We can allow UAs some discretion. It would be a user setting. Not sure how much should go into specification documents. If we do it at a per-set level, it increases implementation complexity. There should be at least a global opt-out.
   * [Don Marti] I'm concerned about the language on ownership. There are so many jurisdictions where a company can be registered. It's so difficult for the enforcement entity to learn ownership. If a news organization has their ownership challenged after running a controversial story. The browser would have to go to a country where they don't understand the ownership records to figure out if it's a valid FPS. Using the word "ownership" adds a lot of complexity.
   * [Jeff] This is definitely a soft spot, where articles of incorporation and ownership can vary locale-to-locale. We welcome alternative proposals and ideas. 
   * [Don] I also filed another issue about going the other way. There are a lot of data stewardship questions here. I'm happy to review and suggest around stewardship.
   * [Martin Thomson] Seems like we're a bunch of 1%ers talking about 1% of problems. Great benefit to entities with lots of resources and lots of domains, and little benefit to those who don't. We're creating a lot of work and damage to solve those problems. We should talk about more important issues.
   * [Kaustubha] I don't think this is a 1% problem. It's common for websites to operate across multiple domains. On the repo, the folks who came to us include gov.uk.
   * [Martin] gov.uk should keep everything separate. The fact that they do common consent management is a good reason to think about consent management as an API.
   * [Kaustubha] Another company is Lucid Software, with 3 domains. lucid.app, lucidchart.com. Limits can help with the extremely large sites. Registrable domain is not the right anchor. We need something better than registrable domains, and we should argue about the right set of use cases.
   * [Martin] I don't think your opinion matters if others disagree. We should talk about the use cases, but we don't have the time here.
   * [Steve] The technical simplicity is hiding the policy complexity. We should look at use cases. Balance FPSes goals vs ways that work for other use cases. Look at WebID, maybe it would make sense to put in the work and have that work for SSO rather than just federated login. Appeals: can the enforcement entity get sued if they're impacting the business of another entity. Please stress test this against a hostile publisher. E.g. a big publisher putting their brand and privacy policy at the bottom of hundreds of sites.
   * [Kaustubha] The policy is the complex part here. I haven't seen a better proposal to tackle the first-party use cases, other than storage access API, which has challenges. FPS has a way to whittle down the use cases. Where are identifiers being shared across parties, which is really tracking.
   * [Kaustubha] We're doing an origin trial, which tries to do this, but that's not a great stress test. Want to ask Firefox folks about Disconnect's entities.json, which is most similar in practice. It's been in Firefox for ~10 years. Are the right folks to talk to the ones who maintain that list?
   * [Tanvi] We've been using that for less than 10 years, and Total Cookie Protection should help phase out use.
   * [Steve] Now at DuckDuckGo.
   * Adjourn.

# Attendees (sign yourself in):        
   1. Adam Days
   2. Alex Cone (IAB Tech Lab)
   3. Allan Spiegel (Adobe)
   4. Angela Ballard (CJ Affiliate)
   5. Aram Zucker-Scharff (Washington Post)
   6. Arthur Edelstein (Mozilla)
   7. Brad Lassey (Google)
   8. Chris Grzywacz
   9. Chris Needham (BBC)
   10. Chris Wood (Cloudflare)
   11. Daniel Rojas (Google)
   12. Don Marti (CafeMedia)
   13. Eli Grey (Transcend)
   14. Emily Lauber (Microsoft)
   15. Erik Anderson (Microsoft)
   16. Erik Taubeneck (Facebook)
   17. Harneet Sidhana (Microsoft)
   18. Harshad (PubMatic)
   19. Hirsch Singhal
   20. Jack Frankland
   21. Jade Kessler (Google)
   22. James Rosewell (51Degrees)
   23. Jason Nutter (Microsoft)
   24. Jeff Lazarus (Google)
   25. Jeffrey Yasskin (Google Chrome)
   26. Jessica Parker (Magnite)
   27. John Wilander (Apple)
   28. Jonathan Bolick (Google)
   29. Kaustubha Govind (Google Chrome)
   30. Kris Chapman (Salesforce)
   31. Laszlo Gombos (Samsung)
   32. Martin Thomson (Mozilla)
   33. Mike O’Neill (Baycloud)
   34. Mike Taylor (Google)
   35. Niarci
   36. Nicole Lesko
   37. Peter Saint-Andre (Mozilla)
   38. Phillip Eligio
   39. Phu Phung (University of Dayton)
   40. Raj Belur (Amazon)
   41. Rob Beeler (Beeler.Tech)
   42. Russell Stringham (Adobe)
   43. Sam Goto (Google)
   44. Sam Weiler (W3C/MIT)
   45. Steven Englehardt (DuckDuckGo)
   46. Tanvi Vyas (Mozilla, co-chair)
   47. Tara Whalen (Cloudflare)
   48. Theresa O’Connor (Apple, co-chair)
   49. Tim Cappalli (Microsoft Identity)
   50. Vinay Goel (Google)
   51. Viraj Awati (Amazon)
   52. Wendell Baker (Verizon Media)\
   53. Wendy Seltzer
