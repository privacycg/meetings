# 2024-09-26 Privacy CG TPAC meeting
* Chair: Erik, Martin, Tess
* Scribe: Nick Doty, Erik


# Agenda
* Intro (10 min)
* Status updates from open work items (20 min) All of the Editors
* Non-Cookie Storage: [https://github.com/privacycg/saa-non-cookie-storage](https://github.com/privacycg/saa-non-cookie-storage) (15 min) Ari
* Consent Management: [https://github.com/privacycg/proposals/issues/48](https://github.com/privacycg/proposals/issues/48)  (15 min)
* Google’s cookie plans and implications for our work (15 min)
* Any other business (balance)


# Notes

Slides for CHIPS, SAA for non-cookie storage, Storage Access API Headers, requestStorageAccessFor, and Google’s cookie plans - 


### CHIPS

Dylan Cutler: metrics of CHIPS adoption, 11% of web requests have a partitioned cookie

Firefox  implemented CHIPS.

Disabled CHIPS in Android WebView, because surrounding application code can’t query cookies, were seeing real-world application breakage.

MT: clarifying.

Partitioned attribute is no longer considered.

Aaron Selya: implemented Ancestor Chain Bit*** , to align partition key of the cookie with the storage key. Provide some protection for top-level sites from A→B→A (nested embeddings) events.

Dylan: partitioned pop-ins and cookie layering (discussed at breakout sessions).

Chrome current implementation has a memory limit per site and per partition of 10kb. Suggestion to lower this to 1kb, but some metrics/developer feedback indicate it might not always be sufficient. Right now cookies have a maximum size of 4kb. Easier to have a memory limit with more cookies but not maximum size ones. Memory limit could be implementation-specific. Least-recently-used algorithm for eviction. Sites that are more active are less likely to be affected. Or could use a priority attribute for importance of cookies when storage limits are hit.

Matt F: total storage requirement seems less than 100kb, how many sites was that spread across

Dylan: I don't have exact numbers, but I think around 100

Ben V: eviction language is around “MAY”

Dylan: status of webkit support? any blockers for webkit, including on the partition limit?

John Wilander: we provided an update in WebAppSec. first open source patch for implementation of CHIPS landed in WebKit. implementation has started. largely an HTTP feature, and the HTTP stack in Safari is not part of the open source repository. Should be available for testing by developers in Safari Technology Preview.


### Login Status

John W.: Login Status API discussed for quite some time in FedID CG/WG. Decision was to not move Login Status API to that group. Use case for federated credential management is narrow. They want a self-asserted signal. But will try to harmonize the shape of the API. Work of Login Status continues in this group.

Johann: wrote-up longer summary, status of this work: [https://github.com/w3c-fedid/login-status/issues/8](https://github.com/w3c-fedid/login-status/issues/8) 


### Private Click Measurement

John W: intend to move to Private Advertising Technology Working Group, if they are interested. Don’t currently have another implementer, but some similarities to Google ARA.


### Storage Access API

Johann: good progress! updates to cookie RFC at IETF. Should soon be able to open PRs on those draft specs for Storage Access.

Added an explainer for the FedCM-as-a-trust-signal proposal, auto-granting when the user accepts a FedCM prompt.

document.hasStorageAccess correctly reflects the third-party-cookie-blocking status of the browser, so that if 3PC are enabled then hasStorageAccess correctly says you have it.

Firefox ships this version of the spec.

Johann: Storage Access Headers and SA Non-Cookie Storage

Only necessary because of some security enhancements to Storage Access API, for availability to non-JavaScript environments where the permission has already been granted. For example, an image could use that functionality too. Optimize some latency to avoid loading more iframe. Currently incubating in Privacy CG. Feedback received on the explainer. Implementation in Chrome for Origin Trial soon.

** link to slide [they are all from the same deck ]

** link to demo site

Ari: access to localStorage and similar. Additional parameter to requestStorageAccess. Available in Chrome and Opera, and preview in Samsung Internet. usage is much lower than normal rSA. But want to see if there’s appetite from other vendors to adopt it. Made to be modular, so other vendors could implement just localStorage or just INdexedDB if you want.

Ben VanderSloot: Mozilla interested but not high priority.


### rSAFor

Aaron Selya: looking at the scope of expanding the scope of rSAFor, subresources that can’t execute JavaScript. current implementation is attached to Related Website Sets. expanding the use of indicating which sites it would be okay if a site called rSAFor. *** CORS and security concern is beyond the scope.


### Navigation Tracking 

Ben Kelly: bounce-tracking mitigations:



* spec issues from Mozilla folks
* finally have WPT tests
* currently investigating how to handle ETags/HTTP cache, but afraid about performance regressions.
* security concerned about some cross-site leaks.
* currently using interaction on the entire domain, but hard to distinguish dual uses of that domain. researching options. could imagine lists of domains, or technical solutions on client, or aggregated classifiers.

Ben to provide a [link to the slide with links](https://docs.google.com/presentation/d/1uH_KONTmzKHbBm4uoGGEAfqUWeASIE1VIgd6_x1eB1g/edit?usp=sharing).


### Global Privacy Control

Aram: presented to PING earlier this week and heard additional feedback. In progress on additional changes to explainer to provide legal contexts. Heard feedback to engage international coordination and hope to engage researchers to give more information about the potential use (or lack of use) for GPC outside of the US.

Aram: still open issues, but the editors don’t think they are likely to require modification of the spec. think the spec is ready to move to the Working Group in its current state. We are considering a slight text change about use.

Martin: Working Group proposal formation had a Formal Objection, a Council was formed. Believe it should be a relatively short Council process and move forward soon. Once the WG is formed, we can move that work item to the Privacy Working Group, since it will be directly in their charter.


### Cookie Consent Management

Andrew: we are all familiar with annoying pop-ups, even if good intention there isn’t great execution. Cookie popups require lots of time to deal with, maybe 32 hours per year per user? Doesn’t provide a strong privacy benefit to users. **** 

User ad-blocking is one solution, but not a solution because it might break a website, doesn’t work consistently.

Instead, provide consent in the browser for faster browsing. Users can select a preference and apply it to all websites simultaneously if they want, or customize. ****

How it works: provides an object for the webpage to interact with, that provides the content preferences. Browser will render ** for the user.

For more complex things, ****

Now talking to the EU Commission, and to you folks today. Talking with big publishers here in Europe to see how we can help them as well. 

Cookie pop-ups will be here forever, so it’s time to do something about it.

Ben v: sounds great, in theory. How do we get more compliance? Is there a legal mechanism that would be useful? Are advertisers (IAB) supportive? Not sure about how to do the consent on the browser side, but already have one mechanism with some legal enforcement available in GPC.

Andrew: talking with the EU Commission. hope there will be clarifications. hope to take this to popular frameworks for cookie management. Right now websites have to do a lot of work to comply.

Aram: how does the site get and retain a receipt of consent.

Andrew: as a JavaScript object, anything on the page can access it.

Aram: warning the user about cookies getting set – if you alert the user on every cookie that gets set, it would be a bad experience for the user.

John W:

1 – the consent data that you capture in this object looks rich. could it be informative/sticky that could be presented to the site. is it a replacement for cookies/fingerprinting

2 – the browser couldn’t do full cookie-blocking if the user has opted out.

3 – some sites are incentivized to keep asking until the user accepts all. sites might not accept something that’s easy for opting out.

Andrew: 

1 *** [something about timestamps that the scribe didn’t understand]

2 the browser could try to determine whether it would break the website, or try to understand the purpose of each cookie. mostly doesn’t break websites. trying different things.

3 

Nick Doty: It’s good to try to address the problem. We’ve been talking about this a while, maybe because it’s challenging. When I heard this suggestion in the past, one problem I heard from lawyers was that consent needs to be quite specific. And so configuring in your browser that you auto consent to something that might be different on every site might not be acceptable to people determining GDPR compliance. Do you have some different signal on this than we heard in the past? What would make auto consent acceptable?

	My understanding is that “Accept all” is not likely to be acceptable.

Andrew: It may be a little more complicated. We have our proposal in the Privacy CG GitHub. Feel free to read the advanced part. If the web site is relying on a complex consent that can’t be represented, the browser will probably just let the web site display it.

Martin: A number of challenges in doing this particular thing. I encourage folks to talk to Andrew and help him develop the proposal a bit better, the challenges that exist in the space, but I don’t think we’re quite at the point where we would say the group will take this on as a work item. But that could change as it develops and becomes more complete.

John: could call it a proposal, and comment on GitHub. This has come up a few times and would be good to track CG feedback.

Martin: If you want to do that Andrew, we can create a repo with an issue tracker so that they can get engaged with you there. Will follow up. Thanks for the presentation!


### Google’s cookie plans and implications for our work

MT: Some may have heard that there has been a change in the Privacy Sandbox and cookie plans. Some questions can be answered and some can’t. This is a safe space.

Johann: Chrome/Privacy Sandbox. some stuff we aren’t ready to announce today. want to give an update and overview on the announcement, and discuss with you.

Recently announced that instead of deprecating third-party cookies by default for all users. Instead provide users with an informed choice about whether to use third-party cookies or not. We will share future updates.

Does not mean we are stopping investment in privacy-preserving API work or ways to support the ecosystem in finding alternatives to third party cookies. We are here and present in Working Groups at TPAC, which is evidence that this work continues. Should help prepare for more browsers that restrict third-party cookies in the future. A lot of APIs are already shipping in Chrome whether third-party cookies are blocked or not.

What does this new stage look like? All third-party cookies being blocked would have been easier to reason about. But now users will have a choice. CHIPS will continue to partition cookies. Storage Access API will also work differently, will use prompt/RWS if cookies are blocked, but will have an autogrant for users that allow third-party cookies. For bounce-tracking, it will only be enforced when third-party cookies are blocked. 

Difficult to handle the migration from unpartitioned to partitioned contexts. How did we tell which users are blocking cookies and which aren’t? This is now an even more acute problem for developers. We are developing provisions for this, like hasStorageAccess, which will return true/false based on that state. Storage Access API headers will provide similar visibility/functionality. Provides an easy/elegant way to check whether they can use third-party cookies in a particular context.

John Wilander: thanks for these details. as a browser that has shipped blocking, we were looking forward to more incentive for developers to adjust and implement more private alternatives. that will be a much tougher challenge now. why won’t websites just try to persuade users to enable third party cookies and not adopt the more private alternatives.

Kleber: we are very aware of this problem. yes, some websites will just block users and tell them they have to use Chrome with third-party cookies turned on. somewhat similar to the browser wars and “this website works best in this version of this browser”. The moral we have learned is that it’s pretty hard to get users to make a global change to their configurations. Don’t think many websites will be able to convince users to make a global change to their browser, but we might be wrong.

Ben: continuing to invest in alternative APIs. want there to be a true choice, not be forced by websites.

Isaac Foster: from Microsoft Ads. for any additional expansion of Mode B, and getting industry feedback. timeline on when there might be additional ramp-up in 2025, like change in defaults?

Kleber: Mode B was a term for 1% of users who have third-party cookies turned off by default in Chrome. No information on timeline. CMA published comment earlier this week where they indicated that they hoped to have some consultation on their opinion on this new plan in Q4. We eagerly await that.

Isaac: any thinking on what the UX would be? different for different countries or regions?

Kleber: many people are sure to have many strong opinions on the subject that we will listen to.

Ben: devil is in the details for the UX on this plan. a checkbox in settings vs an angry modal. I feel like you want to deprecate third-party cookies and that it’s a good thing for the web, the TAG agrees. Is there a threshold of people saying yes that would be successful?

Kaustubha: a tough question. agree that devil in the details and important. CMA has a process and talking to regulators. generally our principle is informing users so that users can make the choice that makes sense to them. 

Nick Doty: This seems like a pretty significant change in plans after years of community work on this process. Regulators, including ICO in the UK seemed disappointed by the change despite the suggestion it was informed by the regulators. If that significant of a change is possible after this many years, is there some reason we should have confidence in the current statement of plan? What might change the plan in the future?

Kleber: [The statement that the UK CMA issued earlier this week](https://www.gov.uk/cma-cases/investigation-into-googles-privacy-sandbox-browser-changes) was that they have heard this new plan and they still think it is still within the domain of competition regulators to take a look at it and the associated competition concerns. If anyone hoped this change would make the competition regulators say they don’t have an opinion, you would be sorely disappointed. We started Privacy Sandbox 5 years ago and said we would listen to large swathes of the ecosystem, including people with very different opinions; many of our APIs and decisions have been shaped by that. Still going to be the case and will continue to listen to feedback. The ability for users to make a well-informed choice is hard, UX is hard, “well informed” is hard, understanding the implications of your choice is difficult. But letting users make a choice seems like a good path right now.

Johann: What we’re doing here in the CG, working with other browser vendors to advance the state of the web, I hope it has already had a positive effect and I’m confident that we will continue to do that work. Important to note that we’ve done a lot of work in the space. Difficult inputs and need to make decisions from that.

Andrew Pascoe: As an advertiser, want 3p cookies to be at least dampened and make the web a more private place. Does Google have any intention of having telemetry or user studies to track the progress? How many users will turn 3p cookies off? Are we making progress as a community? And will that be open information?

Kleber: As Johann mentioned before, the various APIs we already have should make it easy for folks to determine what percentage of users to their site have 3p cookies on or off. Not sure Chrome reporting a global number across all users is particularly relevant. Individual developer should care about the users they care about. I think our goal is to have the browser give you the tools to monitor that yourself.

Andrew: I’m invested in the state of the web as a whole, not just my company or organization. Previously we just discussed, “there might be different web sites with different demands that 3p cookies should be enabled in order to access the web site.” If a large site tries to force it, I might not be able to measure it but I might still care.

Johann: As you know, we don’t have anything right now. Can look at that and what we can build/share. No answer now, thanks for the feedback.

Aram: Want to clear things up. First, confusion in market on the nature of this control. Is cookie consent applied once by the user and applied universally to every site?

Kleber: Yes, global setting for the browser.

Aram: Will it give users the option to change their choice on a per-site basis? “For this site I am okay with 3p cookies while no other site gets 3p cookies”?

Kleber: We’re talking about a browser setting. I think you said “consent.” We’re not talking about consent in the legal GDPR meaning of that word.

Aram: Yes, the browser setting.

Kleber: Setting will generally be there. Users of Chrome who have 3p cookies on can already turn them off for some pages. And vice versa. Will continue to be the case.

Aram: I recently discovered a origin trial that pushes Chrome into “no 3p cookie” mode. Will the OT be extended so we can use it for further testing? Might it become a permanent feature to enable sites to turn off 3p cookies if they wish to do so?

Ben Kelly: We added OT to opt into more 3p cookie restrictions because we heard during the mode B 1% trial that it wasn’t enough volume to prove it won’t break things. Added as a compatibility “prove it out” thing. OTs are not a permanent thing and are heavyweight to extend. We have heard from some partners that they want something more permanent. Security properties of 3p cookies off, someone might want to say an embed gets no 3p cookies… or an embed might say it doesn’t want 3p cookies. No proposal yet, feedback welcome.

Aram: I would love that feature.

Kaustubha: Can you expand more on why you want such a feature? Want to meet the spirit of the commitment with regulators. Regulators will ask why we built it and we would like to be able to explain.

Aram: First, the 1% mode B is not significant enough to understand the impact of deprecation. Longer term, though, as a permanent feature we have seen an increasingly regulatory concern with 3p cookies. Quite a large percentage of users on our site that have chosen to opt out via government-approved flows like GDPR, ?. Not every partner we work with is particularly effective at obeying that signal. We depend on the “consent string” flow to say, “hey, we realize there might be 3p cookies we don’t want in these requests… please ignore them.” But from a compliance perspective and ethical dealing-with-our-users perspective, once we say you don’t want 3p cookies we can enforce a switch with the browser that enforces that. We would use that and other publishers would as well.

Kaustubha: Sounds like mostly a per-user basis. Would like to query TCF or whatever platform and then force the top-level site to do something.

Aram: Would like to do a large percentage first, but long term as a permanent feature… yes, we would like it on a per-user basis. Imagine it implemented as a user opt-out signal for cookies that we save to a cookie and send that on request and return appropriate headers to say “don’t have this session include 3p cookies.” Would be within our capabilities and set of things we often do.

Isaac: You have done a ton of work on Privacy Sandbox APIs, design and implementation-wise. You have publicly stated your plan to continue that work. Can you further elaborate on any commitments to invest in these APIs, make them competitive both from the CMA sense and ad monetization effectiveness. The reason I’m asking is we’ve had a lot of debates on our side about the extent to which that commitment will continue. Would be curious to hear.

Johann: Full commitment on our side on all of the APIs that we both started and are developing. No change of plans. The slide we’re showing do have some considerations about two different variants of Chrome that developers will encounter. And web APIs will need to consider that in our browser at least. Other than that, no change for us. We are making the same investment, expecting a non-trivial number of Chrome browsers to have this. Want the ecosystem to be prepped for that because we expect to see breakage if they do not prepare.

Kleber: From monetization and measurement side of the work, similar to what Johann said. Years spent with the assumption that it’s straightforward and reasonable to monetize web pages with ads even when 3p cookies are off. We have every reason to believe that monetizing web pages for users who have 3p cookies off will be an important part of a successfully functioning ads ecosystem. Ads APIs just as important as they were before. Full steam ahead.

Tess: thanks to the Privacy Sandbox for taking those questions!


## Any other business

Ben Kelly: talked about proposals for something new here. A proposal repo?

Martin: great question, can discuss that. More than happy to make repos available in the general area if it looks approximately privacy-related. If enough interest we will set something up. If people figure out something fits more closely with another group, we can help facilitate that process or you can go straight to them.

Ben Kelly: Other browser still have an option to turn 3p cookies back on. So some of that policy could apply to all browsers.


# Attendees
* Erik Anderson (Microsoft Edge 🪑)
* Nick Doty (CDT)
* Pete Gonzalez (TikTok)
* Brandon Maslen (Microsoft Edge)
* Martin Thomson (Mozilla 🪑)
* Tess O’Connor (Apple 🪑)
* Aaron Selya (Google Chrome)
* Wendy Seltzer (Tucows)
* Hadley Beeman (W3C TAG)
* Fabian Höring (Criteo)
* Aram Zucker-Scharff (The Washington Post)
* Steven Valdez (Google)
* Kaustubha Govind (Google Chrome)
* Joey Knightbrook (Snap)
* Ari Chivukula (Google Chrome)
* Mike Taylor (Google)
* Daniel Masny (Meta)
* Ben Kelly (Google Chrome)
* Charlie Harrison (Google Chrome)
* Ethan Leeman (Google Chrome)
* Tatsuya Igarash (Sony)
* Rick Byers (Google Chrome)
* Andrew Pascoe (NextRoll)
* Michael Kleber (Google Chrome)
* Zachary Cancio (Google Chrome)
* Phillipp Schoppmann (Google)
* Benjamin VanderSloot (Mozilla )
* Lionel Basdevant (Criteo)
* Maxime Vono (Criteo)
* Tim Cappalli (Okta)
* Maxime Guerreiro (Cloudflare)
* Matthew Finkel (Apple)
* Roxana Geambasu (Columbia University)
* John Wilander (Apple WebKit)
* Shivani Sharma (Google Chrome)
* Erica Kovac (Google Chrome)
* Justin Brookman (Consumer Reports)
* Shouqun Liu (ByteDance)
* Alex Koshelev (Meta)
* Emily Lauber (MIcrosoft Identity)
* Nicolás Peña Moreno (Google Chrome)
* Christian Biesinger (Google Chrome)
* Yujie Hao (TikTok)
* Daniel Huigens (Proton)
* Aloïs Bissuel (Criteo)
* Fatma Moalla (Criteo)
* Reilly Grant (Google Chrome)
* Matthew Atkinson (Samsung)
* Manuel Bucher (Mozilla)
* Artur Janc (Google)
* Heather Flanagan (Spherical Cow Consulting)
* Yi Gu (Google Chrome)
* Dylan Cutler (Google Chrome)
* Andrew Verge (Google Chrome)
* Risako Hamano (LINE Yahoo) 
* Thomas Prieur (Criteo)
* Aykut Bulut (Google Chrome)
* Chris Fredrickson (Google Chrome)
* Anusha Muley (Google Chrome) 
* Johann Hofmann (Google Chrome)
* Max Gendler (News Corp)
* Sandor Major (Google)
* Don Marti (Raptive)
