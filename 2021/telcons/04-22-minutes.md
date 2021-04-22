Privacy CG Teleconference, 22 April
* Chair: Tanvi
* Scribe: Brendan Riordan-Butterworth, Peter Saint-Andre


[Private Click Measurement](https://github.com/privacycg/private-click-measurement)
* [Consider using blinded signatures for fraud prevention #41](https://github.com/privacycg/private-click-measurement/issues/41)
* John Willander: Updated issue on PCM.  Described WebKit.  Can’t test it in Safari Technology Preview, because it depends on Crypto lib that’s not bundled.  Wait a little bit.  If you’re interested in OS, you can look at the open-source repository to see how it works        .  Look at automated test case for this transaction.  You can read it all there.  Follows what was proposed in F2F from May.  Change to the flow diagram, pkey is retrieved before the token, because the token is generated in the browser.  
* JW: Some questions since posted yesterday, like ageing out or revoking key pairs, also a proposal to have unique signing key per twople (click source and destination) to confound fraudster that’s trying to get a bunch of keys.  Minimizes the reusability of keys.  This might make the scheme more complicated.  
* JW: I foresee we will continue iterating.  The above features will probably be iterations, if they make it.  It is still useful to fight fraud with the as-built, these are nice to have.
* Erik on the queue:
* Erik T: Another way to address the twople is to have the conversion event signed as well, since then the conversion event could be validated as well.  Makes the conversion harder to repro.
* JW: That is great news.  Click destination site also being able to sign a token.  Thought about having the merchant (or signing vendor).  Are you thinking of Click Source signing token at conversion event?  
* ET: Need to review, it’s been a while.  As long as the destination signs the same nonce, blinded in the browser for them, is still unlinkable, but transaction record is traceable.  
* JW: Probably can’t have the same nonce, as it would be discoverable.  
* ET: The nonce should never leave the browser, unblinded, right?  It’s blinded when it’s sent to the host.  The unlinkability is there, prevents collusion?  
* JW: It’s generated on the public key on the signing party…
* ET: We do propose a flow where both parties are signing, so you can’t commit fraud by arbitrarily clicking.  
* JW: Prefer that we bring stuff to the community group to make sure structure is in place for IP.  
* Chris Wood: Clarifying question.  What Eric said about nonce never leaving the device?  The thing that is being signed is the nonce?  Did I misunderstand? 
* ET: The nonce is blinded and then sent off device.  The blinded nonce is signed, and then the device can unblind it.  Later you can get the nonce and unblinded sig, and you can know the info, but it remains unlinkable.  
* CW: Makes sense.  The nonce leaves the device at the end.  
* JW: Telling everyone, CW is one of the authors of the draft specification of the crypto scheme we’re using for this, linked in the issue, part of the IETF.  Pairs with what we’re proposing! Two new parts here.  
* CW: If people are interested in seeing this progress, I would encourage people to support this at IETF.  Support would be highly useful in making this happen.  




[Storage Access API](https://github.com/privacycg/storage-access)

[Viewing media from different domains - cultural heritage interoperability #72](https://github.com/privacycg/storage-access/issues/72)
* Johann Hofmann: I think I added this one because I thought it would be useful for this group.  Feedback.  People who are trying to make things work, but failing.  Dunno if there’s anyone.  
* Tom Crane: Two of us on the call.  
* TC: Exploring Storage Access API as a way to handle.  The cultural heritage context.  Using a JS client to load images from other domains.  There’s an interaction pattern that allows 2 things: (1) can an user see an image on a different server, and (2), when the permission is granted, send along the necessary things.  
* TC: Removal of 3PC, we’re not sure what we can do to allow the use case to continue.  Looking at Storage Access API, whether we should see other specs.  I don’t really have a direct question.  Are we using Storage Access API in the right way, is there a simpler flow?  If we do get a nice flow working using observed browser behavior, is it stable?  What is the story in Chrome - will the same mechanisms be available in Chrome?  If not, what can we do?
* JH: A few things to address, from a meta discussion.  First observation, this is solvable with storage access API.  Theoretically, it would not be completely invalid, not a “won’t fix”, don’t have to solve at the spec level.  This seems like a valid scenario, but unsolvable without complicated steps.  
* JH: You have clear interaction with a first party before trying to get 3PC in other context.  
* JH: Pop-up heuristics.  Not covered by other proposal, passive access to 3PC in other requests outside of iframe. Do we consider this a won't-fix, or do we want to do something with this?
* JW: Thanks for bringing use case and questions?  As JH, Storage Access API unblocks, but is there better way?  With regards to the “stable thing” question, we’re 2 years in to a process to ensure best similarly between browsers.  Safari is moving to the Per-Page model, from the Per Frame model.  This changes how sub elements have access to storage.  So one iFrame can request, and all subresources get cookies.  It harmonizes Safari with FF, and maybe Edge?  
* JW: Back in 2018, we added the prompt in Safari,.  This was the only case where we made things more restricted.  Since then, it’s only gotten more relaxed, and it’s not changed a lot.  So the direction is “more things are working”.  We are committed to make this a viable path for embedded, authenticated content.  
* Michael (Isaac?): It seems like there are differences across browsers, APIs, cohorts, etc.  Some publisher issues are solved in some proposals, not in others, etc.  Is there a single source of truth or authority?  Or is everyone acting independently?  This query sounds specific to storage access API and Safari?  Is there a common place?  
* Tanvi Vyas: Not sure…  There’s effort ongoing for this work to bring convergence, but there’ 3 implementations of storage access API (edge, safari, firefox) at the moment that we are actively working to converge in this group to move to a standards body to standardize.
* JH: Storage Access API solves a lot of 3rd party use cases.  A bit of rearchitecting needed, but then it solves everything.  
* MI: Would Storage Access API allow individual user tracking with permission?  
* JH: Tech doesn’t tell you what to do with it.  But you could probably do it, and it’d probably annoy users, and exploitative use might get browsers to change.  
* Tom Crane: I think that Storage Access API does allow our flows, but have some stuff to work out.  The browser UI in Safari didn’t show, we can iron out later.  Wider question.  Our flow is quite complex.  Lots of elaborate cross-browser, knowing the facts.  Is there a simpler flow?  But I guess that’s on us to investigate further.  
* TC: Our use case (in the context of Chrome) is that these are not advertising/commercial.  We’re using 3rd party cookies for non-commercial, so will Chrome, in their elimination of 3PC, have Storage Access API.  
* John: You're right that there's a lot of movement in this space. What you should look for is what Tanvi mentioned: at least two implementations. This gives you an idea that this will go on to become a standard. If it's in a single browser, then you might have a more bifurcated developer experience. I'd also encourage you to look at tracking prevention policies, such as those put forward by Safari and Firefox. This will give you an idea of the broad picture. As to the use cases, I think it's easier if we boil things down to the technical aspects.
* TC: We did a produce a reduced implementation. Any comments or guidance would be appreciated.
* John: at-mention me there :-)
* Kris Chapman: I've brought this up before, about what to use to solve a particular use case. Ben Savage created an [ad-tech use cases doc](https://github.com/w3c/web-advertising/blob/main/support_for_advertising_use_cases.md) in the Web Adv Bus Group and something like that would be helpful here. Something that documents "for each browser, here's how to address the problem". There are so many changes going on that it would help if developers could know what path to go down.
* James Rosewell: It’s nice to hear from new groups like cultural heritage or Tableau last meeting. It’s good to get more views from the [second constituent on the web](https://dev.w3.org/html5/html-design-principles/#priority-of-constituencies), authors. As a CG this group doesn't create standards, AIUI it's not until we find WGs for these items that the consensus process kicks in. It's important to ensure that there's genuine consensus before we move forward. [See different types of group](https://www.w3.org/groups/). Moving to a working group to ensure consensus before mass deployed would be ideal.
* Tanvi: We have an upcoming face to face and we can take that up as a topic there.
* Aram Zucker-Scharff: There are a lot of proposals out there and it's hard to follow them all. I second Kris and say that the Web Adv Bus Group use cases document is very helpful. If you have more use cases, please add them there. W3C is a consensus making process, there will be multiple proposals. I've found that chiming in on different proposals can help move things forward.
   * https://github.com/w3c/web-advertising/blob/main/support_for_advertising_use_cases.md
* Kaustubha: As to questions for the Chrome team, we've also heard that people are curious about Chrome's stance as well. Many different uses of 3P cookies and we're thinking through all of those. [audio garbled] We hope to provide a cross-browser mapping and get to a state where there is uniformity across the implementations. Second, question about storage access API. I think we care most about the embed auth flow. Concern that the prompt might not be clear to the user. Does site just need to know who the user is or just that the user is authorized to access a resource? We're working to provide more insight into our thinking on this.
   * https://github.com/GoogleChromeLabs/privacy-sandbox-dev-support
* Tanvi: We do have the Slack channel for ongoing discussion (https://w3cping.slack.com/).




[Let embedees optionally request access to partitioned cookies and storage #75](https://github.com/privacycg/storage-access/issues/75)
* John: There's ongoing work about partitioned cookies. There's broad agreement on caches, IndexedDB, etc. Cookies are a different beast. Broadly used for authentication etc. WebKit shipped partitioned cookies several years ago. Developers found the partitioning to be confusing. Multiple logged in sessions from same browser, hard to log out the user, etc. We had some compat issues as well. Since then a lot has happened, more browsers are blocking cookies in various ways. Perhaps developers are ready and we're willing to try it again, but we want developers to opt in. Chrome team [suggestion to signal that in an HTTP header](https://github.com/DCtheTall/CHIPS). Another approach is to use the Storage Access API. Call a function, when promise resolves you get access to partitioned cookies.
* Daniel Buchner: What about IndexedDB, local storage, etc.? Question about access/partitioning with custom protocol handlers [Issue #22 - privacycg/storage-partitioning](https://github.com/privacycg/storage-partitioning/issues/22)
* Tanvi: Could you mark an existing issue as agenda+ for next time?
* Johann: All the options on the table seem pretty safe. Our biggest concern is the developer experience and further fracturing of the API. We don't want to produce more questions and gotchas for developers. Happy to confer on finding a coherent approach.
* John: To summarize, Firefox would prefer to have partitioned cookies by default, seems to be working on compat. Safari would like it to be something that developers opt into. There's past history here. In Firefox partitioned cookies would allow existing sites to continue accessing their cookies even if 3P. Safari didn't allow that. The differences might result in different compat issues across browsers.
* Kaustubha: Perhaps we should define a new cookie attribute. We did consider the Firefox approach. There's still potential for developers to face a mixed world of partitioned and non-partitioned cookies. I think it's useful to think abot it as a cookie attribute. The nice things about cookies is that you get it on the request header and you don't need to load a script. Ex: there are CDNs that handle things via cookies. We're also looking to improve the security model (e.g., samesite by default, http vs https, require host prefix, etc.). Encourage origin bound state.
* Michael: Is there a main repo for WebKit proposals?
* Tess: Short answer is no. :-) Usually create things in repositories for specific group like privacycg. We do have a [WebKit explainers repo](https://github.com/WebKit/explainers) to start initial discussion before we take it to a more official forum. But that doesn't have a comprehensive list either.                        


Any other business
* Storage Access API co-Editor update
   * Johann Hoffman will join as co-editor, yay!
* [Virtual F2F May 19-20, 2021](https://github.com/privacycg/meetings/tree/main/2021/05-virtual). Please agenda+f2f issues.  Call for topics.
   * Status of various work Items and where we are with each.  Which look like they are converging enough to move to standards bodies.
   * Heather: Also consider workshop related to WebID in WICG because I think there's info sharing need, May 25 + 26
   * Tess: Anyone can propose an issue by adding agenda+ f2f label; if there isn't an appropriate repo yet, you can use the admin repo under privacycg
   * John: We'll have a proposal for IsLoggedIn for the F2F
   * Erik: We can put together a proposal for the aforementioned blind signatures topic
   * Tanvi: next meeting May 13th


Administrative Questions:
* Q: How does one agenda+?  
* A: Add a label to the github issue.  If you do not have permission to do that:
   * Make sure you have officially joined the group and are a participant - https://www.w3.org/community/privacycg/participants
   * Only after you have joined, email the chairs (group-privacycg-chairs@w3.org) with your github user id to be added as a contributor.

* Q: Daniel Buchner - am I going to have time to ask about the IndexedDB impact of storage access/partitioning? → Question about access/partitioning with custom protocol handlers (Issue #22 - privacycg/storage-partitioning)
* A: Please mark it agenda+ for next time.

* Q: Do we need invites for the slack?
* A: Yes, contact the group chairs to request invites at group-privacycg-chairs@w3.org.  You must be a member of the Privacy CG group - https://www.w3.org/community/privacycg/participants first.



Attendees (sign yourself in, alphabetical order please):        


1. Allan Spiegel (Adobe)
2. Andrew Knox (Facebook)
3. Anthony Molinaro
4. Aram Zucker-Scharff (The Washington Post)
5. Arthur Edelstein (Mozilla)
6. Ben Savage (Facebook)
7. Benjamin Case (Facebook)
8. Bill Densmore (ITEGA.ORG)
9. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
10. Brian May (dstillery)
11. Chris Needham (BBC)
12. Chris Wood (Cloudflare)
13. Christine Runnegar (PING co-chair, second half of meeting)
14. Craig Spiezle
15. Daniel Buchner
16. Daniel Buchner (Microsoft)
17. David Reese
18. Devin Rousso (Apple)
19. Dylan Cutler
20. Eric Mwobobia (ARTICLE19) 
21. Ericka Wright
22. Erik Anderson (Microsoft)
23. Erik Taubeneck (Facebook)
24. Gene Radin (Socure)
25. Heather Flanagan (Spherical Cow Consulting)
26. Hirsch Singhal (Microsoft Identity)
27. Hitesh Lad (CJ Affiliate)
28. Jack Frankland
29. Jade
30. James Hartig (Admiral)
31. James Rosewell (51Degrees)
32. Jarjin Kruisselbrink
33. Jason Nutter (Microsoft)
34. Jeffrey Yasskin (Google)
35. Joel Odom (Salesforce)
36. Joey Salazar
37. Johann Hofmann (Mozilla)
38. John Wilander (Apple WebKit)
39. John-Marcus Phillips
40. Jun Kokatsu (Microsoft)
41. Kaustubha Govind (Google)
42. Kristen Chapman (Salesforce)
43. Laszlo Gombos (Samsung)
44. Les Cochrane (Audigent)
45. Liz Hurley (CJ Affiliate)
46. Lola Odelola (Samsung)
47. Mallory Knodel (CDT)
48. Mark Xue (Apple)
49. Micahel Smith
50. Michael Kleber (Chrome)
51. Michael Lysak (Carbon)
52. Mike O’Neill (Baycloud)
53. Naima Conton
54. Nathan Schloss (Facebook)
55. Nelson Brodyk (Dotdash)
56. Paul Bannister (CafeMedia)
57. Peter Saint-Andre (Mozilla) 
58. Raj
59. Robert Sanderson (Yale University (for #72))
60. Russell Stringham (Adobe)
61. Sam Goto (Google/Chrome)
62. Sam Weiler (W3C/MIT)
63. Sean Harrison (Google)
64. Shaun Gilmore
65. Shigeki Ohtsu (Yahoo! JAPAN)
66. Stefano Cossu (Getty, for #72)
67. Steven Englehardt (Mozilla)
68. Tanvi Vyas (Mozilla)
69. Theodore Olsauskas-Warren (Google)
70. Theresa O’Connor (Apple)
71. Tim Cappalli (Microsoft Identity)
72. Tim Reid (SpotX)
73. Tom Crane (Digirati (for #72))
74. Valentino Volonghi (NextRoll)
75. Viraj Awati (Amazon)
76. Wendell Baker (Verizon Media)
77. Wendy Seltzer (W3C)
78. John-Marcus Phillips (Nielsen)
