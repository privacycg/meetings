# Privacy CG Teleconference, 8 April 2021

*   Chair: Tess
*   Scribe: Arthur

## Agenda

* [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    * [Enable the advertiser/merchant to choose not to send attribution reports to click sources #65](https://github.com/privacycg/private-click-measurement/issues/65)
    * [High volume of requests to add domains to the PSL #78](https://github.com/privacycg/private-click-measurement/issues/78)
* [The Storage Access API](https://github.com/privacycg/storage-access)
    * [Scenario Validation (Embedded Component (Tableau)) #74](https://github.com/privacycg/storage-access/issues/74)
* Our next (virtual) F2F: 19–20 May
* Any other business

## [Private Click Measurement](https://github.com/privacycg/private-click-measurement)

### [Enable the advertiser/merchant to choose not to send attribution reports to click sources #65](https://github.com/privacycg/private-click-measurement/issues/65)

John Wilander: We talked about this one on the last call for the privacy cg. We came to the conclusion that we should enable advertisers not to send attribution to the click source if they don’t want to. But they can opt in. But they may have a competitive or business reason to not share the data. But Google’s proposal in the WICG [name], there some folks from the publishing industry commenting. And they seem to have different opinions on what was shared with the last privacy cg call. I want to allow them to revisit and perhaps share opinions here and see if we can come to a better conclusion. Github issue is getting towards -- can we make the two parties agree at least? Maybe they both need to say yes, I understand particular advertiser has opted out and I am still OK with displaying their ads.

Aram Zucker-Scharff: There is a concern with the situation that there is an unequal potential reporting problem when both sides of the transaction theoretically own the information. That’s the core of the argument on the github issue. 

John: I think there’s interest, if you can summarize what’s been said on that issue.

Aram: The core issue is that we’re concerned that conversion reports might go from just the advertiser or just the publisher. If it goes to just the publisher, or if the advertiser allows it to go the publisher, in either case they are revealing conversion performance of links to the publisher (top-level). If the publisher is an entity that owns an ad space or competing product with the advertiser’s product, that would give it insight into how the advertiser’s product performs and potentially out-compete. The flipside is that there is a big problem in the existing ecosystem where the advertiser or middleman entities misreport conversions to publishers to allow them to get more money out of publishers. Essentially if you are familiar with the concept of dark pools in banking, there is a similar concept in impressions at the agency level of ad buyers. We don’t want to create a system where this problem is perpetuated. In the current situation, it is already an unequal status quo, advertisers can collect conversion reports without revealing info to the publisher. But with large vertically integrated entities, usually conversion measurements are through the type of ad tech that is provided by these large companies. If you are advertising on Google AdWords, as an example of a large vertically integrated company, you are already using google’s tech to do your reporting. If you are using local news.com, that’s not the case, and that’s bad for them in the current system. For a new system, equality in reporting is important because the entity that creates the conversion and hosts the ad is the publisher, and the entity that receives the conversion is the advertiser and both should have access to that report. Maybe both sides need to agree -- both sides either get a report or don't. If both sides disagree, maybe they both get a report? If the two sides need to come to terms on the same setting, there may be some ability for one side or the other to pressure the other side, depending on the economics.That’s an issue as well. I don’t necessarily have a solution, but a change in the environment where the advertiser but not the publisher can receive a report is probably not the way we want to go. Publishers need the info as much as any other entity.

John: Thank you for bringing this up. 

Michael Lysak: I’ve been on the fledge call, w3 call, first time on privacy cg. Want to flag that there seems to be a couple of issues about this topic in particular. There are additional issues on publisher transparency which I think is very important. I think that publisher transparency and publisher/advertiser transparency are being discussed in additional issues. Similar conversations about this topic are taking place, on fledge repo, on floc repo. I want to bring up as a matter of publisher transparency, on the fledge discussions, I want to make sure that the publisher doesn’t lose even more access to data than they have now. There’s effectively an ability to cheat or hide info from the publisher. I’m talking about smaller or third-party publishers/partners needing to get analytics. Two problems: one is the walled garden problem. Two is a third-party website dealing with a conversion vs you are on facebook dealing with a conversion. Are these two separate issues that we should discuss? Heard both in the last statement.

John: There could absolutely be two different issues here. I also see based on what Aram was summarizing too. Publishers in the sense that people understand around publishers and then the fact that you are running ads on some website. Maybe we could divide it in that way. I want to suggest we try to have a session on this in the face to face. Balance, transparency [are two aspects]

Don Marti: I wanted to ask about whether we are considering the long-term market design impact of this. If we hide the conversions from the publisher or make it possible for the advertiser or make it possible for the advertiser to do so then we are facilitating the placement of good ads on bad sites? (Hiding conversions can lower the risk of a good ad ending up on a sketchy site.) Are we encouraging ad money flowing to sites with poor practices. There are many levels at which this can be handled if the counterparty is a legit publisher with which you can have a contractual relationship.

James Rosewell: Question for Aram: as you explain that, were you referring to conversion from the publisher to the advertiser where that was happening for a few seconds, or a prolonged period of activity?

Aram: Any conversion covered by the specification. There’s a specific outline for what constitutes a conversion in these specifications.

John: Two ideas I have had on my mind that could go into the f2f. Maybe we could consider that an advertiser has the power that I don’t want to send this attribution to the click source, then that will create an incentive, should I be hiding this data from the click source. (2) Could we get to some kind of categorization of sites and actors on line, click sources that are not involved in other kinds of products and activities, but the advertiser has a say in these vertically integrated things. Don’t know if it’s a viable path, but it’s on my mind.

John Delaney: To respond to Michael’s point. On the notion that there are different types of publishers and advertisers where different behavior might be desirable, I think it’s tough to integrate this into a browser API as you try to keep the API generic. Sometimes it’s easier to kick the complexity onto the ecosystem. Something I proposed in the attribution reporting issue earlier. Want to keep a line between the click proposal measurement and this. Happy to hear future discussion.

Tess: I added this to the f2f agenda.

Aram: I understand the inclination to distinguish between click sources, but I think at the end of the day (and could put same idea onto advertiser source) we have to assume that the difference between a vertically integrated company and a non-vertically integrated company is one that M&A can blur. As has been said from Google, the ecosystem needs to operate in a space where none of the parties trust each other.

### [High volume of requests to add domains to the PSL #78](https://github.com/privacycg/private-click-measurement/issues/78)

Ben Savage: There’s a linked issue that the maintainers of the public suffix have raised, where they have noted an uptick in the number of requested entries. Rightly that this has something to do with PCM and Facebook. Credit this with a rise in the volume. This is not Facebook specific in any way. If you are a business that does not own its own domain, but operate as a subdomain, you are going to have a bad time unless it is registered on the PSL. Our intention is to provide educational material about this. There are two things happening here: we need to go back and be extremely clear. For whom this is appropriate and sensible thing and for whom it is not to make sure we are pre-qualifying traffic. John and Eric and I think we are all aligned. Each of subsequent domains should be partitioned, and where those subdomains represent fully separate businesses and ought to be treated as separate websites. John identified a potential abuse where extra entropy could be gotten out of PCM. How do we ensure that for the right people there is a path to sign up and for whom it does not make sense? If we make guidance extremely clear, maybe the request numbers will go down? Propose that Apple ought to do it, because Apple has created the problem by assigning to registrable domain. Apple is one of the most valuable companies on Earth so they have resources. Also personally happy to review this. How to move this forward to support legit use cases but not abusive ones?

John: As the editor of PCM: the issue tracks almost all the things that are relevant here and Ben summarized accurately. There are two problematic cases: (1) click an ad and go to a specific product page, and now the report tells you what the product is; that’s supposed to be the campaign ID. The other (2) is personalized domains. If we stick the PSL as the solution and get through the vetting of new entries. We’re taking a chance that there won’t be bad actors or we catch them and put them on the blocklist, saying you can no longer use PCM. I think that’s an OK starting point; I don’t really like it. Assume some good faith and have a look at this -- are we looking at tens or hundreds of new entries to the PSL and take it from there. As to who gets to vet this list, I would like to hear from Mozilla since they are maintaining this list.

Peter Saint-Andre: The PSL did start as a Mozilla thing years and years ago. Lost in the mists of time how long this thing has been around. We do host some of the IT infra related to PSL. The one person who was very involved, Gerv Markham who used to work at Mozilla until he passed away a few years ago. It’s not really a Mozilla thing anymore, it’s a volunteer effort. Ryan Sleevi and others have been very active on it. We should ask for some volunteers to beef up the effort. I don’t think it’s appropriate for a single company to take this on. Could be the appearance of impropriety if a single company took over management of the PSL. There’s no governance structure really, just a github repo. If we want to handle more requests, we will need to figure out what the governance is and talk with the current folks who are active.

James Rosewell: An observation that in many of the discussions at W3C, we find domain names are not good proxies for legal entities. Maybe more macro-level thinking is needed and what we need to be identified. Maybe domain names aren’t the best way to solve these problem. [Afterthought: the subject field of the SSL certificate may be a better way of communicating legal entities for the purposes of solving privacy issues]

Michael Kleber: John mentioned earlier the threat of the per-product subdomain and the fact that’s a type of info that’s supposed to be incorporated into the campaign ID. I would like to be clear there are two different sources of info you might be worried about leaking and we have been concerned about in creation of Google’s API version. There’s information that you get about joining up. Info that you learn about one side vs knowing about the other side. I think that should be treated very differently that you need to know in advance at both sides of the conversion report for a conversion to happen. For the subdomain case you need to know something on both sides. If you’re talking about a company that only sells one thing, then they run an ad for that thing, then the conversion is for that thing and there’s no info gained. If you’re talking about a company that sells lots of things but registered a subdomain for a single product. The fact that you’re talking about running a campaign that explicitly says I can only convert on that specific subdomain, and the conversion only happens if they fire a conversion pixel on that domain, and the same ad campaign would not send a conversion report at all on another subdomains on the site is a very different issue from the information flow point of view. The fact that you would have a subdomain at the time of impression rather than time of conversion. Seems almost useless for tracking an individual, for example. Want to distinguish information needed on both sides, vs information that you only have on one side and need to learn on the other side.

Kaustubha: In the context of FPS how we are talking about registrable domains and how that’s not enough. There’s [a question of how origins might not be a site](https://github.com/privacycg/first-party-sets#origins-instead-of-registrable-domains), etsy or shopify cases. You may have momandpop.etsy.com and blah.etsy.com and technically those two are the same entities. There was a question whether FPS should be considered by origin. Would put less pressure on PSL. Entropy limits. Unfortunately that design was insanely complex. We talked about hey, here is one possible solution if other browsers are interested in that. Link will be in meeting chat. Want to highlight that this is another example where registrable domain just doesn’t cut it.

John: Maybe this is the right group to take on, not governance, but at least thinking about it more outside the context of PCM. It is the underpinning of least partitioning in webkit. It has a lot of privacy implications if we can’t rely on the PSL or site owners can’t get on the PSL. I want to reply to Michael Kleber’s comment. The potentially personalized or productized subdomains are a kind of data transfer between two websites. It should be that the destination website doesn’t know that this is a paid click with PCM -- it’s just a page load. This becomes a data transfer method for direct conversions. It’s not really for re-engagement two days later, it’s that you convert directly and data is transferred in the PCM report.

Ben: Thank you to John for openness to non-ideal situation for finding a decent vetting mechanism. Agree that perhaps this group is a good one for taking care of the PSL. I think to be somewhat more critical of PCM I think this speaks of a fundamental flaw in the design of this API. There is not really any firm privacy guarantee. From a differential privacy perspective there is no guarantee from PCM. From the perspective of bit limiting, it doesn’t provide any limits, because you can have very large or very small domains. Registrable domain isn’t a sensible privacy construct for limiting information. The same thing applies to the bits. The same number of bits for every website large and small isn’t sensible. If there is a privacy utility efficiency frontier -- PCM is a bad API that does not live on the frontier. It lives somewhere on the interior. I think something closer is what Google has proposed with the aggregated reporting. It smoothly and naturally adapts to websites large and small. Incremental differential privacy is a small signal for large websites, and for small websites you get a baked in guarantee. Strongly encourage you to re-examine the aggregated reporting API for both better privacy and utility.

Erik Anderson: I think on the PSL thing specifically, I feel like the issue and the big thing it’s highlighting is historically the PSL has been there to sign up for additional restrictions, for enhanced security or privacy guarantees if AppSpot.com or one of these other domains. I don’t really have two subdomains that are owned by the same developer and just don’t want to overlap, that’s where the use case has been historically. Concerning that this could start to grow unbounded, could look like the HSTS preload list which is quite large and a lot of browsers don’t update this quite as frequently as a lot of devs would like to see. You’re talking about many months waiting for a separate ad hoc update. Even if it’s somewhat indirect dependency on the PSL, there’s a real risk of folks getting very frustrated if they want to get on or off it and it takes months or even years. Feels a little incongruous with the historical purpose of the PSL. Think we should be really cautious about things that could encourage a flood of submissions.

Michael L: I agree that I am worried about using domains or bits. Publisher situations --publisher groups that have many domains for aesthetic reasons but ultimately they are the same sites. I would like to see something along the FPS line where we can treat different domains as one entity. Touching on Ben’s point regarding aggregate. If the ultimate purpose is to achieve a privacy objective. Aggregate reporting API does not prevent user tracking through logins, subscriptions or os level identifiers. The issue might extend past web, if we only half-solve the problem, it could create some imbalances.

Related Links provided: [https://github.com/sleevi/psl-problems](https://github.com/sleevi/psl-problems); [https://github.com/privacycg/first-party-sets#origins-instead-of-registrable-domains](https://github.com/privacycg/first-party-sets#origins-instead-of-registrable-domains)

## [The Storage Access API](https://github.com/privacycg/storage-access)

### [Scenario Validation (Embedded Component (Tableau)) #74](https://github.com/privacycg/storage-access/issues/74)

Lee Graber: The short summary is -- this is about the general trend in third-party cookie management. We have an online service as well as an on-prem product that can be installed and managed as a local server, and our component is often embedded in apps, be it a chat app, third-party app you are selling to your customers, …. We require authn or authz for signing on our customers. Experience should be seamless. We manage our sessions via cookies. Standard session management pattern. We often have layers in between us that do management via cookies through load balancers. Some we have control over, some we don’t. We were hit when same-site cookie attribute enforcement was put in place. But the trends of blocking third-party cookies -- you can end up with our customer experience being endless login loops or other negative experiences. I am aware, if you look at documentation for some of our competitors, they also document that they require certain third-party cookies to be enabled. Safari is the one that has currently flipped the switch. For all our customers we tell them they have to unflip the switch. On iOS they have to change it on their mobile app. I have nothing to do with advertising. We don’t track you in any way to try to target ads at you. We are a component of applications getting built. We require authentication and authorization. I have write-ups on how storage access API behaves, how partitioned storage in Firefox has worked. I have read some on FPS but I have some feedback on how they might cause limitations for us. Read a bit through WebID because we have identity providers for applications we are a part of. I need to talk to somebody about our scenario about how the changes will affect all of our customers users and how we can create something that is satisfying the need to prevent tracking while also enabling what we’re trying to do.

John Wilander: I want to start -- it seems like you’re saying partitioned storage is looking promising. But it seems like you are asking to access the same storage across websites. So which one is it?

Lee: It’s not that we need access to the same cookies across sites, it’s just that we need access to our cookie on a given site. We don’t have a problem that I am aware of that says: if we are a part of a chat app and we are a part of your portal app. We don’t care if we have two different sessions. We do expect that if you log in to your chat app, and you logged in to your portal app, then the IdP that exists clearly has access to its cookies so we can get an SSO experience so we don’t get into an infinite login loop. Partitioned storage has worked.

John: Probably something I am missing on how a SSO or third-party login can work with partitioned storage. I can only see that you’d transfer something into the first party and then back into the third party as an iframe.

Lee: If you assume we are SSO with the First party, you have to be already logged in to the first-party. So you have a cookie that is already in the partitioned storage for that first party. Should be able to access it in any flow. 

John: I’m probably missing something. When you described a seamless cross-site id of the user, if that is what you are looking for, then from WebKit’s perspective then that is a case of cross-site tracking. That would be against our tracking prevention policy. We deliberately prevent it.

Lee: That seems like it would be a problem for any product. Looker’s documentation says they require third-party cookies to get federated identity and SSO support.

John: We do offer the storage access API. That is our solution to this. That’s why seamless is important here. 

Lee: The storage access API breaks down in two respects: (1) it requires us to have a user click, we can’t just show them what we were supposed to. (2) my testing shows that different browsers have implemented it differently. Spec leaves wiggle room for different vendors. Some won’t ask the user if the third-party domain has ever been explicitly visited. The end user will never go to blablah.tableau.com. We are components. That requirement makes it not work at all.

Kristen Chapman: Lee and I have talked about this. I want to emphasize that the problem for us as Salesforce is the SaaS nature, and everything we do as a third party on the first party behalf. We don’t do tracking as far as advertising is concerned. I do understand what we do might be tracking as far as Apple is concerned. As we try to step away from third-party cookies, every single browser has different answers and implementations, but we are running into a situation where Safari says SAI, Google does FPS, Firefox does something else, and none of them work perfectly and there are issues for each one. I don’t want to have to go to 3 or 4 different proposals. I would like it if we could coordinate what is the approach that we think of so we can be more directed about it, rather than solving in individual browsers. We’re generally falling into this problem as being seen as a third-party even though we are operating really in a first-party situation. Thinking about setting up proxies or cname aliases. Don’t think this is a great situation from a transparency aspect for consumers, but we’re being pushed in that direction.

Tess: Ultimately the purpose of tech specs is to achieve interoperability. There is an inconsistency across browsers that is causing friction. By fixing interop challenges, hopefully we get to the point where it’s less inconsistent. We’re at a stage that is perhaps more wild west than everyone would like.

George Fletcher: Found Lee’s use case interesting, fundamentally an identity use case. We’re running into other issues, specifically around federated identity logout mechanisms don’t work when third-party cookies go away. A user gets a valid session left at some site that they don’t know about. Tim posted in the chat a link where we’re collected use cases. Lee if you want to add use cases to the list. If some of these use cases go away, what breaks? SSO from the perspective I logged into portal A through okta or something and now I log into portal B unless you are doing full-page redirects, that’s no longer seamless. I understand we are trying to protect against seamless ad tracking. How do we separate seamless identity flows from ad tracking flows? Otherwise identity is going to be a mess.

Peter: We’ve got to document these use cases and understand how these things work. Everybody’s got a different implementation of their web stack. On the Firefox team we’re happy to talk with folks. To stress: that’s the point of these standards discussions. Let’s continue to work on these things together. We’re not trying to break things, we’re trying to protect users.

Michael L: This is possible a wild idea, or maybe a question. it occurs to me there currently is a real-world example that will continue to work, particularly on Chrome at least. That’s Gmail. Unless I am understand gmail will continue to exist on every website, every domain. Wondering if google will continue to use gmail and google login in this way when FLOC goes purpose and will allow people who want to do something similar for non-ad purposes? 

Michael Kleber: Don’t understand what Michael was referring to. There is not magical exception for gmail in Chrome, but third-party cookies still exist in Chrome. Gmail is treated like everybody else.

Michael L: I was trying to practice the floc origin trial and I think what the issue is that I didn’t have sync turned on. It said FLOC required Chrome sync which required gmail login.

Michael Kleber: What you saw had nothing to do with having to be logged in. We’ll talk about this in a venue appropriate for discussing FLOC.

Related Links provided: [https://github.com/IDBrowserUseCases/docs/issues?q=is%3Aopen+is%3Aissue+label%3A%22use+case%22](https://github.com/IDBrowserUseCases/docs/issues?q=is%3Aopen+is%3Aissue+label%3A%22use+case%22)

[https://github.com/WICG/WebID](https://github.com/WICG/WebID)

## Our next (virtual) F2F: 19–20 May

Tess: Our next virtual F2F is May 19th and 20th. Details on the mailing list, more will be coming once we have them. Thank you all!

## Any other business

## Attendees

1. Andrew Knox (Facebook)
2. Allan Spiegel
3. Angela Ballard (CJ Affiliate)
4. Aram Zucker-Scharff (The Washington Post)
5. Arthur Edelstein (Mozilla)
6. Arnoldrw
7. Ben Savage (Facebook)
8. Bill Densmore (ITEGA)
9. Brendan Riordan-Buttworth
10. Brian May (dstillery)
11. Cezary Cerekwicki
12. Chris Needham (BBC)
13. Christy Harris (Future of Privacy Forum)
14. Daniel Appelquist
15. Don Marti (CafeMedia)
16. Ehard
17. Eric Mwobobia (ARTICLE19) 
18. Eric Perret (Salesforce)
19. Ericka Wright
20. Erik Anderson (Microsoft)
21. Erik Taubeneck (Facebook)
22. Fred
23. Gene Radin (Socure)
24. George Fletcher (Verizon Media)
25. Jack Frankland
26. James Hartig (Admiral)
27. James Rosewell (51Degrees)
28. Jason Nutter (Microsoft)
29. Jeffrey Yasskin (Google)
30. Joel Odom (Salesforce)
31. Joey Salazar
32. Johann Hofmann (Mozilla)
33. John Delaney (Google)
34. John Sabella (PubMatic)
35. John Wilander (Apple WebKit)
36. Jon Callas (EFF)
37. Kaustubha Govind (Google)
38. Kelda Anderson (Microsoft)
39. Kris Chapman (Salesforce)
40. Lee Graber (Tableau/Salesforce)
41. Les Cochrane (Audigent)
42. Liz Hurley (CJ Affiliate/Epsilon)
43. Mark Xue (Apple)
44. Marshall Vale
45. Marcal
46. Maud Nalpas
47. Melanie Richards (Microsoft)  
48. Michael Kleber (Chrome)
49. Michael Lysak(Carbon)
50. Mike O’Neill (Baycloud)
51. Mike Senn
52. Miranda
53. Nima Conton
54. Pedro Alvarado
55. Phillip Eligio
56. Prezemyslaw Iwanczak
57. Peter Saint-Andre (Mozilla)
58. Phil Eligio (EPC)
59. Raj
60. Russell Stringham (Adobe)
61. Ryan Arnold (P&G)
62. (Neodata Group)
63. Sean Bedford (Facebook)
64. Sean Harrison (Google)
65. Shaun Gilmore
66. Steven Englehardt (Mozilla)
67. Tanvi Vyas (Mozilla, co-chair) 
68. Theresa O’Connor (Apple)
69. Theodore Olasauskas-Warren
70. Tim Cappalli (Microsoft Identity)
71. Tony Resendiz (CJ)
72. Wendell Baker (Verizon Media)
73. Wendy Seltzer (W3C) 
74.  Alan Toner 
75.  Daniel Appelquist (Samsung)
76.  Lola Odelola (Samsung)
