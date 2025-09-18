# 11 September 2025 Privacy CG teleconference
* Chair: Erik  
* Scribe: Kaustubha Govind, Erik Anderson  

# Agenda
* [Diverged third-party cookie behaviors \#49](https://github.com/privacycg/proposals/issues/49)  
* [Preventing User Dictionary Leaks via ::spelling-error and ::grammar-error CSS Pseudo-Elements](https://github.com/privacycg/meetings/issues/42)  
* Any other business

# Notes

### [Diverged third-party cookie behaviors \#49](https://github.com/privacycg/proposals/issues/49)

* BenV \- Some context \- where unpartitioned 3pc are not available by default (could include Chrome incognito; and default behavior on other browsers). For the last few years, we’ve had a bit of a mess where Chrome has allowed 3pc in default, and blocking in incognito. Safari has had block, with a move to supporting CHIPS. Edge allows all 3pc, except for trackers defined by a list. Firefox default partitions 3pc.  
  * As of the summer, the plan for Chrome was to block 3pc, but opt-in partition via CHIPS. Given Chrome’s change of plans; the default behavior will be to allow 3pc. Safari is still planning to go ahead and ship CHIPS is my understanding. Firefox is in a tough spot, since we currently partition-by-default; and unshipping that is challenging for compat.  
  * \[Firefox slant\] Web compat is only mitigated when “use Chrome” is not reliable advice to solve it. Likely to have an adverse impact on Firefox users. While Chrome has incognito, unlikely that users will abandon the browser if a site is broken on it.  
  * Firefox ran a 1% experiment to unship partitioning-by-default; but enough breakage that caused worry.  
  * Firefox interested in working together to collect sites that would benefit from using CHIPS, but have not done so.  
* Johann \- That context was helpful, was a bit confused by the questions on the issue. Apologies if those responses are inconsistent with the context.  
  * From Chrome’s side \- what you said is correct. It’s very difficult for us to meaningfully help in this situation given the change of plans. But if anything we can do to help web compat story, happy to do so on a best-effort basis. For example, we’ve done a lot of work to assess breakage, much of which is publicly available already; so we can share our learnings. Interested to hear what gets discussed between Firefox+Safari.  
* Erik \- Does Chrome have a setting to say “no unpartitioned cookies”?  
  * Johann: Yes, “Block 3pc”.  
  * Erik: Does it block storage as well?  
  * Johann: Separate toggle for RWS. Don’t remember the relationship with heuristics, but probably off when 3pc are blocked.  
  * Erik: Asking because the statement here says incognito is not sufficient; but could sites still be motivated to adopt CHIPS, etc. to make their sites work better on Chrome if some portion of users opt into that mode?  
  * Johann: CHIPS does work in incognito; but that impact may not be enough in Ben’s estimation. Maybe you’re also asking if we could switch to default partitioning in incognito; but I’d have to discuss that with our leads+privacy team. Could have an impact on user understanding of incognito behavior. We can discuss it; but not confident of what Chrome’s position would be.  
  * Ben: About what I expect from Chrome on where their hands are tied. One thing that did not get a response on the issue \- would like to hear from the community of web developers if default partitioning makes more sense from the historic web compat perspective. Also see some Apple folks on the call now.  
* Matt Finkel \- Curious about the experiment that you’ve run. What site breakage do you see that’s classified as worrying?  
  * Ben: 12% of total breakage reports (which include Strict Tracking Protection breakage). Included major sites, including in banking (Zelle embed, which just needs partitioned state). Worried that this is just the tip of the iceberg.  
  * Matt: Do we know if the site behaves in Safari?  
  * Ben: Reporter did not get back on this.  
  * Matt: Would be interested to test this.  
  * Ben: Can share the bug report.  
* MattF \- we should discuss what the defaults should be. Is changing CHIPS on the table; or do we consider partition-by-default as a separate spec?  
  * Johann: We are in a relatively comfortable spot because it’s a small % of users; although still would need discussion. I think this is a bigger change for Webkit, since you did try this before and rolled it back.  
  * Johann: One other concern \- when we brought up CHIPS, Webkit had a performance concern about what could happen if all 3pc are partitioned; so that’s another issue to work through.   
* Johann: any other reasons around CHIPS decisions, Kaustubha?  
  * Kaustubha: we had wanted sites to prep their sites, migrate over, and then pull the plug. Yes, a lot of design patterns mean partitioning is enough, but others are CHIPS+something else (e.g. FedCM) to solve the use case. Give devs more intentional control on the semantics they want.  
  * Johann: technical purity, opt-in is cleaner. When we did it at Firefox first, not unlike today we were always immediately worried about ensuring web compat. CHIPS seems better from a pure design perspective.  
* Brian Campbell \- some perspective as a web application developer \- partition by default always seemed like the most reasonable, obvious choice for compat reasons. The things we’ve had to do to accommodate CHIPS seemed unnecessary and awkward, just to keep things from breaking. I had brought this up in previous forums when CHIPS was presented, including IETF. It’s been presented as a way to provide control to developers; but it seemed not needed.  
  * Matt: So in your opinion, better to just partition and signal to the site that it’s happened.  
  * Brian: No value to me from my perspective. That is software that’s run by other parties \- I don’t know whether it’s embedded. Having a signal on whether it’s partitioned or not is not helpful. Firefox’s behavior is pretty sane. When CHIPS happened, I found myself having to explain to developers. Probably shows some frustration; but this is on the back of providing this feedback in other forums. Where is CHIPS? Has it been standardized?  
  * Johann: Pretty close to being a standard.  
  * Brian: What forum?  
  * Johann: WHATWG/Fetch, and IETF I-D  
  * Brian: Actual standardization is a pretty long way off. And some of us are still dealing with the benevolent changes of SameSite changes, it was very costly.  
  * Kaustubha: I empathize with you. Speaking from a browser position, as a platform you have to abstract out the requirements and think about all possible situations. In your probably SaaS-style approach where you’re okay to be partitioned, but need to talk to other people who might expect unpartitioned semantics and would be surprised to be automatically partitioned. Maybe having cookies advertise that they have been partitioned could be helpful.  
* Kaustubha \- To Matt’s question \- it might be helpful to also get a perspective from developers who may be expecting a unpartitioning semantic, and may be surprised by partitioning-by-default.  
* Aram \- Wearing my developer hat, I think partitioning-by-default would be easier to maintain and handle going forward. Otherwise might end up with a lot of debugging problems. Having a uniform experience seems more desirable to me.  
* Ben \- Over the past several years, since we’ve had partitioning by default, there was only one bug where the solution was to stop partitioning.   
* JohnW \- Worry that we are missing the perspective of embedded 3ps. When we partitioned by default in 2017-2018; the change just happened to them. They were surprised by it. The 1ps have cookies all along, and they just want stuff to work. The 1p opting-in all of its embeds to partition cookies may be the solution, since they control the embed and have the desire to keep things working.  
* \<scribe switch to Erik\>  
* Aram: This question of, “surprised 3rd parties,” is sometimes desirable. Not to third-parties, of course, but to the first-parties that embed them. Lots of entities out there that take advantage of the standard configuration of the web platform that is currently unpartitioned-and-3p-cookies-enabled for a lot of users. For first-parties, the top-level domain (publishers), don’t desire that behavior no matter how much the 3ps which they could be unpartitioned. There’s a bunch of factors here.  
* John W: Re: Aram’s comment– maybe if 1p could control the partitioning of embedded things, that could work for you too. “Yep, I embedded you, but you don’t get access to your cross-site cookies. I will make you partitioned.” Could even work for browsers that have not blocked 3p cookies by default.  
* Aram: yeah, that would be useful and better. While it would be great for me, I think we would have some percentage of sites out there who would desire to activate that mode, if it’s not the default, but would be pressured not to. “Iframe sandboxing” delivers a lot of things I wish I could deliver to my users, but all of my vendors have teamed up and said if I turn that on, I will not get paid. I want to turn it on but need to remain employed. So, even though as a publisher it’s my preferred state, I do not enact it. Publishers might say they want to activate partitioning and if it happens fast enough we could lock it in, but I worry we’ll see a repeat of iframe sandbox mode where we have a technology that delivers what we want but we have vendors that are incentivized to tell us they don’t want to do it– and they have the leverage and we don’t. It’s not a great answer but it is a problem.  
* John W: You’re absolutely right, but you only have this problem in Chrome. If you’re given the tool, it would be a net positive to have.  
* Aram: Yes, a net positive.  
* John W: other browsers, you are either blocking or they are opting into partitioning.  
* Aram: Yes, net positive, lots of our users in Chrome though. (note: would prefer no 3rd party cookies, failing that partition by default, and failing that this option)  
* Ben: wrapping up, in Firefox I will see what I can do to unify defaults. I’ll see if I can marshall the ability to deliver that. Since the changes of the past summer, I would be very interested to see if WebKit would like to change their mind on their default behavior before such a change were to occur in Firefox.  
* Johann: There was one point in the questionnaire which was about SameSite. We still have divergent SameSite behaviors across all of the browser engines (but maybe Safari and Firefox are similar?). SameSite Lax is pretty weird in its default rules. We are still interested in exploring alignment, but recognize that as very hard. And it’s a security boundary for us, so hard to change. Most natural thing would be for other browsers to upgrade to that security boundary (but hit issues in the past). Not sure if anyone has solutions/proposals.  

### [Preventing User Dictionary Leaks via ::spelling-error and ::grammar-error CSS Pseudo-Elements](https://github.com/privacycg/meetings/issues/42)

* Ari: I’m trying to make sure people are aware.

  Browsers feed user-specific suggestions into spelling and grammar highlights. The info is sensitive. When hints are applied to text fields, it’s not inspectable directly via JavaScript. But if you have enough in the text field that need to have things applied to it, the perf of rendering it is observable and thus there are timing side channels. Indicates if the user has a dictionary. To counteract that, updating the security section of the spec and we are looking to implement something Safari is already doing. Divergent behavior for when hints are painted to the screen. In Safari, user has to interact with the text field for hints to be updated and hints are updated it once. Programmatic text updates do not get new highlights until the user interacts. \[More details about how to pull off the attack and why Safari’s user interaction constraint makes it harder; see the explainer.\]

  I’m hoping to add these stipulations to the security considerations section of those CSS specs– specifically the user interaction requirement (autofocus is not sufficient) to update the hints/highlights. And hint updates once per user interaction.

  Safari already doing both of these. Firefox and Chrome not, would like to update the spec and reasoning and match Safari.

# Attendees

* Ari Chivukula (Google Chrome)  
* Erik Anderson (Microsoft Edge)  
* Brian May (unaffiliated)  
* Joel Antoci (Shopify)  
* Tara Whalen (W3C)  
* Tim Huang (Mozilla)  
* Ben VanderSloot (Mozilla)  
* John Wunderlich (JLINC)  
* Don Marti (Raptive)  
* Ehsan Toreini (Samsung)  
* Kaustubha Govind (Google Chrome)  
* Johann Hofmann (Google Chrome)  
* Chris Fredrickson (Google Chrome)  
* Michael Kleber (Google Chrome)  
* Matthew Finkel (Apple WebKit)  
* Aram Zucker-Scharff (The Washington Post)  
* Tess (Apple WebKit)  
* John Wilander (Apple WebKit)  
* Brian Campbell (Ping Identity)
