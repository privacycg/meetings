# Privacy CG Teleconference, 26 August 2021
* Chair: Erik Anderson
* Scribe: Sean Harrison

# Agenda

[Navigation-based Tracking Mitigations Work Item](https://github.com/privacycg/nav-tracking-mitigations)

[Storage Access API](https://github.com/privacycg/storage-access)
* [Let embedees optionally request access to partitioned cookies and storage #75](https://github.com/privacycg/storage-access/issues/75)
* [Allow forward-declaration of storage access need #83](https://github.com/privacycg/storage-access/issues/83)

[First-Party Sets](https://github.com/privacycg/first-party-sets)
* [Add "Clearing Site Data on Set Transitions" section. #57](https://github.com/privacycg/first-party-sets/pull/57)

[Ad Topic Hints proposal](https://github.com/privacycg/ad-topic-hints) updates

Announcements
* APAC-friendly call time every other month (5 PM EDT for September 23rd call)
* Face-to-face (non-)plan

Any other business	


# Notes
## Navigation-based Tracking Mitigations Work Item ([link](https://github.com/privacycg/nav-tracking-mitigations) to repo)
* [Peter Snyder] We are in the early days, working towards an agreed upon definition. We will start writing up what we know that other browsers are doing, text should be coming/initial PRs EOW
* [Jeffrey Yasskin] Chrome is especially interested in measuring how much of a problem this is, especially to notice use cases we've missed. If people have any suggestions for measurements we should be taking, please send them over. I work on web standards in chrome, previously worked on web bluetooth and web packaging
* [James Rosewell] There are legitimate uses for this that are legally compliant, such as the [swan proposal](https://swan.community/), and would like these legitimate uses be considered in this project.
* [George Fletcher] To be a little more specific, openID and SAML both use redirects like this and we don’t want to break identity on the web, so how do we handle these legitimate uses and that user consent is followed, but that users can still actually login. Because these flows look a lot like bounce tracking.
* [Erik Anderson] This is why this is a work item, different browsers have looked into different mitigations for this. This will be good to let people know where the conversation here is going forward.

## Storage Access API
* [Let embedees optionally request access to partitioned cookies and storage #75](https://github.com/privacycg/storage-access/issues/75)
    * [John Wilander] I flagged this for this meeting because it was brought up in the context of FPS. Title of are FPS good for users (https://github.com/privacycg/first-party-sets/issues/53). If one browser goes ahead and relaxes cookies for FPS, will it create a large compatibility pressure on from websites. One suggestion was to mandate calls for the storage access API. this gives all browsers/implementers a chance to put their policies forward. If for example chrome, wants to allow FPS, it can let calls go through.
        * 1. Require a call to the Storage Access API (SAA) for all unpartitioned cross-site storage access.
        * 2. Introduce a SAA option to state that you explicitly need access to unpartitioned storage.
        * 2. Allow scripts in 1p context to request storage access for cross-site resources within their FPS to not mandate iframes in those cases. (maybe even go into headers for this and not require JS)
        * 3. Browsers set their policies for SAA. For instance:
            * A) Automatic storage access within FPS, partitioned storage by default for all others without prompts, or prompt if outside FPS and 3p explicitly requests unpartitioned storage access.
                * From webkit side, different prompts for FPS member and full 3rd party
            * B) Different language in the SAA prompt for access within FPS.
            * C) Partition all except within FPS.
        * Possible way to solve compat pressure
    * [Kaustubha Govind] There is another issue 42[[link](https://github.com/privacycg/first-party-sets/issues/42)] asking for an async API. I don’t think they were asking for storage access or a different API. I would be interested to hear from developers. I think request storage access API creates a new model for all cross site storage, with every call now waiting for a promise to before reloading content to function. Is this a good trade off, how expensive is this, is this good ergonomics for developers. What is the right solution here.
    * [Johann Hofmann] I want to say that i understand this developer ergonomic concern, not the strongest point of the storage access API. We are not at the perfect state of developer ergonomics yet. A proposed change that somewhat improves these ergonomics, allowing HTTP only cookies on load, there are improvements coming to this that we can work on. We’ve been saying we need some kind of asyn mechanism, ideally storage access, in order to guarantee browser choice and not disadvantage browsers. In the interest of all browsers without taking away this possibility to restrict further.
    * [Aram Zucker-Scharff] These type of mechanisms being behind async is something that is going to happen more and more often. I don’t think having this on async is a blocker. The tools for working with async in the browser are increasingly getting better and better, so less of a problem than it used to be.
    * [John Wilander] This is an important point, this whole era of getting consent or understanding what the user wants is pushing things towards an async model, even if they got the consent 2 weeks ago. Something we need to think about is the persistence of storage access, so that say prompts don’t show up anymore, but it would still be async.
    * [Kaustubha Govind] We have an alternative proposal called [CHIPS](https://github.com/WICG/CHIPS). Where it is partitioned whether you have access or you don’t. The transition from partitioned storage from unpartitioned still needs a little more work, but it is something we could avoid by having cookies be always partitioned. Could we have a separate attribute for partitioned cookies. Some resource CDNs will use cookies for load balancing and just need an HTTP attribute and don’t need script. Is there any merit to pull out partitioned cookies for that.
    * [John Wilander] My view of the storage access API is about access, so the developer can express their desire for access with the request and just use the set/get cookies later based on the partition they picked with the API
    * [Johann] In Firefox we partition by default, so we don’t have a super strong opinion on this. I am very in favor of improving developer ergonomics if it doesn’t sacrifice privacy. We are determining what behavior is okay. I can’t really imagine it, but some browser could not want to allow this, but the async mechanism allows this. With the set cookie attribute you are having browsers dictate ??
    * [Kaustubha] Browser could block all cookies unless async is granted
    * [Johann] Unlike the storage access API we are deciding what is acceptable use of cookies being partitioned.
    * [Aram] Does Chips cover your concerns, yes it does, I will read proposal
    * [Kristen Chapman] We will definitely be using the CHIPS idea, we have not been encouraged with the storage access API, from a SaaS perspective, we will have issues for prompting the users in these scenarios. We will need a different setup for this, even when we do have a direct user interaction and can throw up a prompt, the site devs don’t want a second prompt come up, they would like to be able to pass permissions on. We would like a partitioned cookie under site control, we are trying to avoid adding more prompts that the site itself can’t control.
* [Allow forward-declaration of storage access need #83](https://github.com/privacycg/storage-access/issues/83)
    * [Hirsch Singhal] Idea here is that the UX and ergonomics that has an iframe that user has to click on before the app can load. It caused a lot of concerns with partners who wanted to know why we couldn’t show the prompt ourselves at a better time. Either a FPS is required for the IDP to prompt on the apps behalf, or the app has to forward-declare the permission delegation to the IDP. How do we make sure that it is the right time and appropriate to show this prompt, does the next navigation need to be back to the RP. The storage access api must accept a target domain that needs to be redirected to after the prompt.
    * [John] When we have discussed similar things, one concern that has come up in this delegation model, is that it could be turned into a gate. A social media site will not let you go to this new site until you agree that I can track you there. If were to get two FPS, to resolve SSO in one organization.
    * [Hirsch] We have both, with sites depending on each other for sign in. And we have applications, like salesforce, that rely on SSO. My internal partners would be happy with single FPS. SaaS less so, but with your proposal of a latch we could make this work with our SDKs and will require minimal reworking
    * [John] Worth exploring
    * [Johann] We have the API in a much better shape with comments we have received, and Hirsch we can work on trying to get something like this into the spec. Additional implementer interest would be good.
    * [John] lets work on proposed spec language and we can evaluate the privacy
    * [George] Similar to Hirsch's use cases, I think the forward declaration is a good idea, but am concerned about the FPS limitation, as this can create issues with certain RPs and IDPs. Ability for sites to say I am using this IDP over there so the forward declaration could be opened up.
    * [Johann] There is a version that doesn’t rely on FPS, we are not committing to this usage. There is a mechanism for RPs to register IDPs to allow them to call the storage access API on their behalf.
    * [John] I’m not ignoring the non FPS approach, but we have a new community group that is being spun up to discuss a way forward for federated sign in that doesn't rely on FPS at all.


## First-Party Sets
* [Add "Clearing Site Data on Set Transitions" section. #57](https://github.com/privacycg/first-party-sets/pull/57)
    * [Kaustubha] Problem in 2 parts. One is that user identity needs to split by party, users identity can be fluid and tracked across the set, but not between sets. So what happens when a site/domain leaves one FPS for another. We don’t think the leaving site should be able to take any info from other old FPS members when they move. The second thing is daisy chaining throw away sites that move about a bunch of FPS with the goal of tracking users across sets. 2 parts, preserving user expectations and preventing these attacks. We think we can mitigate this by clearing site data, should we do it on join and on leave of a set? FPS owner acts as a key. Owner is A and there is set ABCD, if A leaves should A’s data be cleared or all ABCD be cleared. There are a few examples (see link above) maybe we don’t clear on join and only on leave. We think this might be a good route as sites aren’t penalized for adopting a new feature, particularly in the transition period. The negative point of this is how does this align with user expectations. Another potential modification, if the new Owner used to be a member of the set it only clears themselves and not the rest of the set? (not sure I got this scenario right). Referencing github examples with owner and child domains. Red annotations indicate clearing of data for that site. Is it okay to only clear on leaving? If an owner domain switches sets, should that dissolve the entire sets data? Or should just the owner data be deleted?
    * [Brian May] If you don’t clear the data when the site joins the set, could sites bring in additional identifiers that the set could use. Can’t new sites being added add identifiers across many FPS?
    * [Kaustubha] Could site in set Foo take data from Foo to set Bar
    * [Brian] I was thinking site has identifier 123 and brings this identifier to the set it could be used to correlate a user across multiple sets.
    * [Kaustubha] A site can only join a single FPS
    * [Brian] And when the site left the FPS it’s data would be cleared for that specific site. If that domain enters the set and says user identifier 123, leaves set and joins another set and says use id 123 again.
    * [Kaustubha] We assume the site isn’t doing fingerprinting so it would have to be fingerprinting the user to do 123.
    * [Paul Bannister] What if the site stores 123 in its first party storage
    * [Kaustubha] We are proposing that all site data for that specific site is cleared so that isn’t possible.
    * [Brian] I’m imagining getting a prompt for a site that i don’t know wants to join the FPS of a site I do know.

## [Ad Topic Hints proposal](https://github.com/privacycg/ad-topic-hints) updates
* [Ben] Thanks for all the commentary and feedback on the proposal, we have put up the first version of an actual document[[link](https://github.com/privacycg/ad-topic-hints)]. Some changes (sharing screen) We thought about the idea of feedback buttons on ads, and wondered how browsers could detect ads and put in these buttons and make them look good, determined to be too hard. Instead the site can do this instead and sites can style the buttons however they would like. This opens up the potential for a lot of abuse, for example the buttons linked up to things having nothing to do with what the user thinks. To avoid this we think we should have some form of confirmation screen run by the browser to prevent this type of abuse. So users can confirm the context matches what they just clicked, but this does introduce more friction for users, it’s a tradeoff. Another thing we talked about was this 3d visualization of ad space, because that is all I can understand, but we will likely be operating in higher D space. We have found that you don’t actually need floating point embeddings, we can just use bits. Each bit represents a different dimensions, makes it really easy to compare the difference between embeddings, just the hamming distance. We also talked about a different way to present this that we can do differential privacy calculations on. Generate n random vectors, vectors closer to what you like get higher scores, and farther away get lower scores, allowing us to characterize the differential privacy as 2 epsilon. I don’t know how many random embedding vectors you will need to get halfway decent answers.
* [John] I assume that ad topic hints will be available cross site, it needs to be clear this feedback is available to all websites. My mental model is that if I provide feedback to an ad on facebook that the ads only change on FB, we need to make sure users understand that these clicks will affect all their ads. I would assume FB and Google are good at pulling out these embeddings for ads, what the ads are about, but not sure if other players can
* [Ben] I think it makes sense to indicate this will change your ads everywhere, the way we have designed it so that it can only affect one site if the user wants to, or the site can call the API to pull up the confirmation dialog so that it affects all sites
* [John] Or it could affect just one site based on user settings
* [Ben] That would kind’ve defeat the point, as to the second point, for creating features for the ads, we could find a way to release information on how to classify ads. The embedding doesn’t have to have a dependency on the wording, it can just be a neural net, and then later the embeddings can be translated to human readable names at that point. I will try and make a prototype of this to see what it will look like with a neural net and web assem
* [Brian] I’m worried about what this will be promising to the end user, as whoever is extracting the features of these ads to present to users, might have a different agenda. You are relying on people to use the signal in the same way, which is antithetical to how people do ads, using their info to do whatever they can in the way they like to do things. There will also not be universal adoption, so it will be a struggle to convince users to take the time to do this if they aren’t seeing all of their ads affected by it.
* [Ben] Will reply on the issue, out of time.

## Announcements
* APAC-friendly call time every other month (5 PM EDT for September 23rd call).  Every 4th call.  2pm PT.
* Face-to-face (non-)plan.  Not planning a second one for 2021.  We can take feedback and see when the next one should be, perhaps early 2022.

# Attendees	
1. Aditya Desai (Google)
2. AJ Knox
3. Angela Ballard (CJ Affiliate)
4. Anuvrat Singh (Amazon)
5. Aram Zucker-Scharff (Washington Post)
6. Ben Savage (Facebook)
7. Brian Lefler (Google Chrome)
8. Brian May (dstillery)
9. Chris Needham (BBC)
10. Christy Harris (Future of Privacy Forum)
11. Chrstine Runnegar (PING co-chair)
12. David Dabbs (Epsilon)
13. Don Marti (CafeMedia)
14. Eric Mwobobia (ARTICLE19) 
15. Eric Rescorla (Mozilla)
16. Erik Anderson (Microsoft)
17. Erik Taubeneck (Facebook)
18. George Fletcher (Verizon Media)
19. Grant Nelson (Triplelift)
20. Harneet Sidhana (Microsoft)
21. Heather Flanagan (Spherical Cow Consulting)
22. Hirsch Singhal (Microsoft Identity)
23. Hitesh Lad (CJ Affiliate)
24. James Hartig (Admiral)
25. James Rosewell (51Degrees)
26. Jeffrey Yaskin (Google)
27. Johann Hofmann (Mozilla)
28. John Sabella (PubMatic)
29. John Wilander (Apple WebKit)
30. Kate Cheney
31. Kaustubha Govind (Google Chrome)
32. Kelda
33. Kris Chapman (Salesforce)
34. Marshall Vale (Google Chrome)
35. Maud Nalpas
36. Melanie Richards (Microsoft)
37. Michael Kleber (Google Chrome)
38. Michael Seilnacht
39. Mike Smith
40. Nathan Schloss (Facebook)
41. Nick Doty 
42. Nicolas Arciniega (Microsoft)
43. Nicole Lesko (Meredith)
44. Paul Bannister (CafeMedia)
45. Paul Jensen (Google)
46. Peter Snyder (Brave Software)
47. Raj Belur
48. Russell Stringham (Adobe)
49. Sam Goto (Google Chrome)
50. Sean Harrison (Google)
51. Shigeki Ohtsu (Yahoo! JAPAN)
52. Steve Englehardt (DuckDuckGo)
53. Steven Valdez (Google)
54. Tanvi Vyas (Mozilla, co-chair)
55. Theresa O’Connor (Apple, co-chair)
56. Tim Cappalli (Microsoft Identity)
57. Viraj Awati (Amazon) 
58. Vittorio Bertocci (Auth0 | Okta)
59. Wendell Baker (Verizon Media)
60. Wendy Seltzer (W3C)
61.  Lola Odelola (Samsung Internet)
