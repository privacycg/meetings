Privacy CG Virtual Face to Face, May 18-19, 2021


## Introductions/Announcements
* Please write something about yourself next to your name on the attendee list.
* Use slack for back channel conversations
* Volunteer to scribe!
* Encourage folks to use video.
* ...
* Note on Upcoming WebID workshop - https://github.com/WICG/WebID/blob/main/meetings/2021/25-26_May_2021.md - for those interested in discussing WebID/Identity further.
* 

## [Storage Partitioning](https://github.com/privacycg/storage-partitioning) (Anne Van Kesteren, scribe Peter Saint-Andrew
Adjacent Standards update (Anne van Kesteren [Mozilla])
* The purpose of SP is that nested documents shouldn't be able to share state with other top-level documents
* This is a big effort because traditionally a lot of state has been shared among these contexts (font cache, cookies, DNS, etc.)
* All major implementations have shipped for network state (not usually used by web developers)
* Things are still in flux for storage in particular
* WebGL and WebGPU are also affected
* *  [Partitioning + Storage Access API](https://github.com/privacycg/storage-access/issues/75) (John Wilander  [Apple])
* This comes down to whether browsers should partition by default - including cookies
* WebKit has been partitioning storage since 2013
* Cookies are a different story
* We partitioned in 2017-2018
* No real privacy benefit compared to blocking 3P cookies, but developers found partitioning to be confusing (just wanted access to their 1P cookies)
* Eventually removed support for partitioned cookies
* More recently have received feedback that partitioned cookies might be useful for some scenarios (e.g., entities that are never first party, embedded content)
* This issue is about whether we could enable entities to explicitly opt in to partitioning
* Tess [Apple,Chair]: I think that makes a lot of sense, we won't have the compat issues we had before, also reduces UX friction
* Kaustubha [Chrome]: Chrome is also interested in having partitioned cookies, we're thinking we could have a setCookie attribute `partitioned` instead of using the Storage Access API; one issue seems that this would require a script to be loaded and that could introduce latency - https://github.com/WICG/CHIPS
* JohnW: would there be a way for the site to know that a browser is compatible with this and did indeed partition?
* Kaustubha: good question, there's an open issue on our repo in WICG on that right now; potentially we could add a header specifying that this came from a partitioned context; not sure what a site/browser would do if an entity didn't understand the new attribute
* Johann[Mozzilla]: would that header indicate all the cookie policies or only partitioned state?
* Kaustubha: this would be specifically for partitioning, not as expansive as all the other policies
* Johann: from Mozilla perspective, there should be no restrictions on this call; one major concern is around interop and adding more API surface that might work differently across browsers (e.g., Firefox has partitioned cookies by default so it'd be a no-op for us)
* Kris[Salesforce]: Salesforce is definitely interested in this to embed content in client sites and also in our own administrative tools; Storage Access API typically doesn't work for us because we're often embedding things like graphs and tables, which users aren't expected to interact with
* MarkL[Carbon]: I have a question about coherent state and allowing the server to understand what context it is in; the real use case is the ability to have consistent state across applications and multi-domain contexts, sass models, consent management; there's a known need to define a consistent context
* Erik: I'm trying to unpack that - some of it sounds like First Party Sets, some of it sounds even more broad; this issue is about third-party embedding and the like, not state across top-level sites
* Lee: similar input to what Kris said
* JohnW: regarding request header, there may be developers that want to know their cookies will be partitioned (e.g., so they don't overwrite their 1P cookies or don't have a confusion of cookies across 1P and 3P contexts)
* JohnW: all along I've been worried about embedded logins; I try to encourage users to take you to your website, login there, and then go back to the 3P context; in a purely 3P iframe you don't have that level of trust (e.g., can't check certificate or whatever)
* JohnW: we have to decide what to do if there are conflicting instructions from multiple iframes from the same origin
* JohnW: if we use Storage Access API, an origin could request a specific partitioning key - we need something broader than partitioning, embedding site could request cookies for all of their domains
* Michael Lysak[Carbon]: a small note, you state that there's a privacy and security bug, concerns about consensus on that, might be helpful to have a privacy definition there instead, via threat model discussion (just starting in PING and more presentations next week) later discussion wanted to flag.
* Erik: I get a little nervous about global changes for a page; sometimes websites have multiple authors, CMSs, etc. - generally in favor of something that is more tightly scoped, like the attribute that Kaustubha mentioned
* Kris: we are supportive for First Party Sets for sure, but the issue is that we own 400 domains, so 5 is an unworkable limit - users have no idea that we acquired Evergage, for instance, and don't have knowledge of or interaction with that domain; we have security concerns about migrating everything to a single auth/login domain; in general we'd prefer a header or attribute approach, not a JS approach (e.g., some clients don't like us to use JS because of potential exposure of loading a script)
* Lee: I'll add that we definitely prefer the header route, but I'm mostly interested in federating identity in an iframe; we do have large customers who federate the identity of the parent page with another identity provider and the OpenID Connect flow happens in the iframe, but if cookies can't be accessed then the flow will fail (things like CSP Frame Ancestors can help here)
* Erik: this proposal doesn't necessarily help with implicit auth flows, could be something that gets discussed in the workshop next week
* Kaustubha  [Chrome]: re confusion between 1P and 3P contexts, we're hoping that a `partitioned` header attribute could solve that; second point about the limit of 5 domains came from an origin trial, haven't settled on that and it was only for the purposes of the experiment, so don't anchor on that (having said that, 400 might be too much, but perhaps not all of them need to be in the same set); also check out WebID and upcoming workshop
* Erik: do note that we'll discuss First Party Sets again later in the F2F
* Mark L: in sandpiper (link to follow) we looked at how the certs can augment the solution
* JohnW: the request header makes me worry because it moves away from the rule that cookies on requests only say name and value but nothing else (when you get cookie back you don't know the expiry etc.); once we have partitioning it might be interesting to explore more use cases such as FPS partitions vs. site-specific and we don't want servers to write code that bakes in certain partitioning logic
* Kaustubha: we'd proposed the __Host cookie name prefix, want to bind cookie to origin; also you're talking about a mix of partitioned and non-partitioned cookies, is that right?
* JohnW: partitioning is a technology that browsers can use to partition, but if we send signals to the server then servers will become dependent on those factors; keep it in the browser, not the server
* Tess: this has been a great discussion, but I want to take a step back: partitioning is a big effort that touches a lot of specs and so much of the platform, I'd like to ask Anne where do you need the most help right now (e.g., on a particular spec)
* Anne: for network partitioning there's more that could be clarified in Fetch (e.g., impacts on HTTP cache, connections, DNS cache, and in general lower layers of the stack); also HSTS has had a bunch of changes; the more difficult things are local storage, service workers, cookies - it's harder to standardize on these things because there's contention about what the behavior should be
* Tanv  [Mozilla, Chair]i: we have a session tomorrow about work item standardization and which specs can be moved forward; perhaps we can discuss tomorrow about how to converge among browser vendors or have an ad hoc session to work through these.
* Anne: if you have cycles, I'll give you pointers on how to contribute
* END OF TOPIC


## [Storage Access API](https://github.com/privacycg/storage-access) (Johann [Mozilla], scribe Ben Savage) 
### Update on Recent Changes
* [Link to Johann’s presentation](https://docs.google.com/presentation/d/1Ix0jJOiCtpGq3VmXD9AWGl90ko-AvN8Lhz2vNuv5dU8/edit?usp=sharing) 
* Johann: Co-editor of storage access API. Quick status update on 2021 progress. API to help sites get out of storage restrictions. Shipped in Safari, Firefox and Edge. Specification is a CG draft. Ad-hoc meeting in Feb to align on several issues - able to find common solutions. Added Johann as spec editor. Unable to align on “active” vs “passive” storage access. Progress on rSA and hSA algorithms. requestStorageAccess will always require user activation. Handled top-level opaque origins (essentially blocking access for them). Formalizing nested iframe support and Permissions Policy Integration. Up next: add better mechanisms for embed-only frames to retain storage access. Reject with meaningful exceptions vs undefined. Avoid races on user activation. Etc. Topics for today: Status on passive storage access after explicit user opt-in. Also, give sites a way to observe availability of storage access. 
### [#2 - Active or passive storage access after explicit user opt-in](https://github.com/privacycg/storage-access/issues/2)
* Johann: Active / Passive Storage access after explicit user opt-in. In FF and Edge, when you request storage access then reload, the same frame remembers storage access. You don’t need to call it again. Doing this for convenience for developers. User-cases benefit from this, break in Safari where iFrames repeatedly have to request (and get a user interaction). Would like input from Webkit folks. Been trying to use Safari and got mixed results. 
* John Wilande [Apple]: Our reasoning around this goes back to “active” and “passive”. We decided it should be an active choice, per page, if a user wants to allow an entity to identify them. You might be on a news website. You choose to engage with a social widget. You say yes. In our view, that should not mean, from that point all your access to the News Website should not be trackable by the provider of that social widget. When you browse further, you may not want to share your social identity. “Active”: On this page I want to do this thing. “Passive”: I did something a few days ago. It shouldn’t affect me anymore. If there was a trustworthy way to identify that a set of domains all belong to an organization. It’s “cross site” but it’s still the same party, we will ask them to request storage access, but we may persist across page-loads. Might be for 30 days or something. You might have seen that in testing as we have implemented a few quirks to fix site issues. We did it for a few domains that we know belong to the same organization. We think that’s an OK tradeoff in the privacy sense. 
* Johann: Thanks John for providing the context again. Disappointed you haven’t changed your mind. 
* Michael Kleber [Chrome]: I just wanted to point out that the persistence of storage problem John pointed out, is one of Chrome’s real problems with the Storage Access API more broadly. This really always keeps happening, if you have the cooperation of the surrounding page. Granting storage once, then storing this in the embedding page works forever. It turns into a long-lived joining. It’s extremely difficult to give people an understanding of what it means to say "yes" to the RSA prompt. This led to the Chrome view of having a bunch of dedicated privacy sandbox APIs that are fit for purpose and do not allow for this long-lived joining of identity to get this benefit once.
* Michael Lysak  [Carbon]: In terms of “active” versus “passive” consent, only half the equation is here. The other part, this wouldn’t stop the user’s identity from being carried by the browser, operating system integration,etc. We tell people “your online identity is secure” then there are these backdoors. I’d like to see proposals like this connected to a security proposal to prevent edge-cases as well.
* Johann: Storage Access API (especially in the Safari version) doesn’t simply put a prompt in front of users, it establishes a relationship through a 1st party user interaction. In Firefox we’ve seen very few calls. It’s really for the ability of a browser to control how much 3rd party tracking can go on. Firefox is a bit more lax as we haven’t seen that much. Chrome, if you’re uncomfortable with that - you can add additional protections. First party sets gives users no control over tracking across sites in that set. You can build the rules around the storage access API as you like. It’s not a “do you want to be tracked or not”. I do understand your point about the iframe’s ability to share that with the top-level site. 
* John Wilander [Apple]: I just wanted to point out in relation to what Michael was saying. There’s a difference between persistent access and persistent storage. This is about persistent access. Now we are saying “should it be per browsing session”. Persistent storage is its own issue. Our stance, and what we are shipping is that all partitioned storage is ephemeral. If you’re granted it as a 3rd party you get access to your permanent storage, and if you copy that into ephemeral storage it’ll eventually go away.
* Michael Lysak. On Johann’s “The point is to give the browser the ability to control how much 3rd party tracking can go on.” Browsers are ad-tech companies too. We want to make sure 1 ad-tech company doesn’t have that control. The user should make the decision or perhaps nobody should have that ability (to track).
* Tanvi: note on browsers having preferences by which users can change defaults
* Steve Englehart: That kind of long-term identity linking exists for 30 days. In the case that there is an access grant and it expires, we could explore options to prevent that.
* Michael Kleber: We are interested in talking about options. Fenced frames is the Chrome proposal for an embedded thing that cannot communicate with the outside world.
* Aram [Washington Post]: There’s the cause for identity. What other cases for long-term access exist outside of authentication. If it’s just auth, then maybe a purpose-built API is better?
* Johann: We agree Storage Access API shouldn’t replace all use-cases. There are other proposals for things like authentication. Some browsers like Safari / Firefox / Edge need this right now because they are shipping privacy protections. Everything else is just ideas / documents. For a longer-term proposal, there will be use-cases for which the APIs that are given are not enough. 
* Lee Graber: What are the inputs when an end user is asked - what will affect their choice of “Yes” or “No”. Not us, those out there who are less technical. When I go to European sites. It prompts me and I just click “Yes”. Do we have data that people are making informed decisions and why they are doing it. Prompting the user seems like a way of saying this is OK. 
* Johann: We are going really off-topic. Yes, we do have data that when prompts are neutrally designed (not the cookie prompts you’re referencing) people can make informed decisions. I can share a link to research from Firefox. I want to re-iterate that in Safari and Firefox, we are showing this prompt very rarely because both have mechanisms that prevent sites from spamming users with these.
* James Hartig: One problem with it being per-page, the iFrame doesn’t know on the page reload that it was already granted permission - doesn’t know it doesn’t need to show the prompt. 
   * https://github.com/privacycg/storage-access/issues/8 
* Jason Nutter: If we are potentially showing the user the storage access prompt more often, are we training them to say “Yes” more often even if it's not in their best interest.
### [#55 - Means to observe availability of storage access](https://github.com/privacycg/storage-access/issues/55)
* Johann [Mozilla]: Proposal: when you have an iFrame that is granted storage access, then the other iframes should have a way to observe that happening. We are in support on Mozilla side. We haven’t talked about it so far. Input?
* Mike O’Neill  [Baycloud]: Some sort of an event to do that? Is that what you’re thinking? 
* Johann: There are a few proposals in the issue. It’s an event you can listen to. It would go to all those who now have storage access.
* Mike O’Neill: It would only be iframes from the same origin that got access right?
* Johann: That’s correct. 
* Erik Anderson: What do you think of Jeremy Roman’s comment. Should there be a before prompt event? (missed this)
* Johann: One problem is that most browsers doesn’t really partition stuff. This is of limited utility across browsers. It’s nice in Firefox. For an initial version I’d try to keep the change minimal and not over-complicate the API.
* Johann will make a PR for this
### [#4 - Storage Types Covered](https://github.com/privacycg/storage-access/issues/4)
* Johann [Mozilla]: We’ve had lots of issues around which storage types are covered. What’s isolated, what’s blocked. Should we integrate that into the spec somehow. Can foresee issues in the future when we want to integrate that into the HTML standard. I’m leaning towards…. There are 2 sides. Either we completely integrate this with the storage partitioning spec, or we make it totally agnostic. I don’t really see a path between those.
* John Wilander  [Apple]: One of the reasons we haven’t made much progress here (originally brought up by Tess). IndexDB has a session concept. Active transactions in flight. One iframe requests storage access, another might be using partitioned indexDB. Do we just sever that connection? Let that one iframe remain partitioned? Connection to service-workers? I just don’t know if we have a solution and how complex a full solution would have to be. Can we let the developer specify? Cookie are special in that you can switch them.
* Ben Savage  [Facebook]: I think there is an interesting tie in between this and the previous discussion - when request cookies to be able to specify you want them partitioned. And Erik Anderson’s comment about switching modes.  Excellent example here.  Maybe we can apply the same logic here, when you request an indexedDB or Service Worker you explicitly ask for the storage you want.
* John Wilander: Also plays into the other issue about an event when the state has changed. 
* Ben Kelly  [Google]: While we are not currently pursuing request storage access, but we have thought about what we would do if we needed to promote to first party storage. There is a “buckets API” proposal. I want IndexDB but in a different bucket. Our idea is to not change the default storage, but expose here is your first party storage hanging off of a different API that doesn't conflict. Riffing on what the other Ben was saying: perhaps they could specify they want the bucket for a different partition. 
* John Wilander: One of the reasons we think a lot about this, is that we have been shipping partitioned Storage for so long, it’s an 8-year established thing that developers depend upon in webkit. We don’t want to break that for interop reasons unless it’s for a really good reason. We’re not against opening up access to IndexDB as part of the storage access API, we just don’t want to break existing use-cases. 
* Johann: I just want to second that I personally like the idea of giving more control. I think we should look into that. 
* Kris Chapman  [Salesforce]: When we talk about developers and what they are doing, it’s true that developers look at things on a per-browser basis. But when things are challenging in one browser, they’ll just say: “Don’t use this browser to access my website/app”. We don’t want to be in a situation like that. We all are here to discuss standards, but I want to stress how important this is. 
* Johann: Thanks everyone. Happy to chat more on the Privacy CG slack.
* End Topic




## [IsLoggedIn](https://github.com/privacycg/is-logged-in) (Melanie  [Microsoft] / John W  [Apple], scribe Michael Lysak)
Quick Recap on IsLoggedIn
* We will start out with a quick recap. It has been paused for some time.
* Very simple api. Website owner can send browser signal when user logs in/out.
* Navigator methods to do this, meta data
* Supports multiple concurrent logins
* a method ‘isLoggedIn’ does not have third party access
* Explainer also sets forth validation strategies, ways to integrate into various federated flows.
* Things like federated auth exist; this is not that. It could be combined with other proposals to facilitate federated auth. 
* This is a feature of the isLoggedIn proposal, how do we get it to work with any kind of identity or login flows.
* Reasons why a browser wants to know the login state exist
John:
* great summary nothing to add


### [#15 Should we standardize how we expect browsers to use this signal?](https://github.com/privacycg/is-logged-in/issues/15)
Melanie
* Cap on long term storage as a privacy consideration
* Storage clearing is too broad; affects all sites including those user wishes to stay logged in for
* Could help reason on privacy budgets, limit apis, or provide higher access to apis
* Helpful to have web dev corroborate login flow.
* This issue is a request from the group with more firm language how the signal is expected to be used. Is it worth supporting.using this api, what design choices are involved, practical use cases?
The IsLoggedIn spec will specify that:
 
* User agents may use the signal in support of end-user browser features.
* User agents may use the signal to enable long-term storage caps.
* Other web platform features may consider using the IsLoggedIn signal to fine-tune fidelity of access to powerful APIs.
 
These other web platform features would be responsible for specifying if and how IsLoggedIn will be used. We may wish to have a pointer to these other specs from IsLoggedIn.


James [51 degrees]
* If a website get more features from setting flag to true we need to understand this
* If the feature is very beneficial more web will be forced to use login
* Concern this exposes a 2-tier web (with login and without) 
* Requires more personal information used in login to be put on the web
* What change in behavior is it meant to drive, and is it desirable?
John
* This has been acknowledged as a risk; Mozilla has mentioned it.
Kaustubha:
* Same point as JAmes
* Is this really the signal we wish to pass, i.e. login
* Login as a signal could encourage login walls
* Other point: should we get more powerful apis behind the login, i.e. how that would interacts with privacy budget, it sounded like that would be relaxed for a logged in user
Melanie:
* Yes, different privacy budgets if logged in.
Kaustubha:
* Privacy budget is 1 threshold (asks for confirmation on tat)
* This gives access to fingerprinting [a]information, and even if you log out, the fingerprint now still exists. 
John
* If you are logged in, you have already established an identity, but once logged out, the fingerprint can no longer be seen until you have logged in again. 
* Is loggedin could allow for more powerful apis to be behind prompt.
Kaustubha:
* fingerprint can still be used because you could log in ‘as a different user’
John  [Apple]:
* You are correct this is a problem
Ben Savage  [Facebook]
* The fact that this issue exists, and that we are discussing the question of: “Should we characterize how we expect the browser to use this signal” is itself interesting. It shows that we are proposing a solution before defining the problem. If we want to change the overall web threat model to suggest that users should no longer expect sites to remember them, and that from now on we think the web should change so that sites no longer remember users by default, that’s a fine suggestion, we can separately discuss that. Perhaps we could file a PR to update the PING [threat model](https://w3cping.github.io/privacy-threat-model/) to express this. But let's first agree that this is a change we want to make to the web, and THEN discuss this potential solution to that problem. At the moment, discussing this potential solution without defining the problem is premature.
Melanie
* Thanks for feedback. We wanted to get a temperature check. 
Theresa  [Apple,Chair]
* This is an incubation venue. As chair I think this should continue to be discussed. We should not stop.
Michael Lysak:
* I wanted to respond to something Ben said. I did a [pull request for the PING threat model](https://github.com/w3cping/privacy-threat-model/pull/41). It touches on some of the matters you were talking about, but won’t get into it here now since it would be a bigger discussion.

This was brought up a few times-- I’m also worried about the potential side effects of forcing a logged in web. There are additional incentives beyond API capabilities such as SEO concerns. If a browser gives preferential treatment to web sites that implement the API. And to implement the API they have to have login. A possible consideration.

I want to make sure that publisher signals… the API shouldn’t steal publisher signals. Login used sometimes for publishers to pass signals, e.g. identity. Publishers may not want to expose that to the browser because of competition with the browser. You mentioned it’s designed to be integrated with other proposals. Depending on what integrations occur, want to consider what autonomy publishers might lose.

Melanie:
   * A good consideration how this interacts with other APIs. Want to keep this API simple, even with potential integrations. “Hey, could this flow automatically set this bit.” If there’s any concerns about competition or line of business, that’s best proposed in discussions on other APIs. Appreciate the feedback!
Michael Lysak:
   * I was recently requested to not file issues like the publisher data issue on specific APIs. I wouldn’t be able to do that there. Can you suggest an alternate place? Should concerns like that be posted on the specific issue? I think I was told “no.” Can chairs provide guidance?
   * Referenced PR https://github.com/w3cping/privacy-threat-model/pull/41 
Melanie:
   * I’ll defer to chairs on that. I was just referring to the Slack and asking folks to look at the PR you referenced if you want other folks to look at it.
Tess [Apple,Chair]:
   * I think that there are a number of concerns around any particular technical proposal that are in scope for discussion in different groups. There are probably topics related to proposals that are out of scope for different groups. In one case, I think you raised an issue to us in the Privacy CG and we pointed you to the PING threat model as a better document to update. That’s an example of where Privacy CG might not be the right venue but PING was the right venue. Not a quick and simple answer-- need to take on a case-by-case basis. For some issues, the right venue at W3C might be one of the oversight or governance organizations who take a more holistic view. For instance, the Advisory Board or Advisory Committee or the TAG. It depends on the specifics.

   * What I wanted to say was somewhat in reply to Ben. When John first described the idea for IsLoggedIn I was somewhat skeptical about it. That’s because I only saw one use for it and it seemed like a lot to add a new API. I’m impressed that John and Melanie have worked on for this question specifically because they’ve come up with 3-4 potentially compelling uses. That’s an indication to me that we’re on the right track here with a simple API that sets or unsets a bit and has potential uses. The ease of integration for sites-- a single call. Downstream, we see benefits to users. I’m gung-ho about this now and wanted to acknowledge the hard work that’s gone in here.
Nick Doty:
      * Having the bullet points for browsers and how they’ll use this would be a useful thing for any spec of explainer. I appreciate the three there now, more detail would be helpful for me and others. On the one about browsers limiting access to APIs in some way-- can we give some examples? I think it’s a good model to have APIs describe how they’ll interact with IsLoggedIn or not, but what’s a quintessential example?
John Wilander:
      * As the Storage Access API was first explained, and still is today, it’s about authenticated embeds. The browser implementation of the Storage Access API to avoid prompt fatigue would be if it knows that the user is logged in or not and, if not, it might default to partitioned storage. Or it could help the embedded piece of content understand how to interact with the user prior to requesting Storage Access. There are some concerns around fingerprinting surface there as Michael Kleber called out.

Lots of APIs in flight in browser engines that lead us to constant conversations about “this is super sensitive” and “we might put it behind a prompt.” I think having the IsLoggedIn signal may provide an opportunity to say if the user is logged in, not a drive by… if you created an account and logged in, maybe we don’t need to prompt as often or block the site from using powerful APIs that might be sensitive. Might unlock power to the web for future things, not necessarily just gating existing APIs.
O’neil [Baycloud]
         * is the idea you will say where the information will be held? cookie, local storage, etc? Could be used to partition, or not partition, or set ttls? If that is the case how would you identify it?
John
         * This has been covered, maybe not enough. See convo about [http state tokens](https://mikewest.github.io/http-state-tokens/draft-west-http-state-tokens.html) in place of cookies 
         * Another case is with which method user was authenticated, could tell the browser this information
         * Cookies are not legacy at present. 
Nate Schloss [Facebook]
         * To me it feels like there are a few different things being lumped in. Each deserves their own reviews.
         * Changing the fundamentals of storage affects web app, standardizing login can affect ability to innovate on login. 
         * Should we split these up into each use case?
         * It feels like we are suggesting a solution before we define a problem. These each deserve their own holistic conversations.
Melanie
         * There will be many discussions on what identity/privacy looks like on the web. 
         * It does feel like a very meaty conversation. Maybe thi spec should go into more details and then focus on each one at a time.
Nate
         * Even the core spec, even just integrating ‘are you logged in or not’ this is not simple or trivial. This is actually a very hard problem, and that deserves a full conversation.
John
         * We are not trying to specify the uses cases; we are trying to say what it can do[b]. 


Aram [Washington Post]:
         * Its clear that there is a spectrum of states, isLoggedin is only 1
         * Centering all web states on this one state is a mistake.
         * It varies from totally known to totally unknown


Theresea [Apple,Chair]
         * Chair prerogative to answer
         * It seems like keeping track of article limit is not at the level of login but above that of someone visiting a site just once.
         * Concern about time
John
         * we can cover it quickly next topic

### [#17 Mediated vs Unmediated Logins](https://github.com/privacycg/is-logged-in/issues/17)
         * Is the browser involved or not in setting the login?
         * Differentiate between browser mediated credentials and browser mediated logins
         * Asking for folks to chime in on issue
At time

### Federation - [#35 Support for federated logins, or the ability to transfer IsLoggedIn](https://github.com/privacycg/is-logged-in/issues/35)
         * Didn’t get to.
### Federation - [#30 What does logout mean in a federated context?](https://github.com/privacycg/is-logged-in/issues/30)
         * Didn’t get to.


## [Bounce Tracking](https://github.com/privacycg/proposals/issues/6) (Johann [Mozilla] / Pete Synder [Brave], Scribe:Michael Kleber )
Erik [Microsoft,Chair]:
         * Has been a great deal of discussion and concerns on this issue. 
         * Wants to keep this focused on what can we detect and how to prevent it, rather than problems caused by this prevention
Johann:
         * Since last year, we have a  protection in FF that purges old data from sites the user doesn't interact with, based on a blocklist.  not entirely happy with it, easy to work around, which we've seen
         * Interested in the same-site-strict jail that John mentioned, for sites that look suspicious — counting redirect chains, heuristics, etc.
Pete:
         * Brave has a 3-pronged approach in nightly right now
         * 1. give users a way to zap state
         * 2. maintain a list, ephemeral first-party storage where nothing is persisted after you leave the site, applied to sites labelled as bounce tracking
         * 3. additional list of "known bounce-trackers" where we can fast-forward the user across the bounce tracker entirely, never touch the server, based on examining the url
         * Additional indexing happening for Brave Search, so more data to detect these things happening and take action against them.  Specced out but not implemented yet.
JohnW [Apple]:
         * Basic bounce-tracking detection as part of ITP.  Classifies a domain if it reaches some threshold number of redirected-to sites that it fans out to. Causes sites to get their data deleted if the user doesn't interact with them, which ties back to isLoggedIn as a stronger indicator vs user interaction to know that the user really doesn't want us to delete a site's data.
         * Bounce tracking collusion detection: Back-propagate the bounce tracker-ness to an earlier domain that redirected to a known one, so that the fan-out detection can't be circumvented.
         * Delayed bounce tracking: work around circumvention by sites that wait a few seconds without interaction before redirecting
         * "same-site-strict jail" based on a blocklist: [authentication flows indistinguishable from bounce tracking](https://github.com/WICG/WebID#navigational-tracking), sadly, so just had to do a blocklist.  The site that we did this with decided to disengage (stop doing it?) so we removed them from the list.  So there is a blocklist, but it's currently empty.
         * If we get to a point where we can distinguish redirects / bounces within something like a first-party set, then we could exempt those since they are SSO


Pete:
         * Are you labeling an origin?  An origin+destination tuple?  What is the granularity of the restriction?
         * JohnW: It's the site itself that acts as a bounce tracker that we target
         * Erik [Microsoft,chair]:
         * Chromium is early in their development, looking for federated auth flows, wait and ask for user consent before proceeding with what might be a login flow
         * List-based: ublock-origin has a list of known bounce trackers, they will pop up an interstitial if you are redirected through one.  
         * Pete: this is the same list Brave uses
         * Looking at OpenID connect parameters
         * Johann: Based on site (not just origin) that the user is not interacting with, storage is purged for that site.
         * Brave is going with the ephemeral storage that does not do bounce tracking, we don't want to purge all of Google's cookies when you pass through it.  So the  bounce-tracking event has storage independent of any other storage the same site has.
         * Johann: Right, you can't just isolate an origin
James Rosewell:
         * I've been doing this now 14months, hard to get up to speed on everything, thank you for the summary.  I'm still struggling to find that information.
         * When Erik introduced, he said "without explicit user consent".  This group is limited to a specific set of solutions around privacy, and with user consent there could be good things done with these proposals.  There are people concerned with highly compressed timelines.  There are legal concerns, if you're following all the rules and laws.  What was just described is not looking at how to differentiate between use cases with people's consent vs ones that don't.  Would it be possible to steer the conversation towards a way that there could be user consent into whether to  use these solutions or not.
         * Separately, we need to look at supply chains.  Special status for SSOs favors people who are highly integrated.
Pete:
         * There are UI/UX concerns that you're bringing up.  What is the right place for them?  Is that your goal?
James Rosewell: 
         * how do we differentiate between use cases with consent and those without, so people who have consent can deliver solutions that continue to work while other solutions develop
Pete: Thanks, let's come back to it.
James: Would appreciate comments from the browser vendors




Mark Lee: I think this is more of a symptom of the problem — first party sets, login, the relation of the parties that are involved.  Blocklists and allowlists don't work: spammers are a longstanding example, lists always end up catching innocent folks and the spammers are best at working around it.  Enterprise software that does something weird gets caught instead.  I'm concerned about it breaking unintentional things.


Mike O'Neill: Agreeing a bit with James: Consent is obviously a legal issue, in Europe especially.  We've had this law since at least 2009[c].  It has not worked — regulators haven't been robust enough at doing anything about it.  The answer has to involve some browsers taking on a role to protect privacy in parallel with legal.  Consent is built into the storage access API, so it has consent but the browser is mediating it.  Just solving it in a technical way will turn complicated and will result in an arms race.  let's instead of something straightforward and simple, ephemeral storage in the first party, limited session state, then storage is removed, unless user agrees that it isn't, and you get to ask the user to change it.


Joshua Koran:
         * Contract law is important.  but I thought I heard your ephemeral state management, and thought Safari was heading there also, that unless the user authenticates / provides some additional information, you can't maintain state even on a site that you visit infrequently.  Are we going to force disclosure of even more personal data to maintain that functionality?
         * Pete: that's not Brave's proposal, at least, no one else's AFAIK


Aram Zucker-Scharff:
         * I do like the idea of requiring user interaction as part of the concern. OAuth has the same behavior, we need to provide some ramp away, otherwise people may use the signals of an oauth process without being a legit oauth process to keep up bounce tracking.  I really think we need to work towards some user interaction must be required.


Kaustubha Govind:
         * same-site-strict jail: when cookies are designated s-s-s then the cookies are not sent as part of redirects.  That's still vulnerable to a double-redirect, from a site to itself.
         * Wilander: We knew that at the outset, the one person who was doing it decided to stop doing it instead of re-architecting to self-redirect.  We've joked about same-site-strict-strict if needed, though.


Pete: Any other thoughts to share?
James Rosewell: Would like a section on differentiating legal vs non-legal use cases.
Pete: What venue?
James: Process CG?  Another venue?  Depends on browser vendors
Pete: I'll leave that up to you and chairs to sort out.  Privacy CG seems wrong to me, it's a technical forum.
James: But it's debating a non-technical subject, so if this isn't the right forum for those discussions then I don't think this work should be here either
Erik: Unpredictability, lack of interoperability, are appropriate for this forum, that's what we're discussing here.  Legitimate use cases may leverage primitives that can be used in other ways too.  Make technical proposals for browser behavior changes, highlight impacts, expect folks to focus energy on where there is interest in implementing.  Refer to W3C leadership on what's appropriate in different forums.
James: Need to understand if browser vendors are willing to come to an appropriate forum.  This group is browser vendors and other constituencies also, web authors etc.  Quite problematic if other people aren't part of the discussion.
Erik: This is about web standards, offer up another group for other things.


Wilander: Issue is a proposal for a Work Item.  Either close this issue as something PrivacyCG isn't going to work on, or make it a Work Item (which I support).
Erik: I see signs of interest
Wilander: If it is its own work item, can be separate issues for all the different questions, helps pull things apart.

Attendees (sign yourself in, alphabetical order please):        


         1. Al McLean (Carbon AI)  - CDO
         2. Alex Cone
         3. Allan Spiegel (Adobe) - Sr. Quality Engineer in Adobe Experience Platform Trust and Governance; TechLab Project Rearc Accountability + Addressability WG
         4. Andrew Knox (Facebook) - Research Scientist working on privacy, cryptography, and monetization
         5. Angela Ballard (CJ Affiliate) - product manager for tracking/attribution at CJ 
         6. Anne van Kesteren (he/him; Mozilla) - work on web standards (& strategy) at Mozilla
         7. Aram Zucker-Scharff (he/him; The Washington Post) - Sr. Engineer working on ad tech for publishers, from a publisher perspective, with our Zeus product. 
         8. Arthur Edelstein (Mozilla) - product manager for privacy & security
         9. Ashkan Soltani (he/him: GPC) - Independent Research/Consultant
         10. Ayu Ishii (Google Chrome) - Software engineer working on Storage
         11. Ben Kelly (Google)
         12. Ben Savage (Facebook, Software engineer working on Privacy Enhancing Technology and ads. Leading many industry engagement efforts)
         13. Benjamin Case (Facebook) - Research Scientist, Cryptography and Privacy 
         14. Brad Rodriguez (Magnite) - Director of Engineering - Data, Privacy
         15. Brian Campbell (Ping Identity)
         16. Brian May (dstillery) - Involved in many industry groups addressing ecosystem changes.
         17. Cassie P
         18. Cezar
         19. Chris Needham (BBC) - web standards representative (mostly privacy and media topics). W3C Media & Entertainment IG and Media WG co-chair
         20. Christy Harris (Future of Privacy Forum) Director of Technology & Privacy Research focused on online advertising and platforms
         21. Chris Wood (Cloudflare)
         22. Colin Sidoti (Clerk)
         23. David Dabbs (Epsilon), DIrector - Identity Solutions
         24. Don Marti (CafeMedia)
         25. Eric Mwobobia (ARTICLE19) - focused on privacy and security 
         26. Ericka Wright (Intuit, but not representing Intuit - here as an interested party who manages advertising tracking)
         27. Erik Anderson (Microsoft) - Privacy CG co-chair
         28. Erik Taubeneck (Facebook) - Research Scientist focused on Privacy Technology
         29. Frederic Jacobs (Apple) - Security Engineering & Architecture 
         30. Grant Nelson (Triplelift) - PM for privacy/identity, recovering privacy lawyer
         31. George Fletcher
         32. Harneet Sidhana
         33. Heather Flanagan (Spherical Cow Consulting) - active in the identity and federation space, mostly with Higher Ed but also with IDPro (professional organization for IAM practitioners). Also working with Google to facilitate WebID discussions.
         34. Hitesh Lad (CJ Affiliate) - Engineering lead for affiliate network tracking
         35. Jack Frankland (no affiliation) - Web developer with an interest in privacy
         36. Jade Kessler
         37. James Hartig (Admiral) - Co-Founder of Admiral, helping online publishers monetize their audience via subscriptions and alternative means
         38. James Rosewell (51Degrees) - Co-Founder of 51Degrees, improving privacy considering the skills of law, economics and engineering - attended 2nd 2 hours only
         39. Jason Nutter (Microsoft) - Software Engineer in Azure Identity (JavaScript Oauth SDKs)
         40. Jeegar Shah (CJ Affiliate) - Director of engineering for affiliate network tracking and attribution.
         41. Joel Odom (Salesforce) - Security & Privacy PM
         42. Johann Hofmann (Mozilla) - Security & Privacy Engineer on Firefox, working on ETP, State Partitioning, Redirect Tracking prevention etc. Co-Editor of the Storage Access API
         43. John Wilander (he/him, Apple WebKit) – software engineer on the WebKit team, lead on Safari’s Intelligent Tracking Prevention, co-editor/editor of the Storage Access API, Private Click Measurement, and IsLoggedIn here in Privacy CG. I hope to get a better overview of the web privacy landscape by getting a feel for where our various work items are at.
         44. Jon Callas (EFF) - Director of Technology Projects
         45. Josh Karlin (Google) - TLM on Privacy Sandbox
         46. Joshua Koran, Zeta Global
         47. Kate Cheney (Apple) WebKit privacy engineer
         48. Kaustubha Govind (Google Chrome) - Tech Lead & Manager under Chrome Privacy Sandbox; co-editor of First-Party Sets CG work item
         49. Kelda Anderson (Microsoft) - Edge Program Manager - focused on Privacy and Security
         50. Kristen Chapman (Salesforce) - Sr Director, Privacy Technology - work in engineering at Salesforce Marketing Cloud, but focus on privacy issues for Salesforce as a whole too.
         51. Kushal Dave (Scroll / Twitter) - Dir. eng / ex co-founder & CTO
         52. Laszlo Gombos (Samsung) - Samsung Internet browser
         53. Lee Graber (Tableau / Salesforce) -- Embedding, federated identity
         54. Lionel Basdevant (Criteo)
         55. Liz Hurley (CJ Affiliate) - Software engineer working in affiliate marketing
         56. Lola Odelola (Samsung Internet) - Web Developer Advocate/Principle Eng w/ focus on web privacy
         57. Mallory Knodel (CDT) - CTO, IETF/IRTF contributor
         58. Marijn Kruisselbrink (Google) - Software engineer working on Storage
         59. Mark Lee (Carbon AI) - CTO for Carbon work on engineering of systems/standards for publisher/advertiser audiences
         60. Mark Xue (Apple) - privacy engineer for Safari and WebKit
         61. Marshall Vale (Google Chrome) - PM Privacy Sandbox
         62. Max Gendler (NYTimes) - Manager, Data Governance
         63. Melanie Richards (Microsoft) - PM on Microsoft Edge web platform, working on trust & privacy among other things. Co-editor of IsLoggedIn, looking forward to the group making progress across a few work items this week.
         64. Michael Kleber (Google Chrome) — Tech lead for the Chrome Privacy Sandbox
         65. Michael Lysak (Carbon AI) - Engineer involved in many industry groups, esp privacy. Working to promote Open Web, Updated Privacy, Industry Fairness.
         66. Mike O’Neill (Baycloud) - ancient software developer
         67. Mike Taylor (Google Chrome) - Eng Manager on Chrome Privacy Sandbox Team
         68. Naïma Conton (Sirdata) - Chief Operating Officer and DPO
         69. Nathan Schloss (Facebook) - Software Engineer on the Browser Engineering team
         70. Newton (magnite)
         71. Nick Doty - PING privacy reviews, fingerprinting mitigation, recovering academic
         72. Peter Lowe
         73. Peter Saint-Andre (Mozilla) - standards & partnerships on the Firefox team
         74. Peter Snyder (Brave Softwre)
         75. Pranay
         76. Przemyslwa Iwanczak
         77. Robin Berjon (The New York Times) — VP Data Governance, trying to make privacy and the Web work better
         78. Rowan Merewood (Google) - DevRel Lead for security / privacy / payments / identity
         79. Russell Stringham (Adobe) - software architect over privacy and consent on Adobe Digital Experience products
         80. Sam Macbeth
         81. Sam Weiler (W3C/MIT)
         82. Sam Goto
         83. Sean Harrison (Google) - Software engineer on Chrome Privacy
         84. Shaun Gilmore
         85. Shigeki Ohtsu (Yahoo JAPAN) - software engineer working on Privacy Sandbox
         86. Shivan Sahib
         87. Steven Englehardt (Mozilla) -- Privacy Engineer 
         88. Steven Valdez (Google) - Software engineer working on Privacy Sandbox (Trust Token and PrivacyPass)
         89. Tanvi Vyas (Mozilla, co-chair) - Principal Engineer Firefox, focused on privacy
         90. Tara Whalen (Cloudflare) - former PING co-chair, general privacy advocate
         91. Tim Cappalli (Microsoft Identity) - Digital Identity Standards Architect: focusing on identity on the web, SSO, etc
         92. Theodore Olsauskas-Warren (Google) - Software engineer for Chrome Privacy
         93. Theresa O’Connor (she/her, Apple, co-chair)
         94. Valentino Volonghi (NextRoll)
         95. Vittorio Bertocci (Auth0) - P. Architect, helping cataloging federated identity scenarios impacted by browser changes
         96. Wendell Baker (Verizon Media)
         97. Wendy Seltzer (she/her, W3C); Strategy Lead and Counsel, happy to see lots of activity here, and eager to see what’s moving toward more formal standardization.
 




