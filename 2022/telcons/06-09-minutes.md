# 9 June 2022 Privacy CG Meeting
* Chair: Tanvi Vyas
* Scribe: Michael Kleber
# Agenda
* Login Status API Integration with FedCM
* Update on Cookies Having Independent Partitioned State - CHIPS
* Any Other Business
   * TPAC

# Notes
## [Login Status API Integration with FedCM](https://github.com/privacycg/is-logged-in/issues/44)
* John Wilander: It's been a long time since we've talked about this.  Used to be called IsLoggedIn(), this is the new name, changing everything over.
   * Recap: It's been a big challenge for browsers to disentangle logins, and the tech used for them, from tracking.  And trackers like that — they can hide and make themselves look like logins.  Then we see other efforts, like FedCM, that is trying to solve for login under tracking prevention restrictions.  So there is definitely work to do here.
   * …if the browser can know where the user is logged in, it can help the user in many cases, e.g. recognize that the user is in a login flow, or keep a site's data for a long time since the user is logged in, or browser could change the language in a particular prompt if the user is logged in on a site, or could help a user who wants to clear much of  their browsing data by not clearing login-related state.
   * …Need a trustworthy signal, though, otherwise all trackers could just claim they are a login
   * …Maybe this is so closely related to the FedCM work that we should merge these, make the flag just a reflection of FedCM login status?  This was a work item, along with Melanie from MSFT though she has left, so Wilander is now the only editor.
   * …would like to hear what folks involved in FedCM have to say
* Sam Goto: Big +1 from the FedCM side — shares the conclusion that if the browser knows you're logged into a website, it can help the user for UI, usability, privacy, and more, once logged-in-ness is a first-class citizen.
   * So far it has been hard to disentangle the signal from the way it is used, to know it's being used correctly, not subject to abuse.  Much clearer in a FedCM world, where we know about the login flow.  Doesn't mean the two ideas need to be merged; they could be complementary — there should be many other ways to log into a website, eg passkeys.  So maybe we should think of this as a <something>manager API that understands all credentials.  
   * Would be best to have this in FedID CG that thinks about identity, with intersection between logins and privacy.
   * If you're looking for a co-editor, happy to talk.  Chrome hasn't yet found a particular answer to what we would do with the isLoggedIn signal, since the signal is separate from how it's used.
   * We build FedID CG to be narrow and small, but as we've evolved, FedCM has moved beyond just federation, but we can work with chairs to form bigger scope group to cover broader set of identity questions.
* Wilander: One way to get this going would be a new co-editor, would be great to be from Google to grease the skids,, also Apple isn't a member of FedID CG yet, so that's a sticking point unless we have another editor.  We've also had requests to change the prompts in the requestStorageAccess API to reflect this status, so we want it to exist.
* Sam: Counter-offer editorship of FedCM if you're interested in the further synergy.
* Wilander: I will try to find someone.  Joining FedID CG will really help, regret that it takes time for us to join
* Sam: Also happy to extend editorship offers to Moz or MS folks.
* Ben VanderSloot: Sounds good, we have some time coming up to talk offline
* Kris Chapman, co-chair of FedID CG: We would be happy to expand to cover logged-in efforts alongside FedCM.  Folks might feel like FedCM is The One Answer to *all* federated identity uses, and that is definitely not the case!  Still looking for more solutions to more use cases, and want more folks involved to cover additional use cases.  When it comes to identity federation, there are many ways to skin the cat and many different existing approaches, and we would like ways to baby-step towards an ideal scenario — can't expect everyone to implement a brand new solution, there is a lot of legacy scenarios, including outside industry (gov't, academic) to keep in mind
* Nick Doty: Maybe look more closely at that Credential Management piece — seems like the site can indicate in a very clear way that the user is logging in.  Q: Are the abuse use cases too much, so we're suggesting that this only makes sense when there is a mediated login?  Or can a site ever just say that the user is logged in and we will be confident it's true?
   * Wilander: We had a lot of feedback in this CG about that question.  Current status: mediated logins (PW manager builtin or extension, passkeys, webauthn, etc) should be seamless way to get that signal, probably conditioned on a server indicating it's happy with t he result.  But for the remaining cases, users or sites that don't want to rely on those technologies, there does need to be some kind of fallback, which could just degenerate to the dreaded "prompt land" — "This site says you have logged in, is that true?"
   * Nick: Simplest thing could be a method in the spec that you should call in the right conditions, e.g. with token and logout link and etc…
      * https://github.com/privacycg/is-logged-in/issues/52 
   * Sam: Does not always need to be mediated by the browser.  We ran into many cases where knowing would be useful while building FedCM, but the question is how to be confident in the signal rather than maybe being abuse.  RIght now FedCM has a pull model, so maybe can solve this via FedCM push, where the IDP tells the browser that the user is logged into the IDP?  Lots of good properties, but also hard in some ways.  Talking to Microsoft about this.  Also enterprise profiles could be supported.  I want to disentangle logged-in status from FedCM, clearly may other use cases, but the abuse vector really does force it to be somehow entangled with the login flow.
