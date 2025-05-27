# 22 May 2025 Privacy CG teleconference
* Chair: Erik  
* Scribe: Kaustubha (first half), Nick Doty (second half)  

# Agenda
* Google's [Privacy Sandbox update](https://privacysandbox.com/news/privacy-sandbox-next-steps/)  
* [Storage Partitioning](https://github.com/privacycg/storage-partitioning)  
  * [Chrome Experimenting with cache sharing for extremely-pervasive scripts \#49](https://github.com/privacycg/storage-partitioning/issues/49)  
* [Storage Access API for non-cookie Storage](https://github.com/privacycg/saa-non-cookie-storage)  
  * [Declaring constructors as methods is strange \#31](https://github.com/privacycg/saa-non-cookie-storage/issues/31)  
* [Storage Access API](https://github.com/privacycg/storage-access)  
  * [How does storage access interact with Dedicated, Shared and Service Workers? \#157](https://github.com/privacycg/storage-access/issues/157)  
* Any other business 

# Notes

**Google's [Privacy Sandbox update](https://privacysandbox.com/news/privacy-sandbox-next-steps/)**
* Johann: Echoing what was said in the blogpost; Chrome is no longer rolling out a prominent user prompt, or blocking/deprecating 3pcs by default.  
  * We are still supporting standards process for the APIs that are in the process; e.g. CHIPS, Storage Access API, we’re still committed to the web standards process.  
* JohnW: Big worry for us, maybe others too \- we will have a real bifurcation of how the web works, if the largest browser by traffic allows 3p cookies by default, and others don’t. Goes against the ?? of the web. I hope the Chrome team can help us mitigate/avoid the situation.  
* Johann: That is something that is very top of mind for me and the Chromium team overall. We have created a web with user agents that have different threat models. With that fact having been established; we (as the larger web platform team) should think about how to maintain interop. There are other Chromium browsers (even Chrome incognito mode) that do have cookies blocked; so we do need to think about interop.  
* Nick: Any learnings from the Chrome folks having gone through this. Harm to users, businesses, and the rest of us working on this.   
* Johann: That’s a tough question to answer off-hand. Can’t comment on this as a rep of Google. We should reflect on the lessons learned, but not able to articulate right now. Michael?  
* Michael Kleber: Interpreting the question as not specifically about third-party cookies, but broadly about the changes we want to make, and engage the large swath of ecosystem about the changes. Historically, we have viewed this as a successful engagement \- getting feedback from the large population of people who care about the web, and modifying our plans based on that feedback is generally a successful story. We have, and should, listen to a lot of people; but what that means is that some section of people will be disappointed regardless of the direction we end up in. Learnings would be more case-by-case, and not generalizable. But a feedback-based approach should be an expectation for all of us.  
* Nick: Not saying that this was a failure; but what I’m hearing you say is that the lesson learned is that this was a successful approach, to invest in APIs based on this approach and then to back it out in a way that helps some stakeholders and hurts others?  
* Kleber: not saying I'm happy with the outcome personally.  
* Brian May: We invested a lot in this future. Lots of promises were made, and lots of people took action based on those promises. The promises were then slowly reeled back in. We should talk about whether the W3C should have a say about whether when a browser says they would make a change, should the W3C have a say in that? Chrome’s participation in the W3C processes should indicate taking the W3C members’ perspective into account.  
* Johann: Hard to look at this and say we need more centralized planning  
* Jeffrey: Thinking about Brian’s comment about whether W3C should approve things for large browsers. Imagine \- if a decade ago, when Webkit said they would block 3p cookies \- if W3C had blocked that, I doubt this group would have been happy. With my TAG hat on, rather than my Google hat, there is an ongoing problem here of one large browser being able to have such a large effect on the ecosystem, that this incident is just a victim of. No roadmap for how to change that.  
* Brian: My concern is that a lot of people got dragged through this last go-around; it’s going to be hard to replace browsers, but if they get pushed around enough, they might find alternatives.  
* BenV: The degree of horizontal/vertical integration, and regulatory engagement is a bigger wrench. From the Firefox perspective, where we find ourselves on is converging across 3 models for cookies \- partitioning by-default vs. blocking \+ CHIPS vs. 3p cookies allow. I would like to find some alignment between where Firefox and Safari are at; so that we at least have two cookie behaviors, rather than three. Partitioning by-default has better compatibility.  
* Erik: This group still has a role in helping these sorts of issues.  
* Johann: I don’t share Ben’s pessimism. I think the cookie behavior is closer than we think it is. We’ve made progress on storage partitioning, getting cookie layering landed in all main-line specs and standards. Still proud that the Privacy Sandbox effort gave me and others the opportunity to work on this.  
* JohnW: Reflections \- I think the standards process worked pretty well. Lots of APIs proposed; but the ones that gained broad consensus have been graduating in standards space. Even privacy-preserving  ads in PATCG is making similar progress. Standards process has been pretty good.  
* Nick: I know Johann warned about not being able to answer re: API plans; but would like to understand some things to help with planning. One example is fingerprinting. E.g. Google Ads is now going to allow it for their ad clients. That makes us wonder if Chrome is likely to make some changes about fingerprinting, or approach it differently? How can we coordinate on this type of work?  
* Johann: Not 100% in tune with efforts; but there are folks thinking about fingerprinting within the org. Unrelated to this announcement. Details of exactly what we’ll do and to what degree we can talk about, will defer to Mike.  
* Mike Taylor: We do still have people thinking about platform hardening and anti-fingerprinting. Certainly on our roadmap; and an area of interest to us.  
* Erik: Are these likely to be mitigations primarily tied to incognito?  
* Mike: We have announced some features, like IP protection, that will be launched in incognito. But we have other work that may ship by default in all modes. Will decide on a feature-by-feature basis.

Apple Webkit’s stance on APIs for fixing breakage / interoperability

* Lee: Would like to ask Apple to talk about their plans. CHIPS support on Apple devices helped resolve a bunch of breakage issues for us. We have to have solutions for all our customers; and Chrome isn’t quite enough for us.  
* JohnW: Let’s discuss as a separate topic  
* Erik: Sure. I’m sure interop will continue to be a conversation.

**[Chrome Experimenting with cache sharing for extremely-pervasive scripts \#49](https://github.com/privacycg/storage-partitioning/issues/49)**
* (scribe switch)  
* Patrick: networking team on Chrome, HTTP cache. very early, when partitioning the cache, was there any performance cost? and if so, was it scoped to a small enough number of resources where we could get some of the benefit without “any” risk?  
  * for example, Google Analytics JS is the same code shipped to every user, present on 50% of websites. knowledge of it being in the cache doesn’t provide any information about the user. target the most likely to impact metrics that we are likely to see. is there any benefit to optimizing cache sharing for these resources? and if we don’t see any benefit, then document that and move on.  
  * if there is a benefit, only something we see in large scales, like .1% impressions  
  * most of these are third-party resources that are loaded async. second-order impacts to business metrics to see if there’s any difference or not.  
  * assumption of a small number of resources that any one user that likely had the same version of the same resource, and a resource that doesn’t add fingerprinting surface or unique user data (static public resource that doesn’t vary based on cookies). and extra hardening of the cache to make probing the resource destructive.  
  * giving a performance benefit to lower privacy or a small number of big players would be unfair. but right now just evaluating whether there is a performance benefit. then consider whether there is a monarch-making problem, or whether there is a privacy-safe way to have a small number of resources like this for users to benefit.  
  * maybe YouTube embed video JS that’s a large file might be more significant.  
  * internal Google privacy teams have already provided input, even for addressing the experiment. tried to make it impossible to use for history sniffing.  
* Lee: a lot of work involved. what was the impetus to do this, given all the worries?  
  * Patrick: we want to see if there’s a benefit. several teams have told us that there were losses recorded because of lack of cache sharing. if the experiment shows a performance benefit and we had a line for known safe resources… Assumed some form of Privacy Sandbox world where you couldn’t just share the info across partitions anyway.  
* John W: very interesting, and Webkit has been thinking about this since introducing cache partitioning. proposal for javascript modules with an inventory in the browser, maybe dynamic based on what this user users a lot, or something static. didn’t move to standardization. has to be a static inventory so that it doesn’t become a fingerprinting or stateful vector. should this make stable good versions be used by websites, including standardizing version numbers? could even have security fixes where library developers could update the cached version to distribute a fix to all browsers. barrier to entry to new libraries (like the monarch concern). effectively create an extension to the web platform. should we instead extend the web platform?  
* Patrick: explicit cache sharing isn’t trying to solve the everyone-uses-jquery problem because of bundling that happens. only for third-party resources that are static and universal at any point in time. for js libraries problem, compression dictionaries – you can use a file that you’ve fetched as a compression dictionary for other resources. we could build a publicly versioned dictionary that ships with the browser that has a lot of common javascript function patterns in it, say 10MB compression dictionary with current versions of jquery/react, etc. origins can compress against that dictionary, any version of jquery will compress well against the dictionary, but not tied to a specific version. whatever is widely used at this point in time would be compressed better.  
* Nick: It seems like from the previous agenda item that we were learning that centralization makes it potentially hard for us to make good privacy decisions for the platform. I’m concerned that performance benefits that would go exclusively to the largest players would reinforce that problem. I understand you’re first trying to gather data to inform how to address the monarch problem, but it seems like we already have good evidence about how it might be harmful. Large perf benefit to biggest players, discourage smaller players such that a more privacy-friendly analytics, video display library, etc. would be penalized vs. the largest player.  
* Patrick: Would have to see how big the performance gains are. Tend to be smaller in measurements because they are not critical path. Only see benefits at large scale, but those that are widely used are also the only ones likely to be seen by every user. Even a new player getting let in might not benefit from it. Might be able to find ways to mitigate the issue. Ideally if you have a better feature/integration it will stomp any small perf gain you see from this. Perf tends to be the last thing after features and benefit.  
* Nick: You’d put less pressure on large players to improve their perf or decrease their size, if assumed that it’s already in the cache or compression.  
* Patrick: Not sure. Maybe.  
* Erik: evaluating asynchronous de-duping that could help the disk space issue.  
* Patrick: Could key the cache on the integrity hash of the resource. more privacy concerning if we do it in the general case vs well-vetted static common scripts. vetting and verification done by the script owners on what would go into shared cache, rather than doing it automatically.  
* John W: things will change after it’s shipped, not just based on what’s available today. super popular websites could seed the cache in order to sway the developer ecosystem. partitioned cache has been a kind of equalizer, where every website has to decide what they’re going to load, and can’t just bet on other popular websites making their expensive resources cached.  
* Patrick: maybe. not sure how much that benefit really is. wasn’t a huge performance regression across the web when the partition was introduced. some parties maybe saw some decrease because websites were abandoned before it loaded.  
* John W: we measured in 2012 and saw no meaningful harm, but that was a long time ago.  
* Patrick: largely the case now, not on the critical path for loading for most users. but still might be important to the business of the web, which is why we are measuring.  
* Ben V: thanks for coming and presenting. would be good to hear that LCP/FCP have no impact, if only to solidify partitioned cache as a no-significant-harm conclusion. difficult when different browsers have different threat models, and potential impression that different browsers will have different performance because of having a weaker privacy model.  
* Patrick: commit to documenting the general results. can document LCP and what we measure as a browser.  
* Johann: push back against the idea that there are no tradeoffs ever. performance is not necessarily interop. Chrome Incognito and Chrome regular mode will have different performance characteristics, but want to not have breakage.  
* Ben: not that there would be no change at all, just the potential impact of major differences.  
* Erik: in the context of different threat models, and changes to privacy sandbox. is there a belief that cache partitioning is still the right behavior for Chrome?  
* Johann: no plans to undo it. benefits to XSS protections, and if anyone changed their mind, would have to address those security impacts.

\<out of time\>

**Any other business**

* For a future call: Lee Graber suggests talking to IDPs about cookies and iframes.


# Attendees
* Erik Anderson (Microsoft Edge)  
* Chris Needham (BBC)  
* Brian May (unaffiliated)  
* Nick Doty (CDT)  
* John Wunderlich (JLINC Labs)  
* Kaustubha Govind (Google Chrome)  
* Ehsan Toreini (Samsung)  
* Lee Graber (Salesforce/Tableau)  
* Chris Fredrickson (Google Chrome)  
* Aaron Selya (Google Chrome)  
* Johann Hofmann (Google Chrome)  
* Benjamin VanderSloot (Mozilla)  
* Erica Kovac (Google Chrome)  
* Patrick Meenan (Google Chrome)  
* Gaurav C (Yahoo)  
* Michael Kleber (Google Chrome)  
* Anusha Muley (Google Chrome)  
* Sid Sahoo (Google Privacy Sandbox)  
* Frank Somich (Google Chrome)  
* Jeffrey Yasskin (Google Chrome)  
* Ari Chivukula (Google Chrome)  
* John Wilander (Apple WebKit)  
* Yi Gu (Google Chrome)  
* Emma Zuehlcke (Mozilla)  
