# Privacy Community Group F2F, 13 May

Chairs: Erik, Tanvi, Tess

## Agenda

* Welcome
* Privacy and Firefox
* Privacy and Edge
* Privacy and Chrome
* Privacy and Brave
* Privacy and Safari
* Privacy and Samsung Internet
* Privacy at the W3C
    * Privacy Interest Group
    * Improving Web Advertising Business Group

## Attendees TBD

## Introductions

Scribe: Peter Snyder

* Erik: minutes go here (in cryptpad)
* …: other details are [in Github](https://github.com/privacycg/meetings/tree/master/2020/05-virtual)
* …: conversation will happen in Slack; send message to the PrivacyCG chairs if you need access
* …: presentations are 10-15 min, and 10-15 min of questions, 25 min total
* …: This is PrivacyCG's first face2face (and virtual f2f).
* …: [166 folks from 60 orgs in PrivacyCG](https://www.w3.org/community/privacycg/participants), including all standards-engaged browser vendors, and involvement from other web stakeholders and invovled parties.
* …: (round of introductions, and some moderate cryptpad debugging)
* …: PrivacyCG is working as per [charter](https://privacycg.github.io/charter.html) taking up [proposals](https://github.com/privacycg/proposals) listed in the [repo](https://github.com/privacycg).  The goal is to use this time to find alighment on moving new specs forward on the web to improve privacy.
* …: There hasn't to date been a dedicated forum for discussing new privacy-focused browser features. 
* …: So immediate goal is to find where there is general agreement between implementors.
* …: Specs incubated in PrivacyCG will eventually be pushed to other working groups.
* …: The time today will be discussions about what browsers have done and are planning on doing.
* …: Tomorrow will be discussion about specific specs planned or currently being implemented.
* …: Went w/ two four-hour slots b/c of virtual f2f.  If we don't complete the agenda or need further discussion, we can have break out sessions, have future calls, and other wise figure out good venues for future discussion.
* Tess: GitHub is where most conversation happens in PrivacyCG.  Its where documents live and are disucssed (through issues).
* …: there is a PrivacyCG org on GitHub that folks should be members of, so that you can add labels, etc.
* …: If you're not currently a member, please message someone from PrivacyCG to be added.
* Wendy S: W3C mostly uses IRC; some groups experimenting with Slack.

## Privacy and Mozilla Firefox

(scribe: Jeffrey Yasskin)

### Presentation

Steven Englehardt presenting. [Mozilla slides](https://docs.google.com/presentation/d/1If6LBvKK0qS3bRRsfHvRqXsy0KlMHLVgQa_hrU42Vt0/edit?usp=sharing)

(Missed scribing of intro slides.)

[slide 7] Our goal: Make the web platform private by default. 
1. No cross-site communication methods by default.
2. Any cross-site communication governed by the Storage Access API, or another application-specific API (e.g., Private Click Measurement).
3. Support stateful applications that users want (first-party and third-party).
4. Monetization options that don’t rely on tracking

[slide 8] Defining privacy-violating uses of our browser. Security/Anti-tracking policy

Implement blocking - questions we have and what we’d like to solve in this group.

Stateful protection

Embedded content around the web, we block cookies from being sent in those embedded cases.
Policy review to add/remove people from the blocklist.  Also have entity list that don't block third party entity that is related to first party.  Take list of trackers and apply Enhanced Trackign Protection to it - blocks storage API and partitions some of the caches and network level connections by top-level ETLD+!.  Not all third parties - just the ones on the blocklist.

Near-term mitigations for site breakage - could cause breakage, users won't understand.  So implemented heuristics that automatically grant access (remove restrictions) upon certain interactions.  If user clicks login with social provider button, that creates a popup, common, then automatically access storage.  Users can also override our protections.  Near-term, allows us to get the protection out.

Longer term want to shift to safer and user controlled apis - Storage Access API prompt user etc.  risks of doing this, risks of saying okay.  Implemented in Firefox, workign on standardizing here.  And other stateful things we want to support, ex login API.  

Perioidically clear cookies from origins that are detected as trackers.  Inspired by Safari's implementation.  If cookies blocked in third party context, try to get user to visit you as first party and bounce through you as navigating through sites.  We removed cookies and site data from trackers once daily.  unless first party interaction.  Hasn't shipped yet, experimenting in Nightly.

Thinking about something that applies to all third parties - ex: paritioning.  All third parties under the website have storage access but the access they received is specific to the current first party (ex: double-keyed).  Different cookie jar than when interacted as first party.

Have First Party Isolation in the Tor browser - know there are web compatability issues like with cookei blocking.  working on how we can make it more web compatabile so we can ship it sooner.  There are some open questions when it comes to parititioning - are there are APIS that we prefer to block rather than partition and block them all the time in the third party cookies.  Ex: safari and cookies.  what about indexeddb, localstorage, how should we handle.  still thinkinga bout.

Questions around - Partiioned third party to non-partitioned third party.  Shoudl we swap?

protections against fingerprinting: use device properties as a fingerprint. harder to protect against than stateful tracking.  In Firefox 72 contenet blocking approach to anti-fingerprinting.  Parties that are known to disconnect as participating in fingerpritning.  Actually shared flagged fingerprinting scripts with Disconnect and they have do a very public review and then release a list of fingerprinters.  If other folks have expertise, would love to get those reports form folks here as well.

People have reached out to disconnect and said we've stopped the types of things we used to do - to try and get off the list.

Potentional direction to epxlore - anti-fingerprinting through API normalization.  Very difficult to ship.  Trying to make all Firefox users look the same regardless of th device they are on.  Requires doing a lot of non-standard things, and breaking lots of the web.  Are there ways to make this API level anti-fingerpritning approaches more compatabile so we can ship.

What's next - on the slides - out of time.  Thanks!

Open Questions: 
* APIs to block rather than partition?
* Should a storage access grant swap partitioned to non-partitioned storage?

(Yasskin switch to scribing now)


### Q&A

Sebastian: Interesting Work. How do you distinguish first parties from third parties? E.g. CDNs, you look at the domain that looks like afirst party, but it's different from the domain the first party is actually using? How do you make the distinction?

Steven: Depends on the type of protection. If we're doing first-party isolation, it's based onthe origin. No exceptions for being part of the "same organization". It's just the data available to the browser. When we _do_ have a list of companies, for Enhanced Protection and tracker-based blocking, we do use the idea that parties are part of the same organization. But we have trouble imagining how that scales to the whole web, in a way that can't be abused. Proabably have to do it at the origin/hostname level.

John Wilander: Thank you for the presentation. We hear a lot about profitability. "More profitable if we can target people and track customer journeys." How do we measure up to that force?

John: And about fingerprinting, we talk mostly about hardware fingerprinting, or browser-specific fingerprinting. How about user-specific and behavioral fingerprinting. Accessibility tools, etc. Have you or Tor done work there?

Steven: On profitability: Can't be ignored, but we should approach this by putting user privacy first. Shouldn't compromise on user privacy. Web platform has historically been not-private by default, and that's led to a lot of problems. Should rethink how to build profitable systems on top of a more private platform. Shouldn't put onus on users. Idea that "we should keep things open by default, and people who care can take steps to protect themselves", is a bad way to frame it. It means only experts can protect themselves.

Steven: On fingerprinting: I see your point. We're thinking about device fingerprinting, and there's a lot more out there. We don't even know how to solve device fingerprinting at scale, so we should start there. Tor has been worrying about what gets exposed when people install extensions. Awesome research papers that show extensions that add things to the DOM make users trivially identifiable. Need to do more work.

Nick Doty: Thank you for working on a lot. Want to talk about when partitioning makes sense, and when blocking makes sense. Want to scal to the whole web, instead of relying on lists. Seems like partitioning gives you all the things that blocking would give you. As long as local storage is partitioned, it doesn't give you a global identifier. 

Steven: Two aspects: Breakage: If we want to deploy this soon, what puts us down codepaths that work with current integrations? e.g. Safari has had a restrictive policy for a while, and what's web compatible for them might be different from Firefox, where third-party cookies have historically been allowed.
Partitioned storage allows a malicious actor to store an identifier in their third-party storage, and over time join sites. It's an even bigger risk when the malicious site is visited as a first party, and search engines and social networks that act as a gateway to the web might be able to correlate a lot of traffic.

David Senecal: I work for a web security vendor, providing anti-fraud, bug detection. We're bound to follow certain regulations like GDPR. Forces us to collect certain data and store it, and dispose of it. I see a problem that the privacy features start to impact the quality of fraud detection and bug detection. Makes the web more private but maybe less secure. Restricting data collection makes it hard to detect htat an account has been stolen. How can browser vendors and security vendors work together so that the browser vendor can recognize that the security vendor is using data responsibly, so the security vendor can still do their job?

Erik: That's a very general problem statement, and one worth more discussion in this group, but we're cutting into the break time, so please continue that discussion over Slack and in the [proposals repository](https://github.com/privacycg/proposals).

## How to join the Privacy CG

1. Get a (free) W3C account at https://www.w3.org/accounts/request.
2. Log into https://www.w3.org/community/privacycg/join with your W3C account.
3. Click "join".
4. (Sometimes) Wait for your organization to approve your request.


## Privacy and Microsoft Edge

Speaker: Scott Low and Melanie Richards

Scribe: Sebastian Zimmeck

### Presentation

Scott Low:
* Update of what the team is working on 
* Bunch of people involved; we are the messengers
* Quick overview of the [browser privacy promise](https://microsoftedgewelcome.microsoft.com/en-us/privacy); based on listening to our users and also what we follow internally; guides our work: protection, transparency, control, and respect
* Tracking prevention implementation is similar to Firefox; also collaboration with Disconnect
* Users want to be protected from the worst trackers (e.g., cryptominers); balanced setting also blocks advertisers; strict setting for most privacy-conscious users 
* From user studies we learned that most users know that web is ad-financed, but they also think that there is no privacy and control and that the deal between users and ad companies is lopsided
* We are exploring ways to give users more transparency and control; the browser should act as a real user agent (how is data being shared and with whom); we are doing usability studies
* We are also working beyond transparency; early on our research; what other types of value proposition we can make to users? Free digital subscriptions, coupons etc.
* Tracking prevention uses list based approach; largely North-American focused and likely underserving other geographies; we are working on machine learning to increase coverage
* We are aware of security concerns that machine learning entails
* Users would like to have more information about the trackers on the sites; how are trackers using resources?; this is completely on-device and Microsoft will not receive any data
* We are also giving users more control over the UX flows
* We are encouraged by standardization efforts
* Storage Access API is another area we are working on; doing user testing on this 
* Try to educated users via a pop up to explain what the particular API is doing
* Another area for our user resarch is the settings area

Melanie Richards: 
* Privacy preserving identity and login; the problem is here that some browser functionality may break; very interesting area we want to continue
* New web economies: how do we bring along the current economic model to the new privacy preserving ideas? We are also interested in new models that could work in the future.
* These are only very brief notes about two areas, but we also want to discuss privacy in this group more broadly

### Q&A

John Wilander:
* Great that you are so far along. Three questions: 
1. User studies. I spent a lot of time on tracking and tracking prevention and these features are still difficult to understand. Do you think users are really understanding the tracking mechanisms and implications?
2. Often thinking about the Western world. If certain domains are exempt from tracking prevention, that could influence how tracking is moving forward. Thoughts?
3. We want to have no compatibility issues. Users should not have to fiddle around with settings.

Scott Low:
1. Partially, the web is free because ads are used to monetize. On a high level that is understood by users. But they do not necessarily know exactly how much data is collected about them. We want to bridge this gap.
2. Geographical bias. When we are using a machine learning classifier. The big thing that we saw is the geographical bias. That is an advantage, but there may be others as well.
3. Agreed. We have an evangelism plan reaching out to sites such that the adopt the Storage Protection API. There are users who want to have more fine-grained controls and have maximum protection, but also able to allow certain sites to track.

Pete Snyder: 
* Concerned about web compatibility of new features.

Scott Low:
* We are early on. Seeing compatibility issues. Buttons do not work, for example. This is something we are interested in taking a look at.

## Privacy and Google Chrome
Speaker: Michael Kleber
Scribe: Scott Low

### Outline

*   Chrome's current default still allows 3rd-party cookies
*   We announced our intention to stop allowing them, with a two-year time horizon: https://blog.chromium.org/2020/01/building-more-private-web-path-towards.html
*   Goal: protections in line with our privacy threat model:
    *   Not possible to identify a single person across two different websites
    *   No website you visit can learn more than a small amount about you from off that site
    *   https://github.com/michaelkleber/privacy-model
*   What do we need so that we can get there?
    *   New ways tchromiumo support vital use cases that rely on 3rd-party cookies today — federated login, web payments, fraud prevention, DoS protection, online advertising, and more
    *   Prevent tracking through other techniques — browser caches, fingerprinting, network-based tracking, and more
    *   Goal: a well-lit path
*   Things we DON'T like:
    *   Restrictions that are easily circumvented
    *   Blocklists
    *   Prompting the user
*   Things we DO like:
    *   The Web
    *   Dedicated APIs, tailored to supporting their use cases while preventing cross-site tracking
        *   Good for developers, who can talk to us about their use cases
        *   Good for API designers, who can build to avoid circumvention
        *   Good for the UX of people using the browser, who can understand what's going on.
*   These are hard problems.  Chrome is willing to put a lot of work into helping develop new solutions, but this all relies on everyone being in an open conversation about use cases and the approaches.

* [Privacy Sandbox](https://www.chromium.org/Home/chromium-privacy/privacy-sandbox)

### Presentation

Michael Kleber
- Software engineer at Google working on Chrome's Privacy Sandbox. We are in a different place than other browsers. Chrome's default still allows 3P cookies
- This is in contrast to Firefox, and Edge, who block 3P cookies based on a list, and Safari/Brave, who block all 3P cookies.
- We intend to stop allowing 3P cookies with a two year time-horizon
- We want to transition to a web with protections that align to the threats identified in our thread model document. We don't want to enable cross-site tracking. We also want to limit what a site you've visited can learn about you when you're not visiting it.
- What do we need so that we can drop 3P cookies?
  - There are a lot of things that 3P cookies are used for that are important to the web:
    - Fraud prevention
    - Federated login
    - Web payments
    - DOS protection
    - Online advertising
    - And more
  - Online advertising is a big part of this discussion
  - A wide range of studies show that sites lose a half to 2/3 of their revenue when 3P cookies disappear
  - The first thing we need is replacements for things that 3P cookies are used for today
  - Second, a lot of money today relies on 3P cookies for use in tracking
    - When we replace 3P cookies, we want this to really eliminate cross-site tracking. We don't want to start the wack-a-mole with other tracking solutions
- Overall, our goal for 3P cookie removal to be part of a well-lit path where there are clear ways to do things that are important to the web platform. We also want transitioning to this new world to be easier than clinging to the old ways
- What do we not want?
  - We do not want restrictions that are easily circimvented
    - We don't want to move to a world where there are "good guys" who respect 3P cookie removal and other parties that resort to more covert tracking techniques
    - We don't like block lists. We maintain block lists for SafeBrowsing, for example, but we are painfully aware of the downsides of this mechanism. There can be unintended consequences, and believe these are unappealing from a tracking prevention use case
    - We also don't like user prompts in general. It is a super difficult challenge to prompt users in the moment in ways that make them happy and leave them well informed. So we are strongly inclined towards solutions that do not involve showing prompts to users
- What do we like?
  - We want to preserve the web as an open platform for diverse and rich content
  - We are interested in dedicated APIs that preserve use cases while preserving user privacy at the same time
    - We like dedicated APIs because we can talk to developers about their use cases and then help give them a solution
      - These discussions are taking place across various W3C channels
    - We also like them because they can be deliberately designed to avoid circumnavigation
    - If the browser ever needs to ask the user a question, the browser is in the right place to ask users that question
- Takeaways
  - These are hard problems. Removing cross-site tracking is hard enough as it is. Doing it so that existing use cases are preserved are even harder. There are lots of "cool kid" words here like differential provacy, secure multi-party computation, blinded signatures, etc. It's key that we understand use cases, however. So we encourage everyone to come together to help articulate their use cases so that we can create better solutions together

### Q&A
- Joel: About the deprecation of 3P cookies, can you give us an idea of when the earliest you'd expect that to happen is?
- Michael: The earliest is sometime in 2022. Predicting anything about the exact timing is beyond my capabilities. Our goal is not a specific day/schedule. Our goal is to address the needs of the ecosystem first so that we get rid of 3P cookies without causing a huge amount of breakage. 

- John: Super happy that Chrome is part of the CG! Three questions
  - Any thoughts/opinions on the Storage Access API in Chromium?
    - Michael: For SAA, we expect to talk about it in the dedicated session tomorrow. At a high level, Chrome is not deeply enthusiastic about the way this API looks right now for a couple of the reasons I mentioned in the talk. The prompting that can happen seems like it is a bad question to ask the user because they have a hard time understanding the implications. We'd be interested in seeing the UX research from MSFT to understand whether they're seeing something different than we are. We'd far prefer there to be dedicated APIs for the dedicated thing that you're trying to do rather than a catch-all access API that makes it difficult to say what exactly you're trying to do. Federated login is an example. Seems better to ask users if they'd like to sign in rather than generically ask them about storage access.

- Sebastian: Dedicated APIs seem to be the central mechanism you're focusing on. I'd like to understand a bit more about what exactly this means. Are you talking about creating new APIs? Or repurposing new APIs?
  - Michael: We are interested in creating new APIs (either JS or HTTP headers) that are custom built for specific purposes. There are a number of proposals (I will include [Privacy Sandbox](https://www.chromium.org/Home/chromium-privacy/privacy-sandbox) link in the notes) which has a list of a bunch of different APIs that are in various stages of discussion. A new first class login API is one such example. Replacing the UA string with Client Hints headers is another. For ads, there are a variety of APIs we have proposed such as FLoC, TURTLEDOVE, Conversion Measurement API, Trust Tokens, Aggregate Conversion Measurement API, etc. 
  
- Tom: I have a question about Privacy Budget and Sandbox. I feel about I hear about these all the time. I feel like y'all haven't made a lot of public development in terms of Privacy Budget and its guardrails. This is one of the more exciting things on the table right now.
  - Brad Lassey: Paul Jensen is leading this work and is also on the call. We've spent most of the time working on infrastructure on how to do the Phase 1 study in a way that could provably show that Chrome isn't collecting fingerprints of users. That has been difficult and very much internal. Apologies for that. We're progressing quite nicely and we're getting close to launching the study. I believe in 85.
  - Paul Jensen: We're working hard to land the infrastructure to measure the entropy of different surfaces in the browser. This should land in the next few months and we should have more data to share after that.

- Maciej Stachowiak: Is Google planning on bringing any of the Privacy Sandbox proposals to the Privacy CG
  - Michael: Privacy Sandbox is sort of an umbrella that's a convenient way to gather all the proposals we're making. A lot of the individual API work we're discussing is in a bunch of different W3C groups that are about the use cases that they're addressing. I'm not sure how to balance the fact that there's a PRivacy CG that's cross cutting with the fact that there is the WICG and other groups that are more appropriate for specific APIs. We'll need to figure the answer out here on a case by case basis.
  - Maciej: Are there any cases that are likely?
  - Brad: We're getting good engagement on the advertising APIs in the Web Advertising Business Group. I do think that the first thing you mentioned (Privacy Budget) and Willful IP Blindness make sense to discuss in this forum as we were only recently able to join this CG. We haven't proposed these yet, but they sound reasonable. 
  - Michael: This and the fingerprinting stuff makes sense to discuss here. 
  
- John: Regarding the two year plan, I'm wondering about what you believe the 3P cookie deprecation will look like in terms of adoption and when the web ecosystem will be ready. It sounds like you're going to ship a bunch of replacements and then gradually remove the privacy-infringing replacements. I won't be surprised if staying with the old techniques will be more profitable.
  - Michael: I would say first of all, that I regret calling Privacy Sandbox Privacy Sandbox because there's not one single API. It's a collection of APIs that we'll turn on one at a time as they become available. Things will be coming online (first one in M84) so that folks can experiment throughout 2021 while they continue making money with 3P cookies. We want to give a long runway so that folks can give us feedback so that by the time we get to the time when we believe that use cases have been met, we've had lots of opportunity for the ecosystem to talk to us and no one is surprised. Yes, I'm sure there will be people who cling to the old way as much as possible, but anyone who is surprised by the old way at that point will be because they have deliberately not been paying attention.
