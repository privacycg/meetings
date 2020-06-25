# Privacy CG telecon, 25 June 2020

[Agenda link](https://github.com/privacycg/meetings/blob/master/2020/telcons/06-25-agenda.md)

* Chair: Erik
* Scribe: Pete Snyder (Brave)

## Attendees
* Erik Anderson (Microsoft)
* Theresa O'Connor (Apple)
* Kris Chapman (Salesforce)
* Joel Odom (Salesforce)
* Christopher Cornett (Salesforce)
* Josh Karlin (Google)
* Przemyslaw Iwanczak (RTB House)
* Bartłomiej Romański (RTB House)
* Bill Densmore (ITEGA)
* Max Gendler (NY Times)
* Jacek Oleksy (Opera)
* David Benjamin (Google)
* Paul Jensen (Google)
* Aram Zucker-Scharff (The Washington Post)
* Chris Needham (BBC)
* Mike O'Neill (Baycloud Systems)
* David Dabbs (Epsilon)
* Shigeki Ohtsu (Yahoo Japan) 
* Sam Weiler (W3C/MIT)
* Mike Smith (Hearst Magazines)
* Ashkan Soltani (None)
* Pete Snyder (Brave)
* Erik Taubeneck (Facebook)
* Sean Harrison (Google)
* George Fletcher (Verizon Media)
* George London (Survata)
* Sebastian Zimmeck (Wesleyan University)
* Jack Frankland
* Russell Stringham (Adobe)
* John Wilander (Apple WebKit)
* Wendell Baker (Verizon Media)
* Shivani Sharma (Google)
* Steven Englehardt (Mozilla) 
* Martin Meyer (Akamai)
* Steven Valdez (Google)
* Christy Harris (Future of Privacy Forum)
* Michael Kleber (Google)
* Melanie Richards (Microsoft)
* Arthur Edelstein (Mozilla)
* Wendy Seltzer (W3C)
* Russ Hamilton (Google)
* Samuel D. Sousa (Triton Digital)
* Marshall Vale (Google)
* Christine Runnegar (PING co-chair)
* Scott Low (Microsoft)
* Theodore Olsauskas-Warren (Google)
* Tanvi Vyas (Mozilla)
* Kate Cheney (Apple)
* Chris Wilson (Google)
* Andrea Marchesini (Mozilla)
* Jeffrey Yasskin (Google Chrome)
* Don Marti
* Andy Sharkey (CafeMedia)
* Arnaud Blanchard
* Brad Lassey (Google)
* Ben Savage
* Brad Rodriguez (Rubicon-Telaria)
* Carlos
* Cory Hammon (Rubicon-Telaria)
* Jinghao Yan
* John Sabella
* Henry Lau
* Kaustubha Govind (Google)
* Kristen Chapman
* Kuba Alicki
* Marijn Kruisselbrink
* Michael Smith
* Paul Bannister (CafeMedia)
* rbelur
* Samuel Weiler
* Soujanya Kocheri
* Thomas
* tkershaw
* viraj
* Garrett McGrath (Rubicon-Telaria)
* James Hartig (Admiral)
* Lisa LeVasseur (Me2B Alliance)
* Mingzhe Li (Akamai)
* Joey Salazar (ARTICLE 19)
* Shaun Gilmore (Apple)
* Kushal Dave (Scroll)
* Arnaud Blanchard (criteo)

Erik: To be added to a future agenda, please create a GH issue and label it for the agenda.
...: also, need more scribes for the future.

## Work Items
### Storage Access API - [Promptless storage access with constrained communication capabilities](https://github.com/privacycg/storage-access/issues/41)

JohnW: Based on a face-to-face conversation with google, regarding access to unpartitioned storage w/o. prompt
...: Would be an isolated frame that doesnt allow cross frame data to leak through
...: I (JohnW) came up with a possible attack, but hasn't generated discussion beyond "seems possible" and interest in mitigation through preventing
...: dynamic frame creation.  Wanted to disucss to see if there is a path for truely isolated iframes.

Josh Karlin: John is right.  Google calls this an enclave internally.  Idea is a page w/in a page, that prevents communication across frames
...: with unpartitioned storage, w/o prompt.  Explainer in the works, targeting the next week or two.
...: Hoping to find a common solution.  Motivation is for storage and other privacy sandbox APIs.
...: want to take cross site information and display something w/o the surrouinding page knowing (ads for example)
...: Known problems: link decoration, but doesn't seem unique to enclaves (both path and query parameter)
...: but, we'd like to solve the problem jut for enclaves if thats necessary.
...: Its "iframe like", but more like a tab thats embedded in another page.  Its not a traditional iframe/

Pete: Brave is working on something similar, locally behaviorally-targeted ad that doesn't leak info to surrounding page
Would like to be part of the conversation

(thank you mystery scribe (np -- kleber))

Erik: looking forward to the explainenr and learning more

Kushal: Link decoration is difficult, what is google considering for enclaves?

Josh K: Toy solution: no query prams, but could just move data to path
...: other static like pages could declare what URLs it accepts, and so the URL wouldn't be user identifying
...: Could also be based on measurements of what other URLs are seen in the wild.

Kushal: What about like buttons?

Josh K: Just need to pass the URL of the embedding page

Kushal: but then the enclave learns the parent URL? Problem?

Josh K: Partial solution is to require user gesture for storage
...: Goal is to prevent wide scale tracker

Kushal: What about gating access to the enclaves' containing URL on user gesture

Kleber: A key use case is TURTLEDOVE approach, where what can go in the enclave isn't determined by the page, but the TURLEDOVE system.
...: possible solution would be to reduce the set of options for what can go in an enclave.
...: Further discussion in discourse, to move this problem to WICG
...: Other option is SPARROW (from Criteo).
...: Will try to work out capabiltieis in WICG, will discuss prviacy concerns here.
...: Brave is welcome, some of the current proposal comes out of convo with some Brave folks ( :) )
...: link provided re TURTLEDOVE use case, Discourse thread at https://discourse.wicg.io/t/advertising-to-interest-groups-without-tracking/4565

