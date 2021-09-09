# Privacy CG Teleconference, 9 September 2021
* Chair: Tanvi Vyas
* Scribe: Jeffrey Yasskin

# [Private Click Measurement](https://github.com/privacycg/private-click-measurement) - [Unlinkable tokens on the click destination side and the triggering event API #88](https://github.com/privacycg/private-click-measurement/issues/88)
* John Wilander: I filed this after working through a design, the same-site pixel API. A way for the click destination to convert/fire a triggering event without having pixels or requests to third-parties that would match the publisher. Talked about converting multiple sources at once, maybe with a wildcard. In combination with unlinkable tokens, this was a challenge because we don't want to re-use the same token in multiple attribution reports. If we had a way for the destination site to convert for "these 5 publishers", we'd need 5 different unlinkable tokens to include in those 5 reports. Instead, suggesting we get 1 token, and we convert for a single source. Or for an "all sources" wildcard, we'd pick the "last clicked" one. After I filed this, I read Google's proposal [link], and my reading of that spec is that they always do last-clicked attribution regardless of where the pixel goes. Say you have 3 merchants. One does the redirect dance. Google's spec looks for all clicks that have taken the user to this destination, and report to the last one. People could discuss that too, and we might harmonize with that. I'm open to that too, since that resolves this issue.
* Erik T: Going to reply on the issue. I agree with your analysis around needing multiple tokens. Off the top of my head I think there are a few different solutions that could use multiple tokens. One would let the destination site specify how many possible clicks, and then blind that many tokens. Gives them the freedom. If the spec defines a limit, they could specify that many. Won't speak to Chrome's API.
* John W: The thing I wrote here would include 1 token and make that last-click, but if you use a wildcard, those other than last-click wouldn't include the token.
* Erik: That would advantage the last click over the others.
* John W: That's a good signal.
* Erik: But it opens the others up to fraud.
* Brian: The number of potential clicks is going to be relatively low. If we start with a default of 3, that would give us enough to satisfy most use cases and don't have to revisit this.
* John: Please also comment on the issue.
* Michael Kleber: None of our experts are on this call, sorry.
* John W: I will ask about this within the attribution reporting repo
* John Delaney: I've joined! What's the question?
* John W: <Repeats the question>
* John D: That's similar. We have a "highest priority" mechanism in the API. Not quite last-click, but rather the highest priority one will be reported. 
* John W: So while you haven't sent out the report, if you get another triggering event you'll send that?
* John D: We have priority on both ends. When you register multiple sources, you can prioritize among them.
* John W: So you only send 1 attribution report when you get a click.
* John D: Right.
* Kris Chapman: I have talked to Charlie about this as well, and it was a conversation in the Improving Web Advertising BG. They were looking at doing last-click/priority first, and then it could possibly be expanded.

# [First-Party Sets](https://github.com/privacycg/first-party-sets) - [Replace owner/controller language with simpler language on controller (and define it) PR #56](https://github.com/privacycg/first-party-sets/pull/56)
* Don Marti: The PR has some notes toward the end that summarize the issue (https://github.com/privacycg/first-party-sets/pull/56#issuecomment-911829254). This is tied into the longstanding debate about defining a party, which is unresolved. In this PR, I'm trying to make things practical from the point of view of the Independent Enforcement Entity (IEE) that's contemplated by the UA policy. There's two areas where an ownership-based definition of FPS would impose a lot of overhead on the IEE:
   * 1) Ownership is present, but the FPS is not valid. e.g. a large conglomerate with lots of subsidiaries with different privacy policies. Or a hosting provider might run a service that lets domains be "owned" by a shell company that can be connected to whatever FPS.
   * 2) When the FPS is actually legitimate, but a motivated attacker can find apparently-valid but fake ownership records. Some FPS of an investigative journalist in a country with difficult ownership records. Subject of a story comes up with bogus records to break the FPS, and now the IEE has to understand the language and law in a place they're not familiar with.
* Don: There's a 2-track support system here. A lot of people here can just call someone, but getting an FPS validated through the front door can be hard. To let the IEE avoid being a very complex organization, and to focus on more meaningful validations, I've suggested alternate language that focuses on essentially the GDPR's controllership, but something that should be practical for the IEE to do.
* Kaustubha: The reason for the current ownership requirement…. We had an ad-hoc call a couple weeks ago, and we published a preliminary UA policy document, and the PR is a change to that. The current language came from multiple sources, looking at various browser policy, prior art like the Do Not Track specification. When we had the TAG review discussion for FPS, we discussed corporate ownership. E.g. Amazon.com is a different owner than Amazon.co.uk, but users think of them as a single thing. I know the TAG is also discussing privacy principles for the web, and there was a recent discussion about how party lines up with user expectations. Legal stewardship of the data is probably better than ownership. The reason for the policy is to prevent abuse. We're not trying to litigate corporate ownership, but rather to make sure the sets that are formed are in line with user expectations. Questions for the group:
   * 1) Do we think controllership is publicly-verifiable? I asked the Disconnect folks how they do it. We talk about the IEE doing random spot checks on submissions and looking for public data on ownership. Ownership seemed easier to check.
   * 2) Are there anti-examples where if we only have the controllership requirement, we run into trouble?
