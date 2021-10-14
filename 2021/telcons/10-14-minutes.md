# Privacy CG Teleconference
* Chair: Erik
* Scribes: Michael Kleber, Kaustubha Govind


# Agenda
* [Navigation-based Tracking Mitigations](https://github.com/privacycg/nav-tracking-mitigations)
    * [Is "wbraid" tracking? #11](https://github.com/privacycg/nav-tracking-mitigations/issues/11)
    * [Define navigational tracking as "without the user's knowledge"? #8](https://github.com/privacycg/nav-tracking-mitigations/issues/8) and [Is using the OAuth front channel navigation tracking? #16](https://github.com/privacycg/nav-tracking-mitigations/issues/16)
    * [Define our threat model #12](https://github.com/privacycg/nav-tracking-mitigations/issues/12)
    * [Communicating people’s choices to all parties #13](https://github.com/privacycg/nav-tracking-mitigations/issues/13)
* [W3C TPAC 2021](https://www.w3.org/2021/10/TPAC/) reminder
    * [Breakout sessions](https://web-eur.cvent.com/event/2b77fe3d-2536-467d-b71b-969b2e6419b5/websitePage:efc4b117-4ea4-4be5-97b4-c521ce3a06db) next week
* Any other business


# Navigation-based Tracking Mitigations
(Was discussed at the APAC-friendlier-time previous meeting, but coming back to it in this time zone)

## Is "wbraid" tracking? #11
* Erik Taubeneck: I'm here instead of btsavage who's moved to Singapore.
    * Key idea is how to define what "tracking" actually is, or what trust model we want to use
    * If a company declares that a certain value is used only for aggregates, but that's not mitigated by the browser (proven to the browser), then what can we do about it?
    * Back-and-forth on issue itself about what this might imply.
* Brian Lefler: Agree that we need to figure out what to do about opaque high-entropy identifiers.  User ID is highest risk, ad campaign ID lower risk, but there's a wide range.
    * Public declarations do factor into Brave's actions, and clearly are somewhat relevant.  Wide spectrum
* John Wilander: We (Apple) prefer technical restrictions — it should not be possible to track users — we don't think it's manageable to handle public statements or policies on a web-wide global scale.  We haven't seen anyone be able to enforce a policy globally.
* Heather Flanagan: With the FedID hat on, we're trying to be clear on what is tracking, comforting that PrivacyCG is sharing the pain.
* Mike O'Neill: Simplest way to think about tracking from the EU perspective is whether it enables the processing of personal data — data that can be identified with a human being.  If it's information about a person, it needs to be restricted in some way.  Hard for a piece of software to determine whether something is used for personal data.  It's an insoluble problem.  Can you constrain an item of storage so that it cannot be used for tracking?  Only way to do that is to restrict the types of storage and how long it's stored for.  Restrict duration, probably OK?  Could be used for complicated-to-understand reasons, so concentrate on things we have a hope of understanding.
* Erik T: Agree with Mike that this reduces to a fairly intractable problem.  There is concern over browser storage, but the main issue is in the URL.  Unless we limit the entropy in a URL, we end up not able to differentiate between item number on a website with a vast collection of items, versus string that could identify a user.  No good way to differentiate those at web scale.  That's the key problem
* Jeffrey Yasskin: Difficulty of distinguishing in an algorithmic way is laudable, but seems difficult without capturing a giant swath of other &lt;lost some audio> benign parameters.  If it's not based on a set of tracking domains, storage is just unreliable on the web.
* Aram Z-S: I wonder whether we could define it in reverse, is this of sufficient complexity?  If a browser were to intervene against things that appeared to be personally identifiable, how good is that?  If there is enough data to store personally identifiable information, take action.  Might be a way to discuss whether something is individual user tracking by design or not.
* Kleber: going back to John Wilander about Apple preferring technical restrictions and not seeing a web-wide-scale way to enforce policy. I would like to point out that on issue #11 is about the wbraid parameter. Ben Savage linked to some info about it which explicitly mentions that it’s used for navigation from an app to a web page. So, it’s a navigation that starts on the app side of the app/web divide. Of course, taking action on policy in the app space is the area where Apple has invested huge amounts of effort recently. I don’t know how important the app/web distinction is but it may be relevant.
* James Rosewell: Re. Mike's observation: I'm not aware of a requirement that information about a person needs to be restricted in some way.  I think the law has restrictions about purpose and use, but not that it must be restricted.
    * Mike: I was referring to limits on personal data processing in ePrivacy directive and GDPR, anything that is personal data you need a legal basis for processing, any data stored in the browser subject to ePrivacy
* Brian May: I think it's very difficult to solve problem technically without putting significant damage to things that are entirely appropriate and necessary uses of the web.  Maybe we need to make a well-lit path for how to do things in ways that are private, encourage people to do those.
* John Wilander: I agree this is hard, but I don't think it's unsolvable.  We've seen innovation in this space, incl. Safari's distinguishing script-writeable from server-writeable storage.  We've invested in click measurement with our API.  Both a technical means and very explicit on amount of data that can go across to websites as part of click tracking.  Once we have a well-lit path provided, we can go back to innovating on technical restrictions on navigational tracking.
* Brian Lefler: Limiting entropy is worrying, because there are a lot of security techniques that rely on entropy, eg CSRF tokens.  
* Kris Chapman: Tracking is no about collecting personally identifiable information, it's about collecting against the user's consent.  My concern is that so much is focused on problems from advertising.  From a Federated Identity perspective, there are so many use cases that have nothing to do with advertising that our focus should be on how to properly get consent for those types of situations, rather than trying to block them altogether just because advertising can abuse it.
* Tim Cappalli: Speaking as me, not as FedID: Idea of collecting — browser is passive, not active, party in many flows today.  We shouldn't be comfortable with the browser becoming active.  If browser intermediates identity flows, we have a new active party, that's a critical change to everything, user might not consent to the browser collecting information, may be very confused by that change.


## Define navigational tracking as "without the user's knowledge"? #8 and Is using the OAuth front channel navigation tracking? #16

Jeffrey: This was my and Martin Thomson's issue, related to Federated Identity case, where the user actively wants to transfer some information about their identity between two sites.  I'd like to have a term in the doc about what we *want* to block, which the browser is trying to push for.  Somehow want to carve out "things the user is trying to do" from what we define as bad.  This issue proposes saying that it must be "without the user's knowledge/intent" to be nav tracking, or maybe we should make up a new term.

Johann Hofmann: Similar to how we can identify aggregate measurements discussed previously.  I think it's hard or not possible to encode user intent.  OAuth *should* be categorized as navigational tracking — so what is our goal?  I'm not sure, but what I want to implement in the browser is something opinionated.  Define Nav Tracking in some neutral, client-observable manner, without passing judgement.  Then from there we can ask what things we want to explicitly enable, like OAuth pattern, and what are some patterns that we see as harmful, that we can practically act against.  Not part of spec, maybe, but something browser vendors can take.  Hard to fully prevent categorically, take smaller steps from there.

Erik Anderson: Reminder: there's side-channel #privacycg slack where folks are chatting

Brian May: Re. non-advertising use case — there are cases where I want someone to track me and cases where I don't.  Want my bank to know I'm the one withdrawing the money.  I do/don't want someone to know who I am.  I don't, as a user, have a means of knowing what a site is doing.  Want clean well-lit path so that we can keep the user informed of what's happening, empower users to make well-informed decisions.  Preventing people from doing things is between the browser and web site owner, we should raise the user up and make them part of the conversation.


## Define our threat model #12

Jeffrey: What do we want to do here? What are our goals? What kinds of trackers are we trying to address? What capabilities do they have? Server vs. client-side? What do we think is possible/impossible to accomplish. Want the group to think about this question.

James Rosewell: The threat is harm. What harm is being done? If I provide my information for the purposes of purchasing insurance and receive higher quotes, then that is the harm. Threat is not a theoretical/fringe case; but something that harms a large number of people in a large way. Threats that harm a small number of people or represent a small harm should be used to justify proposals.

Joel Odom: We need to consider the various tradeoffs, as in security and privacy. There seems to be consensus around large-scale tracking being creepy. I think proactive …. And expanding to first-party sets may be reasonable. I’m concerned about as we move towards models that increase user privacy, we are also getting draconian around restricting entropy. We are really starting to shackle our hands around the future of the web. We need to carefully consider that if we really gate everything for particular usecases, we have less ability to innovate.

John Wilander: A good starting point is to think of the browser as the user agent. On the user’s side, helping/protecting them. In tracking, when the question of server vs. client-side, my opinion is that the browser should do what it can, but it’s not the browser’s responsibility to tackle server-side tracking. But browser should not be complicit.

Jeffrey: I think we should focus on details. People are saying that tracking is bad … the correlation of user identity across sites may be one way to look at it. Do we only want to prevent at-scale stuff, or also identity being via navigational tracking. If information needs server-side work like use of path segments to convey user identity; should we not worry about that; and only focus on query parameters? (CNAME..)

Brian Lefler: I would propose that a useful threat model is an actor that receives across many sites. Ongoing, continuous surveillance is the threat. Within that, those actors have 3p presence, reasonable to expect that they have scripting presence. They don’t need the collaboration of the top-level site, or server-side control if they have scripting presence. 

Kris Chapman: I think the idea that John mentioned about user agents doing what it can… I think that is a disservice to users. If tracking just moves to servers, then it becomes less transparent to users. Browsers should educate users about the risk. Wouldn’t say responsibility, but should avoid creating a false sense of security. I would personally prefer education than trying to block things in the client.

Eric Rescorla: I think part of what’s happening is that we’re getting mixed up between the thread model / what we’re trying to accomplish, and the things that are possible to do with those capabilities. I think Brian did a pretty good job of articulating the problem we’re trying to solve (large-scale tracking). Given the capabilities that we know servers already have, is that a practical objective? If the answer is no, is there still a reduced threat model that we can solve.

Erik Taubeneck: Want to quickly suggest a potential attack. Suppose you have a site that wants to send a user identity to store.example - it would be possible to encrypt the user into a blob. Cutting the blob would break site functionality, but if you allow it, it allows the site to decrypt user identity. I worry about a race to the bottom where it either breaks user experience, or allows tracking. Those are the things we need to think about. These are valid goals and we should be trying to figure out what the UA can do. I agree with John’s point; but to Erik’s point, if we say that we can’t do anything; we need to think about if what we do makes things better, or more transparent.


## Communicating people’s choices to all parties #13
James Rosewell: Been looking at this from the angle of harms. Identify harms being done to user. Web browser is a gateway to the internet, but also looking beyond that. In SWAN.community, we identify harms via transparency and audit. So users can see the l… ML can be used to identify harms. Instead of finding an enforcer. Proposal adds 3 pieces of metadata: party that collected/created data, time that it was created, contract that the user was created under; and makes it available so that any party can be cryptographically verified. Discussion of good vs. bad tells us that we need either some policy/law, or technology. I disagree with people who think that the only solution is technology. The issue talks about what happens if that information is provided on navigational tracking. We have a way of identifying this type of data, well-known endpoints for domains sending/receiving. Contracts that the domain respects. Components of privacy policy, such as standard contractual… used in various jurisdictions. If there is commonality in the data, it will have more information to go on, and therefore that presents other possibilities. If the browser sees a contract for the first time, it might prompt the user to confirm understanding. A contract might be pre-approved e.g. by the ICO. So on a legal basis, we can …. Browser can also aggregate the data. Does that help us address the problem of good and bad that we come across reasonably often in these discussions. Many regulators, there is no one single solution works; the UA becomes the gatekeeper/auditor, and community can be the auditor; and the UA can decide/confirm which contract the user is agreeing to or not. My question is - Jeffrey, have I provided answers to your questions?

Jeffrey Yasskin: I think this is plausibly something this group could be interested in, my question is whether we are. If the author of the query/URL component can say what they’re using it for, it comes to some of the policy questions we’ve been asking about opaque pieces of data. John suggested we stick to algorithm based approaches

Eric Rescorla: I’m skeptical that the user can engage with any contract. We saw how it went with P3P. It’s extremely hard for the user to make any sense of what these assertions mean. Maybe we can come up w/ a quasi-technical mechanisms that the browser can assist to surface legitimate vs. non-legitimate uses; but not practical.

James: I’m not sure I understand that. When I install Firefox, I accept the terms. That’s the basis of our laws. 

Eric Rescorla: The notion that users read agreements is wrong. It’s not practical to expect that users will; read for every possible tracker.

James: Proposal is that consent is per contract. People in Europe might know about the consent on every website. What we’re proposing is that people would consent to a standard contract for the transfer of data. If the problem is that people have difficulty understanding the contract,that is outside the scope of the w3c.

Aram Zucker-Sharff: Seen this proposal before, but may know all particulars. Two big challenges: the size of the situation. The example you gave is nice and clean in the issue, but in reality, there is an excess of 8000 adtech providers. I think you said to limit to 100, but even that is a lot to expect that the user should be expected to read and understand 100 agreements to see an ad. The other big q, you put in the idea of model terms, types of legal agreements that a user would see. An example is that every single user should agree to that set of model agreements, and that.. … and that they are willing to agree to them. IAB released TCF, we see that a lot of major operators simply did not agree to the binding terms. SImilarly, for CCPA, we saw an example. Not meant to call out specific operators; Magnite disagrees with…. I’m not a lawyer,  but this is a big challenge. If we can’t get buy-in for LSPA/CCPA; or for TCFv2 which defines a methodology of processing; it seems like this would have greater challenges. I’m not sure if you’ve talked to adtech providers; but would be interested in what folks who don’t agree to TCF2 or CCPA would say about this.

John Wilander: Wanted to respond to Jeffrey’s response about going with a programming approach. This is a particularly hard one, but looking at the infra we have available, or creating infrastructure. E.g. we could look at URLs and think about whether navigational tracking could go in (e.g. eTLD). Where does the link originate. Enumerating badness/goodness (blocklist/allowlist). We’ve done work in script writable vs. readable vs. http-only storage. Disentangling logins vs everything else that could happen. We’re doing significant work on isLoggedIn (Login Status API new name), and WebID (FedCM new name). Different privileges for different scripts.

James Rosewell: To Aram’s assertion, there are no limits on the number of participants. Potentially 100s of thousands of participants. SWAN is very different from TCF, in that users consent to the contract. This is not about advertising, since this group’s charter concerns privacy, not advertising. W.r.t parties related to advertising, the consultation has taken a lot longer than expected. We have feedback on model terms from some advertisers and adtech companies; we hope to incorporate in the upcoming months. For this context; I’ve generalized to general data sharing, not specific to advertising. It’s very different from TCF.


## Any other Business - [W3C TPAC 2021](https://www.w3.org/2021/10/TPAC/) reminder
* [Breakout sessions](https://web-eur.cvent.com/event/2b77fe3d-2536-467d-b71b-969b2e6419b5/websitePage:efc4b117-4ea4-4be5-97b4-c521ce3a06db) next week
    * Storage Partitioning Breakout
    * Anti-Fraud session - Kaustubha is heading up.
* October 28th meeting cancelled because of TPAC


# Attendees:	 
1. Allan Spiegel (Adobe)
2. Anthony Molinaro
3. Aram Zucker-Scharff (The Washington Post)
4. Brian Campbell (Ping Identity) 
5. Brian Lefler (Google Chrome)
6. Brian May (dstillery)
7. Chris Needham (BBC)
8. Davi Ottenheimer
9. Don Marti (CafeMedia)
10. Eli Grey (Transcend)
11. Emily Lauber
12. Eric Mwobobia (ARTICLE19)
13. Eric Rescorla (Mozilla)
14. Ericka Wright
15. Erik Anderson (Microsoft, co-Chair)
16. Erik Taubeneck (Facebook)
17. Erwan Loaec
18. Grant Nelson
19. Heather Flanagan (Spherical Cow Consulting)
20. Heinz Bauman (Quantcast)
21. James Hartig (Admiral)
22. James Rosewell (51Degrees)
23. Jeffrey Yasskin (Google Chrome)
24. Joel Odom (Salesforce)
25. Johann Hofmann (Mozilla)
26. John Sabella (PubMatic)
27. John Wilander (Apple WebKit)
28. Kaustubha Govind (Google Chrome)
29. Kelda Anderson
30. Kris Chapman (Salesforce)
31. Lionel Basdevant (Criteo)
32. Marshall Vale (google)
33. Maud Nalpas
34. Michael Kleber (Google Chrome)
35. Mike O’Neill (Baycloud)
36. Nick Doty (CDT)
37. Peter KH (Google)
38. Peter Snyder (Brave, PING)
39. Phil Eligio (EPC)
40. Przemyslaw Iwanczak (RTB House)
41. Richard Anton (Amazon)
42. Russ Hamilton (Google Chrome)
43. Sam Goto (Google)
44. Sam Macbeth (DuckDuckGo)
45. Sam Weiler
46. Sanketh Menda
47. Sean Harrison (Google)
48. Shivan Kaul Sahib (Brave)
49. Steven Valdez (Google Chrome)
50. Tanvi Vyas (Mozilla, co-Chair)
51. Tara Whalen (Cloudflare)
52. Theodore Olsauskas-Warren (Google)
53. Theresa O’Connor (Apple, co-chair)
54. Tim Cappalli (Microsoft Identity)
55. Wendell Baker (Yahoo)
56. Wendy Seltzer
57. Yi Gu (Google Chrome)
58. Emily Lauber (Microsoft Identity)