* Wilander: Re Kris's comment: Do note that Login Status API is trying to be across all of the ways to log in, not about any single tech flow, so should be open to lots of ideas
* Lola, Samsung Internet: Do you have an idea which companies are heavily affected by removal of 3p cookies in relation to federated identities?  Maybe not a list, but an idea of what companies?
   * Sam: Yes, there is a list, we have experts on OAuth and SAML who know what is going to break.  Chrome has compiled a list and so has the FedID CG, we're working to merge the two. Front Channel Logout often breaks.  Personalized buttons breaks too.  There is a list of things, severity, varies greatly.  Edu has much less resources than enterprise, for example.  Range of how tech-savvy, how critical the breakage is, what's the deployment technology.  I'll get you that list.  Lots of FedID people come to the Privacy CG is that we were blessed by breakage when 3p cookies went away, and there will be much bigger breakage in the future when bounce tracking protections to into effect — wake-up call on small breakage, time to prepare for much large breakages ahead.
* Ben VanderSloot: Second what Nick said about going through the credential manager.  One way through makes a lot of sense.  Some of the logic of isLoggedIn might be used in the construction of a credential.  One vague bikeshedding idea: attach it to the construction of a credential, now that annotated credential can be used in other places; related to the push-model idea.  Credentials currently origin-bound, so would need some relaxation there.  Indication of how proposals could evolve in a unified direction; interested in pursuing.
* Kris: When we think about federation, the idea that the IDP is a 3rd-party to the RP, requires something more like a spectrum — there is "first-party federation" and "third-party federation".  Maybe when a user logs in directly to a site, it might be a case of identity federation directly with first parties.
   * Wilander: I try to talk about that case using the phrase "Single Sign-On".
   * Kris: That's how I used to use it too!  We got into trouble when we tried to use that language, so now "first-party federation" vs "third-party federation".  yay language.
* Wilander: Thank you, I'll follow up with Sam and Ben, see if we can find a co-editor and see if another CG is a better home.


## [Update on Cookies Having Independent Partitioned State - CHIPS](https://github.com/privacycg/proposals/issues/30)
* Being adopted as a work item of Privacy CG — transferring repo, adding to charter. Dylan and Kaustubha are co-editors, looking for another one.
* Wilander: I've socialized it with our CF Network folks, who usually don't show up here, making sure they see it.
* Nick: Supportive.  Explainer in WICG repo could use some more generalization if it's going to be a standardized work item and is specific to Chrome, talking about "phasing out 3p cookies", so might want to update some of that language to make it aligned with formal cross-browser incubation.
* Lola: This is CHIPS by itself, not with First Party Sets (which has moved into a different CG)?
   * Tanvi: This is about CHIPS which sets information about whether a site wants their cookies to be partitioned or blocked when they are a third party.
   * Lola: CHIPS and FPS have a soft dependency on each other in the Privacy Sandbox POV.  Samsung is interested in CHIPS, not in FPS, so we want to be sure CHIPS isn't a back-door way to pull in FPS.
   * Erik: No dependency.  CHIPS is just about itself, not about FPS.
   * Johann Hofmann: There is mention of FPS in the explainer, but we're committed to designing the API so that everyone can implement CHIPS, we will ensure there is no requirement for any position on FPS.
