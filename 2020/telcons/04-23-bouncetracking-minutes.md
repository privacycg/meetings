# [Ad-hoc meeting on Bounce Tracking](https://github.com/privacycg/meetings/issues/5#issuecomment-612254081) - 23 April 2020

* Chair: Tess
* Scribe: Erik

## Zoom

https://zoom.us/wc/join/769153986?pwd=

## Attendees

* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* John Wilander (Apple WebKit)
* Sam Macbeth (Cliqz)
* Kushal Dave (Scroll)
* Jeffrey Yasskin (Google)
* Thomas Steiner (Google)
* Kris Chapman (Salesforce)
* Pete Snyder (Brave)
* James Hartig (Admiral)
* Steven Englehardt (Mozilla)
* Jack Frankland
* Johann Hofmann (Mozilla)
* Anne van Kesteren (Mozilla)
* Pranjal Jumde (Brave)
* Carlos Bracho (Axel Springer)
* Nicolas Arciniega (Microsoft)
* Joel Odom
* Allan Spiegel (Adobe)
* Bill Densmore (ITEGA)
* Peter Saint-Andre (Mozilla)
* Andrew Knox (Facebook)
* Sam Weiler (W3C/MIT)
* Brad Lassey (Google)
* Krzysztof Modras (Cliqz)

## [Bounce Tracking Protection](https://github.com/privacycg/proposals/issues/6)

### What is Bounce Tracking?

Top frame navigation with redirect(s) in at least two steps.

* Source → tracker → source
* Source → tracker → destination
* Source → tracker1 → … → trackerN → source/destination

Redirects can be HTTP 3xx or client-side.

## Relation to Link Decoration

Pointed out by Mike West (Google) on Twitter: Is a special case of link decoration since data needs to be transferred to/from the source/destination.

### Why Are Trackers Doing Bounces?

Needed in Safari to become “visited” and seed the domain's cookie jar.

Nowadays to pick up cookie data in browsers that block third party cookies.

### Solutions

Shipping solution: Delete website data for classified or block listed domains without user interaction.

Possible escalation: Delete website data for all domains without recent user interaction. Possibly with an exception list.

#### SameSite=strict Jail

Detect and count unique bounces. Over a threshold ⇒ rewrite cookies to be SameSite=strict.

Has problem with same-site redirects.
Source → tracker1 → tracker1 → source/destination

Available in the next Safari Technology Preview (assumed 105): Develop menu → Experimental Features → SameSite strict enforcement (ITP)

Possible escalation: SameSite=strictStrict (half joking)

Issue: Federated login flows

#### Delayed Bounce

Detect and count unique bounces. Over a threshold ⇒ delay them to "reveal" the intermediary domain with an interstitial.

#### Confirm Bounce

Detect and count unique bounces. Over a threshold ⇒ require a confirm of some sort to "reveal" the intermediary domain. Could be done with an interstitial or a prompt.

