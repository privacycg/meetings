# 12 May 2022 Privacy CG Meeting
* Chair: Tanvi Vyas 
* Scribe: Jeffrey Yasskin, Nick Doty

# Agenda
* [Cross-site cookies standardization continued(https://github.com/privacycg/meetings/issues/19)
* Any other business

# [Cross-site cookies standardization](https://github.com/privacycg/meetings/issues/19)

## Interaction of cross-site cookies and SameSite=None.
* [Slide deck - Assorted cookies issues](https://docs.google.com/presentation/d/1OBR1mfp_EEBOQJr26tQd6LUmU8CmZOClQqVBMjSabd4/edit#slide=id.p)
* Anne presenting:
   * privacycg/storage-partitioning#30 needs review. It creates a section for cookies.
   * Filed httpwg/http-extensions#2084 on cookies. Please review this too.
   * Question of what a cross-site cookie is. For A1->B->A2 nested documents, does A2 get SameSite=None cookies?
   * When the B->A2 link is created by B navigating to A2?
   * We haven't done telemetry to measure how common this is.
   * Aram: I have seen this particular scenario. Publishers doing direct ad sales can do exactly this. They host an ad, which is served through an adtech manager. E.g. washpo.com -> gam -> washpo.com.
   * Tanvi: Do you prefer a particular outcome?
   * Aram: It's common for us to use the top site's cookies for personalization within the ad itself, and we think it's desirable to retain that. An ad might have a bunch of stories, and we read out a user preference cookie (since they're logged in) to see what stories they like, so we pick a subset that's related to, e.g. cars. We want to keep doing that.
   * Lee: It does happen. Not super common for us. You can embed in us, and us inside other things, and people will embed their own content in their own artifacts, and embed that in our portal. We want consistency. No strong opinion, but has to be consistent.
   * Jason N: At MS, top frame and bottom frame might be in the same set, maybe same site. 3p embeds that might want to embed MS auth server. A1 and A2 would be in same FPS; we'd want to be able to not make user sign into each embedded component.
   * Anne: Do you want SameSite cookies to go to the innermost one.
   * Jason: In theory, yes.
   * Kris: This scenario takes place with federated identity. The top level should be the consideration, and everything under should be able to communicate back. When it comes to federated identity, it's typically using 3p cookies, but obviously that has to switch to 1p cookies. This consideration of how the permissions flow needs to include federated identity as a key use case. Only other option is to do a server->server communication. Usually used for an IdP to say the user's logged out. That goes through cookies, but using server-> server for this would be a security problem because you have to tunnel through complicated networking conditions.
   * Erik: Nomenclature: Which cross-site are we talking about? If we're saying it would no longer be sent, it would make these scenarios postMessage down the chain. But that increases security risks since the intermediates might grab the token or not sufficiently protect it. My preference is to not force people to do workarounds, so allow cookies to be sent. Prefer that A2 gets the SameSite=None cookies.
   * Ben K: My understanding is that SameSite=None cookies, is to protect framing attacks for sites that don't expect to be framed. Still need to consider that. Second, cookies over network vs JS access? With JS access, I think A2 and A1 can synchronously script each other, which would let it interact without any postMessage. But then it has to understand it's framed and has to opt in.
   * Jeffrey: Probably this is implicit, but i wanted to make it explicit, since this question is only about samesite cookies, for the nested groups want to be able to continue to use top level cookies, is there a separate attribute you can add for this. 
   * Anne: Sounds potentially interesting. Instead of None, you'd use another value, like "I don't care about my ancestor chain.
   * Aram: I think that would be useful. Having an additional value instead of a new value might be a better way to approach it. "None, Nested". Having a third value would be hard
   * Jeffrey: We can never add new samesite values.  We would always be adding another cookie parameter
   * Jeffrey: Can’t add more because a browser who doesn’t understand it treats it badly.  (Could maybe use some digging from chrome folks to better help explain this.) [Erik: see this for a summary: https://www.chromium.org/updates/same-site/incompatible-clients/]
   * Ben V: General agreement with Ben K: Want the two definitions to be in agreement, and don't want to relax samesite protections for cookies because of that unexpected embed context. If there are alternatives to SameSite cookies, I'd prefer to use that in these nested embedding cases. Want more detail about the particular scenarios.
   * Lee: I might not understand about adding a new parameter. Why would people who set SameSite=None be upset? Missing the security issue, since I said I'm ok being framed, and set my cookies to SameSite=None to allow it, and I got framed, but there's an intermediate. If there's another attribute, I'd add whatever attribute, since I want to be framed.
   * Anne: Maybe None is sufficient. In a world where we use a bit of "do I have cross-site ancestors", A2 is cross-site, so even SameSite=None doesn't apply anymore … could maybe, but it's a little weird. It crosses the boundary that we're trying to enforce. Might be ok to make an exception, but it changes the semantics some.
   * Lee: A->B->A, if your partition key includes all ancestors, it's confusing. Want to re-read why we want all ancestors from a security standpoint. 
   * Anne: Confused Deputy Attack -> A2 might not realize that B is getting the instructions. Maybe it opted into this by using SameSite=None.
   * Allan: What's the threat model here. How could it be abused if we treat it as same-site?
   * Anne: Threat model is confused deputy attacks. A1 and A2 also have direct script access, and given the cookie spec's threat model, that's also problematic. It was very useful to hear the Washington Post's example. this does happen, and we can't completely break it. Have to be careful than I was hoping we'd have to be. :) Maybe there's a middle ground. Can get stricter partitioning key, but maybe we make an exception for SameSite=None. Where you're opting out of confused deputy attacks.
   * Ben V: Thought the attack was in the default case, so SameSite=Lax. Sending cookies for same-site contexts, where A2 didn't think about X-Frame-Options so didn't set it. That's why the SameSite-ness is different from the cookie perspective. With SameSite=Lax, they dont' get the cookies, but they do have access to same-party storage. So attack isn't a problem for =None, just =Lax.
   * Anne: If we partition A2, we end up with partitioned storage, but your SameSite cookies aren't. That's a weird edge case.
   * Ben K: For Chromium storage partitioning, non-cookie storage partitioning will have something similar to the ancestor bit there. For confused deputy, when a Service Worker is present (which can act like a cookie), need the ancestor bit. Don't know the status for the CHIPS proposal.
   * Anne: You'd use that instead, where A2 would set a partitioned cookie.
   * Ben K: It wasn't as clear-cut, because there were some use cases around this, maybe the ones that were brought up here. CHIPS may or may not use the ancestor bit.
   * Johann: We're making up 2 different meanings of cross-site, one for security and one for privacy, and that's not very developer-friendly. Maybe we should avoid doing that.
   * Anne: This discussion was super useful. We should go back and think about this more, and next time we can discuss it more concretely, with particular options. And we need to think about what's workable webcompat-wise.
   * Tanvi: There are use cases, and several ways to handle this.
   * Kaustubha: CHIPS today doesn't take the ancestor chain into account. For cookies, SameSite=None is opt-in, so that opt-in handles the security risk that caused storage partitioning to use the ancestor chain. So we think this is ok, but wanted to check for privacy leaks.
   * Anne: I will file an issue for this, on storage-partitioning.
* Anne:  When A1 embeds B, and B navigates to A2, does a2 get samesite=none cookies? Doesn’t seem cross-site with any cross-site ancestors.
   * Aram: don’t see any particular risk.
   * Kaustubha: do we want developers to opt in to partitioned state in this case?
Anne: I would not expect A2 to get partitioned cookies in this case.
   * Lee: sometimes I might embed B and get redirected to A2 like in an auth provider case. Is that going to end up with unexpectedly different cookies?
      * Anne: I think A2 is not partitioned, but doesn’t get Lax cookies because cookies do take into account navigation.
* Anne: when A navigates to B via a POST method (e.g. form submission), does B get samesite=none cookies? I think yes, but there could be navigational tracking concerns.
   * Kaustubha: depends on what we do for navigational tracking. We have seen this in identity workflows. Would we have to distinguish between identity or payments, or specific cookie attributes for fedcm or other purpose-built APIs?
   * Aram: I could see malicious navigations by embedded ads, and if it creates privacy risks, I expect that would be exploited.
   * Brian Campbell: for top-level navigation, seems like the primary use of SameSite=None. Don’t think changing SameSite attribute is the way to mitigate attacks. SameSite=None is being used today for identity workflows in cross-site POST submissions, to secure single-sign-on transactions. Shouldn’t be changed.
   * Anne: not saying these scenarios will change, just trying to gather evidence on how it’s used or what the privacy risks are.
   * Brian: lots of breakage when there was a change to default SameSite Lax, but providers have now mostly switched to explicitly specifying SameSite None.
   * Kris: from fedidcg, it is expected behavior and would like to keep it. Federated identity has very wide and varied deployments, including education and government. Not all developers may have time/resources to change.

## Adding more contextual information to requests: 
## Sec-Fetch-Ancestors
* [Sec-Fetch-Ancestors? w3c/webappsec-fetch-metadata#56](https://github.com/w3c/webappsec-fetch-metadata/issues/56) & [Fetch-Metadata to indicate when the browser is in a partitioned context](https://github.com/w3c/webappsec-fetch-metadata/issues/80)
* Anne: Should we give servers more contextual information? 1) How should we tell if a nested context is partitioned? 2) Reveal some information about the ancestor chain? Documents could use this for security purposes. Might not want to do any computation cross-site. Could create Sec-Fetch-Ancestors for this. Parallel proposal for a Sec-Fetch-Partitioned: header. Can we combine this?
* Kaustubha: Should we think about cases where the ancestor chain is cross-site but not partitioned? A1->B->A2. So I think we do need the Partitioned bit. For enterprises, browsers will probably need an allowlist for legacy behavior. Even not A->B->A, maybe B has been allowlisted so it comes up just for A->B. 
* Ben V: Ties into an issue we opened for the storage access api: How clear to the developer do we want to be about whether they're partitioned and whether they've been granted storage access. 2 views: want to make it unidentifiable whether the user clicked accept or reject, because the embedded site might be malicious about it. Or we should be as clear as possible with developers, and there are too many side channels anyways. Sec-Fetch-Partitioned is very similar. I lean toward giving them the information, but I hear opinions from both sides.
* Aram: I see both sides, but I think it's worth knowing  that anything that can be used for fingerprinting will be used. Any signal like this will be used for fingerprinting. Mitigations in other parts of the API might make this useless or more important. e.g. for folks who promise invalid-traffic-detection and anti-fraud. 
* Anne: For enterprise scenarios, does B also get unpartitioned storage? How big a change for those scenarios?
* Kaustubha: It's a good question; we're thinking about it as only for cookies right now. Cookies are more web-observable. The policy is for compatibility, and since cookies have that deterministic behavior, while cache is opportunistic. It's a different team doing cache partitioning, so I can ask them if they've seen enterprise scenarios.
* Anne: If it's just for cookies, it seems like a storage access thing. Not partitioning, since that's all of the things, but storage access is just for cookies. If we say you're not partitioned, but most of your storage is partitioned, it's not the right approach.
* Tanvi: There are more concrete questions to discuss; we're out of time, so postponing to slack and agenda+'ed issues.




# Attendees (sign yourself in):         
1. Alexandre Nderagakura (IAB Europe)
2. Allan Spiegel
3. Andrew Aikens (TripleLift)
4. Angela Ballard (CJ Affiliate)
5. Anne van Kesteren (Mozilla)
6. Aram Zucker-Scharff (The Washington Post)
7. Ben Kelly (Google)
8. Benjamin VanderSloot (Mozilla)
9. Brian May (dstillery) 
10. Brian Campbell (Ping)
11. Chris Needham (BBC)
12. Claude Vervoort
13. Daniel LaLiberte
14. David Dabbs (Epsilon)
15. Don Marti (CafeMedia)
16. Eli Grey (Transcend)
17. Eric Mwobobia (ARTICLE19)
18. Erik Anderson (Microsoft Edge, co-chair)
19. Erik Taubeneck (Meta)
20. Frantz Alexis
21. Gabby Schnaid
22. Guillermo Movia
23. Harneet Sidhana (Microsoft)
24. Jason Nutter (Microsoft)
25. Jeegar Shah (CJ Affiliate)
26. Jeffrey Yasskin (Google Chrome)
27. Johann Hofmann (Google Chrome)
28. Kaustubha Govind (Google Chrome)
29. Kedla Anderson
30. Kiran Gopinath
31. Kris Chapman (Salesforce)
32. Lee Graber (Tableau/Salesforce)
33. Mark Xue (Apple)
34. Mike O’Neill (Baycloud)
35. Nick Doty (Center for Democracy & Technology)
36. Philipp Pfeiffenberger
37. Polikseni Pojani (CJ Affiliate)
38. Sam Weiler (W3C)
39. Sean Harrison (Google)
40. Stefan Popoveniuc (Google Chrome)
41. Tanvi Vyas (Mozilla, co-chair)
42. Theodore Olsauskas-Warren (Google)
43. Theresa O’Connor (Apple, co-chair)
44. Tim Huang (Mozilla)
45. Vinod Panicker
46. Wendell Baker (Yahoo)
47. Wendy Seltzer (W3C)
48. Yi Gu (Google)
49. Allan Spiegel (Adobe)
50. James Rosewell (51Degrees - 18mins past start)




