# Privacy CG telecon, 27 August 2020

*   Chair: Erik
*   Scribe: Arthur

## Work Items
*   Storage Partitioning - [effects of granting storage access](https://github.com/privacycg/storage-partitioning/issues/12)
    *   Anne: One of the questions with Storage Partitioning is how 3rd parties should start out. What we envision at Mozilla is that 3rd parties should still have functional storage APIs so they can still store data and make use of that. It’s just that they should not be able to share that data with different first parties. The question is the transition from partitioned data to non-partitioned data. We have an implementation, but we still need to do some in-the-field research. Are others interested in trying to make that work as well? In giving third parties functional storage APIs from the get-go. The alternative mentioned is blocking third-party storage outright. You get non-partitioned data only.
    *   John Wilander: [lawn mowers]: In an effort to try to align Webkit’s implementation with other browsers, we have on-trunk enabled partitioned indexedDB, whereas current shipping behavior is blocking IDB. We have aligned it with our local storage partitioning which means it is also ephemeral. Have not hooked up IDB to the storage access api, but we are making progress in aligning what Anne and others are shooting for in standardization.
    *   Peter Snyder: Almost exactly what Brave is implementing and testing. Sites get unrestricted access to APIs in their own storage bucket. Go through the storage access API flow. No code path in Brave where the APIs will be hard blocked. \
[pete addition: basically, its what Safari does, but including cookies (never sent on network), except no persistent partitioned storage, its top-frame length. Progress is [here (brave-browser/issues/8514)](https://github.com/brave/brave-browser/issues/8514) if of interest]
    *   Mike O’Neill: Problem with blocking 3d party storage completely is there is no session state. If you go to an iframe and the user refreshes the page or goes to another page, it would be nice to have some ephemeral storage so the iframe knows if it's on the same session.
    *   Erik Anderson: There is an interesting link to WG storage repo. Interesting to think about what should happen if storage is granted. Does it go away [after session?]. Is there a benefit for dedicated third party storage that would stick around?
    *   Josh Karlin: Chrome is trying to explore what the other browsers are doing. Good to hear that IDB is going to be consistent with local storage; sounds like ephemeral. Anne said partitioned and persistent. Do the use cases warrant it? We think the answer is yes, they probably do. Our question -- what to do about cookies. We know Apple tried and decided there were too many headaches for people. Maybe an opt-in partitioned cookie? A cookie can save you a round trip so it’s worthwhile to have for people. Like to hear the thoughts of others.
    *   Anne: The cookie thing is still a difference. We are still trying to do partitioned cookies in Fx. We may run into the same issues with what Safari team did. Maybe there are some subtle differences that will work out. Definitely an area that needs some attention. Would be nice if there is a consistent story across these various storage things. 
    *   John: Want to mention where we have seen cases where partitioned cookies could have helped, as well as cases where things were broken. It goes both ways. Just to make sure you know: we also made partitioned cookies ephemeral so that was consistent in our implementation. Do like the idea of opt-in partitioned cookies. Want something that’s sent in my requests, which of course localstorage and idb is not.
    *   Peter Snyder: What we are about to ship is not sending cookies on network calls but partitioning cookies in application space (JS land)
    *   Kaustubha: We are looking into partitioned cookies as well. Looking at the blog post. Developer confusion. Proliferation of state. (each partition). Forget third one. If have something like a cookie attribute where the dev could opt in to specific cookies. Exploring internally and trying to talk to some of the Google folks and see if that’s useful. Also useful for developers to know what the top-level page is for the embedder. Having an opt-in attribute. Potentially setting request header. And top-level site. Are others thinking about going down that route?
    *   Josh: Definitely interested in the opt-in approach if we can’t do default. Same Site was a big change and a hard change to the ecosystem but in the end it paid off and it worked. We could make Cookie default if it can refer to the top frame. Could there be a specific third party storage mechanism? Or do we want the full suite? Something we are interested in as well. \
(opt in vs default , partition some subset of third party storage or everything?)
    *   Anne: To get back to Kaustubha’s question -- concerns about exposing the top-level site. There are some existing issues around this with the ancestorOrigins property on the Location object, that Chrome and Safari exposed but Moz does not. We don’t want to reveal who you are embedded by. We did propose an alternative design -- we haven’t gotten around to implementing that. We would expose it unless the referrer policy said not to expose it. Something like that might also work for this (equivalent properties). We haven’t made much progress in spec or implementation. Neither Safari or Chrome have picked it up either.
    *   Kaustubha: Using referrer policy makes sense. If that sounds reasonable, I will try to write something up and put it out. The embedee would like to know. YouTube, eg is being used as a hosting service. In those cases the embeddee might want to know. YouTube could simply say, if you don’t give me the referrer I’m not going to provide the service. Ancestor origins -- hopefully for Chrome and Safari it’s consistent that’s sending on the header.
    *   Michael Kleber: I do like the idea of making the top-level domain available to children in some way, especially if it’s a cookie partitioning key if it’s the default and letting the top-level set its referrer policy. There’s an extra wrinkle: we often lump together subresource referrers with navigational referrers. For the privacy reasons that we on Chrome are paying attention to, being more strict with navigation referrers makes a lot of sense. Subresources referrers I have a harder time justifying trying to crack down on. Harder for top-level pages to talk to child iframes on the page. We’re not breaking the partitioning of info by site boundary. Subresources are a place where we can afford to be more relaxed.
    *   Erik: Is this repo the right place to discuss these things? Follow up offline
    *   Further discussion on zoom chat:
        *   John Wilander: Something to consider is the intent and functionality of document.hasStorageAccess() and partitioned storage. We could have additional info in the resolved promise say whether the storage is partitioned or not and potentially what the partition origin/registrable domain is.
        *   Anne van Kesteren: FWIW, I think I agree with Michael Kleber’s assessment of the subresource/navigation referrer division.And please raise issues against privacycg/storage-paritioning. It’s easy to redirect as needed.
        *   Michael Kleber: I like John Wilander's thinking on the API revealing both the is-partitioned state and the top-level site because it's the partition key, makes a lot of sense
        *   Michael Kleber: Key-Value storage where part of the key is secret makes me inherently uneasy
        *   Anne van Kesteren: Yeah, modulo that if you want to hide your location from an iframe, you should still be able to.
        *   Michael Kleber: Sure, and in that case we could report some clear signal ("unavailable") to indicate that it is partitioned but we won't tell you how.  Caveat Develeptor
        *   Anne van Kesteren: heh
*   First-Party Sets
    *   [Trade-offs of FPS vs. requiring sites to unify on a single parent domain ](https://github.com/privacycg/first-party-sets/issues/19)
        *   Kaustubha Discussion that happened in the proposal thread. Steven and Anne brought up the point that domain and URL are the thing. The existing thing that everyone understands. I think the concerns are more around do users understand what FPS thing is and would it violate their expectations? Anne suggested a common parent domain is needed. If businesses want to share user data they should be compelled to use the same domain. Want to open it up as an open discussion. In the apps world, e.g., there is a notion of vendor or developer. If you go to the play store or app store it will show a single developer can own multiple apps. Registrable domain serves as a logical boundary, security boundary. It’s still imperative that the user understand the registrable domain and that site’s logical and security boundary. Important that they understand the privacy boundary and that these sites can share info without any restriction on the browser. I called out examples. We have to separate from separate domains for things like cookies. There is every possibility that cookies can get compromised. Things like gov.uk. On the public suffix list -- every government agency gets treated as their own site. But they want to share user consent. Other case I mentioned is commercial ccTLDs such as amazon.com and amazon.co.uk. Mostly want to leave this open and see what folks thought about this topic.
        *   Peter Saint-Andre: We’ve trained people for 25+ years on registrable domains or origins as the basis for decisions about trust. Broadening that could cause a lot of confusion. We’re not saying we should force domains under a single parent. We’re not saying that’s what people will have to do. More of a business decision rather than a technical decision. Users expectations are paramount and user trust. Would I expect IMDB to be linked to Whole Foods and Zappos. YouTube would be linked to my Gmail identity? Friend said “I’m off of FB, see you on Instagram.” He doesn’t know Insta is owned by FB. We’ve trained people for a long time that that’s how things work. Changes of ownership. Procter and Gamble -- those things change all the time. P&G has MR Clean. In UK and IE, it’s a different company. I think it’s really different for us to not change things around on people that would cause significant confusion and degrade trust in the overall ecosystem.
        *   Kristen Chapman: It is about user expectation. The idea that that domain is the thing that users expect is incorrect depending on the branding. Next to impossible to find an adult in Boston who doesn’t understand that This Old House and Masterpiece/Mystery , Sesame street  are all owned by WGBH and that’s associated with PBS. Real loss if you make it about the domain. Users can interact with the community of TOH by going to TOH.com. If you can make orgs -- you need pbs.org/wgbh/thisoldhouse/community. That is less helpful and less useful to users. There is a risk, it is not completely has to be about the domain. Has to be about how these things are branded together and whether consumers understand the association or not.
        *   John Wilander: We support Mozilla’s view on this. We share that view and those concerns. Wanted to mention that there is prior art here that might be useful at least for browsers considering FPS. We had a similar proposal in the Web Appsec working group. Associated or affiliated domains. Website with an EV cert, we would show the org behind that certification in the URL bar and not the domain. It was even in a special pill in the URL bar. EV certs are not a hot thing any more for various reasons, and Safari does not have that behavior anymore. But it was something we shipped for a few years where we highlight the org that owns the website, rather than the domain. Could imagine showing both things -- a domain and a pill for who owns this.
        *   Chris Pedigo: Represent publishers. Totally agree with what Kris said with consumer expectations and user expectations. Broader than the simple domain. Comes back to common branding. How you educate consumers about who is collecting the data and using it. In terms of policy, totally agree with the concern that you wouldn't want to create a policy or standard that would allow unrelated sites to band together. That work around would be a violation of consumer expectation and would be a bad outcome. Tracking group under w3c. We landed on a policy that would try to map to consumer expectations. Required a first party to consist of a collection of domains as long as there was a common privacy policy, ownership and branding. Borrowed that approach from the [FTC’s 2012 report on privacy](https://www.ftc.gov/news-events/press-releases/2012/03/ftc-issues-final-commission-report-protecting-consumer-privacy). Looked at what consumer expectations were for first parties. That might be a good approach here.
        *   George Fletcher: General consumer range of who understands relationships between orgs varies widely. There a good percentage of the population that understand Disney, ESPN, ABC are all owned by the same company. Tech bits to throw your stuff under the same domain ends up being about busy work. Not a huge impact on publishers besides making their URLs look weird. If browsers could do something to vend data out of the privacy policy of the site that the user is going to and make that understandable who the real owner of the site. We might need to do machine codification of privacy policies. That would be a much larger more valuable mechanism than trying to segment on domains.
        *   Further discussion on zoom chat:
            *   Pete Snyder: From one Peter to another, strongly supporting this concern, and that anything that ties 1ps together in a way thats not easily grokable should be a non-starter. Agreed that domains are not ideal, but still seem least-bad (of discussed options to date)
            *   Pete Snyder: John: if it goes in the pill, how to let users know before the browser has already made decisions off the information? e.g. if we think its important for users to know, presumably its important to let them know pre-the browser making decisions
            *   Anne: John: if you show both the domain and a name, don’t you make it harder for users?
            *   Anne: The big problem with EV was that the user actually had to remember more rather than less in order to stay safe.
            *   John: I could make it harder. I just wanted to mention that we’ve ha that behavior and it’s a thing that belongs on the menu.*It could
            *   Peter Saint-Andre: Chris Pedigo: can you provide a link to the research? John: did Apple ever publish anything about your experience with affiliated domains?
            *   Chris Pedigo: [https://www.w3.org/TR/tracking-compliance/#party](https://www.w3.org/TR/tracking-compliance/#party) \
here is the Tracking Protection Working Group’s definition of “party”
            *   Peter Saint-Andre: Chris: is https://www.ftc.gov/news-events/press-releases/2012/03/ftc-issues-final-commission-report-protecting-consumer-privacy the report you mentioned?
            *   Chris Pedigo: yes, that’s it
            *   Peter Saint-Andre: Great, thanks, I'll add it to the minutes.
    *   [Desirable elements of "UA Policy"](https://github.com/privacycg/first-party-sets/issues/20)
        *   Kaustubha: There is a documented policy, a lot of common elements around the browser. What makes an acceptable set? Term ownership, and privacy models. Link in a section to the explainer in issue #20 that pulls out quotes from each of the browsers. Ownership and owned and operated. There is disagreement 14, 17, 18. Instead, UX should be the signal that tells the user how their data is being shared and there shouldn’t be an organizational requirement. Want to confirm -- are there folks on the call who have opinions on the ownership part. Also calling back to the previous discussion around user understanding? You have to give us what the branding is. You have to let the browser display owner prominently. Get ideas from the group.
        *   Paul Bannister: We represent small publishers and bloggers. Written a lot in the github. Organizational part feels impossible for browsers to be the arbiter of what that is. Disney splits ownership 50/50 with Hearst, A&E. They own over 50% of Hulu but not the entire thing. Somebody brought up: these ownership metrics are different in different countries and regions. Don’t see how (1) users can understand what these orgs are, (2) or how browsers can arbitrate that fairly. George’s point -- machine readable privacy policies are consistent and useful -- that feels like an important thing that can be grouped together. Common branding is a really good point. Common branding and privacy policies seems important to the user.
        *   Henry Lau: [from zoom chat:] had an issue with unmuting. I actually wanted to get an update on how browsers/this group are approaching the global opt-out setting that the CCPA has enabled and whether it's been discussed previously
*   IsLoggedIn - [various issues](https://github.com/privacycg/is-logged-in/issues?q=is%3Aopen+is%3Aissue+label%3Aagenda%2B)
    *   Managed vs Unmanaged Logins ([https://github.com/privacycg/is-logged-in/issues/17](https://github.com/privacycg/is-logged-in/issues/17))
        *   John W: Started working on implementation and had to work on naming. Naming is hard! To be able to reason about cases where the browser is already involved and has a good signal that there was a login, such as WebAuth, a 3rd party pw manager or a built-in manager, maybe using the credential management API. Decided to call that a “managed login”. Rest, things that browser doesn’t understand (out of band login mechanisms) are called unmanaged logins. Then browser and spec can reason about what the browser does for unmanaged logins. Ask user -- “this site claims you logged in. Is that true?” Before we start talking about managed and unmanaged logins in the spec. Are those good terms?
        *   George Fletcher: I’m not sure I liked “managed,” because of browser was doing an openid connect flow, that would be just as managed if browser were using some in-browser tech to manage login. In terms of identity, we’re trying to get rid of pws. WebAuthn is a good one. If you look at self-sovereign identity flows. Scan a QR with your wallet, and effectively your authentication is a on your device.
        *   John: Browser managed or user-agent managed?
        *   George: That would definitely help. Browser supported? The browser is involved, or browser is involved.
        *   Peter Saint-Andre: Browser mediated. 
        *   George: Had ideas: could a RP with a .well-known give a list of IDPs? Browser goes to one of the listed IDPs and browser knows an identity flow is happening .Browser could put a popup at that point. If the user says yes, I don’t need to intermediate anymore, because the user has agreed that they have logged in. Could see the browser mediating more things than just the webauthn and password managers.
        *   John: Happy I brought this up -- we won’t solve this today. At least notified community group that I want help in naming this.
        *   Peter Snyder: Depends on what is being gating behind the feature. If it’s just storage, maybe it makes more sense than if it’s a larger bundle of capabilities or permissions
        *   Zoom chat:
            *   Tim Cappalli: I think Managed is a loaded term
            *   Kaustubha Govind: "browser mediated"
            *   Tim Cappalli: I like mediated
            *   Anne van Kesteren: Browser logins vs website logins?
            *   Joey Salazar: mediated +1
            *   Peter Saint-Andre: Oh y'all already mentioned browser-mediated. :-) GMTA.
            *   Anne: Yellow logins vs purple logins.
    *   Should we standardize how we expect browsers to use this signal? ([https://github.com/privacycg/is-logged-in/issues/15](https://github.com/privacycg/is-logged-in/issues/15))
        *   Melanie: Steven raised -- should we standardize how we expect browsers to use this signal? Discussion in last meeting, several ways in which this logged in signal can be used by user agents. Maybe some folks were not aligned on how we were going with that. So we should discuss how much we want to specify in the spec how this should be used. Scope for browser innovation but also giving authors an expectation for “what does this get me”. A couple of larger buckets:
            *   Empowerment of websites where users login status is useful 
            *   Restrictions on sites where user does not have that relationship. User management for clearing long term storage.
            *   Enabling helpful user experiences -- here’s everywhere you are logged in, button to help you log out.
            *   Use cases for the developer as well. Check if you have a logged in user to know if it’s useful to ask for storage access.
        *   Andrew Knox: Fundamentally different from a lot of other privacy mechanisms. Every other privacy proposal I have seen is about cross-domain, cross-party sharing. First one I have seen about same-party. Trying to manage user expectations when they visit same place over time. Still a pretty fresh idea and quality of commentary may not be as high as for cross-site things where people spend a lot of time thinking already. Idea between being remembered and logged in is not a perfect match .Not sure how much of that is just a language thing. It is tricky there is obviously a sense of user expectation. Not clear that isLoggedIn is the right method for that. It’s really difficult when you start thinking about browser-mediated logins. People’s ability to have first-party relationships with the website. Dashboard relationship with logins seems a lot nicer. A lot of concern that this may lead to some weird expectation user flow things. Much more restrictive than people’s expectations. May inhibit a user’s ability to do business with a website over time. One extra click could lead to a huge drop off to people’s purchases etc. Beneficial to a lot of parties involved if that’s a very smooth transition.
            *   Zoom chat- Peter Snyder: Andrew: Fingerprinting concerns come up often, which are often about 1p re-identifying.  Happy to point to PING review
        *   John: Address what Andrew brought up. Don’t think the isLoggedIn signal is purely about a first party and user relationship. We have two really challenging cross-site tracking issues. One is bounce tracking (through redirects) -- a site becomes a first party for a fraction of a second. By knowing whether the user is logged into that site or not we can protect users from bounce tracking. Second, a lot of tracking being stored in first party storage. Both of those are things that we could tackle from that angle of trying to disentangle login state and not logged in state, and putting restrictions on the persistence of storage. As Melanie mentioned, it also goes the other way. If we know the user is logged in, we can help the user stay logged in, and use it as a stepping stone to allow more powerful things to happen or accepting login flow that we would otherwise say is bounce tracking. Empower sites. My view is if we can keep isLoggedIn as pure as possible that it’s only about allowing sites to inform the browser as to when the user is logged in or logged out, then other specs can refer to that. We’re going to gate our stateful feature on the isLoggedIn signal. Other states would leverage isloggedin.
        *   George: There are many different ways that identity flows get used. We have talked in the past about FPS but autoblog.com and aol.co.uk and aol.com all use the same identity provider. It’s known to the user when they click the login button. There’s no confusion or trying to hide anything from the user. Technically cross-domain where they are all part of the same organization. Academic communities have completely different deployment models. This site said the user logged in isLoggedIn, maybe that’s the base of the building block, but there are many different deployment cases that are not true cross-party.
        *   Steven Englehardt: Push in the other direction -- feel like we should standardize this signal as part of isLoggedIn. Concerned that sites will have expectations. Safari gates webvr behind isloggedin. Firefox says that’s not a great idea. Concerned that it’s hard to get behind the signal without knowing how it will be used.
        *   John: You’re right -- if they go read up on this things, use this API a bit gets set and you can log out. Maybe we can do both. Make sure that we document any dependencies right there in the isLoggedIn. Maybe we shouldn’t standardize it in isLoggedIn. Storage implications should be in the storage spec. Should have a section on dependencies where you can go to one place and know when to use isLoggedIn.
        *   Zoom chat:
            *   George Fletcher: Yes, I've involved with WebID and in discussions with Sam on that effort :)
            *   From John Sabella: George:  but to a consumer, this is actually dangerous, because a bad-actor can use that feeling of safety in a bad way
            *   George Fletcher: If the IDP does not allow any arbitrary site to request authentication then I'm missing the "bad-actor" attack
            *   John Sabella: It’s what is displayed on the UI to a consumer, from an email link (for example)
            *   George Fletcher: @John IDP's don't support open-redirects so again, if the user gets a phish email with a link and they click it they end up at the IDP Login page and will only be redirected registered URLs. \
Happy to take the discussion offline:)
            *   John Sabella: Yes, thanks :)
    *   
    *   Discuss on github and/or during a future call:
        *   Cater for existing logins, a.k.a. upgrade to IsLoggedIn ([https://github.com/privacycg/is-logged-in/issues/18](https://github.com/privacycg/is-logged-in/issues/18))
        *   Support "Remember me" functionality ([https://github.com/privacycg/is-logged-in/issues/9](https://github.com/privacycg/is-logged-in/issues/9))
    *   


## F2F announcements
*   Tanvi: The F2F is Wednesday and Thursday Sep 16-17 8am PT to noon. We are trying to do one small break, one large break. 5 work items. If there are any new proposals. If there is a topic you would like to discuss, please add to the proposals repo and mark it “Agenda+f2f”. Coming up sooner than we expected. In 3 weeks! Sent an email to editors. Editors -- please consider how best to use the time to move your work items forward.
*   Peter Saint-Andre: Is there a time we would like folks to get their github comments in? We don’t want people to post a minute before the meeting.
*   Tanvi: Will discuss with editors to make a deadline. Give people review time that’s not just the day before. There have been some interesting Zoom chat which we have tried to capture in the notes. We do have the privacy CG slack channel. Want to encourage folks to discuss issues there. 
*   Erik: If you don’t have access to slack, just email our chairs.

### Attendees
1. Erik Anderson (Microsoft)
2. Theresa O’Connor (Apple)
3. Sam Weiler (W3C/MIT)	
4. Kaustubha Govind (Google)
5. Arthur Edelstein (Mozilla)
6. Tanvi Vyas (Mozilla)
7. Scott Low (Microsoft)
8. Michael Kleber (Google Chrome)
9. Wendy Seltzer (W3C)
10. George Fletcher (Verizon Media)
11. James Hartig (Admiral)
12. Sean Harrison (Google)
13. Mike O’Neill (Baycloud)
14. Jason Nutter (Microsoft)
15. Eric Mwobobia (ARTICLE19) 
16. Kris Chapman (Salesforce)
17. Christy Harris (Future of Privacy Forum)
18. Paul Bannister (CafeMedia)
19. Pete Snyder (Brave)
20. Lionel Basdevant (Criteo) 
21. George London (Survata)
22. Melanie Richards (Microsoft)
23. Tim Cappalli (Microsoft)
24. Chris Needham (BBC)
25. Andrew Knox (Facebook)
26. Bill Densmore (ITEGA)
27. Jack Frankland
28. Russell Stringham (Adobe)
29. John Wilander (Apple WebKit)
30. Paul Jensen (Google)
31. Eli Grey (Transcend)
32. David Fischer (Read the Docs)
33. Steven Englehardt (Mozilla)
34. Russ Hamilton (Google)
35. Theodore Olsauskas-Warren (Google)
36. Brian Campbell (Ping)
37. Arnaud Blanchard (Criteo)
38. Max Gendler (NYTimes)
39. Ryan Avecilla (Neustar)
40. Mike Lerra (Google)
41. Christine Runnegar (PING co-chair)
42. Steven Valdez (Google)
43. Anne van Kesteren (he/him; Mozilla)
44. Aram Zucker-Scharff (The Washington Post)
45. Nicolas Arciniega (Microsoft)
46. Jordan Mitchell (IAB Tech Lab)
47. Josh Karlin (Google)
48. Bartłomiej Romański (RTB House)
49. Chris Pedigo (DCN)
50. Jeffrey Yasskin (Google Chrome)
51. Kate Cheney (Apple) 
52. Mark Xue ( Apple) 
53. Henry Lau (Privolta)
54. Mike Smith (Hearst Magazines)
55. Joey Salazar (ARTICLE19)
56. Sam Macbeth (Ghostery)
57. Marshall Vale (Google)
58. John Sabella (PubMatic)
59.  Jade Kessler (Google)
60.  Shivani Sharma (Google)
61.  Taha Dharamsi (Postmedia)
62.  Allan Spiegel (Adobe)
63.  Peter Saint-Andre (Mozilla)