Ben Savage: Concern is unique-interest-groups can become uniquely identifying.
...: What if we could make ad requests from enclaves identifying, but w/o knowing the context.
...: Would degrade perf, but would be allow targeting ads to an individual w/o context
...: Is that compatable with web privacy threats

John W: Interesting idea, thanks to google folks for explaining the motivation.
...: Seems removed from what Storage Access API is designed to solve.  Idea of allowing personally identifing ads, with servers knowing
...: user identity, seems like a very diff matter than Turtledove and other concerns.
...: But the use case seems distinct from Storage Access API.

Kris C: Appealing to prevent the embed from knowing the parent context, for example ad brand safety, and advertisers not wanting to 
...: place ads against some content.

Don M: Also fraud concerns of hiding location from the ad.

Josh K: Some bits will need to go from the parent to the child.  For example, maybe eTLD+1.

Kleber: This isn't meant to be overview / critque of turtledove; the W3C Web Advertising Business Group is discussing these issues.
...: All are welcome to participate in that discussion.
...: Render the ad w/o the ad knowing the context is a diff, but related, concern from the rest of this.

Josh K: Similar concerns with embedding videos.

Erik: Its all tricky; thanks for the disucssion!

## Proposals
### [Implementing privacy rights](https://github.com/privacycg/proposals/issues/10)

Sebastian: CCPA gives CA folks privacy rights, including ability to opt out of selling informaiton.
...: … and sites should respect signals from browsers and plugins.
...: This proposal is to understand what users would like and how those signals might work.
...: CCPA gives many rights (deleting, preventing sale, etc).
...: This issue is to make things more specific to selling data, and more concerete.
...: Two prong'ed approach, the spec, and the tool (e.g., prototype browser extension being developed).
...: We're studying if users are interested, can understand the tool, diff between ads and "dont sell" request.
...: So we're interestd in disucssing with browsers, publishers and advertisers.
...: Agenda is linked from the issue.
...: Goal -> move specification forward in the next few weeks.

Aram Z-S: This is needed, CCPA has lead to diff legal guidance than market was anticipating.
...: One diff is that prev users had to initiate the signals, but that no longer seems to be the case and that
...: users dont need to initiate the signal.  "Do not track" signal does not to be sufficient or equal to "do not sell".
...: Browser or extension signal would be additionally beneficial, so that pages could hook into it for the CCPA opt-out process.
...: Could be like the DNT (Do not track) signal, but might not be good way to go; CCPA might be too diff.
...: Eager for questions on thread.

James Hartig: How could the "do not sell" signal map to GDPR?
...: What should folks not in California do with the signal?

Sebastian: Personal opinion: "do not sell" signal is different from DNT signal
...: However, Microsoft (and maybe others) have said they would recognize signal even for folks not from california?
...: There is overlap between the signals, but should be distinct
...: In Firefox, you could just add another option like "do no track"?

Ashkan S: I was invovled in CCPA and CPRA (CCPA v2).
...: DNT and do not sell can be thought of as different, and law envisioned that they would be similar but distinct as DNT has existing baggage.
...: Envisionsed something like DNT header instruction, or something more user initiated in site (?)
...: Either would be sufficient.  Could be a standard
...: Some companies have already said they'd uphold the standard, so a voluntary standard (and not a standards body approach like DNT) might be more effective
...: [offline note from Ashkan: also forgot to mention to look to CPRA language since that might be standard in 2021]

