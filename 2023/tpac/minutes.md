# 2023-09-12 Privacy CG @ TPAC Meeting
* Chair: Tess, Erik, Martin
* Scribe: Aram ZS


# Agenda

* (~9:30) Setup, Welcome, Code of Conduct (chairs; 10m)
* (~9:40) [Storage Access API Graduation](https://github.com/privacycg/meetings/issues/31) (20m) ([slides](https://docs.google.com/presentation/d/1rE4J-dLG_q5YltY6r8GmT3IPn_NDjjKO3TO0whL71sg/edit#slide=id.g27d8615aed1_0_0))
* (~10:00) [Heuristics to allow 3P cookies](https://github.com/privacycg/meetings/issues/30) (25m) ([slides](https://docs.google.com/presentation/d/1Ao67urE4Y5eoklngX5QtysRqN7_3aossG9cS3cFHTc8/edit))
* Break at 10:25 (5m)
* (~10:30) [Invasive Fingerprinting Protection](https://github.com/privacycg/meetings/issues/28) (30m)
* Break at 11:00 (30m)
* (~11:30) [Storage Access API Future](https://github.com/privacycg/meetings/issues/33) (50m) ([slides](https://docs.google.com/presentation/d/1SOCYJvUzjJHsVLlzqlYVKtU7yg-RWhGJWW4Om6dCinE/edit#slide=id.p))
* (~12:20) [Login Status](https://github.com/privacycg/meetings/issues/32) (20m)
* (~12:40) [Pop-up partitioning / isolation](https://github.com/privacycg/meetings/issues/34) (10m) ([slides](https://docs.google.com/presentation/d/1INuMF3t6Px3OF2aezbACAFtYnfnBI26hjyJrYM5yi6I/edit#slide=id.g27c097bfe24_0_6))
* (~12:50) [State of bounce tracking](https://github.com/privacycg/meetings/issues/29) (10m)


# Notes
Scribe: Aram


### Storage access API 

* How to get it on to the standards track? 
* Johann: Editor of the Storage Access API - one of the editors here, another (Anne) is also here. Welcome to TPAC 
    * We have two questions for today 
    * The first is about how to handle it towards graduation
    * Expecting it to go into the HTML spec
    * It has been going for a long time now and we want to give a status update as a year ago we checked in and since the we have made an update 
    * We also want to talk about the future of SAA after this initial graduation 
    * Made a lot of progress since last TPAC 
    * We have a lot, converged spec 
    * Integrated with permissions 
    * Converged on a single model 
    * Ther Permission spec and per frame model are all close to shipping 
    * This is in line with expectations 
    * A couple things to be figured out 
    * We considered a demo and wanted to show that here (slides not updated for that) 
* Christian Dullweber: Worked on the storage access API and now we have a demo for the Storage access via this site data tester API 
    * **_URL(s) below_**
    * This access storage and writes some cookies 
    * This shows how we solved the problem with everyone’s input 
    * It also shows the link to a help center article when a prompt appears to ask if the site can access your storage 
    * You can reload and the permission is not there until the request storage access event happens again
    * You can change it access it and move around. You can see the storage access settings in the Chrome settings which allows you to block the prompt totally if you’d like. 
    * We also show how to reset everything and get the prompt 
    * In Chrome it is currently an experiment that is released to 1% (SStorage Access API permission UI) 
    * Demo URLs:
        * Enable chrome://flags/#permission-storage-access-api in Chrome 117+
        * Visit [https://xchrdw2.github.io/browsing-data/siteDataTester.html](https://xchrdw2.github.io/browsing-data/siteDataTester.html) and click “write”
        * Visit [https://xchrdw.github.io/browsing-data/siteDataMultiEmbedded.html](https://xchrdw.github.io/browsing-data/siteDataMultiEmbedded.html)  and click “request storage access”
        * Settings: chrome://settings/content/storageAccess
* Johann: That was the demo 
    * There are a couple of smaller editorial and other things that need to be fixed but not super hard to do. 
    * There are some pain points though, hopefully not impossible to solve 
    * 1: How does this interact with cookies? 
        * What should implementers do with the flag? When they execute a permission request for data access 
        * We’ve been talking about this since last TPAC 
        * Storage access flag should do something 
        * We’ll be talking more about cookies on Wed at the Cookie Layering breakout session. 
    * 2: Prior user interaction requirement 
        * Chrome and Safari both ship a requirement for a prior top level user interaction with the site before a prompt is shown. Firefox will show a prompt without this. 
        * It seems that we could align on this, but there is some concern that if it is not clear developers will be confused by the requirement 
        * We think that after the prompt is shown the permission will be persistent (?) 
    * 3: Workers
        * How does storage access work for workers ? Web workers - if you spawn the request from a context with storage access the question is how does it interact with other documents with different storage access states? 
        * What happens when one doc has the permission and the other doesn’t. It isn’t a big question of privacy b/c the storage access is still partitioned to each context. It is more about consistency and security. 
        * We’ve established that getting access is a little bit of a security risk. It sounds like a minor threat to me that could be overlooked, but we need a final decision here. 
* John W: Say two things
    * First on the current interop we are all aligned - but I’m not sure actually where we are on per page access scope but we want to be aligned per frame and I want to check in on what other influencers have done here 
    * You mentioned that top-level user interaction is a signal. We see it as a heuristic proxy signal here, that the user might be logged in, and might want to grant access
    * We decided to let successful usage as a user interaction so once you are in the green you stay in the green. 
* Johann: 
    * Per page vs per frame? 
        * I think we agree on per frame 
        * Chrome and firefox have made the change, Chrome shipped, Firefox is in progress 
* Paul Zülcke: I don’t know exactly where we are but we are aiming towards per-frame. 
* Johann
    * That’s mostly it, maybe folks have opinions or things we can hash out in discussions 
    * This is something we def want to make progress on for TPAC of all things it seems like the one that we can resolve and someone can write something down. 
* Nick Doty:
    * Good morning, thanks for the demo and discussion 
    * I think the thing that might be relevant is how users might understand this
    * We discussed this and there is an issues from Sept 2022 and I don’t actually know if we know users understand this [the prompt?] some users might have less knowledge of web platform than I do \
[https://github.com/privacycg/storage-access/issues/116](https://github.com/privacycg/storage-access/issues/116) 
    * Have we done user testing how do we make sure users understand the prompt 
* Helen Cho: Google chrome - great question 
    * For some people who are more familiar and some people are just going to click through
    * We just started rolling this out and the intent is to continue iterating 
    * We’re trying to bridge the gap between more tech savvy and less tech savvy users in regards to the permission prompt 
    * If users want to learn more we have a link to make it easy to understand what they are agreeing to. 
    * We are committed to finding the right purpose and taking steps towards that. 
* Nick:
    * I’m concerned about having documentation that people can read as the expectation
    * That seems like a good fallback transition and it seems like the spec already talks about X as a potential concern and has specific use cases and we shouldn’t be asking them a question they don’t understand 
* Johann: 
    * I think that is  a concern 
    * I think the iteration process is how we work this out and I do think we’ll be able to do more iteration on this consent and get a clearer message to the user that they can understand 
* Theresa: Where there are specific purposes in these APIs
    * Cases where these specific API requests happens will have clear messages, but they may have to fall back on the Storage Access API 
    * As we have more specific APIs they will have clear more straightforward prompts and user confusion will decrease over time because they will be prompted with these other APIs first and then the SAAPI will end up being used less. 
* Nick:
    * Can we note that we prefer more purpose built alternatives? And that we consider this a fallback from more purposeful use cases?
* Johann:
    * I think fallback behavior is hard to define 
    * By nonexistent policy especially by saying something I’m not sure how useful it will be 
    * We can add something to the spec, but I’m not sure how useful it will be 
* Nick:
    * When I’m thinking about the future and future APIs and use cases we should remember that this is our plan 
* Ben Kelly:
    * There are use cases we haven’t thought about yet. There is a need for a fallback
    * If we only allow purposeful use cases we risk the web becoming stagnant. 
    * I just wanted to put that out there because that is a concern. 
* Johann:
    * I think that’s it is there anyone else? 


### Heuristics to allow 3p cookies 

* Ben Kelly:
    * Hi. I’m from Google let me set up to present. 
    * I’m going to present on what we call Heuristics 
    * I might call in some other folks on our team on specific points. 
    * Basic idea of what we are calling Heuristics 
        * Other browsers have turned off 3p cookies and we’ve noticed they have taken certain steps to maintain web compatibility to keep things going where they can
        * We’re trying to assure that we stop breakage for existing user facing functionality, particularly for auth flows 
        * We’re going to talk about what they are and what they’ve done
        * We’re particularly focused on auth flows. 
        * We want to foster discussion on this and will have a Chrome proposal at the end 
    * First Heuristic is based on 
        * Current ecosystem pop up flows often used for authentication flows. 
        * Scenario is that the main site opens a pop up to the auth system, the user signs in via 3p page that is popped up and then iss returned to the site 
        * If the user has interacted with the sign in system in the past then that is part of what is used for heuristics in other browsers 
        * We’re look to add it and committed to using stuff like this when we remove 3p cookies last year 
    * 2nd is redirect heuristic
        * Site A redirects user to log in to Site B
        * There are some limits, looking to see the Site B 3rd party has been interacted with in the last 30 days. 
        * We believe this has been implemented by Firefox but not Safari 
        * Our colleagues on Privacy have a few concerns with this behavior. 
        * The main concern is that from a privacy perspective the first and third party could collude for tracking purposes
        * There is some amount of ability to map an interaction state to the user through this
        * The first party attacking using the 3p to leak the interaction state. 
        * From a security perspective
            * We’re hoping that the ecosystem as a whole will move away from 3p cookies to begin with since we’re the last browser to turn these off. 
            * We would prefer not to reopen those holes 
            * Could allow an attacker to reopen 3p cookie vulnerabilities that people have turned off protections against because they assume it isn’t a threat anymore. 
            * The 1p could open a connection to a 3p and then have access to 3p cookies, which would be bad. 
            * The attack could allow a crosssite leaks attack
            * A lot of these concerns from a security perspective were brought up on storage access api last year and changes were made to minimize these attacks 
    * Mitigations:
        * During this flow we have to have it reduce the grant duration 
        * We could have a clearer messaging mechanism
        * ???
    * We do think this is something Chrome wants to implement like the other browsers though we are not sure we can align completely. We do want to follow a similar path though.
    * We have an explainer we just published that goes into more detail on this based on this presentation and some concerns 
    * We haven’t got an intent to prototype yet but it will be coming soon
    * This is where we are leaning to go in this space and we’d like to bring it to this space
    * If all browsers pop this up it will become a standard by default so we should put this in a spec but which one? 
    * We’re looking to prototype this
    * We’re trying to close privacy and security risks where we can. We want agreement on what we actually ship. 
    * That’s what we’re doing so questions/discussion - other browsers are shipping this and we’re curious what your experience has been, how load bearing has this been? Have you experienced privacy and security issues yet? Any topic questions concerns? Other heuristics to look at? 
* Theo:
    * Hi, from Chrome
    * Talk about motivations - can you explain some of the motivations behind this work, both in Chrome and in other browsers?
    * A lot of the things we are trying to ship need some help on the heuristics and it pops up interactions 
    * Is this the catch all bucket or is this a measure supplemented with APIs related to this? 
* Ben Kelly
    * 3p has been on the web for many years now and it is a ton of work for the new solutions to be adopted so anything we can do to avoid breakage when we turn off 3p is appreciated. 
    * Ideally we’d all like to see this thing all go away, but is it a big enough problem to invest in resources to later depricate? 
    * It’s hard to know but the hope is that we will have FedCM take off and that will be incentive for people to move in that direction and not be reliant on these mitigations. 
    * This solution needs these interactions but FedCM wouldn’t
* Theo:
    * We’re talking about spec and alignment 
    * I don’t want everywhere a pop up heuristic 
* Ben K
    * I don’t know where we landed exactly
    * We didn’t have a specific depreciation date in mind. This is something that could be in the explainer or noted in the spec. 
    * Other browsers? 
* John Wilander:
    * We have been thinking about this a lot 
    * Proposal here for preventing popup tracking. 
    * We have a popup heuristic [in safari]
    * It is mostly about calling the storage access api in the pop up resource to open it up on behalf of that resource and we grant it as a key. 
    * We view this as a workaround currently around the storage access api but we absolutely want it to go away
    * We may have to add some friction to make this less attractive to developers so they move to SAAPI or FedCM
    * We are thinking about more partitioned pop ups - put can you get out of the pop up?
    * We are thinking noopener by default - solves one side of the problem, the pop up talking back to the opener and then you need something more transparent to the user to get the opener relationship 
    * Should we use the storage access prompt in the pop up or before the pop up? 
        * They want to open the pop up to the domain but harder to explain that it could be sharing your identity 
    * A prompt in the pop up itself might delay things but would be worse for the user b/c it requires an answer before loading?
    * Could we wait to get a user interaction before loading the pop up but then we’re in a prompt waiting situation for the user. 
* Ben
    * Clarifying q
    * Apple is looking to increase friction soon?
    * A desire in the future for timeline to remove?
* John W:
    * We don’t have a precise timeline but we have been clear we intend to remove.
    * We’re waiting for alignment on blocking cross site cookies first 
    * If it is just us: complaints 
* Artur:
    * So from security perspective 
    * The pop up heuristics go way beyond IDPs (identity providers) 
    * There are way more websites than sign in providers 
    * Assuming we don’t deprecate is there a world where we can provide an opt in from the top website 
    * There is a relatively small number of IDPs can we get an opt in model for those that apply to websites?
* Ben K
    * Are there other use cases beyond IDPs? This is something we want to look into
    * Are the sites that are breaking maintained or not? 
    * If maintained this may not be too onerous but not all are
    * I think it is worth looking into
* Johann:
    * I think I would choose to do this sooner.
    * Can we commit to a timeline that does not have the security issues in the future 
* Artur:
    * How load bearing is this - we need to know better to make a decision about how long this sticks around
* J:
    * How quickly do we replace this is something we have to figure out. 
* Sam:
    * Hard to hear some people, e.g. Theo; please repeat questions.
* Paul Z:
    * From Mozilla 
    * Long term we would also like to see the heuristics gone but the path is difficult. 
    * Stricter and more friction and encouraging the use of dedicated APIs are the next step
    * We def need to align or we risk big breakage issues 
    * Short to mid term we need to align 
    * We also have metrics on how often these get triggered we can share in the future. 
* Sam Goto:
    * Some context - how hard it will be to move people - context: 
    * Enterprise moves slowly 
    * Heuristics  will allow us to develop the alternatives over time with less pressure and it will let us make better choices and allow people to migrate over reasonable time. 
* Theresa
    * Break time now. 


### Invasive Fingerprinting Protection

Zainab (Google Chrome) presenting:

Other folks working on Fingerprinting in Chrome: Mike West, Ben Kelly, Shubhie Panicker

What is Fingerprinting? Techniques that tell what makes a user’s computer unique. Passive and active FP, passive is thing that are already available, active requires active JS calls. We want to focus on active FP using web apis

Other browser today: List based-blocking, various lists e.g. Disconnect, DDG. Domains on the list are blocked in 3P context. 2nd approach is modifying APIs. Either add permission prompts, noising, blocking, mocking. Usually have different protections in default vs. opt-in modes.

Chrome: 3PCs going away. Investing in IP protection. Looking into an approach with tracker lists blocking known fingerprints. Have drawbacks: evasion, web compat, UX breakage.

Discussion: Will tracker lists become more ineffective as more browsers adopt them due to evasion techniques? Are there alternatives we should be aware of.

John: We have tried to be more clear that FP is a problem for cross-site tracking and per-site user recall. Want to increase the uniqueness of users per-site. Not intuitive.

Aram: List are good in the short term but long term we need web API changes and we can see where, fingerprint.js is open source. Will have consequences for anti-fraud work. Would be nice to see FP APIs produce more noise. Hopefully we can develop AF tech that will make this easier.

Tim: Experience from Firefox: Have anti-FP from Tor Browser. Changing API behavior. Had some bad side effects from some interventions, it’s very important to consider the impact on the user experience, how can we protect users best?

Ben: John, can you give me examples on the per-site user recall that you’ve mentioned.

John: Unique ID per site is the worst situation. Can do cross-site tracking and users can be re-id’ed when they clear their storage. Even when you solve the first, you still have the latter. So we should allow the FP to change per website, if the user wants. This primarily affects anti-fraud.

Brian May: Heuristics discussion points to an important change as Chrome aligns more with other browsers: Chrome has been the place to go when something didn’t work in other browsers, now we need to replicate heuristics across browsers otherwise users will jump around browsers, so we need to standardize them. This also affects fingerprinting/anti-fraud.

Z: Question: What has been your experience with modifying web APIs? Any UX research on adding browser prompts?

Tim: Canvas prompt? (Z nods) It’s still behind a pref right now. Thinking about how to best protect users. Prompting users can be annoying, thinking about how to avoid this.

Matthew Finkel (WebKit): Have been experimenting with modifying web apis, had challenges, particularly around canvas. Iterated a lot on it. Have been looking into how you allow repeated access of this data without allowing for FP, currently enabled in Safari 17 Beta Private Browsing. Have been adding a key (noise seed) that gets recreated when the user clears website data.

Tim: We have shipped font FP protections in private mode, where we restrict certain fonts to see user installed fonts in PB. We haven’t seen much breakage yet. Got one bug report about breakage but not much.

Matthew: Permission prompts are impossible for users to reason about, asking users whether or not to allow fingerprinting. 

Nick: Agree on not asking users that question. Permission prompts do curb the “drive-by” aspect of querying the API.

Shubhie: On Chrome we do work on reasoning about entropy you get from these APIs. Two schools of thought: 1) could focus on low hanging fruit / top source 2) unless you get all of the entropy, [not worth it]. On Chrome we’re on the former side. For everything else we might have detection and label that fingerprinting is happening vs. not.

Johann: For the longer term view, can we explore replacing APIs such as font access with “picker” patterns instead of permission prompts?

Aram: strongest fingerprinting signal judging by how often we’re asked for it is screen dimensions and window position. I encourage focusing on how we want to handle those as well. Had vendors telling us just to send IP and window.screen object. Can’t say how effective that is but it is the thing that fingerprinters are most strongly interested in. Notable that it’s the most asked for by people who want to do fingerprinting but we don’t have solutions.

Artur: A side note that preventing per-site user recall is a more difficult threat model to tackle. Would also have to unload and refresh all websites as they can store information in the DOM. Will be difficult to tackle this without UX challenges.

Nick, +1 Johann, help with fingerprinting-guidance update, some guidance is here: [https://w3c.github.io/fingerprinting-guidance/](https://w3c.github.io/fingerprinting-guidance/). Would really appreciate help with that guidance. Needs updates, including on new approaches like randomness/fuzzing.

Matt: agree with Johann, we should provide APIs that don’t have this problem. How we get there is another question. Aram, this problem is known and being looked at, ran into some web compat issues but would like to tackle this avoid screen size uniquely revealing users.


#### Storage Access API Future

Slides: 
[https://docs.google.com/presentation/d/1SOCYJvUzjJHsVLlzqlYVKtU7yg-RWhGJWW4Om6dCinE/edit#slide=id.g243c8331bfd_0_0](https://docs.google.com/presentation/d/1SOCYJvUzjJHsVLlzqlYVKtU7yg-RWhGJWW4Om6dCinE/edit#slide=id.g243c8331bfd_0_0) 

Johann:
* Talking about "new ideas for the SAA"
* Variety of topics from various members of the team
* Ari will talk about non-cookie storage
* Chris will talk about response header support
* Johann+Ben VanderSloot

Ari:
* We want to provide a way to provide access to non-cookie storage in addition to cookies
* We don't want to push people to cookies from other storage just because that is all SAA supports
* Proposed API shape:  `let handle = await document.requestStorageAccess({ all: true }))`
* The handle provides various endpoints to different storage types
* Why would you want this?  Example use case using chat to sync state across multiple windows, etc.
* Another example, if you have many services in a RWS (FPS), then the services can more tightly integrate.
* Questions?

John:
* What are your thoughts on stateful connections?  Existing service workers or IDB transaction.  How does that impact this proposal?
* How does this work for browsers that won't use RWS (FPS)?

Ari:
* Sites will need to do work to use the new API

Ben Kelly:
* We’re pursuing this approach with the handle to not break the existing connections, you’ll have access to both endpoints. We exclude ServiceWorkers because of fetch weirdness and security.

Nick:
* Can you explain the chat use case better?  Do users really want shared chat history across windows?

Ari:
* If you have a video call in some tab in an iframe and you want to open it in a new document, and then have the video call available in the document.  To transfer the call we there needs to share state.

Johann:
* This is also a compat issue for existing sites in the web and they felt SAA was forcing them to cookies that exposed data to the server on the wire.

Ben Kelly:
* In a broad aspect sense, there are lots of things built on non-cookie storage on the web.  Some are switching to cookies, which is sub-optimal. That’s a privacy risk. From a privacy perspective, there is no privacy benefit to withholding indexedDB access. We didn’t want to force people to use cookies when they don’t need to.

Nick:
* My confusion was about the stated use case and whether it’s something for back-compat or even the user’s intent, that’s all. Agree on similar privacy stance between cookies and other storage.

Ben VanderSloot:
* We have an open issue on how to handle workers for SAA and we might need to bootstrap from documents. More importantly: is Storage access needed?

Johann:
* This proposal is slightly orthogonal to the SAA in workers issue.

Ben Kelly:
* Is this about how a worker gets access to unpartitioned storage?

Ben VanderSloot:
* No, they can get unpartitioned access before creating the worker.

annevk:
* Does this create an incentive to use APIs that are susceptible to abuse (worker exhaustion)?
* Would prefer not to extend use for localstorage because of performance issues

Johann:
* Personally think localstorage has not replacement from an ergonomic standpoint today
* Deprecating localstorage is orthogonal to SAA

Annevk:
* More API shape?

Ari:
* See the explainer please [https://arichiv.github.io/saa-non-cookie-storage/](https://arichiv.github.io/saa-non-cookie-storage/)

Chris F:
* Starts talk on response header
* Describes status quo with how an iframe uses SAA today
* Current model includes CSRF protection by requiring each frame individually ask for cookies
* Status quo is fine, but requires the iframe js to run and may be suboptimal for UX
    * Unnecessary latency, can require reloading resources that results in excess network usage, etc
* Proposal: The server can opt-in using a response header that allows cookies to be used immediately if the permission is already granted.

Benjamin, Mozilla:
* Is it complicated for webdevs to intermingle partition keys among resources in the same document?
* It seems potentially an easy mistake to set the header incorrectly.
* Misunderstood!

John Wilander, Apple WebKit:
* on WebKit’s requirement for user interaction in the frame for each granted storage access. Maybe the other implementations don’t have that and allows for.  Wouldn't that not be compatible here?

Chris:
* The browser would be free not to honor the header if permission is not granted

Annevk:
* How does the frame defend against xs-leaks

Artur:
* This is an opt-in that you only set for frames that you want to have 3P cookie access.
* This is not fundamentally different from a JS API to opt-in.

Annevk:
* Does the further encourage a pattern where there are different experiences where you are logged in or not?  That would further open this up to sibling attacks (?)

Chris:
* Need clarification on this

Martin:
* How does the site know whats happened?  There is no feedback that it worked or not to the server.  How does the embedded site make choices based on the result?

Chris:
* The scripts can call hasStorageAccess().  Maybe there is some way we could expose this in another way?

Martin:
* The iframe will not get a signal from the browser that indicates whether the request it makes will be/has been given.

John:
* Martin, are you getting at that we need a request header telling you have been granted storage access.
* (group nods)

Johann:
* That is a good idea

Johann:
* Starts SAA for cross-site identity flow talk
* Current state:  working on FedCM, SAA, RWS (previously called FPS), heuristics (discussed earlier today)
* Chrome has received feedback from partners that there are identity flows where sites are struggling to adapt to a world without 3P cookies given current solutions.
* We found that the popup and redirect heuristics are quite load bearing for web compat on firefox/safari.  Per earlier discussion we're interested in working on these as well.
* Issues with SAA:
    * IDPs often use invisible iframes which makes it hard to get interactions
    * IDP has to change to somehow make this clickable, then get prompt, then have user click prompt
* Some friction here is good, but understand developers struggle
* Some developers have to write things like "click on this extra button because the browser requires it", which is suboptimal
* SAP example:  Auth flow creates a popup, does a SAML request to 3P, popup redirects (complicated, not sure I caught it)
* The popup heuristic helps a lot for things like the SAP example
* Is there a way this could work without the popup heuristic?  Is there a clear user opt-in for 3P cookies or cross-site integrations?
* Can we allow for more flexibility between 1P+3P relationships (not limited to RWS)
* Can we reduce user friction and still maintain privacy properties
* Still need to prevent security attacks
* Previously proposal from 2years ago: Forward Declared Storage Access:  gives prompt in the redirected or popup before going back to original page with storage access
    * Had some interest, but no one implemented
* Next previously proposed idea:  requestStorageAccessFor() allows the 1P to request storage access for a 3P they embed and only requires a click on the 1P page
    * problem, no opt-in for 3P
* Mozilla is exploring Top-Level Storage Access…. Ben VanderSloot?

Ben VanderSloot:
* Similar to requestStorageAccessFor(), but requires 3P opt-in
* IDP folks say can assume interaction with IDP, but cannot assume a popup in the flow
* Results in prompt "do you want to link identity" when the flow happens

Johann:
* Current work on FedCM is also a potential solution here in the future.
    * This has not been easy with FedCM in the past
    * But there is work going on to potentially support this flow in FedCM (authz)
    * This would allow a popup in the FedCM flow which allows the IDP to customize the flow to some degree
    * There are some short comings, but maybe they could be overcome.  For example, the need to (mediate 3P cookies?)
    * Does this really give adequate flexibility to developers?
* We need something like these proposals to replace popup heuristics

John:
* 1. Whether or not OAuth top frame redirects should be considered an existing solution here or if we only consider browser-mediated cross-site identity solutions?

Johann:
* That is a fair question and something we should look at.
* It’s going to be hard to differentiate between tracking and non-tracking
* Developers are unsure if these solutions will be closed off by browser in the future

John:
* 2. On the cumbersome flow with cross-site popups. Could a SAA prompt before opening cross-site popups solve here (no need for subsequent call to SAA?

Johann:
* It's an interesting suggestion we should explore.
* Always thought it was more intuitive to the user to answer the prompt just before returning to the RP.
* Asking first might make sense as well.

John:
* We found this approach maybe nice because its a better onboarding to SAA for the IDP.

John:
*  3. How is the SAP Analytics example not considered cross-site tracking – is it a user-initiated login?

Johann:
* Talking to SAP it did not appear to be tracking.
* (John: analytics sounds like potential tracking, or at least not a login)
* (said something about it being a wording issue in the slide and it was more about monitoring the service internally ?)
* SAP example was just a dashboard, not about directly doing analytics through this flow; confusing about “analytics”.

John:
* 4. If IdPs would be fine holding the load of their iframe before the user makes a decision, to avoid the first, uncredentialed load?

Johann:
* (back-and-forth about who calls the API, what is the signal to trigger the prompt)
* (back to top-level storage access slide)

Martin:
* The iframe or 1P could indicate in some way the iframe should only be loaded if access is already present

Ben VanderSloot:
* Nothing in the current top-level storage access proposal to exclude this approach
* Separately:  FedCM is still blocked on adoption by IDP in community group due to extensive infrastructure changes that are required

Johann:
* Could focus on FedCM more to try to make things easier, but that could water it down in a way that would cause it to fail to meet its original goals.

Aram:
* Want to talk about the FedCM authz options
* The popup is better for users and they are used to it
* Google has been successful in using the overlay in auth flows and I am concerned that extending that specific UX to other login providers would confuse users who would assume it was Google
* Internal to the page sign-in is worse than popup because inside the page there is a greater risk of spoofing.  Harder to spoof the popup

### Login Status (Johann and Sam Goto)

Sam:
* Chrome has been working on FedCM in the FedId CG
* In early implementations we ran into a timing attack found by Ben VanderSloot and have been working on it
* Have been aware of the login status API and trying to be compatible with it
* Becoming more confident that the approach could work
* Want to discuss with group to see how everyone feels about this approach
* login status API was originally was proposed by John from Apple and Melany from Microsoft
* API shape: navigator.recordLogin() and recordLogout()
* How the bit is used or read was not included in the original proposal beyond potential use cases
* In FedCM exploring a similar idea with perhaps some additional constraints for IDP use cases
* For example:  IdentityProvider.recordLogin()
* Could then use FedCM API to get the credential state
* Proposal:
    * Remove the FedCM-specific approach
    * Instead use login status API
    * but extended so reading the bit through FedCM is possible
    * maybe also extend to SAA:  requestStorageAccess({ rejectedUnlessLoggedIn: true })
* Not going into FedCM uses specifically because its too complicated for time allowed
* But the gist is the IDP needs to take different action based on the logged in state
* And if they are lying to the logged in state then they are just lying to themselves
* No incentive to lie
* Further ideas:
    * Could augment login status with names, avatar images, etc
    * No idea if this is feasible or not, but matches intuition to potentially improve trust
    * Not directly working on this further extension
* Small issue to be aware of:
    * signing-in and signing-out happens through 202s and not JS
    * so we would need header mechanisms in addition to login status API existing shape
* Looking to ship IDP sign-in status feature for FedCM by the end of the year
* Don't have plans to use login status API in other use cases currently
* Question for group:  Should FedCM pivot to using login status API instead of FedCM specific API?

Johann:
* We had this idea it could conditional on login
* We should keep this option open, but right now its just an idea
* It would solve some problems (missed what it was)
* As long as rejections look the same as a prompt being declined then we would not leak login state via SAA promise rejection
* Maybe the prompt could be more informative (at some risk of eating into FedCM utility)

John:
* On Login Status API usage. We think an explicit trust level signal in the API could unblock FedCM while keeping the original intent of a trustworthy login status signal. We also want to solve:
* 1. Detection of success state of authentication
* 2. Formalized logout, and
* 3. Hidden remember me token reveal on successful, trustworthy login to remove the desire for longterm user IDs beyond logouts or fingerprinting for user recall.
* At the lower trust level, the browser would likely not need UI to defend against sites not signaling logout. The thought has not escaped us that the whole world of trust levels (2FA, Passkeys, etc) could be encoded here somehow. That question pops up now and then but is mostly for sites themselves, not the browser. It’s important to remember that Login Status is not a login state or token, it’s just telling the browser what the status is.

Johann:
* Agree in broad sense, but seems we need a real concrete way to detect actual successful login.  Asking the user doesn't seem like a great approach.

John:
* Something like Passkey would provide adequate signal

Ben VanderSloot:
* 1. Is browser-legible only enough? Conflict of trustworthiness.
    * Who is trustworthy here?
* 2. UI hints of profile pictures are hard because of requirements of freshness
    * In some jurisdictions need to be fresh on the order of hours
    * This can be a hard problem

Aram:
* 1. I’m not up to date on this but I’m unclear from this presentation - does this focus on integrating with FedCM theoretically encourage IdPs over a site running it’s own login in order to unlock more features/browser ui?
* 2. Is there a risk for a logout event signal as part of this that *is* exposed to the site? It would be useful to have such an event.
* 3. Remember Me Token sounds like a good idea!
* // Note: If there are tickets around this already or a preferred place to file them, let me know and I’ll interact on there. 


### Popup Partitioning

Ari:
* If you have comments or concerns please write them in the queue.  Super short on time.
* What is the concern:
    * Partitioning 3P storage
    * But you can open a 1P popup page and then you can comms to 1P and can join
    * There are some popup barriers, but once you get there you can join completely
* Proposal 1:
    * Considering ways to sever the opener on cross-site navigation (if the the original page navigates while the popup is still open)
* Proposal 2a:
    * Transient storage for top-level windows that has a cross-site opener
    * this could result in confusing behavior for sites and users
* Proposal 2b:
    * Severing openers more aggressively (all or additional heuristics?)
    * Within the popup we require some trust signal (TBD what that is) that would be enough for browser to restore window.opener access
    * This is similar to storage access API

Erik:
* How can the user understand what is going on here from a prompt?  Needs UXR

John:
* Sometimes we think of popups on desktop, but on mobile they function just as tabs
* Users may result in a very different and confusing UX on mobile

Ari:
* We are still exploring and open to feedback
* Prototyping some opener severing
* Please discuss more on [https://arichiv.github.io/opener-storage-partitioning/](https://arichiv.github.io/opener-storage-partitioning/)


#### Bounce Tracking

Ben Kelly: In Chrome, just starting to roll out to stable channel some bounce tracking mitigations. Only blocking bounce tracking when 3p cookies are blocked. In addition, scoping… user interface elements that allow users to fix breakage on web sites when blocking 3p cookies (shows an eye in the omnibox in Chrome); want to support that as well. The UI folks felt it was good to support same mechanisms. For this specific bounce site (in the 3p sense) on the originator or destination of the redirect, if there’s a cookie exception maybe we shouldn’t delete the storage for that third party. We did this for the UI, however we realized there’s divergent behavior– SAA also creates access grants for 1p/3p pairs, so it was possible for a site to use RSA and then do a redirect and the bounce redirect mitigations wouldn’t kick in.

… my question to the group is, we didn’t plan this, but it seems like in principal if the user gave permission, do other browsers agree it should be allowed? Do other browsers agree? The SAP example before had a redirect and they might need this to avoid needing to change their site. Other implementers would need to consider support.

John: To clarify, you’re suggesting that if a cross-site iframe is granted storage access and then it initiates a navigation to its own web site as a 1p and goes back to the opener, it should be allowed– no link decoration heuristic should kick in, it would have access anyway, and it should be allowed. Is that what you’re saying?

Ben: essentially.

John: Thinking bounces back? IF it’s a login flow with an IDP you might stay there for a little while. Not like a bounce, right?

Ben: checking originator and destination separately. Supports bounce through. We’re very lenient. We intended to use it for the user fumbling around trying to fix their broken experience. Honoring both sides of it.

John: You can boil it down to if they gave storage access, they go cross-site as a first-party, feels like the way to go… yeah.

Ben: Will open an issue.

## Attendees
* Erik Anderson (Microsoft Edge)
* Theresa O’Connor (Apple, TAG)
* Johann Hofmann (Google Chrome)
* Martin Thomson (Mozilla)
* Brian May (dstillery)
* Ari Chivukula (Google Chrome)
* Sam Weiler (W3C)
* Ben Kelly (Google Chrome)
* John Wilander (Apple WebKit)
* Sarah Nogueira (Criteo)
* Steven Valdez (Google Chrome)
* Brandon Maslen (Microsoft Edge)
* Mike Taylor (Google Chrome)
* Chris Fredrickson (Google Chrome)
* Richa Jain (Meta)
* Aram Zucker-Scharff (The Washington Post)
* Zainab Rizvi (Google Chrome)
* Brianna Goldstein (Google Chrome)
* Alexandru Mihai (eyeo)
* Nick Doty (CDT)
* Pascoe (Apple)
* Mihai Cirlanaru (Google Android)
* Sean Turner (sn3rd)
* Kyra Seevers (Google Chrome)
* Marek Blachut (HM Government)
* Don Marti (Raptive)
* Lionel Basdevant (Criteo)
* Artur Janc (Google Security)
* Tom Van Goethem (Google Chrome)
* Joey Knightbrook (Snap)
* Sota Akiyama(Yahoo Japan)
* Scott Hendrickson (Google)
* Christian Dullweber (Google Chrome)
* Benjamin Case (Meta)
* Fabian Höring (Criteo)
* Mike West (Google Chrome)
* Alexandre Nderagakura (No affiliation)
* Kaustubha Govind (Google Chrome)
* Anne van Kesteren (Apple)
* John Wilander (Apple)
* Shubhie Panicker (Google Chrome)
* Tara Whalen (Cloudflare)
* Ben Wiser (Google Android)
* Sam Macbeth (DuckDuckGo)
* Jonathan Kingston (DuckDuckGo)
* Bartosz Niemczura (Meta)
* Jeffrey Yasskin (Google Chrome)
* Theodore Olsauskas-Warren (Google Chrome)
* Matthew Finkel (Apple)
* Maud Stiernet (No affiliation)
* Warren Fernandes (Media.net)
* Vedant Seta (Media.net)
* Benjamin VanderSloot (Mozilla)
* Tim Cappalli (Microsoft Identity)
* Paul Zühlcke (Mozilla)
* Tim Huang (Mozilla)
* Vinod Panicker (Amazon Ads)
* Filipa Senra (Google Chrome)
* Risako Hamano (Yahoo Japan) 
* Edwin Leong (IMDA)
* Lee Chein Inn (IMDA)
* Yi Gu (Google Chrome)
* James Laskey (Google Chrome)
* Christian Biesinger (Google Chrome) (joined at 11:30)
* Nicolas Pena Moreno (Google Chrome) (11:30)
* Alex Christensen (Apple)
* Nicola Tommasi (Google Chrome)
* Tomofumi Okubo(DigiCert)
* Daniel Appelquist(TAG)
* Philipp Pfeiffenberger (Google Chrome)
