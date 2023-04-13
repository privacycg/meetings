<!-- Output copied to clipboard! -->

<!-- Yay, no errors, warnings, or alerts! -->


# 2023-04-13 Privacy CG Meeting



* Chair: Erik
* Scribe: Cameron Boozarjomehri, Erik


# Agenda



* [Bounce Tracking Mitigations #27](https://github.com/privacycg/meetings/issues/27)
* Any other business
    * [Should FedCM use the Login Status API? privacycg/is-logged-in#53](https://github.com/privacycg/is-logged-in/issues/53)


## Notes



* Ben Kelly - Chrome Bounce Tracking Mitigation Demo
    * Ben Kelly
        * Slides are at [https://docs.google.com/presentation/d/1H79kGmFaRlN1icMYLj_X8-o69QDYCKBk8_aYo96-Hvw/edit?usp=sharing](https://docs.google.com/presentation/d/1H79kGmFaRlN1icMYLj_X8-o69QDYCKBk8_aYo96-Hvw/edit?usp=sharing)
        * Update on Bounce Tracking work we are doing in chrome
        * This is an elaboration of a presentation I gave at TPAC, hope to spread general awareness of this topic. Went over what Bounce Tracking is and our general plan to tackle it.
        * Since TPAC (past September), we moved the explainer into Privacy CG as part of the nav-tracking-mitigations repo. Started collecting use cases reported to us at TPAC and discovered since then. Issue tracker with labels that has at-risk use cases. Collected metrics, ran experiments, hone in on a good solution. Have been implementing and prototyping in Chrome for a minimum viable product/initial launch.
        * Questions on solution space going into this– what is a good trigger for identifying that a bounce might be happening? Some different options we had in mind. In Firefox I believe if storage is on a site at all and it redirects at all. We wanted to look a little closer at these different options.
        * Also what about if a user cares about a site and doesn’t want to delete it? Firefox uses interactions and we haven’t thought of a good solution that doesn’t use interactions yet. Maybe some future things. I want to highlight that’s an area that could change if we come up with something better. Different time thresholds for interaction.
        * Ran an experiment. We didn’t have a lot of other ideas for interaction, so largely focused on trigger conditions and time thresholds. Experiments collecting data, doing simulated deletions, keeping track of what sites are deleted. We have a “UKM” mechanism (URL-keyed metrics) that plug into the CrUX report that Chrome reports. Caveats– only using cookies for storage (but we think that’s the majority use case). UKM doesn’t give us perfect visibility into what sites were impacted because we’re really only allowed to see URLs that are considered “public”; doesn’t let us see sites users don’t normally visit. Good at giving us info that we’re hitting sites that shouldn’t be deleted, but don’t have perfect info on all sites.
        * Going into triggering conditions– investigating 3 different ones. Storage only, which is what Firefox does. Basically look for first-party storage access which means cookies today, but in future would include localStorage and IndexedDB. Also looked at bounce with a redirect in the chain that the user doesn’t really see. Wrote a bounce detector that handles both server-side and client-side redirects, regardless of if they use storage or not. The network stack can be stateful and so visiting any site is stateful, so we’re considering this option because of that.
        * The more complicated condition is “stateful bounce” which means it accesses storage during the redirect process (either on server side request or during client-side navigation which can access more storage types).
        * The results of our experiments on triggering condition–
            * Storage-only one hit many more sites than the other two triggering conditions which you’d expect since it’s not as strict a filter as the other ones that only look at things that bounce. Some concerning things on the site list, including some popular sites like Tik Tok. Also had other sites where we couldn’t explain how they were doing bounce tracking and don’t have a great understanding yet. Needs more investigation to either discover bugs in our implementation or discover what’s going on. Not going with this path right now.
            * Bounce only– showed some stateless pure redirection sites that were not storing state. Clearing the state, including HTTP cache, could result in performance penalty. For those reasons we’re worried about hitting performance penalties for sites we don’t think are actually tracking and could be high volume. Also chose not to go this route.
            * Stateful bounce– hit fewest number of sites which was expected given the tightest filter. Didn’t see problems with it, but has downside of being the most complicated. Would have been nice to go storage-only which aligns with Firefox, but results drove us toward this more conservative outcome.
        * Protection mechanism– user interactions are our best idea right now. Would like something else because there are known downsides like incentivizing interstitials that get users to click on the site. We don’t want to incentivize that, but it’s arguably an improvement over them getting covertly tracked and they will understand there’s a site there and cause some friction. Don’t know if these will become common because of the friction; we’ll have to monitor it in the future. The other downside is that users may not understand the implications of clicking on a page. Not hard to get a user to click on a page but, again, at least we’re showing them a page and they can see an URL in the omnibox. In the future, something like FedCM– which creates an authentication concept in the browser that sites can integrate with– could allow us to move away from interactions. Would have to see a lot of adoption first, though. Also could look at if a user has been looking at a site for a long time, but it doesn’t really replace interactions because there are use cases like authentication where you do the login quickly without using the page for a long time. So, going with interactions.
        * Time thresholds– three constants:
            * How long do user interactions protect a site from being deleted. Going with 45 days which we believe aligns with Firefox. Tradeoff between too short and too long. Too long and the friction goes down. Tuning; this is our initial approach.
            * Grace period from when we detect the stateful bounce to when we delete it; gives the user time to interact with it first. Going with an hour which is based on experiments that look at how long it takes for interaction to come after a bounce typically.
            * Timer runs every one hour. That’s the fastest a site could get data deleted. At slowest, it could be almost two hours.
        * Other notes– we made the decision to enforce bounce tracking only if the site is allowed to be a third-party on other sites. If the user allows 3p cookies everywhere (current default), there will be no additional protection from this implementation. Will only affect users that have opted into 3p cookie blocking (or in Incognito by default we block 3p cookies). Part of this is because we view tracking protection as a way of simulating 3p cookies, so it’s aligned with that intent. Also why we don’t do this if 3p cookies are allowed. It also allows us to have users set cookie settings exceptions across their whole fleet and they can use that to disable bounce tracking where it might matter for them. The implication of that is when Chrome does block 3p cookies by default, bounce tracking will roll out with that.
        * Cookies only right now but will expand to other storage soon. Also clearing HTTP cache in addition to cookies and DOM storage.
        * At risk use cases–
            * Email-based workflows (issue 28). Raised by Salesforce; they have a product that sends emails to people which include links that call out to Salesforce which then redirects to the proper step in the workflow that the user should be in. Very similar to bounce tracking situation and the user may not have ever visited Salesforce. This is one area where we have some ideas how to mitigate in the future. Want to get more feedback on if it gets used extensively outside of enterprises; otherwise cookie setting exception might work. Could also look at having Salesforce indicate destination should get partitioned storage.
            * Uncovered in experiments that there are auto sign-in approaches in auth providers (issue 36), e.g. using TLS certs in managed mobile devices. Active Directory integration in some cases. Basically, users are following an SSO flow but never interact with the SSO site, so bounce tracking mitigations will delete those cookies. Very enterprisey/managed solution, so cookie exceptions a reasonable solution there wince enterprises can set the policies. If the cookies get deleted and the user gets auto-signed-in next time it seems more like a perf penalty than a functional issue.
            * Link shorteners (issue 27): feels a little like cross-site tracking. If we pursue partitioned storage approach that bit.ly could opt into we could possibly add something to the web platform if it makes sense.
            * Again, not enabled for all users yet– only those that turn off 3p cookies yet.
        * Project status–
            * MVP implementation is nearly complete. Will start asking for external feedback soon. Have been writing a specification which are in the same place as the bounce tracking mitigations report. See draft spec link in slide deck.
            * Next steps– TAG review, based on feedback and if nothing major comes up, will ship MVP. Want to ship something before 3p cookies get blocked in Chrome by default to avoid the ecosystem moving from 3p cookies to bounce tracking. Solution will evolve as we learn.
    * John Wilander: thanks for presentation, interesting stuff as always in this area. Some callouts from Safari since we shipped this ~6 years ago. In our case, we age it out after 30 days of browser use. Two questions:
        * 1. Delayed client-side bounces. We saw trackers move to after we shipped this. Took a step in 2019 to mitigate it. Have you seen this?
        * 2. I think you’re referring to block 3p cookies, but Chrome is going to go to partitioned 3p cookies, right?
    * Ben Kelly: was worried I would get something wrong but understand Firefox better. I did call out that Safari has a leading implementation at TPAC; thanks! We’ve said we plan to deprecate 3p cookies. We have a CHIPS proposal which is an opt-in thing. Won’t partition 3p cookies by default.
    * John: I see, so you mean they will be blocked by default but can opt into partitioning.
    * Ben: for delayed bounces, are you saying a client redirect uses document.location. Are you saying there’s a long delay there?
    * John: you land on the page the user wants to go to, and 3 seconds later JavaScript bounces you off to a tracking domain and back to the page. To circumvent our detection of the bounce. Which was a super bad user experience and we wanted to do something to tell them to stop this.
    * Ben: we do track across client-side redirects even if there’s a delay. We have a threshold for if we detect it which is tunable. I’ll have to think more about that scenario; haven’t looked at it specifically. Trying to confuse what is the initial site with an auto redirect away and back outside of a redirect chain. I think that’s a case we would handle in our current solution.
    * John: you’re thinking even if not treated as part of nav, but it would be treated as a standalone bounce?
    * Ben: yes. Could be part of initial redirect as one long chain or a separate one as long as it’s a separate domain.
    * Tim Cappalli: one quick comment on enterprise management. The current browser management model you can’t assume is an escape hatch for the majority of orgs. You need to push it at the OS level, MDM. Can’t push down policies for a specific profile. Enterprise architectures also apply at universities where many devices aren’t managed. Not for this group to solve enterprise policies, but not a complete escape hatch.
    * Ben: I would love to learn more, so if you can engage on that issue I linked it would be great. It feels like for users that are not in a managed environment, they’re more likely to be in normal SSO flows where they type in credentials and our normal interaction heuristic will detect them.
    * Tim: there’s a large gap between those two users. Accessing work resources from personal devices and if we want these policies to be escape hatches it can’t require device management.
    * Cameron: there’s a lot of research & education community in FedCM community who have captured this already in FedCM login flows. Might get a lot of context there on how bounce tracking may or may not be crucial for them to manage a host of devices.
    * Ben: if the device is not managed, how are they getting automatic login?
    * Tim: you can get Azure SSO without being managed. The ideal solution is when you create a new browser profile and have policies come down directly there. Reduce some restrictions there on that profile specifically. Not sure where to have conversation in W3C on that, but it’s incomplete right now.
    * Ben: might need to be some web site changes at some point that we need to talk about through a new feature or in this case it would be reasonable for auth site to say “I’m logging you in as user X, click here to acknowledge.” Wouldn’t be a large change or very punitive in terms of user friction. I acknowledge I’m ignorant in this space right now.
    * Aram: on link shorteners. Generally I see these as a vector to identify users cross site. I realize they’re not only for that. I’m unclear what the need is for them to identify unique users to begin with. I understand this would potentially turn that off. Have you had any conversations with any link shorteners? Or we just think it’s going away and don’t know if it should?
    * Ben: it would go away and unsure of if it should. I can see both arguments. If a user visits a site in a first-party context it should be able to remember the client was there before. Depending on criticality of this, could do nothing and letting this use case be blocked. Have some other ideas that would allow link shorteners to opt into partitioned storage depending on the redirect destination. Would add complexity. There are options and we need more feedback from that group.
    * Aram: I’ll note from the perspective of a publisher, I don’t view this as a particularly desirable use case. No need for bit.ly to know who users are when they’re routing to someone else’s site. Not sure what reason would exist other than to do cross-site tracking of some sort and to do fingerprinting. I’m generally opposed to preserving link shortener’s capability to detect unique users.
    * George Fletcher: there are identity flows where the IDP is largely an intermediary and not something the user directly interacts with but which may be the authoritative entity for the user’s authenticated session. It seems like the user would get bounced to the IDP, bounced to somewhere else (my company to my company’s IDP) but then I get a Google auth entity. How do we handle that use case? Research and education would have similar use case and a common pattern from the identity side of things. Is domain here eTLD+1 or a more granular domain that the user is interacting with? Often times people will put their IDPs on a separate FQDN from their sites, and sometimes even different domains (which FPS might help from a Google perspective) and would maybe have interaction or not.
    * Ben: it’s eTLD+1 and schemeless site. Only eTLD+1, not scheme. By default cookies get sent to both HTTP and HTTPS. Explicitly not using FPS because we think it would adversely benefit companies with large sites that get interaction. I fully believe there are identity flows I’m not aware of, would love an issue to be filed with more details. I did present this to the FedCM group at TPAC and they didn’t raise that concern there, so I don’t feel like an expert to speak about it here. Please do file an issue so we can dig in.
    * George: worried about scenarios where user doesn’t directly interact with IDP. Is it tracking? Yes, but for benefit for user to not go through MFA flow every time.
    * Paul Zuehlcke: John touched on this– for user interaction flag and expiry you said it’s 45 days. Is that real time or days of browser use? Firefox uses real time and we’re looking at days of browser use which has some advantages like if the user goes on vacation and later returns, the counter will not have reset. Also, you’re looking at stateful bounces specifically, can bouncers still use state like the HTTP cache to do tracking?
    * Ben: we’re using real time calendar days, not time of use of browser. We could consider it. It extends out the time and makes it harder to reason about for site services how often to send people through flows again. Pros and cons. Mostly using real time because it’s simplest right now. \
 \
We’re not currently looking at networking state for bountiful tracking. If we include that it would be the “bounce only” heuristic and every bounce would be stateful. Appealing from that perspective, but it’s harder to use it for tracking and don’t believe it’s happening at scale right now. Perf penalty to wipe the HTTP cache for pure redirector sites. Today only looking at cookies + quota storage-based storage.
    * Paul: do you consider it out of scope for this proposal or still up for discussion?
    * Ben: as an ongoing effort, still an ongoing discussion but need to be careful about pursuing that approach. Open to adjusting our heuristic but want to be careful to penalize who should be penalized and minimize tax on legitimate things. We have a little bit of time pressure on our side because we want to get something out before 3p cookies are off which is currently stated to be sometime next year. Open to evolving over time, particularly if we see people abusing network state.
    * John Wilander: an anecdote on why we moved to days of browser use instead of calendar time. It was early feedback on Intelligent Tracking Prevention where someone brought up the vacation point. They said “what if I go to jail for half a year and I want to be logged in when I come back?” Our 30 days of browser use has been very successful at maintaining logins. Was a lot of worry in 2017 that we would delete logins users wanted to maintain and there was a push that it should be 90 days. But has been successful at maintaining logins and could probably even go lower (though no plans in that direction). Re: George’s comments– our thinking on domains that need to hold state about you and need to do these bounces; we think it’s not great and we want users to be aware of sites that hold state about them, especially if it’s an IDP. Can we get users to go to the IDP site and acknowledge what the IDP is going to hold on about them? Or is it necessary for some reason to be unknown but still hold on to state?
    * George: there’s a service “Hello” that’s a sign in service that supports social login and whatever else. If I’m a relying party and I want to get people to log in, I could integrate with one thing rather than integrate with a ton. Is there potentially badness there? I don’t know. I can understand RPs wanting to leverage infra. In OpenID foundation we’ve tried to make things standard but still idiosyncrasies in IDPs. If you must click on a page to acknowledge that this publisher is using this service to get logged in and then don’t need to see it anymore, maybe sufficient. But maintaining that relationship over a long time is, if you call a customer care rep they tell you to clear their cookies and state. If we have a way to establish the relationships in a pattern that isn’t commonly cleared, I don’t personally have any issues with something that provides transparency to the user as long as it isn’t an ongoing continuous basis. If the site says we’ll do X, Y, and Z to ensure you have a good experience maybe that is sufficient to move the industry overall but it would have to be something one time or infrequent over something that happens every 30 days. I’m not involved right now in intermediary IDPs but I know it’s common.
    * John: the tradeoff between nefarious interstitials and real use cases is a concern. Thanks.

* Erik: due to the lack of interest and topics for a virtual F2F in May, we're choosing not to have one. Our next opportunity will likely be at TPAC in the Fall.


## Attendees
1. Cameron Boozarjomehri (Mozilla)
2. Erik Anderson (Microsoft Edge)
3. Johann Hofmann (Google Chrome)
4. Kaustubha Govind (Google Chrome)
5. Ben Kelly (Google Chrome)
6. Matt Reichhoff (Google Chrome)
7. Russell Stringham (Adobe)
8. David Dabbs (Epsilon)
9. Tim Huang (Mozilla)
10. Chris Fredrickson (Google Chrome)
11. Justin Brookman (Consumer Reports)
12. Paul Zuehlcke (Mozilla)
13. Chris Needham (BBC)
14. Achim Schlosser (European netID Foundation)
15. Michael Kleber (Google Chrome)
16. Allan Spiegel (Adobe)
17. Andrew Aikens
18. Claude Vervoort
19. Sam Weiler (W3C)
20. Tara Whalen (Cloudflare)
21. John Wilander (Apple WebKit)
22. Jeffrey Yasskin (Google Chrome)
23. Nick Doty (CDT)
24. Tim Cappalli (Microsoft Identity)
25. Sam Goto (Google Chrome)
26. Aram Zucker-Scharff (The Washington Post)
