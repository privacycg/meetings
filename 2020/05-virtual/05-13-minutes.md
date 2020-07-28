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

## Attendees

* Allan Spiegel (Adobe)
* Andrea Marchesini (Mozilla)
* Andrew Knox (Facebook)
* Anne van Kesteren (he/him; Mozilla)
* Arthur Edelstein (Mozilla)
* Bill Densmore (ITEGA.ORG)
* Brad Lassey (Google)
* Brandon Maslen (Microsoft)
* Charles-Henri Henault (Criteo)
* Charlie Belmer (DuckDuckGo)
* Charlie Harrison (Google)
* Chris Needham (BBC)
* Chris Needham (BBC)
* Chris Pedigo (DCN)
* Christine Desrosiers, independent consultant
* Christy Harris (Future of Privacy Forum)
* Clare O'Brien (ISBA)
* Daniel Appelquist (Samsung)
* David Benjamin (Google)
* David Reischer (Permutive)
* David Senecal (Akamai)
* Devin Rousso (Apple)
* Erik Anderson (Microsoft, co-chair)
* Frederik Wordenskjold (Google)
* George Fletcher (Verizon Media)
* Hannes Kuhl (Trakken Web Services)
* Jack Frankland
* James Hartig (Admiral)
* Jeffrey Yasskin (Google Chrome)
* Joel Odom (Salesforce)
* Joey Salazar (ARTICLE 19)
* Johann Hofmann (Mozilla)
* John Wilander (Apple WebKit)
* Jullian Bellino (Criteo)
* Kate Cheney (Apple)
* Kaustubha Govind (Google)
* Konrad Dzwinel (DuckDuckGo)
* Kris Chapman (Salesforce)
* Krzysztof Modras (Cliqz)
* Kushal Dave (Scroll)
* Laszlo Gombos (Samsung)
* Lisa LeVasseur (Me2B Alliance)
* Lucas Adamski
* Lukasz Olejnik (independent researcher)
* Maciej Stachowiak (Apple)
* Mark Xue (Apple) 
* Marshall Vale (Google)
* Melanie Richards (Microsoft)
* Michael Kleber (Google)
* Mike O'Neill (Baycloud Systems)
* Naïma Conton (Sirdata)
* Nick Doty (Berkeley)
* Nicolas Arciniega (Microsoft)
* Paul Jensen (Google)
* Peter Saint-Andre (Mozilla)
* Peter Snyder (Brave)
* Pranjal Jumde (Brave)
* Russ Hamilton (Google)
* Sam Macbeth (Cliqz)
* Sam Weiler (W3C/MIT)
* Sarah Cometa (Permutive)
* Scott Low (Microsoft)
* Sebastian Zimmeck (Wesleyan University)
* Shaun Gilmore (Apple)
* Shivan Sahib (Salesforce)
* Shivani Sharma (Google)
* Shuran Huang (Google)
* Steven Englehardt (Mozilla)
* Tanvi Vyas (Mozilla, co-chair)
* Theresa O'Connor (Apple)
* Thibault Montanier (Sirdata)
* Thomas Steiner (Google)
* Tom Lowenthal (Brave)
* Walz (Google)
* Wendell Baker (Verizon Media)
* Wendy Seltzer (W3C)

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

## How to join the Privacy CG

1. Get a (free) W3C account at https://www.w3.org/accounts/request.
2. Log into https://www.w3.org/community/privacycg/join with your W3C account.
3. Click "join".
4. (Sometimes) Wait for your organization to approve your request.
5. Email group-privacycg-chairs@w3.org with your GitHub username so that we can invite you to Slack and our GitHub organization

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

## Privacy and Microsoft Edge

