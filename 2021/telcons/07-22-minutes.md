# Privacy CG Telecon, 22 July 2021
* Chair: Erik Anderson
* Scribe: Kaustubha Govind (first 30 minutes), Nick Doty (last 30)

# Agenda
## [Ad Topic Hints proposal](https://github.com/privacycg/proposals/issues/26)
* Ben Savage: Is it appropriate to move this out of a long chain of comments into concrete proposals that I can edit? Comments have been significantly modified. I think we’re ready for its own folder structure with issues tracker.
* Erik: Yes, we can open a repo to help with this discussion
* Ben: To reach out to Erik out-of-band
* Ben: Plan to go deep on the DIfferential Privacy idea. Made a visualization to explain. The simple basic idea to anchor with is: we can take ads and transform them into vectors, where the vectors are topics. You can imagine a sphere, where regions/blobs correspond to topics. e.g. Ads for alcoholic beverages, political ads, fridge magnets. \
 \
People have preferences (exclusion: alcoholics don’t want to see ads for alcohol; someone who suffered a loss don’t want to see ads about kids, inclusion: like to see ads about travel). \
 \
I want you to imagine that someone has provided feedback about a couple of ads. \
 \
Feedback on the proposal was not to derive behavioral preferences, but only be based on user preferences. Two lists: List of ads with relevant topics; not relevant topics. \
 \
Browser would store the embedding vectors of the ads that the user did and didn’t want. \
 \
In reality this will be a higher dimensional space, but viz is 3-D.
 \
    [Presenting code]
 \
Imagine two vectors with likes/dislikes
 \