* Don: Main question is the level of resources that are available to the IEE, and how to prioritize the amount of effort the IEE is using. There's also a [companion PR](https://github.com/privacycg/first-party-sets/pull/65) talking about what are the priorities, and how can FPS members address issues that are actually visible to users. Ownership structure isn't visible to users, but there are also visible controllership aspects that users can see.
* James Rosewell: Re "brand", I don't think we can rely on brands for the purpose of First Party Sets. Papa John's.com and Papa John's.co.uk are completely different companies. When it comes to identity of parties if we have an enforcement entity, we need a notary, so they can be sure who the party is. A notary is an accepted way to do this. Can prove identity. Re the communication mechanism, if we rely on Twitter to establish communication, will that be usable? Solution could be interesting, but most of the work will be on identifying owners, contracting with the enforcement entity, contact mechanisms. There will also be legal liability to the enforcement entity if it can turn off entities, and those entities need a right of appeal.
* Don: It's reasonable if the browser, IEE, and entity are in the same country. Can go to the notary's office. But if an alt-weekly in a small country runs a story, and they're in an FPS, and the IEE gets a notarized set of ownership records in a language the IEE doesn't speak saying the FPS isn't valid. The IEE needs to be funded sufficiently to validate notary statements. If the IEE gets a budget large enough for that, it might be a better use of funding to do other checks on things that actually show up for users. E.g. the privacy policy says they don't send spam, but spam showed up. Lots of privacy policy violations that an IEE could more profitably check.
* Aram: Idea of using the controller concept is better than using "brand" or corporate entity, but none are particularly great. The idea of enforcing this becomes messy. Chasing these down gets difficult. Corporations don't work the same way in every country. Even in the U.S. there are ~4 ways to incorporate with radically different structures. When we get down to this, we spend a lot of time thinking about how to make this work. We're assuming that corporations are strongly structured, but they're not like that. If national structures are unable to regulate corporations, which seems to be the case, the IEE won't be able to do it either, regardless of how we define first-party-ness. This change would make it easier, but maybe not easier enough to be effective. Any of these proposals create an IEE that needs resources like an Interpol, and I don't expect that to occur. And I don't think this group can put together a sufficiently-independent group to be trustworthy. Don't know if it makes sense to keep digging into this when there's such a big blocker in terms of realizing it.
* Don: Building an IEE that can go out and check the members of a Nevada LLC would be a remarkable exercise in investigative journalism and forensic accounting, and might be good, but depending on that will hold back an FPS proposal. If there's a reasonable-size IEE, it needs to be focused on uses of people's personal information that have an impact on the users. We can come up with a set of cost-effective ways to do a significant proportion of the iEE's tasks.
* Kaustubha: People are thinking of the IEE as being much more expansive than we envisioned. We wanted a combination of transparency, where the IEE maintains a log of evidence of FPS-ness. Mechanism for reporting issues, challenging an FPS. Don't want the IEE to check every single one. Want it to be scalable. So it's transparent, there's a way to report, and the IEE spot-checks them. IEE wouldn't check every single submission.
* Aram: I understand that. It's not that I expect that it would check all of them, but even checking a single objection (and advocates will object to a lot of things immediately) is a huge amount of work. An investigative team could take a year to check one claim thoroughly.
* Don: If a motivated attacker wants to break an FPS in a jurisdiction where the IEE doesn't have people, it could be a big project.
* Mike O'Neill: I think we're worrying about things that we don't need to worry about. There are national bodies that deal with this stuff, and I see how putting something into browsers that might create a privacy hole, we might need to trust the regulator. Let's concentrate on the majority use cases that we're worried about, and use FPS use cases we're worried about. E.g. use the FPS to restrict cookie lifetime.
* Nick Doty: Some of the debate is confusion over necessary vs sufficient conditions. E.g. Branding alone isn't sufficient to show that there's a common party. But it might be important for user concepts. Similarly on ownership and controllership. Document previously referred to the Do Not Track document, which required all three. Probably we're not going to define something that will address every privacy concern about who they're interacting with. Specifying those conditions and having them enforceable after the fact, could be an improvement. Don't understand why the PR is talking about reducing the number of conditions. Using "and" would help.
* Don: The question seems to be "where do issues actually matter to users". What does and doesn't have a user impact in this area? I agree that common user-understandable branding is a requirement, and this needs to be comprehensible to people using assistive technology, and not just to people who can compare a logo. Common privacy policy is required. But when there's a place that an FPS can be broken by false ownership records, that increases the amount of expertise and budget needed at the IEE. And that takes budget away from more user-visible checks.
* Nick: So the threat model is falsified records by a third party? Presumably companies with common ownership can show that to an auditor.
* Don: And companies with different data-handling policies can make up records that appear to show them sharing ownership. There's an example in the mortgage area, where different companies pretend that all mortgages are "owned" by a single company.
* James: Notaries do work internationally, very effectively. If the question asked of the notary is "based on this rule, is the organization a single organization?", then if the notary provides the proof, that solves the problem. Could even be provided digitally. There's a problem when we confuse ownership with "good or bad". Notary can vouch for common ownership, but not for right and wrong.
* Sam Weiler: Nick may have been on to something earlier about making statements. The IEE has a lot of a-priori work. Even just spot-checks are a lot of work. Would we be better off with a statement of shared liability, where parties that want to use FPS say they're each liable for the actions of the other, under the most restrictive of the policies. It wouldn't be a big deal for entities with real common ownership, but would be a big disincentive for unrelated parties.
* Kaustubha: Might tie into something we have in the policy proposal. I asked lawyers if a public declaration of common ownership would have legal force, and the answer was roughly yes. [quotes from the policy about companies being liable for misrepresentations] That's not exactly Sam's language, but the declaration is meant to create the liability.
* Sam: Want something that more clearly claims responsibility.
* Don: There's a question about a statement like that in one jurisdiction that is meant to be used by people in another jurisdiction. Companies do that in practice.
* Sam: Would love to not have an IEE.
* Don: It's a necessity for FPS. The goal is to optimize it and consider the users first.
* Aram: That's the problem: someone has to verify that the FPS is legitimate or declare it's not. 2 challenges that are in place. GDPR is meant to make all the parties liable when there's a violation, and it's turning out to be hard to enforce. We're proposing a concept for which there isn't a law. Another problem is how conglomeration will occur to challenge FPSes. We're designing this against an idea that companies that do different things are different companies, but they're already being acquired so that they can share data on the back end. We're restricting things on the front end, but that'll just create even more incentive to merge companies. It can create a weakness for independent actors. If the goal of FPS is to provide a level of privacy guarantees to the user, we rely on the IEE to be an investigative body, or we rely on national regulatory bodies that don't yet exist, or we rely on corporations to avoid doing wild merger and acquisition stuff that they already do a lot. We have an open PR for this. The idea of affiliation as a corporation could be useful, but using it for a privacy guarantee, seems more and more difficult to prove out as we talk about it. It seems impossible to get multiple nations to create enforcement for this. None of the ways to make FPSes improve privacy seem viable.
* Kaustubha: As you said, there are incentives to do back-end joining. I hope we can encourage corporations to be more respectful, show users that it's a single brand or entity. It's useful for a browser to forget the whole FPS at once when the user asks to clear one of them. There are some clear improvements to do FPS on top of the reality of today with back-end joining.


