# 24 February 2022 Privacy CG Teleconference
* Chair: Erik Anderson
* Scribes: Heather Flanagan and Sean Harrison


# Agenda

* [Login Status API](https://github.com/privacycg/is-logged-in)
    * Call for editors
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [Implementer check-in](https://github.com/privacycg/storage-access/issues/97)
* [First-Party Sets](https://github.com/privacycg/first-party-sets)
    * [Update Applications section with proposed Applications #84](https://github.com/privacycg/first-party-sets/pull/84)
    * [Update explanation of Clear Site Data in README.md](https://github.com/privacycg/first-party-sets/pull/77)
* Any other business
    * Reminder: [our next meeting on March 10th](https://www.w3.org/events/meetings/811c2c7a-ef52-46e5-8bcd-8ee8fc4f09c4/20220310T120000) is at the APAC-friendly time.


### [Login Status API](https://github.com/privacycg/is-logged-in)


#### Call for editors

Erik: currently have one editor, looking for another.

John: Just me. I used to co-edit with Melanie Richard while she was at Microsoft. Would like another person from another company (other than Apple). Anyone interested in credential management, webappsec. This has the potential to go beyond the browser into OS features. If anyone is interested, please contact John Wilander (slack, chairs alias)


### [Storage Access API](https://github.com/privacycg/storage-access)


#### [Implementer check-in](https://github.com/privacycg/storage-access/issues/97)

Erik: wanted to do an implementer check in. Seen the work item iron out a few of the remaining interop questions. We have three implementations. 

Tess: There has been a lot of convergence of the last 6-9 months. Lots of strides towards agreeing in principle on converging behavior. What has changed in actual shipping implementation that we should captured in the spec that we haven’t captured yet? Any issues that can be closed out? If there is someone from each implementation that could speak to recent changes.

John: We seem to converge on this being for cookies, and not for other means of storage. There are certain complexity around service db and workers. It’s hard to switch between partitioned and unpartitioned without breaking pieces of the running JavaScript. 

Tess: quick question - in the longer term, if we wanted to make it possible to  have access to both partitioned and unpartitioned storage, I assume we could do something around explicit APIs about storage buckets, rather than have the bare API change what storage its talking to. 

John (Apple): absolutely. We aren’t saying we will never give access to unpartitioned storage. The version we’ll try to take to the HTML spec will deal with cookies, and level 2 work will figure out how to get access to unpartitioned storage. There was also an idea originally pitched form Microsoft around reversing the order, so the party that becomes the third-party that requests storage access where they can do something beforehand. This would be primarily for login scenarios The embedding party says “I want to allow this other party to allow storage access under me” and then there will be a prompt under the login flow. We have a proposal for that, but there is interest to see if FedCM would respond to this, but there are already implementations for this other idea, so it’s a question. 

Brandon (Microsoft): we’ve been maintaining our implementation in Chromium. It is primarily for cookies. Haven’t done any thinking on unpartitioned storage. 

Ben (Mozilla): made a few improvements, mostly aligning with other implementations. Some are subtle and small that weren’t called out as points of misalignment. We released a blog post ([https://hacks.mozilla.org/2022/02/improving-the-storage-access-api-in-firefox/](https://hacks.mozilla.org/2022/02/improving-the-storage-access-api-in-firefox/)) detailing some of that. Other than those changes, it’s been a lot of maintenance. Right now, Gecko currently unpartitions all storage, including index db and service workers. That’s quite complicated; it’s on the work plan to change that and align with other browsers. So we will remove the partitioning of non-cookie storage from that. 

Tess: We wanted this on the agenda to get a sense for whether the implementations are actually converging on what’s in the spec. Sounds like everyone is moving in a solid direction. As far as the status of the spec, there are sections that still need to be written and editorial work that needs to be done. One thing we’re trying to do now is to get this to graduation, moving it from PrivacyCG to a standardization venue (e.g., WHATWG or a W3C standardization group). So if you know of any open issues on this API that should be resolved before we take it to standardization, there is a label of “resolve before graduation” - please add it to any issue you think we’ve missed that needs to be addressed. We don’t need to address everything prior to moving it forward, but we want to be in a strong place. We just need a solid draft. 

Heather: you mentioned FedCM. We’re making progress on that and have calls on Mondays. One of the big problems is we’re looking for other browser vendors to come in and give feedback, help test, info on if it’s a direction they would support or not. Pretty lacking and a concern for the FedCM CG.

Tanvi: John W. talked about the multistep login flow. Do we want to wait to level 2 to work out those details, or do you think it’s fast and we can get it into the first draft?

John: we could commit to making those changes in a reasonable timeframe, but if other implementers don’t have time we would need to reconsider. There is no consensus on it yet; there is a proposal from Microsoft, but more work is needed.

Tanvi: suggest we talk about this offline and figure out if other implementers are ready to start talking about it and move quickly

Anne: important for graduation of the spec is testing. We have added an automation section to the spec, but not sure the status of getting it into the browser. How many web platform tests are there for this feature? 

Tess: great question. Don’t know offhand on coverage, but we want there to be coverage before we take it to standardization. Regarding the driver feature, not sure if it’s implemented in webkit. 

John: We have a bunch of automated tests, but not sure about the web driver tests

Brandon: As part of the Chromium implementation, we submitted sum tests for that, but that was prior to the web driver addition. Any extra contributions that leverage the web driver work would be welcome

Nick: would like clarification on the ability for an embedding site could ask for this. There was some PRs, but is there an open issue for changing this re: the embedding site should allow for an iFrame

John: it’s called “allow forward declaration of storage access” 

[https://github.com/privacycg/storage-access/issues/83#issuecomment-989205411](https://github.com/privacycg/storage-access/issues/83#issuecomment-989205411)

Erik: are you asking for that or asking about the control of if an iframe is allowed to call the API? 

Nick: thought those were the same thing? 

John: you may be talking about permission style. Way that will be controlled is currently through the iFrame sandbox, where you need to explicitly allow it to call the API. 

Nick: That’s not for the login use case? 

John: the embedded party is the one requesting storage access. The IdP might ask under the RP if they can have access. 

Erik: you might be an IdP iFrame on an RP site, and you might forward declare. This needs more spec and implementation to land on the details. 

Ben: Responding to Heather - Working on gecko, and do plan on tuning in and participating in FedCM. Plan to be there next week.

Tess: Apple internal process for getting approval to join groups takes a while; still working that internally 


### [First-Party Sets](https://github.com/privacycg/first-party-sets)


#### [Update Applications section with proposed Applications #84](https://github.com/privacycg/first-party-sets/pull/84)

Kaustubha: Did some work to rewrite the application section on the explainer. It was written before from Chrome’s perspective, but that didn’t capture other browser’s positions. So they’ve listed different applications re: what web platform features could use FPS. Split up the section into two areas: Top one captures relatively more mature features (e.g., bounce tracking protection shipping in Safari) where we think FPS could help and preserve flows. That’s something that Safari might be interested in using FPS for. While Chrome doesn’t ship anything like that today, Chrome is philosophically aligned. Next two are more specific to Chrome and Edge re: cookies; as long as the domains are in the FPs we would relax access to cross-site cookies. Next one is cookie partitioning (see: CHIPS) where many website provide third-party services to provide user flows, and we’re saying where there are SaaS providers that provide workflows via multiple domains and that use consent cookies, we’re proposing cookies could be partitioned on the top level site. Below this is more in-progress work, more future looking. These are potential applications, such as storage partitioning key. Not sure it’s worth going into detail on each, but have linked to issues for each for more discussion. First one is storage partitioning. There are security concerns here, and we want to be clear that cookies by default are not a security boundary. As we do the partitioning work, we know there are both security and performance implications. There will be a tradeoff in the work to balance security and performance. Below storage and cache are cross-site refresh restrictions. This is more future-looking stuff. There is another team in Chrome that’s working on cross-site pre-fetches for performance improvements, currently limited to same storage and same site. Another one, proposed by Edge, was whether you could scope PWAs to multiple origins if they are in the same FPS. For security reasons, the browser says the PWA comes with a manifest file, but developers would like to serve pages across multiple origins. Could FPS be useful there? PWA should have their own mechanism called scope extensions, but we could layer on a browser check that relies on FPS. Next are related to storage access API. Another idea suggests that storage access API could relax some requirements (e.g, how long the permission persists for). There are also concepts of directed identifiers in FedCM, but that’s a very future thing that’s being considered but not being developed right now. 

**<scribe swap>**

So that’s for FedCM, FPS could be used here. Privacy budget is a future looking proposal to keep track of fingerprinting entropy, and if sites are in a FPS they would all consume the same budget instead of by each site. We also have this origin of same-site and cross-site, could we define a similar concept for a same party token, This was brought up in respect to CORP

John: Thanks for the run through, and the other use cases for FPS. In the proposals we have brought up we lean on the concept of things being a trustworthy source, including for FPS, such as a small set. There could be a connection from this to the login status API, if something has a stated purpose for a stated set, and say this is about login for the user. So I’d like to see where you are on the trustworthiness.

Kaustubha: 2 points to address. With respect to the trust, one aspect is the size and one thing we struggle with, it will be a curated list called a UA-policy. So we have looked at various W3C docs that use party, so we have tried to reuse this definition, such as this concept of GDPR data ownership. This is where we have started as it is a common theme. The other is user understanding, browser UI should make this clear but the site should also show this. The other is privacy policy, as some sites have differing policies for sites that would be in similar sets. The technical limits on size, the struggle has been what is the numerical limit, 5 has come up, but folks from microsoft have a few hundred domains. For Chrome, with this used for multiple applications, we want to make sure they are alls supported, will link issue []. Is the limits the primary control that you see?

John: No, we also worry about what happens after this is shipped, as we have one world view today, but what will happen in the future, mass consolidation of sets, that could bring back cross site tracking to the web.

Kaustubha: For storage access API we could add that metadata to the ??, I will link that issue [] to here more about it.

Kris: Salesforce has quite a few domains, we would have issues with a small cap on the FPS size, then we couldn’t get our sites functioning with FPS. But we understand the issue of FPS’s becoming too global, as that would be too permissive. We will be talking about this next week. As a SAS provider we would like the sets to be more dynamic, so we can provide the flexibility to site owners to provide them with customized lists, so site owners could grant permission to the FPS’s and not just us saying these 20 domains are in the set, and want the site owners to specify it themselves. The thinge we are most interested in is a way to manage these FPS in a way that works.

Aram: Interested in this idea of FPS allowing login providers. But worried how this would conflict with FPS being limited some way. If it would support login providers we would need an amendment to allow login providers would need to be included in a large number of sets, and login providers a la Google would need an exception

Kaustubha: Need to figure terminology, when we say FPS login we mean first party login providers, with login provided in the same set, say playstation and sony with the redirect to login.sony.com and back to playstation. For FedCM is the canonical use for 3P login providers.

Aram: Makes sense to me, not sure why they can’t put it on the same domain though. Resolves some questions going on around FPS.

John: When I say single sign on, I mean the same organization, Federated login means another organization providing the login servies. So for FPS, saying this is my login domain and want it to have special privileges in my FPS. Some domains might not be for login but may instead be for ad tracking.

Kaustubha: Please do take a look, especially other browsers, at the PR and leave your comments/concerns/questions.


#### [Update explanation of Clear Site Data in README.md](https://github.com/privacycg/first-party-sets/pull/77)

Kaustubha: More of an FYI, for FPS we have said that if a domain changes ownership and leaves the set, that it is not taking user data from one set to another. How do we define this clearing of site data, the clear site data header seems the right choice, but this would need to only clear site data in that partition and not across all partitions, because that would create a side channel for tracking. It is kind’ve awkward as it is written today, so wanted the rooms feedback on the right way to define this, mostly an FYI, no need to chat here, more of a standard and spec thing, looking for feedback on how to write it down.


### Any other business

* Reminder: [our next meeting on March 10th](https://www.w3.org/events/meetings/811c2c7a-ef52-46e5-8bcd-8ee8fc4f09c4/20220310T120000) is at the APAC-friendly time.
    * 7 hours later than standard call, plan is every other month first meeting fo the month (second thursday)
* FYI - [Requested ad-hoc on Cross-Site Cookie behavior](https://github.com/privacycg/meetings/issues/16) - Chairs will work to schedule soon.
    * Please do take a look at this, opened by Ana?
* Kris: We should try to set up a FedID and PrivacyCG joint meeting to discuss things and had everyone in the same place at once instead of bits and pieces.


# Attendees
1. Allan Spiegel (Adobe)
2. Aman Bhurji (CJ Affiliate)
3. Angela Ballard (CJ Affiliate)
4. Anne van Kesteren (Mozilla)
5. Aram Zucker-Scharff (The Washington Post) 
6. Benjamin VanderSloot (Mozilla)
7. Brian Campbell
8. Brandon Maslen (Microsoft Edge)
9. Bruno Selva
10. Christian Biesinger (Google)
11. Christine Runnegar (PING co-chair) (part)
12. Cornelius Witt (eyeo)
13. Dan McArdle
14. Daniel LaLiberte 
15. David Zabaleta (CJ Affiliate) 
16. Don Marti (CafeMedia)
17. Dongoh Park (Google)
18. Emilia Paulski (CJ Affiliate)
19. Emily Lauber
20. Erik Anderson (Microsoft Edge)
21. Eric Mwobobia (ARTICLE19)
22. Garima Dhingra
23. George Fletcher
24. Ghassan Maslamani (Zaatdev)
25. Harneet Sidhana
26. Heather Flanagan (Spherical Cow Consulting)
27. Hitesh Lad (CJ Affiliate)
28. James Hartig (Admiral)
29. Joel Odom (Salesforce)
30. John Wilander (Apple WebKit)
31. Kadin Van Valin
32. Kaustubha Govind (Google Chrome)
33. Kelda Anderson
34. Kris Chapman (Salesforce)
35. Marshall Vale (Google Chrome)
36. Matt Liu
37. Mike Smith (Hearst Magazines)
38. Nick Doty (Center for Democracy & Technology)
39. Paul Zühlcke (Mozilla)
40. polpojan
41. Russell Stringham (Adobe)
42. Sam Goto (Google)
43. Sam Weiler (W3C/MIT)
44. Sean Harrison (Google)
45. Steven Valdez (Google Chrome)
46. Tanvi Vyas (Mozilla, co-chair)
47. Theresa O’Connor (Apple, co-chair)
48. Tim Cappalli
49. Wendell baker (Yahoo)
50. Wendy Seltzer (W3C)
51. Yi Gu (Google)