[Presenting visualization] - example [here](https://github.com/privacycg/proposals/issues/26#issuecomment-883219080)
 \
Blobs of points corresponding to areas where the user said it is a relevant topic. Areas where there are no dots or low concentration where the user doesn’t want ads. Some way for users to say “more over this area”, “less over this area”. \
 \
Fingerprintability risk? If you can 500 times in a row, get 500 different topics from the browser; you might be able to infer information about the user. But if we limit the number of ad topics (e.g. per session), that could reduce the risk.
 \
 Also added random Laplacian noise to the data. E.g. 20% of the time, I pick a random ad topic. It would be a negative user experience for a user to always see irrelevant ads. Not sure what the threshold is; need to find a balance. E.g. I like to see random ads about 50% of the time.  \
 \
 Another thing to put in is dispersion (how much random noise to add). ???? might increase re-identification risk. On the other extreme, it could be all washed out and be just random noise, at which point the user preferences become meaningless.

* John Wilander: Three questions: (a) Already touched upon the fact that users won’t make a choice. Have you considered safe areas on the sphere that could be the default if the user hasn’t expressed choice. Or could that make them identifiable? But again, we need to give user agency on asking their preferences. (b) The big risk is being able to tie a user prefs to an existing ID. Over time, you can read user prefs and tie it to their profile. You can hold on to this data virtually forever. You can figure things out about people. This reminds me of Google;s TURTLEDOVE’s on-device auction. Where the signal to kep on-device. Any interest there? © We always think about partitioning storage. Could prefs be partitioned per first-party, which would not carry the risk of carrying this info cross-site.
* Ben: (a) Yes, default should be completely anonymous. You are going to get random points, a sphere will points drawn on the surface in a uniform distribution. There will be no data revealed in this, pure randomness.
* John: But would we want to denote safe areas? Ie. safe for all users.
* Ben: Since these are only topics, not sure what “safe” means. What does unsafe area mean?
* John: Legislation. You need to know something about the user before showing alcohol ads.
* Ben: Perhaps there are sensitive topics that you need to opt-out of. This proposal takes a uniform approach, but you could have some user agents take a more paternalistic (opinionated) stance, while others might take a uniform approach. UAs might need to do some differential privacy  / privacy budget analysis to decide when to limit the number of topics. How would you differentiate when the user has provided some preferences? Not sure how to deal with that. I think the only way might be to be random, and have opt-outs, We’d have to decide if that’s an okay experience.
* John: Recommends having a section on “lying intelligently” and not just going completely random. Combine safe area, and exposing a signal so that you can’t determine whether the user expressed a preference or not.
* Ben: That’s a controversial idea. If the user has 10 topics, would you prevent that from being sent …???
* Brian May : Have concerns about ad effectiveness, … If you’ve got a highly appealing image (e.g. beach), advertisers may call that a “Travel” ad just to qualify for that topic. Even if you could do that for a meaningful subset of the population, the model… might look for keywords (e.g. product mentions like brand names tied to type). Aspirations are laudable, but vehicle is wrong. We need to be careful when we tell users what we will and won’t do in advertising. Unless everyone fully agrees what rules we’re going to have in place. E.g. agreement about not showing alcohol ads to alcoholics, may not be honored by all advertisers.
* Ben: learn empirically to see if ads do see similar embedding vectors
* Brian: but if people categorize ads differently from the ML …
* Ben: if users are giving feedback about an ad because it’s part of Category X, they need to know that it’s actually Category X
* Brian: so many signals in a creative ad, people have inconsistent inferences about its meaning/category
* Ben: if a header provides an ad vector, there’s no way to be sure that the ad network would even use it. Browser shouldn’t oversell what happened with expressed preferences. Could ask IAB Tech Lab or some other venue whether advertisers would use it.
* Brian: ad tech generally has been trying to do this for a long time, if we could come up with good notions of the topic of an ad … still going to be players who want to provide their own new topics rather than relying on the browser
* Ben: ad tech has to been trying to infer interested topics based on browsing behavior. Would a signal related to a stated preference be more effective in some way? Advertisers are likely to try to use both. But if it gets harder to collect cross-site behavioral data, then expressed preferences will become more valuable.
* Christine Runnegar: thanks for a clear explanation! +1 to wilander. How to reduce the risk of fingerprinting when the user chooses the granularity of randomness?
* Ben: to prevent use as fingerprinting vector, need formal differential privacy analysis. Browsers should provide sensible defaults and limits, on levels of randomness / dispersion.
* Aram Zucker-Scharff: 1) how to handle sensitive topics -- legal situations, in the US and elsewhere, about not advertising certain things to children under 13. Worth thinking through for this spec. 2) Helping users to understand what they are opting into, and why they are getting a particular ad -- not whether the user thinks it’s about beaches, but that this ad was shown to them because of the ad topic interest of beaches. Enable more purposeful lying -- Mozilla’s TrackThis experiment let users try out a cookie of someone else. Algorithmic red-lining of job offers might not be shown. Users choosing different ad profiles. Could extend data portability to let people interact with other people’s profiles or intentionally selected profiles of a certain kind.
* Ben: data portability would help with doing research on a topic, so a journalist could see more of the political ads, for example, and be able to export and share with others. Don’t expect most users would set up different profiles, but could certainly be configured by the browser. Regarding sensitive topics, this shouldn’t be the system to manage legal compliance with advertising regulation (re minors, for example). But could at least have a common set of plausibly-sensitive topics. \
 \
Really like the idea of being able to tell people why they see an ad. If the algorithm was as simple as my example, could imagine browser showing UI about what ads the user indicated interest in in the past, or explain the randomness.
* Kris Chapman: too simplistic? Choice of an audience is more complex than just a single topic. More context about a user. Segmentation and audience targeting includes content on the page, context of where the user is, etc. Needs a more layered and complicated approach in order to do this well.
* Johann Hofmann: explainer needs a paragraph of what differential privacy guarantee is being provided by adding noise. Would go further to say that data points don’t reveal the user’s choice of topic, or provide sufficient plausible deniability about my interest in that topic. What is being achieved by the noise?
* Ben: goal is to make it not useful for cross-site tracking. If the user is relating their non-random preference, then over the long term, we expect that some revelation of preference will happen. This seems okay if the user understands that they are stating a preference and we will relay it on to websites. I think it would be okay if Facebook learned that I’m interested in travel ads.
* Johann: I would be fine if it took a year, but concerned that it would be revealed immediately.
* Erik: partitioning, user controls, application to different sites.
* Sam Weiler: meaning of the sphere/graph?
* Ben: cosine similarity as the way to compare vectors between users. API would return a vector in multi-dimensional space on the surface of that multidimensional sphere.
* Tanvi Vyas: vectors could be saved by websites and combined with PII? Sites already gather lots of PII. what if ads were targeted just based on that ad topic vector? How could that work?
* Ben: we should presume that sites are going to amass multiple vectors from the user over time. Alternative would be blind rendering of ads (per wilander suggestion), but that’s a big lift. I think the risk of revealing expressed preferences isn’t so large that we need blindness.
* Tanvi: not just the first party that would save the vectors.
* Ben: yes, should assume it’s being disclosed to a whole bunch of third parties.
* Michael Kleber: for the proposal where you can join data with first-party data. Feedback from floc origin trial: 1) adding randomness to a system may not help users feel better about their privacy. 2) for user understandability, hard to explain a random vector that’s being sent out. Promising, but not sure how a user can understand what the hint means.


