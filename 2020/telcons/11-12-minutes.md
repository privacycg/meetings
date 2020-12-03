# Privacy CG Teleconference, 12 November

* [Agenda](https://github.com/privacycg/meetings/blob/master/2020/telcons/11-12-agenda.md)
* Chair: Tess
* Scribe: Ben Savage, Arthur Edelstein, Tanvi Yvas, Wendy Seltzer

## Attendees

1. Allan Spiegel
2. Alan Toner
3. Arnaud Blanchard
4. Andrew Knox (Facebook)
5. Andrew Pascoe (NextRoll)
6. Anne (he/him; Mozilla)
7. Anthony Molinaro (OpenX)
8. Aram Zucker-Scharff (The Washington Post)
9. Arthur Edelstein (Mozilla)
10. Ben Savage (Facebook)
11. Bennett Cyphers (he/him; EFF)
12. Bill Densmore (ITEGA.ORG)
13. Brian Campbell (Ping Identity)
14. Cezary Cerekwicki (Opera)
15. Chris Pedigo (DCN)
16. Christinr Runnegar (Privacy co-chair) (last 15 mins)
17. Christy Harris (Future of Privacy Forum)
18. Daniel Appelquist
19. Dave Harbage (DuckDuckGo)
20. David Benjamin (Google Chrome)
21. David Fischer (Read the Docs, Inc. / EthicalAds)
22. Devin Rousso (Apple)
23. Diarmuid Gill (Criteo)
24. Don Marti (CafeMedia)
25. Eric Mwobobia (ARTICLE19)
26. Ericka Wright (Intuit)
27. Erik Anderson (Microsoft)
28. Farah Zaman
29. George Fletcher (Verizon Media)
30. Henry Lau
31. James Hartig (Admiral)
32. James Rosewell (51Degrees)
33. Jason Nutter (Microsoft)
34. Jeddy
35. Jeff Wieland
36. Joel Meyer (OpenX)
37. Joel Odom (Salesforce)
38. Johann Hofmann (Mozilla)
39. John Sabella (PubMatic)
40. John Wilander (Apple WebKit)
41. Jordan Mitchell (IAB Tech Lab )
42. Josh Karlin (Google Chrome)
43. Kate Cheney (Apple)
44. Kaustubha Govind (Google Chrome)
45. Kristen Chapman
46. Liz Hurley (she/her: Epsilon/CJ Affiliate)
47. Mark Xue (Apple)
48. Marshall Vale (Google)
49. Marçal Serrate (Hybrid Theory)
50. Melanie Richards (Microsoft)
51. Michael Kleber (Google Chrome)
52. Michael Smith
53. Mike Lerra (Google Chrome)
54. Mike O’Neill (Baycloud)
55. Miriam Pena
56. Paul Bannister (CafeMedia)
57. Peter Dolanjski (DuckDuckGo)
58. Peter Snyder (Brave)
59. Pi
60. Przemyslaw
61. rbaudisc
62. Rowan Merewood (Google)
63. rstratto
64. Russ Hamilton (Google)
65. Ryan Avecilla
66. Sam Macbeth (Cliqz/Ghostery)
67. Sam Weiler (W3C/MIT)
68. Sean Harrison (Google)
69. Sebastian Zimmeck (Wesleyan University)
70. Shaun Gilmore
71. Steven Englehardt (Mozilla)
72. Steven Valdez (Google)
73. Tanvi Vyas (Mozilla)
74. Theodore Olsauskas-Warren (Google)
75. Theresa O’Connor (Apple)
76. Thomas
77. Tim Cappalli
78. Valentino Volonghi (NextRoll)
79. Viraj (Amazon)
80. Vittorio Bertocci (Auth0)
81. Wendell Baker (Verizon Media)
82. Wendy Seltzer (W3C)
83. Ashkan Soltani
84. Eli Grey (Transcend)
85. Edward Brawer (Utreon)
86. Thomas Müller (Utreon)

### Agenda

* F2F Survey Results
* [The IsLoggedIn API](https://github.com/privacycg/is-logged-in)
    * [Support for federated logins, or the ability to transfer IsLoggedIn](https://github.com/privacycg/is-logged-in/issues/35)
    * [Supporting display name and avoiding misuse of them](https://github.com/privacycg/is-logged-in/issues/36)
* [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    * [Define structure of JSON used for conversion reports](https://github.com/privacycg/private-click-measurement/issues/30)
    * [Send report to both click source and destination](https://github.com/privacycg/private-click-measurement/issues/53)
    * [Require description of the click content and if it is an ad, information on who purchased it](https://github.com/privacycg/private-click-measurement/issues/54)
* [First-Party Sets](https://github.com/privacycg/first-party-sets)
    * [First-Party Sets naming](https://github.com/privacycg/first-party-sets/issues/27)
* [Proposals](https://github.com/privacycg/proposals)
    * [Standardizing Do Not Sell](https://github.com/privacycg/proposals/issues/10)
* Any Other Business

## [The IsLoggedIn API](https://github.com/privacycg/is-logged-in)

### [Support for federated logins, or the ability to transfer IsLoggedIn](https://github.com/privacycg/is-logged-in/issues/35)

John: In original explainer. Not fully baked proposal. Had conversations about Federated Login. About “Sign in with X”. Wanted to see if it’s possible to provide (by spec) a transfer of power to set “isLoggedIn”. User has specified Identity Provider “A”. Would tell the browser: “Hey, I’m initiating a federated login”. That would give the Identity Provider a smooth way to set the “isLoggedIn” state. It’s a “Transfer of the power of setting isLoggedIn”.

Not tied to the particular thing written down there - but want to explore this. John is “pitching” this. Please chime in on GitHub with feedback.

George Fletcher: Yes, we need Federated Login Support. In this scenario - the browser needs to understand that the user went through a login mechanism. There are redirects one can inspect. Most protocols today are redirect based. E.g. SAML. Transfer of tokens happen backchannel (e.g. in OAuth). The other thing that was confusing in the explainer, one couldn’t transfer if they were not *already logged in* with the Identity Provider. We should handle that use case. The IDP may NOT want to log the user in - just to get authorization to get a code (for example). Let’s not assume we *always* necessarily want to set the “isLoggedIn” state.

John Wilander: I’m going back 2 years… as I recall what we were thinking when we wrote that. If we start the steps on the IDP website - then we don’t need to do something different for the “already logged in” vs “not already logged in” cases.

George Fletcher: Setting the power to set the “isLoggedIn” state, doesn’t mean you WANT to set them as logged in.

John Wilander: Noted. This use case is likely not covered yet.

Brian Campbell: When SAML is being used, the tokens are going through the browser via an automatic form post - initiated via JavaScript. There’s a huge number of cases where the Txn starts at the IDP. A click on the IDP results in a distribution of the token.

John Wilander: You’re right. It could just start at the IDP. If we make sure that the formal steps in the Txn start at the IDP, we can cater for that too.

Vittorio Bertocci: I want to highlight that federation is just one hop in these examples. In reality there is often more than one hop. CRM as an example. We need to support multiple steps.

John Wilander: I believe the idea we have outlined, that start on the chosen IDP - is covered. You’ve already gone through this intermediary step.

Vittorio Bertocci: As long as each step can set its own login. So that if you go back, you’re already signed in.

John Wilander: I might be missing something there. “isLoggedIn” is not the token itself. It doesn’t get in the way of how tokens are created. If you think something is missing please file an issue.

Aram Zucker-Scharff: John, have you looked at the Web-ID proposal from Google? The idea is to replace our mechanisms for federated login with something new. Might be more effective. Maybe trust tokens?

John Wilander: Yes, I have been following that interesting work. My view is that we only need *some way* to convince the browser that the user is logged in somewhere. There will be some cases where the browser doesn’t understand that the user is logged in, and we need a solution for that too. But Web-ID would likely fall into the category of “Browser Mediated” login solutions.

### [Supporting display name and avoiding misuse of them](https://github.com/privacycg/is-logged-in/issues/36)

Melanie Richards: Certain metadata might be shared along with setting the isLoggedIn state. One piece is a display name. User has a username, user has a display name that they might recognize better. The main value is helping the user understand their state somewhere. The problem with a display name which could be exposed in UI for the user, is it could be abused by putting a string that misleads the user as to their saftey. “Secure”, “Private” Logged Out, Anonymous. Is there a way we can safely display display name metadata? Something in the spec that is appropriate for user agents to follow. Typically we don’t standardize UI, so are there creative ideas for how we can mitigate this concern?

## Private Click Measurement

### [Define structure of JSON used for conversion reports](https://github.com/privacycg/private-click-measurement/issues/30)

John Wilander: PCM issue that may have had the most activity. Conversation with John and Charlie Harrison with Google. How to make this report as closely aligned with PBM and Google’s proposal. The existing implementation in webkit is URL based. What should go into the JSON? The tag has a general guidance saying, don’t be too specific in this if it’s not technically enforced. Deliberately avoiding any advertising language in this report. The browser doesn’t know if it was an ad or it was a conversion. Secondly, we are going to put a version number in the report to make it easy for the sites who receive the reports to understand that something changed. The pending change is the addition of fraud prevention tokens. Maybe we need to revise the crypto behind the fraud prevention tokens in a couple of years when the has algo is obsolete, or we add other data or support other kinds of attributes beside click thru. This work is quickly getting to finalized, and if you have opinions or feedback please share.

John: A lot of feedback around the name of the keys around the JSON structure. We are currently going with “source engagement type” and that will be “click”. “Source site” which will probably using WHATWG of scheme + eTLD+1. ID, “attributed on site”, “triggered data”, and finally “Version”. There have been discussions on googd names for devs and not talking about ads has been the discussion. Robin has been bringing up concerns on the version number. Shouldn’t that be obvious to the consumer of the report what it contains?

(ben, john, valentino add their own minutes:)

Ben Savage: Supportive of a version number.

John Wilander: Check the GitHub issue for alternative views.

Valentino: Are you looking to support alternative types of attribution - notice the “click” being specified.

John Wilander: It’s meant to be flexible and forward looking. PCM is about clicks (as the name implies). However, in principle others may implement other things.

Valentino: Is this meant to be the minimum set of fields necessary for support of PCM and then each browser can add more?

John Wilander: We haven’t discussed that yet, there are cases in which fields may be different.

Wendy: The standardization pathway -- what are you thinking about movement toward formal standards track? Is this something that you want to see, moving to a W3C working group? How can we help to advance the harmonization among interests?

John: That is a very good question. My interpretation has been that we need to get the formal proposal for the fraud prevention tokens up. We are working on that, but those things are not public yet. If Mozilla says “yes” that looks good to us, I think we have a path for requirements for moving ahead and that is when I would like to start that process.

Johann Hofmann: Another thing I think would be cool if we could get some clarity on PCM vs Google proposal situation. Maybe get some clarity on which moves to which proposal stage and move to just one eventually.

John: I do talk with Googlers about their proposal. They want to pursue their proposal, it’s in WICG. I asked if they do have multi-implementer interest for it. I don’t think they view it as a major problem, they want to pursue it and when it is fully baked. PCM got interest for Mozilla early on and that is how we are pursuing it. Our guidance is to make them as aligned as possible. There have been things we haven’t been able to aligh, such as number of conversion events…

Michael Kleber. IIUC from Google side, I believe the key unresolved skew between the

### [Send report to both click source and destination](https://github.com/privacycg/private-click-measurement/issues/53)

John: Send same report to both click source and destination. …?

(Not sure if Arthur lost connection to the doc)

Ben: Generally supportive of sending to both sites.  May be a smaller number of destination sites that receive and process those. Many no-op or return error if return that endpoint. Which should be fine.  Fraud prevention spec means it could come with signatures, so potentially even if only one party receives the events they can be validated by all

One question: I can imagine if you have 99% uptime, each side might miss a different set of events. This could lead to a different set of results. Is there a retry policy?  Might be a headache.  A retry policy could help.

John: interesting idea.  Ben can you file an issue on this?

Another potential fraud case  At conversion event, don’t know if pending ad click or not.  Now there is a signed value that could be attached to a fraudulent click by fraudulent site that is claiming attribution.  But if report goes to both, that could potentially be detected.

Ben: all come down to how it's signed.

Johann: Moz position: generally not opposed, but might need new review of the whole thing for privacy properties. See benefits to the Google approach of having a generic parameter for whom to send to, not just the site from which it came, since provider might set up CNAME anyhow. Tuple might not be the best. Not opposed

JohnW: ETLD+1, so CNAME shouldn’t be an issue

Johann: true, it’s not CNAME, but other schemes for forwarding might exist

JohnW: our thinking, if the browser sends the data to the first-parties involved, it’s clear to user that the responsibility is on the site. Even if they then have business deals that send the data elsewhere. Responsibility is on the site for sharing data in accord with user preferences.

Valentino: Resonate with what John said. Resonate with “no surprises”. However, I find it unlikely that each company rebuilds their own marketing stack. There’s going to be (from my POV) and intermediary in there. If that party doesn’t receive the same report, it’s going to be blind. I understand your goal is for these to be side agreements - but I’m not sure that’s more transparent. Why not instead just transparently list the ad-tech vendors who will also receive this report.

John: Yup, thank you.

Mike O’Neill: I made this point on the issue, but restating: If there are no intermediaries.. And you have a dispute… and you both receive an ID they can be matched… entropy limited… the issue of setting up a protocol to avoid payment disputes without relying on intermediaries…

John Wilander: You do bring up this “matching ID” idea. I’m skeptical. We generally DON’T want these two reports to be linkable. Even if it’s randomly generated in the browser. What we are trying to do is make the accounting line up, not help them link events between two different websites.

Aram: I’m generally in favor of the destination… best ad systems trust no parties, so sending to both seems most reliable. I am curious about the destination / landing-page. They often forward through multiple URLs. Is it the first? Last? How do we make sure that we aren’t leaking user data through that process.

John: We talk about pages, not sites in this spec. The way it’s written (and works in our implementation). The link the user clicks on needs to state the destination site. The browser needs to make sure that’s correct. There can be redirects, but the “onload” needs to be from that site. The conversion needs to happen on that same site.

Aram: The question then is: if the intermediary site is passed through, does it still get the data even if it is not the final landing page.

John: It’s where the user clicked, and where the user has converted.

Aram: I could chain events to track that.

John: This API doesn’t help you do that.

Marçal: The advertiser is paying - clearly they should receive the report. Question on this workflow: It seems is the publisher is the one placing the conversion pixel to the advertiser site, how it will scale then when the brand would potentially be advertised in thousands or millions of publishers

John: Perhaps this can be resolved in the modern JS approach.

Michael Kleber: From the Chrome POV, I’ll say that we still think it’s important to report to a 3rd party that is neither the publisher nor the advertiser. That’s in line with how the web works. E.g. browsers can load resources from 3rd parties. There is no novel privacy threat here. We are generally big fans of people being able to rely on 3rd parties. That’s the philosophical rationale. Putting that aside, I think that the right way to think about it here is, if Chrome is willing to allow the specification of other domains and Safari is NOT, how can we make that split decision as easy for developers as possible. Why not for the click / conversion events to specify the domains they should go do, and on Safari it’ll only go to a subset, on Chrome it can go to others as well. The report can also indicate the domains it was actually sent to. Let’s make it clear what the browsers did. It’s their right to have their own policies - just inform everyone.

John Wilander: Since we have no expressed that we want to support 3rd parties, I haven’t filed an issue. 3rd party reporting domains will be set up to shard the population, as an additional tracking mechanism. Ad-tech vendors can shard the population in any way they want.

Michael Kleber: Agree this should not be a mechanism for circumventing limits. Those domains must be present at ad click and ad conversion time. That way they cannot utilize this info in that way.

John Wilander: You’re saying you’d limit the number of conversion domains. Let’s go to GitHub.

(kleber says: maybe the relevant github issue is [https://github.com/privacycg/private-click-measurement/issues/48](https://github.com/privacycg/private-click-measurement/issues/48) ?)

## [First-Party Sets](https://github.com/privacycg/first-party-sets)

### [First-Party Sets naming](https://github.com/privacycg/first-party-sets/issues/27)

Mike Lerra: FPS may not be intuitive.  Accuracy of the terminology important.  Don raised in github issues.  Party may not be correct precise term.  We want the name to be understandable to the highest percentage of users possible.  Party may not be idea, first-party in particular may be less understood.  And finally, a good name could reinforce that this is a privacy boundary, but not indicative of any other boundary or grouping.  Given the limited time, I have a bunch of terms and ideas that I’ll throw in a response to the issue on github afterwards.  Other ideas or goals, please bring up now.  On things we should be optimizing on for.  Otherwise, can continue offline in issue thread with ideas and discussion.

Tess:  2 or 3 favorites?

Mike: noun / adjective approach.

*   Groups, sets, associations.  Boundaries as mentioned referencing privacy boundary.
*   What makes it up - site, entity, organization.  Something liek privacy may actually work too.
*   Mixing or matching of those terms.  Privacy groups, site groups, organization boundaries.
*   [Site/Entity/Organization/Affiliated/Data] [Groups/Walls/Limits/Spheres/Sets/Associations/Collections/Boundaries/Domains]

John: did originally call this “affiliated domains”

Johann: we’d like to veto the privacy term on whatever comes afterwards.  Privacy buzz word thrown around a lot, doesn't help user in understanding in what that group does.

## Deferred until next time

* [PCM: Require description of the click content and if it is an ad, information on who purchased it](https://github.com/privacycg/private-click-measurement/issues/54)
* [Proposal: Standardizing Do Not Sell](https://github.com/privacycg/proposals/issues/10)
