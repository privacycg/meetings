### Privacy CG Teleconference, June 10, 2021
*   Chair: Erik Anderson
*   Scribe: Sean Harrison

## [Agenda](https://github.com/privacycg/meetings/blob/main/2021/telcons/06-10-agenda.md)
*   [IsLoggedIn](https://github.com/privacycg/is-logged-in)
    *   [What does logout mean in a federated context? #30](https://github.com/privacycg/is-logged-in/issues/30)
        *   [John Wilander] Left over from FTF, filed a while ago, if you are logging out from the RP, should that affect the login state with the IDP? I don’t think it should, you could be logged in with many sites with your IDP, it would give a lot of power to the RP. When you log out of IDP should it log you out of all RP. You have a relationship with the website, used IDP to log in. Ask for opinion on this, to avoid confusing users. IsLoggedIn is only the bit, for the purpose of IsLoggedIn should on party bit changing affect the other.
        *   [George Fletcher] Logout from a user side is a mutually exclusive problem. If I login to opentable via Google and then go and click sign out on opentable, there is no concept to me that I am still logged into Google. Someone else could come to the computer and I’d still be signed into google. Same identity flow across domains, identity is used between collaborating parties to represent disconnect parties with federated identification. Users expected state with RP and IDP is applicable to bit in the browser, since we are talking about the isLoggedIn bit. The sessions should be connected through the bit, refresh token fails when site tries to refresh loggin. Nuanced scenario with multiple desires. RP lets you know to maybe also log out of IDP? Log out of IDP and want hiking trails site to still be logged in. IDP should inform all RPs that logout has occurred, handoff to RP for login control.
        *   [JohnW] Distinction between FL between different orgs and same orgs. SSO vs federated login flow, so single organization can take care of this with SSO. Opportunity here for FL with login to IDP right before RP, then RP can inform user to sign out of IDP on logout. A way to enhance the experience
        *   [Jason] Oauth, yes and no, if you want RP logout to affect other RPs, gives other RPs chance to clear user cache and logout user, gives sites opt-in choice. Give a front channel logout opt-in by providing URI to receive signal.
        *   [John] Not obvious this would tie to isLoggedIn as it is a single bit
        *   [Jason] Key is to make it possible for this to happen
        *   [Lee Graber] From Tableu/salesforce. For enterprise identity (different from other identity) when user signs out of enterprise all enterprise sites should all logout at once. All systems should be logging you out. Currently solving bugs where this is in fact not happening. Want to make sure that identity is really you for all use cases. It is really you not just you signed into Gmail
        *   [JohnW] Is this all in one organization, SSO
        *   [LGraber] This is generally SSO?
        *   [JohnW] Then I have the other term I use Single Sign Out, very important counterpart
        *   [Sam Weiler] In most federated use cases, one logout not affecting other use cases, SSOut should be parallel. Play with the naming?
        *   [Brian May] Risk of someone remaining logged in when they think they are out is substantial. On logout of one site, present list of sites user is logged into and option to logout of the other sites, to mitigate.  \
[Allan Spiegel] +1
        *   [Peter Saint-Andre] really hard to get right, UX and consent will be close to impossible, raising red flag. We need to play close attention and seek other input to get it right
        *   [JohnW] tend to agree, have not spent too much time on federated loggin case, bridge with webID to be solution to federated login. There is enough complexity for a single site on login/out
        *   [Tim Cappalli] one of the challenges is federated and SSO, mean different things to different people. To me, SSO is an experience that can result from an active session.
        *   [Erik A] share any more thoughts on the open issue, a lot of broader identity things to think through
    *   [Support for federated logins, or the ability to transfer IsLoggedIn #35](https://github.com/privacycg/is-logged-in/issues/35)
        *   [JohnW] these 4 issues are very tightly integrated. Ability to transfer isloggedinbit to have IDP transfer the bit reliably. That’s how we were thinking of supporting federated login, just the bit, no actual loggin token. Next issue is SSO, opportunity to use FPS for SSO, if we can agree on the rest of FPS. Could mandate states mandate FPS use SSO for a single domain. Let’s assume a simple case with a small set, this is my login domain, accoutns.google.com, etc. Have really nice support in isLoggedIn for SSOut, so now the browser can set the IsLoggedIn bit for the set, so the loggin semantics and out semantics create nice FPS support. 
        *   [George Fletcher] Clearing the bit for FPS, what does this mean. Bit is cleared for the set, logout, or does this mean local state for all FPS domains is wiped. The understanding of this will be very important to IDP for these sets. But will help in cross domain cases in the same org.
        *   [JohnW] so far just been thinking about the bit. But in this case the consequences changing the state of the bit. This could affect how long the browser holds onto data
        *   [JohnW] not good enough to be generic, but here are the cookies for the auth state and these could be cleared on logout, others carried across sessions
        *   [JohnW] see this in the proposed JS APIs so you can talk about what token you are using for login state, this is the dedicated auth token, hoping for dedicated tokens, but could be cookies
        *   [Sam Goto] There is a lot of intersection to converge on, learning more about the use cases of isLoggedIn, use cases, concerns, UX. Exchanging notes there is valuable to both proposals, same or seperate specs is too soon to know for now. Loggout scenario just discussed, IDP embeds the RP in iframes expecting them to have 3P cookies so RP can perform logout, clearing whatever is needed. Without 3P cookies this breaks. One question for IsLoggedIn, if front channel logout is not possible without 3P cookies, there is no event for IDP to call on logout. What are the implications of settings IsLoggedIn. If the browser is aware of logg
        *   [JohnW] These are hard questions due to complexity. Prefer a breakout session with this group and others for a high level description of uin, would this allow the setting of cookies for IDPs to RPs. We can’t escape the question of the semantics of IsLoggedIn.se cases and agree terminology - what is SSO, SSOut, federated, etc. for there to be sections in the spec for this. So we can have a more coherent discussion on IsLoggedIn.
        *   [Sam Goto] One step ahead here, OpenID Foundation have started enumerating the use cases. Giving names to these things, with rankings of importance. Things expect to be broken, etc. A useful starting point
        *   [Sam Goto] Does this cover SSO, SSOut etc.
        *   OpenID's work on enumerating use cases
            * https://github.com/IDBrowserUseCases/docs
        *   [George Fletcher] covers all the use cases, so use cases each have an issue, if there is something missing please add it. Cover all of these things. Pure UX things will likely be added later. Things that make login on mobile browsers easier. Identity methods, how is this browser associated with this user, how much to challenge the user on sign in. Please add anything you see missing and find more volunteers, please contribute on these different flow descriptions.
    *   [Potential use of First Party Sets for Single Sign-On #43](https://github.com/privacycg/is-logged-in/issues/43)
        *   [JohnW] already covered, saying that if the purpose string in FPS, so owners of sets can say this is a dedicated SSO domain, to provide IsLoggedIn for this.
        *   [Kaustubha] Intriguing idea, brought up in context of Storage Access API, FPS adds a lot of interesting interactions. MWest suggests using FPS to share authentication across domains. This could be a way that orgs could provide additional metadata. Don’t know how squarely this fits into the current scope about creating a security boundary. Is it typically that these domains plus in in other ways, is WebID a better mechanism or is FPS better, or is this just for enterprise use cases or is this commercial, where google/facebook sign in works on other domains as well. On the FPS issue, do we need to worry about i18n for the strings, because there are variations in terminology, standardize or flexibility. Many questions about exact requirements, but don’t need to answer them all now.
        *   [JohnW] In my mind thinking about the consumer case, signing into skype with microsoft, etc. Not thinking only enterprise, mostly consumer. Enterprise has a lot more domains than the plan for FPS. On strings, using a single string in all the storage access api prompts for sites in the set, use the prompt around login purposes, not for other purposes.
    *   [Integration with WebID #44](https://github.com/privacycg/is-logged-in/issues/44)
        *   [JohnW] Google has now done this work for WebID where WebID and isLoggedIn could become one spec.
        *   [JohnW] are there connections between WebID and IsLoggedIn to work together.
*   [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    *   [Offer same-site pixel "API" as alternative to a JavaScript API #71](https://github.com/privacycg/private-click-measurement/issues/71)
        *   [JohnW] this allows an alternative for PCM and attribution api with a redirect to a well known location, you should trigger an attribution report. Originally wanted to add JS for more full control of the conversion report. Feedback from this group was JS is hard to get merchants to deploy, takes too much time. Same-site pixel, allows the merchant site to redirect to a well known location on their own site, allowing this to be set with URL parameter instead of JS. HTTP get for the image only, no POST body.
        *   [John Delaney] So one of the things around same-site pixel and JS in general, this provides additional utility of not needing to know the site the click happened on. If advertiser has this same site pixel, they don’t know which publisher it was sent to? Don’t need to deal with this in attribution reporting. Losing a bit in this agreement about publisher getting the report
        *   [JohnW] Have both a way in same-site pixel, really be able to pick these 5 publishers and is what I want to measure. I am advertising this product on these 5 sites and only want to convert for them, works the same as 5 pixels on the page. Wanted a wildcard option, this creates the worry you have?
        *   [John Delaney] yes
        *   [?]  It will take longer and everyone will use the wildcard one, is that your concern?
        *   [?] yes, this will save a lot of bandwidth on the request
        *   [?] this is a great privacy improvement
        *   [John Delaney] advertiser now needs to understand what the consumer needs from these reports, click on a.com and b.com, those repots have the same set of encoding for trigger side data, puts a lot more onus on the advertiser to do this
        *   [JohnW] would it better to just not support wildcard? You have to list everything
        *   [John Delaney] hard for me to say, attribution reporting doesn’t have this issue, but it is a bit utility win not too need to know every site you are running ads on
*   [First-Party Sets](https://github.com/privacycg/first-party-sets)
    *   [Proposal: require websites to call an async API to request dating sharing within a first-party set #42](https://github.com/privacycg/first-party-sets/issues/42)
        *   [Kaustubha] Arthur brought up this issue. Firefox isn’t doing FPS, what can we do with design to increase interop. Ideally have design everyone is happy with. Biggest sticking point is the policy, hoping for one off call to discuss this and reach consensus and find a way forward. Looking at all browsers anti-tracking policy, no one says anything outside domain is tracking. Idea with FPS is to come up with better idea than domain for 3P tracking. For a browser that wants to inform user about FPS relationship, with interoperability. Treat FPS as a privacy boundary, will add to explainer as well. The thing the chrome team is very interested in is how this applies to cookies, same party explainer, additional attribute to add to cookie so FPS will allow same FPS cookies to be passed according to explainer. The imp I’ve seen from cross site cookie access is storage access API. How can we apply FPS to what other browsers are working, use the FP manifest file and surface it. Same issue as same party cookie. How do we reconcile. This is what we want to standardize, doesn’ need to have a script loaded to get access to cookies, it makes HTTP only cookies problematic, and this is what Chrome has been pushing HTTP only attributes. HTTP state tokens as a replacement (Mwest), we have been pushing HTTP only attributes. This makes things more complicated. Johann added a new idea that seems interesting to integrate this into a prompt
        *   [Johann] don’t need to say anything, good summary. It isn’t one bit issue with FPS. several issues with the spec. Building user interfaces for FPS is hard to give consent before data sharing happens, we don’t see this in the current spec. Being good privacy citizen, sites setting FPS size to one, etc. Arthur asks every member of set, when every member of set needs to call an async api, ask up to a certain point and then we don’t ask. Websites will now have this explicit failure mode built in. Failure does not become a web compat problem but something website expects. Have only the parent ask (SSO example) for cookies on other domains, when it is setting the cookies are you okay with setting up this set, UI is handwavy. I see that there is an issue with forcing users to opt into an entire set at once if you are really privacy conscious, connecting youtube and google sure, but the other 100 sites, not so sure. It is a solution that has a prompt with a failure mode, but solves the problems you were mentioning, at the time of asking there won’t be dependent documents loaded, you could pull HTTP only cookies without too much of a problem. We are not saying let’s do this and now we are on board, we are thinking how to intelligently build consent into spec.
        *   [Vittorio] One thing to raise, often post name domains are not designed to be human readable, part of a domain name might be unreadable. It would not make a lot of sense in a prompt, so naive solution where you inject a consent, to handle FPS might not work all that well. Getting prompts for certificate where the user is exposed to the SHA of the cert which means nothing even to technical people, the injecting of consent will not be trivial in all cases.
        *   [Johann] This is not only about consent but also user participation, to prevent about of this API, user participation is an additional mechanism is an additional layer.
        *   [Vittorio] We need to think about mechanism to make this effective, not just lawyering up, so that the user can actually understand it
        *   [Johann] I concur
        *   [Lola] from samsung internet. Happy to hear you are thinking global consent at once is a bad idea. How to prevent websites from penalizing users who only want to consent to one website and not the entire FPS
        *   [Johann] this is problem I am talking about, forcing user to consent on every site
        *   [Kaustubha] some of these questions apply to request storage access api
        *   [JohnW] apple webkit, from webkit perspective, I think we are aligned with mozilla on how to handle FPS with regards to cookie sharing for the set. Mostly interested in the signal FPS can provide for the UX it can provide the users, for example in login purposes, or for expanding the scope of storage access, expanding the scope from the page to the whole browsing scope for example. But not you need to globally opt in to use any of the sites.
    *   [Technical-only enforcement of "UA Policy"? #43](https://github.com/privacycg/first-party-sets/issues/43)
        * We ran out of time for this; will pick it up first during our next scheduled call.
*   Any other business
    *   [Erik] We had the FTF survey, results will be shared next time.


## Attendees
1. Allan Spiegel (Adobe)
2. Andrew Knox (Facebook)
3. Anthony Molinaro
4. Aram Zucker-Scharff (he/him; The Washington Post)
5. Ashkan Soltani
6. Ben Savage
7. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
8. Brian Campbell (Ping Identity) 
9. Brian May (dstillery)
10. Cezary Cerekwicki (Opera)
11. Chris Needham (BBC)
12. Christine Runnegar (PING co-chair)
13. Christy Harris (Future of Privacy Forum)
14. Daniel Appelquist (Samsung)
15. Dave Harbage (DuckDuckGo)
16. Don Marti (CafeMedia)
17. Edison
18. Eric Mwobobia (ARTICLE19)
19. Erik Anderson (Microsoft, co-chair)
20. Erik Taubeneck (Facebook)
21. George Fletcher (Verizon Media)
22. Hitesh Lad (CJ Affiliate)
23. Jack Frankland
24. James Rosewell (51Degrees)
25. Jason Nutter (Microsoft)
26. Jeffrey Yasskin (Google Chrome)
27. Joel Odom (Salesforce)
28. Joey Salazar
29. Johann Hofmann (Mozilla)
30. John Delaney
31. John Sabella (PubMatic)
32. John Wilander (Apple)
33. John Wunderlich
34. Jon Callas (EFF)
35. Kaustubha Govind (Google Chrome)
36. Kelda Anderson (Microsoft)
37. Konrad Dzwinel (DuckDuckGo)
38. Kris Chapman (Salesforce)
39. Lee Graber (Tableau/Salesforce)
40. Lionel Basdevant (Criteo)
41. Liz Hurley (CJ Affiliate)
42. Lola Odelola (Samsung Internet)
43. Mallory Knodel (CDT)
44. Marshall Vale (Google Chrome)
45. Maud Nalpas (Google Chrome)
46. Melanie Richards (Microsoft)
47. Michael Kleber (Google Chrome)
48. Mike O’Neill (Baycloud)
49. Mike Seilnacht (Intuit)
50. Mike Taylor (Google)
51. Nathan Schloss (Facebook)
52. Newton (Magnite)
53. Paul Bannister (CafeMedia)
54. Peter Lowe
55. Peter Saint-Andre (Mozilla)
56. Przemlyslaw Iwanczak
57. Raj Belur (Amazon)
58. Richard Anton (Amazon)
59. Robin Berjon
60. Russell Stringham (Adobe)
61. Sam Goto (Google)
62. Sam Weiler (W3C/MIT)
63. Sean Harrison (Google)
64. Steven Valdez (Google)
65. Tanvi Vyas (Mozilla, co-chair)
66. Theodore Olsauskas-Warren (Google)
67. Theresa O’Connor (Apple, co-chair)
68. Tim Cappalli (Microsoft Identity)
69. Valentino Volonghi (NextRoll)
70. Viraj Awati (Amazon)
71. Wendell Baker (Verizon Media)
72. Wendy Seltzer (W3C)
73. Lola Odelola (Samsung Internet)
74. Ashkan Soltani (Self)
75. Vittorio Bertocci (Auth0)