## Announcements
Join the slack!

### [Federated ID Community Group](https://www.w3.org/community/fed-id/)
* 65 participants so far
* Heather Flanagan: Have first call scheduled for . Invitation was sent to the internal list. Will re-send in a week or so. Agenda for first-call will be admin: chairs, charter, WebID (potentially be broken into pieces). Will likely be a bi-weekly call. Not sure if these will be split up to be Asia-Pacific, and Europe/Americas friendly; will depend on participant list.

### [First Party Sets Ad Hoc Meeting](https://github.com/privacycg/meetings/issues/14)
* Thursday, August 12th
* 2pm PT, 5pm ET

# Attendees:
1. Aditya Desai (Google)
2. Allan Spiegel
3. Andrew Knox (Facebook)
4. Angela Ballard (CJ Affiliate)
5. Anne van Kesteren (Mozilla)
6. Anthony Molinaro
7. Aram Zucker-Scharff (The Washington Post / Team Room F)
8. Ashkan Soltani
9. Ben Savage (Facebook)
10. Bill Densmore (ITEGA.ORG)
11. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
12. Brian May (dstillery)
13. Cezary Cerekwicki
14. Chris Needham (BBC)
15. Christine Runnegar (PING co-chair)
16. Daniel Appelquist (Samsung, TAG)
17. Eli Grey (Transcend)
18. Erik Anderson (Microsoft)
19. Erik Taubeneck (Facebook)
20. Frederic Jacobs (Apple)
21. Grant Nelson
22. Harneet Sidhana
23. Heather Flanagan (Spherical Cow Consulting)
24. Ian Meyers
25. James Rosewell (51Degrees)
26. Jason
27. Jeffrey Yasskin (Google)
28. Jessica Berman
29. Joey Salazar
30. Johann Hofmann (Mozilla)
31. John Sabella (PubMatic)
32. John Wilander (Apple WebKit)
33. Jun Kokatsu
34. Kate Cheney
35. Kaustubha Govind (Google Chrome)
36. Kelda Anderson
37. Kristen Chapman (Salesforce)
38. Mark Xue (Apple)
39. Marshall Vale (Google Chrome)
40. Melanie Richards (Microsoft)
41. Michael Kleber (Google Chrome)
42. Mike Smith
43. Newton (Magnite)
44. Nick Doty
45. Pedro Alvarado
46. Peter Saint-Andre (Mozilla)
47. Raj Belur (Amazon)
48. Richard Anton (Amazon)
49. Rowan Merewood (Google Chrome)
50. Russ Hamilton (Google Chrome)
51. Russell Stringham (Adobe)
52. Sam Goto (Google)
53. Sam Macbeth (DuckDuckGo)
54. Sam Weiler (W3C/MIT)
55. Sean Harrison (Google)
56. Tanvi Vyas (co-chair, Mozilla)
57. Theresa O’Connor (co-chair, Apple)
58. Tim Cappalli
59. Viraj Awati
60. Vittorio Bertocci (Auth0)
61. Wendell Baker (Verizon Media)
62. Wendy Seltzer (W3C)