Aram Z-S: Many folks see CCPA and GDPR as diff and so can't share an overlapping signal.
...: When someone has indicated do not sell, need a way of applying even when people are behind VPN, Tor, or otherwise hiding IP address
...: So need a way to tie CCPA-coverage to an account / person, not to IP
...: Still good if people are saying they will respect DN* signals, but needs to be an active signal
...: Sebastian's interface might be a way of doing so, so that the page can reply and say "no we're not respecting the DN* signal(s)".
...: So that we can make it clearn when we dont respect the "do not sell" request

Wendy S: Good to hear people are interested in the area, might be good for a task force / deep dive side call
...: Especially bc the regulatory environment may have enabled new options

Joel O: Interested from implementation / Salesforce side
...: When a user has enabled the signal globally, how to deconflict it with possibly conflicting signals received elsewhere

Ashkan S: Newer law handles some cases like this (e.g., "opt back in" options, or the priority / how to consider different signals)
...: law also considers how to verify its actually someone covered by the law.
...: Some companies are consdering announcing their support for a signal, but want to avoid companies not being aware of the law
...: or ambiguity in how to understand conflicting signals.

Erik: As Wendy S said, folks have experence with DNT (vendor and standards body alike)
...: this is an important topic, important to think through what folks will do w/o a standard signal.
...: And seems difficult to do slowly in this forum.


### [Prevent Service Worker as Cross-Site Proxy](https://github.com/privacycg/proposals/issues/15)

Jack F: This proposal anticipates trackers trying to bypass vendor imposed restrictions
...: trackers might integrate with 1p (server side, CNAME'd subdomain, etc)
...: propsal presumes that the above is bad and vendors should stop it
...: Service workers can be used similarly to backend proxies
...: So it looks to the brower that the "third party" is actually the first party
...: Does this group agree this is bad (is it diff enough from just having 3p script)
...: also, what to vendors think? (Mozilla has registered some complaints)

Erik: Has been some previous conversation (e.g., partitioned service workers, partitioned storage)

Jack F: if i understand correctly, the partitioning is under the first party, but since the SW is registered under the first party
...: seems like it'd be partioned with the first party, and not treated seperately.  So, doesnt seem relevant (?)

David D: Isn't it the case that SW don't have access to cookies? If so, there is a proposal to rework cookie access in SWs

Erik: Whats your goal / next steps on the proposal?

Jack F: First goal is to understand if this is something worth worrying about, or otherwise a concern vendors have
...: for example, certain forms of bounce tracking allows different parties to get 1p storage access.  So, is this a SW level concern
    pete s [just that brave def is concerned about this, and mitigates in in some ways]

David B: Whats the diff / unqiue to SW here?  Aren't SW basically just cross process communication? Solve by transfering CSP to SW

Jack F: the concern is that it looks like the request is coming from the 1p, conceptually these are different parties

David B: What are the specific attacks you're concerned with?  If the SW has the same privilages as the main doc, what is the additional attack surface?

Jack F: Browser are concerend with calls initiated to third parties in the main document; since the SW operates as 1p, it might be treated diff

David B: So concern is possible confusion when applying different policies to different kinds of fetches, with this information lost at the SW?

Jack F: Yes, folks might treat the SW as 1p, even though it was created / initated by a 3p

John W: Partitioning of SW and cache is relevant b/c it seems like the biggest concern regarding tracking abilities

Michael K: In my experience, if script can convince the 1p to allow the script to install a SW, they can prob convince the 1p to do other equiv things
...: But it seems worth while to note that SW can set HTTP headers, which is harder for 3ps to do
...: But 3p executing script in 1p context doesn't seem like good barrier


## Any other business
### Cryptpad

Tanvi V: There have been problems with Cryptpad, doesn't seem to be up to load of ~70 users
...: Also makes things tricky for taking notes, queing, etc
...: Other parties in W3C use IRC
...: Lets use Google Docs for a few weeks
...: Not available in China, but no one in this group is from China currently

Wendel B: Does Google Docs scale to ~70 users?

Michael K: yes

Tess O: Would that require a google account to scribe / follow

Tanvi V: No, would make it so that you dont need an account.  Chairs can handle the window of non members editing 

Ben S: What about IRC for queueing, elsewhere (cryptpad) for scribing

Ashkan S: Google docs has revision managmeent feature

Tanvi V: there is version history, but not 100% granular

Erik: We're at time, lets discuss further on Slack
...: We'll give google docs a try

From zoom chat - Jeffrey Yasskin - https://tcq.app/ is what TC39 uses for queuing.
