# Privacy CG Teleconference, 23 September 2021

* Chair: Tess
* Scribe: Heather

## Attendees

1. Aditya Desai (Google)
2. Ben Savage (Facebook)
3. Brian Lefler (Google Chrome)
4. Brian May (dstillery)
5. Chelsea Gong
6. Chris Needham (BBC)
7. Christine Runnegar (PING co-chair)
8. Emily Lauber (Microsoft)
9. Eric Mwobobia (ARTICLE19) 
10. Eric Rescorla (Mozilla)
11. Ericka Wright
12. Erik Anderson (Microsoft, co-chair)
13. Eli Grey (Transcend)
14. George Fletcher (Yahoo)
15. Heather Flanagan (Spherical Cow Consulting)
16. Heinz Baumann
17. Helen Cho (Google Chrome)
18. Ian Meyers (LiveRamp)
19. James Rosewell (51Degrees)
20. Jeffrey Yasskin (Google Chrome)
21. Jeremy
22. Johann Hofmann (Mozilla)
23. John Wilander (Apple WebKit)
24. Kaustubha Govind (Google Chrome)
25. Mark Xue (Apple)
26. Marshall Vale (Google)
27. Martin Thomson (Mozilla)
28. Michael Kleber (Google Chrome)
29. Michael Smith
30. Mike O’Neill (Baycloud)
31. Nathan Schloss (Facebook)
32. Nick Doty
33. Peter Saint-Andre (Mozilla)
34. Peter Snyder (Brave Software, PING Co-chair)
35. Russell Stringham (Adobe)
36. Sam Weiler (W3C/MIT)
37. Sanketh Menda
38. Steve Englehardt (DDG)
39. Tanvi Vyas (Mozilla, co-chair)
40. Tara Whalen (Cloudflare)
41. Theresa O’Connor (Apple, co-chair)
42. Tim Cappalli (Microsoft Identity)
43. Viraj Awati
44. Vittorio Bertocci (Auth0 | Okta )
45. Wendell Baker (Yahoo)
46. Yi Gu (Google Chrome)
47. Gene Radin (Socure) -- joined very late :’(

## [Agenda](09-23-agenda.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Navigation-based Tracking Mitigations](#navigation-based-tracking-mitigations)
  - [Are a lat/lon/zoom triple in a URL decoration/tracking? #4](#are-a-latlonzoom-triple-in-a-url-decorationtracking-4)
  - [Are unsubscribe links with a user ID link decoration and/or tracking? #5](#are-unsubscribe-links-with-a-user-id-link-decoration-andor-tracking-5)
  - [How does user intent fit into navigational tracking? Issue Define navigational tracking as "without the user's knowledge"? #8 and PR Added a section explaining users can agree to pass information between domains via the URL. #9](#how-does-user-intent-fit-into-navigational-tracking-issue-define-navigational-tracking-as-without-the-users-knowledge-8-and-pr-added-a-section-explaining-users-can-agree-to-pass-information-between-domains-via-the-url-9)
- [Any other business](#any-other-business)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## [Navigation-based Tracking Mitigations](https://github.com/privacycg/nav-tracking-mitigations)

Peter: Current state of the doc: this is a term started to use re: bounce tracking. Trackers are trying to link storage areas in the browser. Click on site A, bounce to site B (profiler in some way), forward to destination. Different browsers have different countermeasures. This is a case of a broader kind of tracking where there is some middleware info like link decoration impacting tracking. The doc is trying to establish the vocabulary around this behavior. 

### [Are a lat/lon/zoom triple in a URL decoration/tracking? #4](https://github.com/privacycg/nav-tracking-mitigations/issues/4)

Peter: most common is to have a specific identifier which corresponds to a unique user or database id, push this across the storage boundary, then sync up. But what about things that aren’t a database/user id but are still uniquely identifiable. 

Martin: thanks for the context. In looking at related work, discovered that identity matching as discussed is achievable with very little info exchange across the navigation. We should focus on simple case where there are no redirects. The source site and the destination site can exchange info if the link contains any information other than what’s necessary to get the target resource. The target resource may be unique and targeted to the individual by the source site. If two users retrieve resources that are functionality identical, but the URLs are different, that *might* be tracking. But there are reasons this might be reasonable behavior. To the specific question here re: maps link, the answer is probably “maybe”, if there are more decimal points than necessary, that info could carry information for any number of purposes, and we won’t be able to tell by looking.

John Wilander: Gnarly problems to define! Important to not just talk about linking identity across two sites. If one site learns something new about its user that happened on another website, that should be included in the definition. 

Peter: Another wrinkle, there are some cases where unique id’s are capability URLs. That’s probably not in the same bucket as Facebook -> Instagram. 

Tess: It’s hard to distinguish between these two cases. 

Mike O’Neill: difference between link decoration and bounce tracking - link decoration is dynamically creating a URL to get to a resource. It could contain user-identifying information. Important thing about bounce tracking is that it’s allowing the passing of a first-party state for the top-level site into an embedded context, which will then be visible on other sites. People are trying to mitigate issues by mimicking first party states. RAther than concentrating on what data is passing via  dynamically generated URL, should instead concentrate on the communicating of state and the specific issue of passing state from one site to another via an embedded resource. 

Jeffrey: re: Martin’s comment - on the one hand, we’re trying to say how t o mitigate things, not just now to define stuff, but we haven’t gotten there yet. It would be reasonable that if two users see the same URL that might be tracking, that’s link decoration. 

ekr: this is a situation where formal definitions and intuitive definitions won’t match very well. It’s not unusual to start with [www.resolve.org](www.resolve.org) and resolve.org. These are aliases for the same thing, and saying that’s link decoration is not helpful or meaningful; but every one of these things is an opportunity to pass information. It will be helpful to distinguish between intent and the technical situation. What capabilities are assumed in the doc? Need to be realistic with what we can or cannot do. As an example, I don’t think it’s plausible to detect every case where the link is different but the content is “the same”, give how easy it is to make something substantively identical but technically non-identical.

Vittorio: in most of the federation protocols, the ability to decorate the link with the hint that indicates the user is a feature there by design. IT may be because you have more than one session, and you need to specify which session. Or you may want to skip a discovery step. Or you may want to sign out the correct user. By some of the criteria discussed so far, may trigger some of the logic described. Would be ideal if we could devise a heuristic to define if its part of a sign-in/sign-out operation. 

Brian Lefler: navigation across state boundaries are what offer specific harms. There are a large set of concerns when resources are fetched across site, but it’s most useful when you have the shared identity. When two sites have  a shared context of identity, then privacy becomes a problem. Understanding user intent seems different than something that’s happening passively. 

James Rosewell: Agree with Brian and MIke about state. The state boundary question comes to the security and privacy boundary, and the use of registered domain names is not a good way of doing that. Maybe focus on the foundational, architectural issue of the web, rather than deal with methods to work around state-sharing mechanisms which are not themselves bad. If it’s about state, we should look at the security/privacy boundaries, not one method of many for sharing information. Also need to be clear on what problem we are trying to solve. 

Tess: to the first point, the answer is “yes, and”. It’s not about doing x before y, it’s about doing both. 

Martin: we went from the definitional question immediately to what are we going to do with this. I was trying to refrain from doing this. The boundary the navigation cross is relevant to the definition. Knowing what the boundary is will ultimately be important, but for now we can say it’s roughly registerable domain-shaped and otherwise wave hands about it. The question of what we do about this is still open and probably needs a separate issue. There’s not much to do technically with the given definitions. 

Jeffrey Yasskin: Wanted to note we’re going to come back to Vittorio's concern in issue 8 and 9. On this question, sounds like we may be asking to table some of the details of the definition and come back after people have proposed mitigations. That seems plausible at this stage. As a side-note, I want to be clear I’m trying to remain neutral as an editor; other people will talk about Google’s proposals.

### [Are unsubscribe links with a user ID link decoration and/or tracking? #5](https://github.com/privacycg/nav-tracking-mitigations/issues/5)

* this is the question of a capability URL.

Johann: difficult to find definitions in this problem space. Been looking at the work that Jeffrey and Peter have been thinking of and throwing edge cases at it, like capability URLs or unsubscribes. When we posed the questions we asked if it was link decoration or navigation tracking. We decided it probably is link decoration. It is a capability URL, but we have to think about which kinds of capability URLs support tracking. But it serves the user intent and is user-initiated functionality. It also only involves a single party (in the unsubscribe case). 

Tess: if I use a web-based email client, and reading an email from a service and want to unsubscribe, and so click on a link in the email. The registered domain of the email service is different from the destination service. I am going from an email that came from them, but the browser doesn’t know that. 

Johann: it’s hard to write code that knows this. I’m proceeding different ways people are looking at this; capability URLs might not support what users expect. If we can identify one of these that we can write into the spec, we also need to think about how attackers abuse this definition and what exactly is the intended functionality. These are hard questions and almost impossible to resolve.

Martin: when talking about the last one, John mentioned that if the site learns anything about the user by virtue of them navigating/requesting a resource, that could be tracking. If you know the URL, that separates you from all the people that don’t know the URL. That can be highly identifying. It’s down to the source of the URL, who is presenting, and whether they are cooperating with the target of the URL to exchange information. 

John Wilander: a few people are pessimistic about our ability to solve anything here. I’m optimistic, we have many tools in the tool box. We can ask the user if this is what they expect. Or we can count the number of unique sites that are being sent link decoration. If you’re telling 20 other sites, that could be an obvious problem. We need to go beyond pure technical and think more broadly.

Peter: Most of the focus in deployed mitigations in the conversations so far is that the destination is colluding with the forwarding, but there are situations where that may not be the case. The destination may or may not wish to receive the information coming from the origination site. 

Johann: it would be fair to say we’re explicitly excluding cases where two parties are not colluding. It doesn’t mean we don’t think they are problems for privacy, just that they are out of scope for this specific proposal. This definition does not need to be in the proposal.

Peter: seems like a good conversation to have in an issue

### How does user intent fit into navigational tracking? Issue [Define navigational tracking as "without the user's knowledge"? #8](https://github.com/privacycg/nav-tracking-mitigations/issues/8) and PR [Added a section explaining users can agree to pass information between domains via the URL. #9](https://github.com/privacycg/nav-tracking-mitigations/pull/9)

* Jeffrey: these two issues talk about how users intend to pass information. Issue 8 came about because federated login is intrinsically identifying the user is the same on two sites. Don’t want to call it navigational tracking. At the same time, James sent the PR that says if a user agrees to pass PI between sites, the PR must not interfere with that. So, how do we deal with user intent and agreement?
* James: we have to be clear that if the user chooses something, we need to make it seamless for their choice to be exercised. There are proposals being discussed outside the W3C. 
* Tess: browsers are software. When we talk about user intent, we’re talking about what’s happening in someone’s head. In order to write software to take that into account, that has to be something the user expresses. We can set up prompts, or via implicit consent via files. Clicking a link is something people do all the time, and the intent to navigate seems clear, but that is a weak signal from the user that they intend for link decoration/navigation tracking to happen. If you click an unsubscribe link, you probably expect the destination to know your email address. But other cases are not as clear. How does intent fit in? How do you know what the intent is enough to say what the software should do? Probably no one wants to cause modal interstitials every time someone tries to follow a link. 
* Peter: at Brave, we ask consent as just one input. We don’t ask users to grant consent for things where they don’t understand the implications of the question. It’s not the only input; is not deterministic by itself. We reserve the right to question whether the user really meant it and nudge them toward a different decision if our experts think this looks like a problem.
* Mike: problem with this consent is whether the browser can understand if the user actually understands what they consented to. If a site/resource says something to get consent, that is recorded in the browser and the user can find it again, that could be visible to regulators too. The browser could help standardize this.
* Martin: seems like we’re again gravitating toward what legislation we might enact on top of this weak definition we have. That’s getting ahead of ourselves. We need to separate the things we need to protect from the things we don’t, but we don’t know how to distinguish between the two things. Don’t want to get into the legislation side of this; want to talk about what options we have available to us and making certain types of tracking infeasible at scale or with some constraints. This framework is not mature enough yet to make firm decisions on.
* John: we also see the prevalence of dark patterns to get some definition to get consent from users and leveraging that. With this work item, we have an opportunity to help the user agent to step in and have a more consistent way to ask the user about what is going to happen. Regarding more tools ,maybe have a breakout session about that? We can always try to define safe defaults. If the browser doesn’t understand, then safe default. Second step: offer APIs for the legitimate use cases that site owners can implement if they don’t like the defaults. Third piece is to look at how actual misuse happens and break that up. There are many places we can apply pressure in new ways. 
* Brian Lefler: plus 1 - concerned about the user agent visibility, and who monitors what the user agent has done. ARe there other mechanisms or consent prompts that exist before we create new proposals. Don’t want this on all the browsers and then keep backdoors open because consent happened outside the user agent (so it doesn’t know about  it)
* James: we settled on the educate-ask-once, and then capture that. Don’t hassle again unless you have to, asking again every 90 days (at max). That’s been made available and is in the public domain. Thank you, Martin, that if people chose to do something, the user agent has to respect that.
* Jeffrey: acknowledge that there will be a gap between the truth of a thing and the browsers understanding of a thing. An example of how we can get browser understanding closer to the truth is the WebID effort. Federated login will be a challenge, so there are efforts to express this in an API. we will have a disagreement as to what people are capable of consenting to. See a SWAN example: [https://current-bun.uk/](https://current-bun.uk/); 
* Tess: this conversation about intent is bringing my brain back to what Martin and ekr said earlier: if you define link decoration in an information theoretic way (http vs https, do you have the www or not) those appear to be decorative ways of conveying information. There are two different intents that are ambiguous here: user intent, and intent of the web developer. If some refer to themselves as www or not, that might just be a style choice. There might be obvious behavior that we can come to consensus is bad actors doing bad things, but off the top of our head, we can think of less obvious ways to convey information. If we mitigate the things that seem obviously bad, are we just pushing people to the less obvious things? Is this an arms race? Where does it bottom out? 
* James: would like to come back to Jeff’s statement and then come back to Tess’s. We have a number of amusing domain names, and the demo user interfaces are purely for demonstration. That demo does contain information that is being used in consultation as lawyers are reviewing the text in the [model terms](https://github.com/SWAN-community/swan/blob/main/model-terms.md). Second point, about how this information is signalled to the browser. There isn’t an API at the moment that allows that signal to be presented to the browser, and so there does need to be some new state mechanism based on data controller contracts rather than registerable domain names. Regarding the arms race concern, working on those issue so that people have choice is important. 

_(Bravo Heather!!)_

## Any other business

* Still considering the timing of this meeting, APAC folks please provide feedback.