#### Disentangle Federated Logins and Bounce Tracking
Formalize federated login flows
One bare bones proposal: [IsLoggedIn](https://github.com/WebKit/explainers/tree/master/IsLoggedIn)


### Minutes
John: what Bounce Tracking is: top-frame navigations that redirect you through one or more domain in order to pick up cookies as first-party to identify the user or store information.

…These types of redirects are also involved in things that wouldn't be deemed cross-site tracking, e.g. login flows or normal redirects. Not all redirects are bounce tracking.

…Twitter discussion involving Mike West from Google called out link decoration. Bounce tracking is a special case of link decoration. Info needs to be transferred to or from the destination via link decoration.

…Why is this done? Going back 17 years, it's been needed in Safari because Safari has a default restriction that sites can't use 3rd party cookies until they've been visited as a first party. These bounces may have been added for that reason. Now that other browsers are doing similar things, this is being used to get around 3rd-party cookie blocking.

…Regarding existing solutions: Safari as part of ITP deletes for classified or blocked domains without recent user interaction (currently 30 days of Safari use). Mozilla may be considering a similar thing (defer to them).

…Potential solution called "same site strict jail" which detects flow that _could_ be doing bounce tracking and counts the number of unique flows per-domain, if a threshold is hit, that entire domain (tracker/intermediate domain) will get cookies changes to be treated as SameSite=Strict.

…This has the drawback that the tracker can add an extra redirect to itself (tracker 1→tracker 1→destination); that middle one will send SameSite=Strict cookies. SameSite was more about security than privacy. SameSite=StrictStrict needed?

…Next ITP tech preview has this implemented (SameSite=Strict enforcement).

…Has a potential problem with federated auth flows. If you use a specific federated auth provider heavily, you'll likely hit the threshold which is a potential issue.

…Two more things: (1) delayed bounce, we do the same kind of counting, but when threshold is met, instead of rewriting to SameSite=Strict, delay a bit to give the user a chance to understand what's going on (e.g. an interstitial page), (2) a confirmed bounce, instead of just delaying, after the threshold is hit for a domain, block the redirect at the intermediary and get the user to confirm they want to continue; this could help make federated auth flows work with a delay and/or confirm step. Users using federated auth provider would presumably not be surprised to see the federated auth provider in the chain.

…Need to separate federated auth flows to give the browser awareness of what's going on. Barebones proposal for this in the Apple repo (see link).

### Discussion
Johann Hofmann: can speak to Mozilla's plans for purging. I work at Mozilla and reviewed or wrote most of the code for our purging implementation. 

…Intent to ship link: https://groups.google.com/forum/#!topic/mozilla.dev.platform/0JjAHt59j7s

…We are currently running it in nightly, finding bugs. Generally planning to ship this. Similar to the Safari one based on what I know about Safari's. We based it on our block list (Disconnect list); we will delete trackers that are on the Disconnect list and have cookies or other storage (e.g. localStorage). Very similar but based on the blocklist. Not sure if Safari purges quota storage, but we do.

…We're relatively happy with this approach. Blocklist has limitations. Overall we have two problems (1) detecting bounce tracking and then (2) what to do with it. Not sure if we should standardize both; would be happy if we could. Between Mozilla and Apple we have one thing sort of standardized (We delete the cookies), the thing we haven't standardized yet is list vs. heuristic with ITP.

Steve Englehardt: from Mozilla. John-- you're clearing things like cookies that you think are doing bounce tracking. Why do you feel the SameSite Strict approach? It seems like a party could also circumvent this by stopping an HTTP request, doing a subresource request that would be SameSite, and then move on.

John: the reason we're doing more than we did before is we've seen domains that get regular user interaction as first-party now engaging in bounce tracking. ITP won't delete their data since it's getting user interaction. That's why we have to do something.

…Re: the stop and set thing, absolutely. Client-side redirects need to be handled too. With two http 3xx redirects that's the quickest/easiest way right now.

James Hartig: is there some sort of way we can use the Storage Access API on the intermediate page to gate cookie access. UA could show a dialog/confirmation rather than some new confirmation that was mentioned earlier. Any thoughts on that? That would allow federated logins to work, presumably they already have 3p storage access or the user would be okay giving it to them since they'd be familiar with the IDP.

John: quoting "confirm bounce" info above-- "item threshold implies a prompt/interstitial." Yes, choice could be remembered to not prompt all the time. Don't know if we could go further than that-- the intermediary would have to stop and call the Storage Access API. Was thinking the browser detecting it and showing a prompt on behalf of the page rather than the page asking for it. The reason I bring up federated login flows (and single sign on), there are a bunch of legacy ways these things work. For instance, you might sign up for the first time with the federated login provider, you log in there/confirm you want to login with the other site, that creates a set of redirects that look like bounce tracking from the IDP to the original site. There are multiple ways that existing login flows look exactly the same as bounce tracking. Might need a confirmation step or technically disentangle.

James: I brought up Storage Access API because it seems like the user will already be doing some activity on the page, e.g. "do you want to connect your account to this other site?"

John: small set of sites involved that would fall into the "are they bounce tracking or federated auth flow?" bucket.

Pete Snyder: sharing Brave info. I posted a link about what Brave is doing in the issue in the github issue (https://brave.com/redirection-based-tracking/). Monkeying with query parameters, cookies, etc. to see if it changes the redirection path. If so, then we build lists to deny storage for sending/receiving on those domains. We saw very little of this which may be because we have a Chrome UA. Medium-term plans on Brave side is to do an interstitial which is very similar to uBlock origin which has similar UI. Last thing we're doing in addition to list building, we have a proposal which we haven't started prototyping, having the client collect statistics to influence a global list (with offline auditing), would be list based off existing list + Brave addditions.

Sam Macbeth: from Cliqz and Ghostery. Stats on sites/trackers using this technique that can be shared? Would like to build up techniques for detection.

John: we don't have data to share, but Pete from Brave mentioned they have a crawler, maybe they want to share. But it could differ between UAs. Might try with a Safari UA string to see what behaviors are seen.

Brad Lassey: I want to point out that Sam Goto from Chrome put out a Web ID proposal to help with federated login. Another approach to help with the classification problem (https://github.com/samuelgoto/WebID). From the Chrome perspective, we've been trying to avoid list-based approaches or heuristics. Things that can be consistent across domains and be well-reasoned. Has Apple thought about adding a requirement for user interaction for cross-domain navigations? I haven't thought deeply about that.

John: re: user interaction for cross-domain navigations-- we sort of already have it. If an origin hasn't had interaction in a 1p context but hasn't been seen as something that looks like a tracker it gets access, but could move to a model where it always requires it.

Kris Chapman: put out a plea for reporting or tracking on the complete URL that you see a problem with, especially where you use lists. Salesforce has run into this because we're a vendor and have a lot of subdomains for clients; we don't have a lot of control over some of those. People identify something and we can't get out of the blacklist (e.g. Disconnect). We can't get a report of where the activity was seen which we want to track down where the problem is to get it fixed. As we build these things, if we can find out after the fact _why_ it was called a tracker.

Johann: at Mozilla, we're adding telemetry (which is public by default) that doesn't have domains, but calls out how much we're purging per day, per user (things like that)-- trying to get something useful out of it. Re: user interaction idea-- we've noticed during QA cycle that it's easy to get user interaction on these sites accidentally. If user interaction is a requirement, sites will forcibly get user interaction on them. Skeptical about interstitial dialogs-- too little and they're not effective, too much and it's a big annoyance. Want to be cautious; prefer a solution without a lot of user choice-- super hard to convey what is being asked.

Kushal Dave: at Scroll, we're constantly running up against the 30d cookie purge as a federated auth provider. Nervous about things that show interstitials, playing whack-a-mole. isLoggedIn would help. Our whole goal is to be behind the scenes; if we can get cookies without unnecessary user interactions that would be helpful.

John: re: worry about interstitials-- totally share that, lots of folks from WebKit worried about it. Don't want prompt/interstitial fatigue. At least in our case it's so few domains that it would be that it might not be a huge problem if the browser remembers the user's choice.

… re: Kris's comments. The reason we share across registerable domain is cookies can be set across the entire registerable domain. Once you're on the naughty list you can't get off of it historically, but with new approach where we treat as SameSite=Strict we reset the counter.

Tanvi: Pete mentioned on the issue of having the concept of a staged storage space. Maybe talk about that separately to see if we can come up with an alternative solution.