Speaker: Scott Low and Melanie Richards - [Slides](https://github.com/scottlow/PrivacyCG-F2F/blob/master/2020.05.08%20Privacy%20CG%20F2F.pdf)

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

## Privacy and Brave

Speaker: Pete Snyder

Scribe: Melanie Richards

### Presentation

* Will talk about what Brave deploys right now, and two places where we're doing privacy protections in different ways than eeryone else, and longer term ideas.
* Going on in Brave right now....
* Shields analogy, contains a bunch of params. We ship a bunch of blocklists on by default. EasyList/EasyPrivacy, Disconnect, uBlock Origin...not just the filter lists that come with that but also replacement scripts. So instead of blocking a tracking lib, replacing it. Scripts we inject in pages to block nasty parts we can't replace. Also Fanboy Notifications for blocking annoying popups
* [adblock-rust](https://github.com/brave/adblock-rust).
* We run SlimList on iOS, shipping just the most important rules there, with opt-in for additional
* HTTPS Upgrades, exactly what HTTPSEverywhere does, looking at another implementation, upgrade where useful
* Cookie policy: by default block 3p, don't send cookies to 3p origins. Cap JS cookies to 7 days, most useful in 1p context. Follow what Safari pioneered. Relative to eTLD+1 of top frame
* Also in Shields: fingerprinting
* Not direct Shields toggles but also controlled: cross-site request...
* Shields grab bag...running experiments over the summer. Exceptions to all these things: cookies, referrer, fingerprinting, filter lsit exceptions, github.com/brave/*
* Fingerprinting protections: transition period right now, only apply protections in 3p frames. All those protections w/ small exception, basically disable APIs. Standard privacy through generalization. Moving to a different system, shipped some of this stuff, under active development. Call this farbling: minor rand. to confuse fingerprinters. "anonymization thru generalization does not sufficiently protect anonmity." So much diversity, if we try to fight by making everyone look the same, that's a fool's errand.
* Instead of everyone generating same fingerprint, have everyone generate diff. fingerprint each session.
* Our goal: sidestep web compat concerns if we're returning ran values. Generate session seed, mix that with eTLD+1, generate unguessable seed for top-level domains, randomize for that seed. Random values for each session...3p frames embedded in 1p context all use the same seed, preventing new tracking if each frame got their own seed.
* Under active development: move everything to farbling, 3 different levels of config. Off (do Chromium), default: alter returned values with noise, make sure different on each query. Goals: confuse fingerprinters, expect that for at least some, susceptible to some statistical attacks. Want to drive wedge between benign and [nefarious] use.
* Last we have max setting, don't return underlying values, pure random, prevent statistical attacks.
* How we're doing farbling for canvas: readback APIs, that's usually what gets addressed in fingerprinting libs. Flip low-order bits deterministically based on session seed. If someone wants strongest possible, we return random-looking white noise derived from seed. In places where you get different responses (like Boolean), difficult to do this noise, leave it alone.
* For web audio, generate random noise from seed.
* Shortlist of additional APIs for farbling: (listed on Github, issue #8787)
* Where we're going slightly different direction from others is storage. Limit cookie lifetimes set by JS, don't allow storage in 3p frames, (eLTD+1 frames do get storage)..we have short exception list for domains. Most of that is equivalence domains, one company owning two domains etc.
* In short term (day to week), fixing a quirk to Chromium process, if you try to do introspection into storage, throws an exception.
* Medium-term goals, give 3p frames frame lifetime storage.
* Longer term, places we know we need exceptions, using Storage Access API as escape valve
* Things longer down the line, not sure, experimenting: garbage collecting JS storage, known script tuples we can block in 1p. Impose interventions.
* Other longer term research projects: concerned about cross 1p tracking via link decoration. Want to build out protections programmatically. Project to generate privacy-preserving resource replacements. Also want to farble additional endpoints almost inevitably. Last, don't have good deployed answer to font fingerprinting, working with W3C and internally on what we can ship in web-compat-friendly way.

### Q&A

* Scott Low: super informative, love the work. Curious if you've seen any known compat issues arise in Nightly around fingerprint randomization? How have you been able to measure? Are signals looking pretty good?
* Pete: almost across the board, dramatically better. Because currently deleting APIs, mostly fine but may break stuff...one exception w/ web compat issues, drawing application where it's almost impossible to, the modifications are so subtle, applied at readback, those use cases are going to be modified if you play with those APIs enough. Take it as a known issue, not sure what to do with it. But long tail...main message is, almost completely web compat win for us.
* Steven Englehardt: thanks Pete, appreciate innovative stuff. Thanks for the work. How do you handle and discover and handle site breakage? You have an aggressive default set of protections deployed. We did a study trying to turn on 3p cookie blocking, we lost 1% of users from that study in a week (back in ~Dec 2018), so we know they were hitting breakage.
* Pete: people using Nightly have been terrific in filing issues, of course not scalable. Also use Page Graph system, very hi fi recording of interactions on a page, use that to build a classifier of issues...up to 75% accuracy, not there yet as first line of defense but hopeful...difference in resource requests, mostly an aid in labeling web compat things. Other channels where we work with partners on web compat things.
* James Hartig: curious how a site that operates in 3p iframe might detect that cookies are blocked or how that works in practice. Can cookies be set and immediately thrown away? Exception thrown?
* Pete: you write something to document.cookie, nothing gets record, and if you try to read localstorage, exception thrown. All the upstream stuff. Not really ideal, but point developers to these signals.
* Maciej Stachowiak: interested in the noise-based fingerprinting stuff and how it's working out. Seems like a cool approach, I've always wondered whether noise could be added at low enough threshold to not break things. Any math analysis to whether this noise is robust to tricks for removing it? There are some obvious ways to back out the noise, have you done deeper analysis.
* Pete: thanks for the feedback on Twitter, super appreciate it. It terms of robustness of attacks...we expect at least some of these will be addressable, special-caseable etc. Goal is to confuse naive fingerprinters, wedge between benign and malicious. For version 2, responding to issues you raised. Defeatable through 100s of 1000s of queries, but someone doing that many canvas readbacks w/o user activation: pretty strong signal to lay a hammer down on. Haven't done mathematical modeling across the entire platform for privacy benefits, but we're confident most not reversable and others would look suspicious.
* John Wilander: question on standardization on anti-fingerprinting measures. Do you expect we could standardize, or an optional in the spec about UA introducing noise?
* Pete: early days, haven't pushed for anything to be standardized. Mostly raising issues on what we're modifying, let's consider addressing this, if WG wants to standardize randomization, great. Not married to solution, more want to make sure it's got legs right now.

## Privacy and Safari

Speaker: John Wilander

Scribe: Wendy Seltzer. 

Presentation: [add link later]

### Presentation

John Wilander: Webkit team at Apple. Privacy and Safari
* Privacy is about people. Consumers, customers, users. 
* It should be safe to click a link. Not just security, privacy included. Bad things shouldn't happen by linking.
* Threats, policy, implementation, future.
* Threats: 
 * to protect identity (not just PII); browsing activity; personal characteristics, e.g. behavior; autonomy, being free from manipulation. Affects our view of APIs: can the user be tricked or harassed into something?
 * attacks: who could be an attacker. ad tech, data sellers, political operatives, even browser vendors; 
 * attacks: how: client-side state, fingerprinting, collusion, grabbing stable identifiers
 * attacks: why: financial benefit, political influence
* Policy. https://webkit.org/tracking-prevention-policy
  * Prevent all covert tracking and cross-site tracking. Stateful tracking e.g. HSTS supercookies; fingerprinting; any hidden data-storage w/o user control
  * Limit capabilities if we cannot prevent. 
  * if we can't limit, ask for user consent. see Storage Access API
   * no exceptions. no blocklists. As stated in our policy, same rules for all, don't enable circumvention by new domain; but we'll apply specific rules if we see bad actors
* Implementation timeline
 * 2003 - Safari 1.0 blocks 3rd-party cookies
 * 2005 - Safari 2.0 offers private mode
 * 2010 - Rise of social media
 * 2011 - Do not track
 * 2013 - Partition 3rd-party caches and DBs
   * rise of ad-blockers, history leakage, retargeted ads, microtargeted malicious content,
 * 2017 - ITP (Intelligent Tracking Prevention)
 * 2018 - Storage Acess API
 * 2019 - Private Click Measurement - still in standards process
* Stateful tracking defense. 
* Fingerprinting defense. 3-pronged approach
 * Don't implement fingerprintable features, those that reveal hw-specific things, custom installs. can revisit if we can protect them.
 * Remove entropy, e.g. fonts, DNT, software update
 * Alter features to add protection: require user permission to access device orientation or motion; prevent fingerprinting of cameras and mics when using WebRTCV
* Future
  * Storage Access API 
  * Private click measurement
  * IsLoggedIn. disentangle logins from cookies. 

### Q&A

Sebastian: Thanks. re disentangling login from cookies, can you explain more? As I understand, most logins use session cookies. 

John: The [isLoggedIn proposal](https://github.com/WebKit/explainers/tree/master/IsLoggedIn) is in the webkit repo. Not that cookies have to stop being used for login, though we'd be happy to see e.g. HTTP state tokens. We're interested in the binary state of is user/site logged in, to know to treat it differently, stay logged in if the user wants, and if not logged in, don't persist data. 

Zach Edwards: you said one of the reason for creating these controls is bad content, such as ads foll; how does Apple determine what is good content? 

John: Don't think it's based on Apple's determination of what's good content. Concern that high-value content loses, story from Walt Moskowitz recode.net; advertisers only needed to run ads briefly on recode.net, get the id, and then retarget those users elsewhere where ad inventory is cheaper. 

Lukasz: Don't implement fingerprintable features. Any specific features you didn't implement? 

John: lots of factors go into what features we implement: performance, priorities. fingerprinting plays a role. Battery status, bluetooth, reading sensor data, connecting a peripheral. 

## Question re: Trust Tokens

Bonus question from Maciej: Google folks, do you intend to bring things to PrivacyCG. Curious about trust tokens, which seems an interesting building block. 

Brad: Trust tokens is currently in WICG. Happy to discuss further here if people want. 

Tanvi: If someone wants to request an ad-hoc 1/2 hr meeting, just ask.

## Privacy and Samsung Internet

Speaker: Laszlo Gombos

Scribe: Pete Snyder

### Presentation ([slides](https://docs.google.com/presentation/d/1urhi2Wd0OPkknppqznQuQjBESf3SFwCmq4R8gVA_eKQ))

Laszlo: Samsung browser is mostly for android devices, targeting Samsung galaxy, but also supports other devices
chromium based, with privacy enhancements.  Privacy is one of the reasons S built a browser.
What is privacy? to give users control and transparaecny. Includes DuckDuckGo and Quant as search engines.
Browser includes options to clear storage / data, but category.  Also option to toggle cookies.

Browser timeline
 - 2016: add content blocker using Safari API, list based.
 - 2016: add "secret mode", similar to private browsing, but covers the entire application (instead of one tab).  Requires auth, and gives differnt default settings
 - 2017: add tracking blocker, w/ disconnect list
 - 2019: tried to improve tracking list through ML (i.e. smart anti tracking, SAT).  ML results are augmented by static lists.  Whole feature is opt in
 - 2020: changes to referrer policy, improve compat with logins.  Feature is still opt-in (and can be either global or only for secret mode).  Also advertised with onboarding / first use
 
Future plans:
 - better privacy mode, between SAT and current default.  Want this new method to be default on, web compat focused
 - add extension API; working on ways to do this in a privacy preserving way
 - excited about Storage Access API, hoping to ship
 - looking for other ways of allowing sites to monitize, w/o tracking
 

### Q&A

Tanvi: With SAT v2 (giving storage access on heuristic basis), is that in chromium source?  Upstream?
Laszlo: downstream currently.  But interested in sharing if others are interested

npdoty: thanks for discussing extensions; what kinds of privacy threats are you seeing / interested in?
Laszlo: its a defacto standard. Samsung tries to preserve the current API, but the current APIs expose page content to the extension; a problem!  Current approach is to handpick / allow list which extensions are installable, though scaling problems

npdoty: do you have any solutions in mind
Laszlo: just curuation currently, but may try to reduce APIs going forward

Tanvi: re Storage Access API heuristics, what are the heuristics?
Laszlo: User engagement (if the user has interacted with the domain)

Tanvi: So not relevant to frame?  Just any interaction in any context?
Laszlo: yes

Tanvi: Do you think mobile browsers bring different expectations than desktop browsers? e.g. anti-tracking, diff per platform?
Laszlo: We treat them similarly, and know that mobile browser can be used similarly to desktop browsers.  We don't see a fundamental difference

## Privacy at the W3C

Speaker: Pete Snyder and Wendy Seltzer

Scribe: Michael Kleber

### Presentations: Privacy Interest Group (PING) & Improving  Web Advertising BG

#### PING ([slides](https://www.peteresnyder.com/static/slides/2020-privacycg-f2f-ping.pdf))

Pete Snyder (Brave): Privacy Interest Group (PING)

* PING's main role: to review other peoples' specs, find issues that need to be addressd to preserve privacy platform, specs that are coming out of WGs and IGs, often at transition points
* Things we find may be brought up as PING, or as individuals, or as some combination
* Documenting known privacy conerns, which might make their way into non-normative text
* Discussions with WGs, either in github or in phone calls, to figure out conerns and best ways to deal with them
* File issues on github for what we find
* Not very often: file objectios to specs going forwards, escalate
* Issues are filed in the owner's repo and a tracking issue in PING's repo
 * (process changing over time though)
* Also try to document best practices, to give guidance to groups and share PING's expertise, to smooth everyone's experience and so they know what to expect
 * Questionnaire on privacy, akin to the security questionnaire
 * Fingerprinting guidance doc
 * Blog post about patterns in standards that are privacy concerns
 * In progress: Privacy Threat Model
* Privacy Threat Model
 * Under development, led by Jeffrey Yasskin
 * Define the important boundaries that should exist on the web to make it respecting of privacy
 * What is in scope, what is out of scope
 * Hope to reduce conflict over privacy issues
 * Keep one spec from un-doing protections we're trying to provide elsewhere
 * Welcome more authors and more involvement
* We're eager for more people to become involved.


#### Improving Web Advertising Business Group
([slides: Web Advertising BG and W3C](https://docs.google.com/presentation/d/1maebmIa6dmAj7-7MaB2QukKJd_k4UrUZhFLiA4i67lE/))

Wendy Seltzer (W3C): Improving Web Advertising Business Group

* Wendy: W3C strategy lead, looking at gaps in the platform and where the w3c needs to bring in new communities
* W3C welcomes you!  It is a _voluntary consensus standards organization_, looking for cooperative solutions that everyone chooses to agree to
* Standards work well for a shared techcnical problem, a "good enough" technical solution that you all come up with, and ecosystem commitment to ship it.
* Web Ecosystem has many participants -- people, publishers, advertisers, web browsers, tech
* Want to meet user expectations of the privacy of the web
* Web Advertising Business Group goals: Stop individually-identified cross-site tracking, and provide monetization techniques to support the open web
 * Business group's goals include developing a list of the business use cases that we can try to support — see "Table of support for advertising use cases" https://github.com/w3c/web-advertising/blob/master/support_for_advertising_use_cases.md
 * What new APIs and designs can we add, as tracking goes away because browsers decide to not support it any more
* In the W3C process, Business Groups are not spec-producing
 * Produce business requirements, use cases
 * send those requirements and use cases on to:
  * Working Groups that can produce Recommendations
  * Interest Groups and Community Groups, that can incubate, including this one
  * External groups, including WHATWG, IETF, IAB Tech Lab, etc., depending on who is in the best position to build specifications and consensus around solutions


### Q&A (on both PING and Web Adv)

John Wilander: I've attended a few web-adv meetings, and I know rules differ from group to group.  I've gotten the feeling that things rarely leave the business group — some tech news publications have printed leaked stuff from the BG, but things are not supposed to be attributed
Wendy: Discussions are supposed to be a members-only space, but most of the material produced is in publicly-visible github repositories, and we encourage people to engage that way.  Other than meeting minutes being treated more confidentially, we do want open communication.  I'll take your input to push in that direction.

Maciej: You mentioned that BGs don't produce specifications, instead produce use cases.  How do you square that with Google's use of the BG to develop things like the ad work they've talked about there

Wendy: We're not producing specs, we're discussing proposals to understand what a solution would address if it were made a spec.  Does it serve the needs?  If Google were to move something towards being a spec, it would move elsewhere

Brad Lassey: That's exactly the same as a community group like this one, where things can be incubated but not specified

Maciej: Are BG's supposed to be doing the same things CGs are?  Some outcomes are not specs, but are things that look spec-like, to be taken up by WG's that do produce specs

Wendy: When there's work on a proposal, it happens in either a community group or a working group

TomL: This certainly leads to confusion among people less familiar with w3c process flow and nomenclature

Nick: Great to see these two together.  How would we resolve differences between the two processes?  What if web-adv BG comes up with use cases that conflict with the PING threat model?  Sounds like web-adv has one principle shared with PING (no cross-site user tracking), but will that be updated over time?

Wendy: As new threats & concerns are added, we want to bring that to the overall picture of what's going to be overall acceptable on the platform.  Web-adv BG's goal is a privacy-preserving platform that has opportunities for monetization, but not willing to compromise privacy in order to get there, which we're hearing from implementers as well.

Pete: From PING side, we would treat it like anything else: We raise concerns on specs if we think they are going in a problematic direction, and we're happy to give that feedback at earlier stages before spec process too.  We have lots of options, including formal objections

Wilander: At TPAC we had several conversations about where to go, for targeting ads in a more privacy-friendly way among other things.  Those conversations are happening _only_ in the web-adv BG.  Question that should be addressed early-on include "is there multi-vendor support for on-device targeting at all?"  It's kind of one-sided if this is only discussed in web-adv, and not as a platform-wide "should we do this at all?" thing.

Wendy: We are hearing the use cases, and I'd encourage folks to come and participate in the web advertising group to share the perspective on what proposals would be of interest to multiple parties.  Ultimately w3c is about platform-level standards and what can get implementation across the platform.  We want privacy to be met by standards.  This is early-level discussion, and if one party is interested in considering things that would meet some of the needs, we're interested in letting everyone see what the trade-offs would be.  This is where we can find whether there is interest in finding something to standardize.

Kris Chapman: In the ad tech ecosystem, they are much more likely to find the web-advertising business group, because from their perspective they don't think of these topics as _privacy_ features, they see them as business questions.  I'm in both, and I see more ad-tech and mar-tech folks showing up in the BG, hoping to find the browsers there, don't think/know that they should come to privacy groups, want a one-stop shop.

Tess: For Pete & Wendy: Privacy CG is new, this is our first face-to-face, still figuring out how to get better at what we want to do.  Any suggestions for what we could be doing now, to work better on topics of mutual interest?

Pete: PING vs PrivacyCG has nicely aprtitioned the space, and we have a lot of membership overlap.  It's particularly useful to PING to knwo where Privacy CG are planning new features, so that other specs (where we do horizontal review) don't fight against that.

Wendy: W3C sends resources to groups to help them understand how to participate in the standards process.  We can send them and their proposals to this group when at the right stage to move towards incubation.  Let's keep working together to not duplicate discussions, but keep them moving forward

Maciej: Wendy, what's your recommendation for folks interested in the discussions about the proposals in the web-adv BG but are not comfortable with its secrecy rules?  Almost everything else is in the open; keeping discussions secret is uncomfortable for us.

Wendy: Most discussion is happening in public, in github repos and the public mailing list, so participating in those should be available.  I agree that at the point it comes to spec development, it should move to a forum that's fully public.  If people want a space to talk about how to phrase our concerns so that they can go into a public forum, I don't mind hosting such a space

Wilander: Re. Kris: I agree that a lot of people have found the web-adv BG and are using it as the interface with the browsers, and I'm worried that it is not that kind of a group, and we're misleading people into thinking they're in a forum with multi-browser interest, but really it's not.  I think it would be beneficial to open the group up, to get other browsers into the discussion, and we know that if we reach consensus, it will have multi-browser support.

Wendell Baker: Verizon's Perspective on web-adv BG (but has been sitting on privacy CG also): We want to understand what the next web platform is going to be about, and if possible contribute to that.  I see Wilander worried that people are misled into thinking it's the right venue to talk to the browsers, and the fact that browser vendors are there does give that impression.  We just want to know what the future will look like.  From the perspective of someone in commerce, the idea that there is a privacy sense isn't the key value — we all want it to be safe to click on links, we want a web that's safe, but also one where we don't need to rebuild things every few years because the get broken.  We want to know what we can build on that will remain reliable, what we can sell on in the future.  I know a lot of work is supposed to be open, but it's very troubling when I'm in a meeting, there are articles about it in the trade press the next day, and I was there and the reporter wasn't there and the report is wrong.  So there definitely is benefit to having a private venue for these conversations, though if that's a blocker for getting more participation, maybe we could give some of that up.

Kris: +1 (from slack)
