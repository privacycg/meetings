Privacy CG Teleconference, June 24, 2021
* Chair: Tanvi Vyas
* Scribe: Sean Harrison


## Agenda
* [First-Party Sets](https://github.com/privacycg/first-party-sets)
    * [#43 - Technical-only enforcement of "UA Policy"?](https://github.com/privacycg/first-party-sets/issues/43)
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [#83 - Allow forward-declaration of storage access need](https://github.com/privacycg/storage-access/issues/83)
    * [#82 - Consider: Unblock federated logout by making storage access allowance symmetric](https://github.com/privacycg/storage-access/issues/82)
* [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    * [#57 - Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)
* [Proposal: Ad Topic Hints](https://github.com/privacycg/proposals/issues/26)
* Any other business
    * Face to Face Survey Results

## [First-Party Sets](https://github.com/privacycg/first-party-sets)
* [#43 - Technical-only enforcement of "UA Policy"?](https://github.com/privacycg/first-party-sets/issues/43)
   * [Kaustubha] Verification is a big part of FPS, most browser policies and the Do not track policy has this definition of first party, being same owner and data controller with user understandability. We’ve proposed here that we’d like this to be a standard policy across all browsers, at least all supporting browsers. An independent entity that can verify FPS submissions and verify integrity. ONe recent call with TAG, a possibility of a technical enforcement of this policy to be more sustainable in the long term. Edge gave a few suggestions and I’ve made a few suggestions [below]. Hoping to merge this into the main repo. One of the ways for this to be enforced, suggest to developers that they can get FPS only for sites owned by the same org with checks. Person who is submitting set has control over all of the domains. Let’sEncrypt uses the ACME Protocol that uses DNS records to issue keys. This kind of check will at least filter out frivolous submissions. This also suggests adding transparency logs, such as the public suffix list, with changes made to the PSL openly merged and requested. A similar mechanism using github trackers with public entity lists. This provides a mechanism to manage requests and add accountability and auditability to the process. In addition, there is an element of self verification, but need a way to revoke FPSs that are fraudulent. There is revocation process for certificates for example. The first 3 steps that allow this are all technical, no single entity or verifier doing this. There were a few ideas from Erik Anderson, we can decide there is a minimum set size with lower levels of verification. Techniques such as GREASEing some % of the time ??. Any other ideas for FPS, that are either lightweight or technical only to enable FPS. At the start there was no policy verification at all, but got feedback that we need a way to stop bad faith actors, resulting in a policy based process. Is self-assertion and revocation enough.
   * https://github.com/krgovind/first-party-sets#relying-solely-upon-technical-enforcement
   * [John] Part of the TAG review conversation, I’d prefer if the FPS didn’t specify how the sets are to be used. I think Chrome plans to relax some 3P cookie blocking based on FPS. It would be helpful if FPS just specified these are FPS and this is how you can create them and they can be enforced, then UA’s can handle them as they may
   * [Kaustubha] SSO domain serving multiple domains, do you think the way the policy is defined should be different based on the application. One possible application could be around 3P cookie blocking or isLoggedIn. All of these are essentially, relaxing restrictions based on FPS status, just trying to apply one coherent boundary. Apologies for this vague “privacy boundary” But this boundary depends on UA implementation. Do you think this should change anything about the policy?
   * [John] I don’t think anything needs to change because of this, but just need to reason and what we need to do to move this forward. You can provide examples of how this can be used, 3P cookies, bounds, etc. But it would be nice if FPS was just FPS and then browsers can decide how to use FPS  
   * We are getting ready to merge a change into the explainer. I think we need to keep the explanation of how we will tackle 3P cookies, or allow browsers to make their decisions based on FPS status. How could we make sure that FPS isn’t abused. 
   * I would have to go discuss with some coworkers, with some different mechanisms to define FPS, different browsers and non-browsers involved.
   * [Don] Very encouraged by the prospect of an automatic check to avoid future policy debates. There are some non-W3C, but industry standards (such as ads.txt), which is something a browser or site could check to see if sites are ineligible to be FPS to each other. Any interest in a general provision that a browser or crawler could verify these industry standard files to determine if something is a valid or invalid FPS?
   * [Kaustubha] interesting idea, but nothing at the moment.
   * [Don] happy to discuss offline, very interesting use case around sets of domains (ads.txt makes a statement about business relationships that would be useful as an input to show whether or not a pair of sites are under common administrative control) 
   * [Daniel] Thanks for engaging with TAG feedback, and cautiously positive on this middle ground. Still a little confused on this example with a.example and b.example https://github.com/privacycg/first-party-sets/pull/45#discussion_r655988770  being too far afield. Gets to the root of the issue with scope creep of how browsers will use FPS and thereby eroding the concept of origin, where certain new technologies might choose this privacy boundary over origin. I’m not clear about this aspect of your PR.
   * [Kaustubha] sounds like the PR didn’t do a great job explaining this. If a cookie or storage is bound to a domain, only a specific origin can access it. In FPS, a.com is never accessing b.com’s cookies, in this case could a.com get cookies on b.com (but not b.com’s cookies) when 3P cookies are disabled. Does this cripple the web or does this enable users to provide consent for some users critical use cases. This is our solution for domains that are in the same org, saying a.com should not have 3P cookie blocking when a and b are in the same set.
   * [Aram] One thing popped out in the explanation, this idea of not checking small FPS though the verification system. Fake sites could setup as fakeWP.com and WP.com is a small set and now has more access to WP.com when it is not associated. Another issue when not checking these small sites is small pop-up sites from grey SEO companies, and they chain these FPS together ?? These sites could create small FPS and you have a/b b/c c/d and make daisy chained FPS that aren’t being verified. We have seen this sort of grey SEO sites relation showing up in the past, and this strikes me as a concern. If I had a blackhat on this was my first thought to take advantage of this.
   * [Kaustubha] going back to first question, if a bad actor made fakeWP.com, the fake site does not gain access to WP.com cookies, but if  fakeWP.com was embedded on WP.com it has cookies. 
   * [Aram] fakeWP.com could embed WP.com to try and peek in and see some information it shouldn’t on WP.com, this could potentially be a big security problem and that is not even getting into the privacy issues in the daisy chaining problem
   * [Kaustubha] It sounds like we need a better explanation here, I’ll put my newer version in the chat.
   * In closed queue - Lee Graber -- (just for record) -- For John’s comment on differing treatment of FPS, if that were to happen, wouldn’t developers still be left building different solutions to try to work on different browsers making the standard not quite as useful?
   * [James Rosewell] – [nonverbal comment] - Simeon Thornton Director, UK Competition and Markets Authority – [CEPR 17th June 2021](https://cepr.org/content/privacy-antitrust-integration-not-just-intersection-0) – “1st party and 3rd party data distinction harmful and would lead to excessive integration, harming competition. A prime example of the need to consider competition impact of privacy rules and possible unintended consequences.” This proposal, and others that are justified on the 1st and 3rd distinction, need to be reconsidered in light of this direction of thinking from regulators.


## [Storage Access API](https://github.com/privacycg/storage-access)
* [#82 - Consider: Unblock federated logout by making storage access allowance symmetric](https://github.com/privacycg/storage-access/issues/82)
   * [Hirsch] This idea around federated logout, the idea here is after you sign in, we want to make sure sign out works. After the sign in gesture you get access to the IDP’s cookies, on logout we open iframes to the IDPs website, the cookies aren’t accessible, so that sign out no longer functions. Proposing making the cookie access symmetrical to handle this. We run into the issue where IDPs as embedded iframes still don’t have access ? so this will help with the high level problem of signing out.
   * [Johann Hofmann] As is mentioned in the original issue comment, we are significantly expanding what websites are allowed to do here and not analyzing the new attack space. Generally I can understand the use case, and we want authentication to work with the storage access api. We need to carefully analyze this from a privacy perspective.
   * [john] Sorry for not commenting earlier. 2 pieces of feedback. 1.) as soon as you have a 1 to many situation the risk of privacy problems increase immensely. The IDP can now secretly talk to all RPs and talk to all of them about a specific user, passing information along. IDP can become a dispatch center for user information through hidden iframes. 2.) even if you were to scope it down to just allow the iframes to log the user out (they don’t get access to their cookies but the site can tell them to log the user out). This creates the same problem where clear site data cannot clear partitioned data, allowing one domain to control information on another domain? We can limit this to places where we know the user was logged in. We have isLoggedIn to cover more of a logged in scenario and webid to handle a federated login scenario. Using more dedicated specs for loggedIn state rather than storage access.
   * [hirsch] Purpose built APIs are better suited for this, but working with what we have (storage access API). After the user has gone though and said I am okay with the IDP tracking me on this site and passed a directed identifier from the IDP to RP. This is not subverted when the IDP broadcasts directed IDs to other RPs. They can talk about the user behind their back, but it is a lot of work, and already possible after login? 
   * [John] nefarious bad things can be done through servers and other bad technology. We need to make sure the things we design do not allow nefarious things. A third thing that ties into that is that say RP is a news publisher, now the IDP can show you news in iframes using loggedin state, and this would be surprising to users.
   * [Lee Graber] One question for John. First time I’ve head that user prompt where we are talking about the user making an informed decision. This user prompt for federated identity, you 2 are working together to keep me signed in and clicks no and is sad when it doesn’t work. User prompting here is opening too many doors, but in other cases it is okay. I agree targeted APIs are the best, but there is value in discussing using existing technology and using that to make the web safer without requiring everyone to update their software. It is interesting to explores and not stick to the strictness of the API should be well defined for what it does. There is value in not forcing people to do upgrades.
   * [John] Sites (RP) that are relying on IDPs shouldn’t need to change anything
   * [Hirsch] That is not the case today, you need to show the iframe, and today almost everyone is using an invisible iFrame, it will be hard to force everything to a visible iframe

*  [#83 - Allow forward-declaration of storage access need](https://github.com/privacycg/storage-access/issues/83)
   * [johann] The idea is that you can define a mode, so you don’t need to have user interaction on the RP to an IDP iframe to give the storage access to allow login, but puts the IDP in more control of the sign in flow and is better for the user. The flow jason proposed is that both the RP and the IDP need to call an API to enable storage access to 3P, and IDP needs to call API to send prompt to user, and forward to the RP. allowing these 2 sites to continue to cooperate to sign users in. This really comes back to the question of should we support these use cases with storage access API, and I think yes, because we need something to support login before webid and the like are adopted. 
   * [Jason Nutter] We would like to make the storage access api to be better for users and sites around auth and identity. A lot of feedback is the prompts are unintuitive, and the flow is unintuitive. Need a way to ask the user for permission in a way that is more understandable. As opposed to how it is today, where we have to go back to RP, then show a full frame to show where the identity came from. We want to make the more intuitive for users particularly for browsers not happy with the storage access api as it is.
   * [John] I have commented on this one, I am tentatively supportive of if the RP and IDP agree through API calls, I don’t see a problem with the prompt on the RP side, but if it is one way it could be used to setup as a gatekeeper thing where the IDP needs cookie access for any of this to work.
   * [Jason] I think we are totally on board with this
   * [John] isLoggedIn has something like there where we would like this flow to work. If we think that storage access api can make these things work lets go ahead
   * [Sam] I empathize with johanns position and webid is a bit farther out. We are working on logout specifically based on observing the login. Building a dedicated purpose API for logout. We would welcome checking if our design is compatible. We would welcome collaboration as we will run into some of the same levels. Should logout be a high level logout API, or an API to allow 3P cookie access for logout. Should the API be one time only, sync/async. We are open to working together to go through this. I have posted the proposal and prototype in the github issue, so you can read more about it there.

## [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
* [#57 - Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)
   * Defer to next time

## Any other Business
* Face to Face Survey Results - 
   * Pasted in slack
* Reminder of option for ad hoc meetings for Work Items - request here - https://github.com/privacycg/meetings/issues/new
   * More info on ad hoc meetings can be read here - https://github.com/privacycg/meetings

## Proposal: [Ad Topic Hints](https://github.com/privacycg/proposals/issues/26)
* [Ben] I put up a proposal in the privacy CG repo. The internet is trending to private by default with identity shared by domain or FPS. how will a website select a relevant ad to show a user on their site, not an issue for big sites, but for small sites and blogs, where they would like to show relevant advertising. The floc proposal is interesting, collecting behavioural data through the browser and the browser bucking users into interested groups. A new proposal, allow the user to be in control, and the user can select their own interests, and the browser can forward these interests on to sites. This skips a lot of what is done today, and just has the site ask the browser what sort of ads should be done. A few ways this could be done. FB using ML to look at ads and classify ads, browser could do this for ads the users see and interact with, and allow users to say I hate this type of ad never show it again, vice versa. By centralizing this somewhere the user is in control, it puts the user in a more meaningful position of control. It is also highly unstable, unlike flow which is stable over 7 days. What do you all think
* Twitter conversation link - https://twitter.com/johnwilander/status/1406646001352331268?s=21 (maybe?)
* [John] 3 things On device auctions shouldn’t expose a signal to 3P, don’t store PII, there are other concerns with on device auctions. 2 It is very important to explore that no explicit signal from the user and explicit signal from the user should be not identifiable. 3. Users can shape their own persona, and select whatever they like even if untrue.
* [brian] I have been working in ad tech for 20 years and considered this consistently over time, and using this as the basis for advertising. Users should have the opportunity to shape their online identity and only expose what I would like to. In cases where the user doesn’t want to share anything, just use contextual ads. I noticed a couple days ago John has a twitter thread where he can define himself from TPAC 2020, so this is a great way to proceed, what have we missed in the last 20 years.
* [Jabari] This is a great idea, we have a similar proposal, which is a comment on the proposal, that introduces an in browser profile with user interests, and we can extend that with general info about the user that they choose to expose about themselves. Make sure that info does not escape the privacy sandbox. 
* [James] To Brian's point, this only works if all parties do it. If facebook,apple, google commit to doing this and not use their mass of data to advertise. If they do then advertisers will receive comparable return on investment across all platforms as the highly integrated companies will no longer have an advantage from their scale and one-sided terms and conditions. Without such a commitment it’s not viable. There are economic issues that need to be discussed before this can happen.
* [Johann] This could possibly expose a lot of sensitive info about the user, and the train of thought is that the user is in control, and that a significant percent of users go ahead and use this feature. What happens if 95% of users on Chrome don’t do this at all. What happens when users decide not to care? I’m not negative about the proposal in general, but we need to make sure to not trick users into exposing this info.
* [Ben] I want to understand exactly the issue Johann, are you worried about the default state sharing, or the differential privacy way of sharing the selected info. Presumably the user setting this is understanding this.
* [Michael] This is a possible future evolution of the FLOC proposal, we ran one way of combining people into cohorts. One thing that Tanvi proposed to us earlier. This a discussion about the possible future of flow
* [Kristen Chapman] We have a very browser and ad-tech view of this. A lot of the time when we are delivering a single ad, we care about the journey. There is value in what that is doing for the user. Having users in control of this is an excellent idea. There is more going on then just content targeting, there is the whole marketing journey that we are missing in these conversations.
* In closed queue - Aram Zucker-Scharff -- Just adding a note that I like the direction of this idea and generally moving more user control into place around what data users reveal and giving them the capacity to control it is really interesting to me. 


## Attendees (sign yourself in):        
1. Aditya Desai (Google)
2. Allan Spiegel (Adobe)
3. Andrew Knox (Facebook)
4. Anne van Kesteren (Mozilla)
5. Anthony Molinaro
6. Aram Zucker-Scharff (The Washington Post)
7. Arnold RW
8. Ashkan Soltani
9. Ben Cochran (Dotdash)
10. Ben Savage (Facebook)
11. Bill Densmore (ITEGA.ORG)
12. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
13. Brian Campbell
14. Brian May (dstillery)
15. Chris Needham (BBC)
16. Chris Wilson (Google)
17. Christine Runnegar (PING co-chair)
18. Christy Harris (Future of Privacy Forum)
19. Daniel Appelquist (Samsung)
20. Daniel Rojas
21. Eli Grey
22. Eric Mwobobia (ARTICLE19) 
23. Erik Anderson (Microsoft)
24. Erik Taubeneck (Facebook)
25. Grant Nelson
26. Harneet Sidhana
27. Heather Flanagan (Spherical Cow Consulting)
28. Henry Lau
29. Hirsch Singhal (Microsoft Identity)
30. Hitesh Lad (CJ Affiliate)
31. Ian Meyers (LiveRamp)
32. Jabari Pulliam (NextRoll)
33. Jack Frankland
34. James Hartig (Admiral)
35. James Rosewell (51Degrees)
36. Jason Kint (DCN)
37. Jason Nutter (Microsoft)
38. Jean-Luc GARNIER (Kortex)
39. Jeff Lazarus (Google)
40. Jeffrey Yasskin (Google Chrome)
41. Jessica Berman
42. Jlukas
43. Joel Meyer
44. Joel Odom (Salesforce)
45. Johann Hofmann (Mozilla)
46. John Wilander (Apple WebKit)
47. Jun Kokatsu
48. Kaustubha Govind (Google)
49. Kelda Anderson(Microsoft)
50. Kris Chapman (Salesforce)
51. Lee Graber (Tableau/Salesforce)
52. Lionel Basdevant (Criteo)
53. Mallory Knodel (CDT)
54. Manuel
55. Mark Xue (Apple)
56. Marshall Vale (Google Chrome)
57. Maud Nalpas (Google/Chrome)
58. Max Gendler (NYTimes)
59. Michael Kleber (Chrome)
60. Mike O’Neill (Baycloud)
61. Nathan Schloss (Facebook)
62. Newton K. (Magnite)
63. Nick Doty
64. Paul Bannister (CafeMedia)
65. Peter Dolanjski (Duck Duck Go)
66. Peter Saint-Andre (Mozilla)
67. Peter Snyder (Brave Software, PING co-chair)
68. Phil Eligio (EPC)
69. Przemyslaw Iwanczak
70. Raj
71. Richard Anton (Amazon)
72. Rick Byers (Google Chrome)
73. Robin Berjon (NYT)
74. Russell Stringham (Adobe)
75. Ryan Arnold (P&G)
76. Sam Goto (Google)
77. Sam Weiler (W3C/MIT)
78. Sean Harrison (Google)
79. Shigeki Ohtsu (Yahoo! JAPAN)
80. Shivan Sahib
81. Shivani Sharma
82. Steven Valdez (Google)
83. Tanvi Vyas (Mozilla, co-chair)
84. Theresa O’Connor (Apple, co-chair)
85. Tim Cappalli (Microsoft Identity)
86. Viraj Awati (Amazon)
87. Wendell Baker (Verizon Media) 
88. Wendy Seltzer (W3C)
89. Ashkan Soltani (Self)