* Dan Veditz: Mozilla's standards position lists FPS as Harmful, but I believe our position on CHIPS is that it's worth looking into.  We think CHIPS can be looked at without dragging in FPS.  CHIPS is about partitioning, and whether the partition is a site or an FPS is orthogonal to the question of whether you partition things.
## Any other business
* TPAC - Sept 12-16, 2022.  Hybrid - remote or in person.  In Vancouver.
* We have requested a 2 hour slot.  Requested earlier slot in Pacific Time, to be better for folks in Europe.  Also asked to avoid conflict with some other groups whose interests overlap
* We will share more info as we get it.
* https://www.w3.org/blog/news/archives/9503
   * Johann: Might make sense for a specific session about changes on 3rd-party cookies specifically?  Maybe a way to submit plenary ideas still.  Interest?
   * Tanvi: So this would be a separate session of whether there is interest in this topic?  There's CHIPS, there's the layering idea from our last meeting.  Unless we want to spend a lot of time on this in PrivCG specifically?
   * Tanvi: A separate session sounds worthwhile.  Chairs will reach out to Johann to coordinate.  We can also have one-offs or telecon time to discuss.  We will be spending more time on CHIPS since it's now a work item.  Action - Chairs to followup.
   * * Ben: Any idea when in the week?  A lot of people leave on Friday, so could be helpful to know
   * Tanvi: We didn't request any particular day.  Could update if needed.  Action - Chairs to followup.

-finished early, :45-


## Attendees (sign yourself in): 
1. Aditya Desai (Amazon)
2. Allan Spiegel
3. Alexandre Nderagakura (IAB Europe)
4. Andrew Aikens (TripleLift)
5. Anne van Kesteren (Mozilla)
6. Ben Kelly (Google)
7. Benjamin VanderSloot (Mozilla)
8. Brian May (dstillery)
9. Cameron Boozarjomehri (Mozilla)
10. Chris Needham (BBC)
11. Christian Biesinger (Google)
12. Claude Vervoort (Cengage Group)
13. Dan Veditz (Mozilla)
14. Daniel LaLiberte (Google Chrome)
15. Don Marti (CafeMedia)
16. Dylan Cutler (Google)
17. Eli Grey (Transcend)
18. Emily Lauber
19. Eric Mwobobia (ARTICLE19)
20. Ericka Wright
21. Erik Anderson (Microsoft Edge, co-chair)
22. Erik Taubeneck (Meta)
23. Frantz
24. Gabby Schnaid
25. Harneet
26. Jack Frankland
27. James Hartig (Admiral)
28. James Rosewell (51Degrees)
29. Jeegar Shah (CJ Affiliate) 
30. Jeremy Ney
31. Joey Trotz (Google Chrome)
32. Johann Hofmann (Google Chrome)
33. John Wilander (Apple WebKit)
34. Kaan Icer
35. Kris Chapman (Salesforce, FedID CG co-chair)
36. Lee Graber (Tableau / Salesforce)
37. Lola Odelola (Samsung Internet)
38. Marshall Vale (Google)
39. Matt Liu
40. Michael Kleber (Google Chrome
41. Mike O’neill (Baycloud)
42. Nick Doty (Center for Democracy & Technology)
43. Nicolas Pena Moreno (Google)
44. Paul Zuehlcke (Mozilla)
45. Peter Kotwicz (Google)
46. Polikseni Pojani (CJ Affiliate)
47. Pragya Seth (Arkose Labs)
48. Raj
49. Russell Stringham (Adobe)
50. Sam Goto (Google)
51. Sam Weiler (W3C/MIT)
52. Sean Harrison (Google)
53. Stefan Popoveniuc (Google Chrome)
54. Steven Valdez (Google Chrome)
55. Tanvi Vyas (Mozilla, co-chair)
56. Theresa O’Connor (Apple, co-chair)
57. Tim Huang (Mozilla)
58. Vinod Panicker (Amazon Ads)
59. Vittorio
60. Wendell Baker (Yahoo)
61. Yi Gu (Google Chrome)
(62 participants in the zoom call)