# Announcements
* APAC-friendly call time every other month (5 PM EDT for September 23rd call).  Every 4th call.  2pm PT.
* Queue - please avoid adding an entire paragraph. Just your name and maybe a couple words.
* Community Group, to run it we need scribes from the community.  Please consider volunteering for half or a full meeting.


# Attendees      
1. Allan Spiegel
2. Angela Ballard (CJ Affiliate)
3. Anuvrat Singh (Amazon)
4. Aram Zucker-Scharff (The Washington Post)
5. Bill Densmore (ITEGA.ORG)
6. Brain May (dstillery)
7. Brendan Riordan-Butterworth (eyeo GmbH)
8. Brian Lefler (Google Chrome)
9. Chris Needham (BBC)
10. Chris Wood (Cloudflare)
11. Christine Runnegar (PING co-chair)
12. Christy Harris (Future of Privacy Forum)
13. Don Marti (CafeMedia)
14. Emily Lauber (Microsoft)
15. Eric Mwobobia (ARTICLE19) 
16. Ericka Wright
17. Erik Anderson (Microsoft)
18. Erik Taubeneck (Facebook)
19. Frederic Jacobs
20. George Fletcher (Yahoo Inc)
21. Heather Flanagan (Spherical Cow Consulting)
22. Hitesh Lad (CJ Affiliate)
23. Jack Frankland
24. James Rosewell (51Degrees)
25. Jeffrey Yasskin (Google Chrome)
26. Joel Odom (Salesforce)
27. Johann Hofmann (Mozilla)
28. John Delaney
29. John Sabella (PubMatic)
30. John Wilander (Apple WebKit)
31. Jun Kokastsu
32. Kaustubha Govind (Google Chrome)
33. Kris Chapman (Salesforce)
34. Marshall Vale (Google Chrome)
35. Michael Kleber (Google Chrome)
36. Mike O’Neill (Baycloud Systems)
37. Mike Seilnacht (Intuit)
38. Nick Doty
39. Paul Bannister (CafeMedia)
40. Peter Saint-Andre (Mozilla)
41. Phil Eligio (EPC)
42. Raj Belur (Amazon)
43. Richard Anton (Amazon)
44. Russ Hamilton (Google)
45. Russell Stringham (Adobe)
46. Sam Weiler (w3c)
47. Sam Goto (Google)
48. Sean Harrison (Google)
49. Shigeki Ohtsu (Yahoo! JAPAN)
50. Shivan Sahib (Brave)
51. Tanvi Vyas (Mozilla, co-chair)
52. Tara Whalen (Cloudflare)
53. Theodore Olsauskas-Warren (Google)
54. Theresa O’Connor (co-chair, Apple)
55. Tim Cappalli
56. Viraj Await
57. Wendell Baker (Yahoo)
58. Wendy Seltzer (w3c) 
