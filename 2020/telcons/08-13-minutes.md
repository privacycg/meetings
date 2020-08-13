<b>Privacy CG telecon, 13 August 2020</b>

* Chair: Tanvi
* Scribe: Michael Kleber


<b>Administrative</b>
* [Announcing First Party Sets as a Work Item](https://github.com/privacycg/admin/issues/17)
* [Announcing IsLoggedIn as a Work Item](https://github.com/privacycg/admin/issues/18)
   * That means we now have five work items — full plate!
* [Virtual Face-to-Face for TPAC](https://github.com/privacycg/meetings/issues/8)
   * Wednesday/Thursday September 16th and 17th
      * Groups will meet sometime around TPAC meeting time
      * Doodle sent last week to ask for time preferences:
   * 8am PDT to 12pm PDT
   * Work Items
      * Editors: Please think about what you'd like to accomplish in the f2f time
   * New ideas/brainstorms/proposals
      * OK to submit proposals even for not-fully-baked thoughts for us to talk about
   * Label items agenda+f2f if they want to discuss them

<b>Work Items</b>
* [First Party Sets](https://github.com/privacycg/first-party-sets) - Kaustubha Govind (Google Chrome), David Benjamin
   * FPS Motivation and Overview
      * Web evolved over time; originally most sites were primarily single-domain, but as things evolved, lots of sites are composable, including domains that want to remain distinct for security reasons (e.g. uploadable content)
      * Lots of companies also use multiple domains for business purposes, or for per-country top-level domains for performance
      * Even though these domains are treated as third-party and cross-domain today, when there were not restrictions about communication across domains, this wasn't a problem.  Now that browsers are imposing restrictions on what third parties can do, the difference between real 3rd-parties and same-organization 3rd-parties has become important
      * Places like gov.uk has put themselves on the PSL (Public Suffix List), so that it has lots of different eTLD+1's (effective top level domain +1), but it is actually a single organization
      * Summary: Lots of multi-domain first parties today
      * Goal: Define the privacy boundary for the web — sites within the privacy boundary wouldn't incur privacy-type protections from one another, but would still have the classical security restrictions by domain
      * Some existing browser privacy work already makes exceptions for companies on some list of domains owned by the same company.  Would be better to organize and standardize, rather than have an ad-hoc list
      * Perhaps the split cache work could also use the privacy boundary, rather than the security boundary
         * Q: Christine Runnegar: Who would be defining the privacy boundary for a site?
         * A: Site would propose the list of domains, but then would be subject to an approval process
         * Q: Steve Englehardt: Uses? Beyond login, consent management?
         * A: Link decoration mitigations, trimming URL parameters, auth tokens
         * Q: Lisa LeVasseur: It's odd to not talk about privacy in the context of people — they are the object of privacy.  Would encourage attention to the user
         * A: This is a key part of the policy: What is an acceptable first party set?  All about user expectations and understanding.  Surfacing in browser UI, etc, are ways for users to understand what's happening
         * Lisa: [IEEE-7012](https://sagroups.ieee.org/7012/) working group —  working on machine-readable user-proffered privacy terms, to give UAs attention
            * Also [this work](https://en.wikipedia.org/wiki/Communication_privacy_management_theory) to align or ground the idea of Privacy Boundaries  into human behavior
   * Questions for open discussion:
      * Need for a solution to define multi-domain sites’ “privacy boundary”
         * Kris Chapman: I agree that there is a need for a solution — absent this, push for companies to use the same domain, which obscures for users the different parties.  Want to avoid multiple prompts for permissions, etc.
         * Q: Jordan Mitchell: I agree that there is a need, but it may be internal — for companies to manage their content internally, while for consumers we want to respect attributes of the people themselves.  For ad fraud, for example, this seems useful.  I'm interested in the IEEE-7012 reference to bring this to the consumer
         * Kaustubha: On Chrome we have other ideas, like Trust Tokens, to let organizations communicate about trust and fraud issues, so some of that may be out of scope
         * Steve Engelhardt: Mozilla starting with user expectations.  Right now we think eTLD+1 is the method to do that: users look at URL bar, they know what company they're interacting with.  We think putting HuffPost.<theirowner>.com in the URL bar would be bad for the user; relaxing the boundary without putting it in the URL bar would be more confusing.  We like targeted interventions like explicit user login, rather than FPS.
         * Kaustubha: Log-in is very different; users understand that option explicitly.  Other uses are not nearly as clear to user what's going on (country-code TLD, uploaded content, etc).  We wondered whether we actually needed it at first, whether sites could just move to subdomains.  We think this is better experience for user than getting their attention for these uses.
         * Christine Runnegar: Steve raised important points. Also need to worry about how this could go wrong, how could privacy boundaries be misused.
         * Kaustubha: Agreed; the policy mitigations are in place to prevent abuse, chose this over options like after-the-fact review since it may be too late.  Need to spell out the threat model.
         * Ben Savage: Q to Steven: I agree that privacy boundaries should align with expectations — what other ideas do you have, for privacy boundaries aligning with eTLD+1?
         * Steve E: Not clear that anything else is possible.  Maybe URL decoration with registered company name?  But extended validation went in this direction and found hard problems here.  Doesn't solve navigation — do you also decorate navigation events?  Everything is build around URLs, so seems very challenging to overlay anything on top of that.
         * Ben S: To date, FPS proposal is all about automatic inclusion of multiple eTLD+1's in a privacy domain.  Have we contemplated explicit user interaction to do this, to align expectations?
         * Kaustubha: Current thinking: Company submits their set, gets validated, policy-checked, etc, then happens automatically.  You're taking about another step where before the first time a privacy boundary within FPS is crossed, the user gets asked?
         * Ben: Yes, explicit user interaction on top of FPS listing
         * Kaustubha: Many of the implications of FPS generally need to happen at site load time, so asking after the page loads might be hard for some use cases.  But others might make sense for after-load-time user prompts.  Could reconsider for some use cases.
         * Re. Steve: The Chrome security people were quite hesitant about additional information in chips for extra information in the URL bar; have had problems in the past.
         * Tanvi: Ben reminded me of a UI flow — We Firefox created Facebook container, with FB in a different container; when user did some explicit action like "log into AirBnb with my FB identity" we prompted user about whether they wanted to move that site into the FB container.  Take a look at the flow & UI - way to group sites together manually.
         * Kaustubha: WebID proposal, for explicit log-in, is along these lines.
         * Tanvi: Not explicitly about login, but more about the UI flow of intervening and prompting the user to group things together
         * Ben: WebID doesn't make a shared cookie jar, does it?
         * Kaustubha: Correct, it does not
         * Arthur Edelstein: How does Storage Access API fit into this?  Seems like that gets explicit user consent.
         * David Benjamin: You could imagine FPS as a way to adjust the heuristics for the Storage Access API prompt.  Would only work for flows with initial user interaction, so as before, works for some cases but not others — doesn't make sense for link decoration mitigations, for example.  This would also end up prompting for no particular reason, in cases where domains don't do any information sharing.  The goal here is *not* to treat things as the same domain
         * Arthur: If Storage Access API is required then you have explicit consent, so why can't you just use that?
         * David: Explicit consent would cover some flows, but we don't want to prompt users all the time — "if you see the storage access API prompt and need to click yes to make sites work", then it degrades ability for user to distinguish between cases where there is vs isn't actual information flow.
         * Kushal Dave: Precedent for this in app stores — apps from same vendor have access to same browser storage; there is a model to let users know that they are from the same vendor.  Also model of cookie consent — when users are asked about allowing cookies, they could be asked about the scope they're consenting to.
         * Kaustubha: The way sites work today, there is some domain that stores the "user clicked yes on a cookie banner" cookie, and any time you'd want to ask for consent, you can consult the cookie on that domain.
         * Kushal: Was thinking about the browser getting consent at the time that the site displays a GDPR-style prompt
         * John Sabella: How do you see sites moving in/out of a set?  Process to apply for it, or more fluid?
         * Kaustubha: Initially a process, need to rethink if it needs to scale up.  In TLS world there is a way the process has been automated, used by LetsEncrypt.  We can find ways to automate majority of them, but requires transparency, public log, after-the-fact auditing.  First need to agree on what the policy ought to be.
      * Insights from other browser vendors, web developers, and participants about what constitutes “first-party” (deferred to next call)
* [IsLoggedIn](https://github.com/privacycg/is-logged-in) - Melanie Richards
   * Overview: Proposal championed by John Wilander (Apple), not here today.
   * Attestation from web developer to UA that the user is logged in.
   * This API doesn't manage the user's identity or logged-in state.  It's just a signal to the browser indicating that the developer wants the browser to consider the user logged in
   * By default today, first-party site can store information forever about the user — strange that this information remains permanently
   * With an IsLoggedIn signal, could limit storage duration if you're not logged in, or relax limits if you are
   * Browser could show you a list of all the places you're logged in to
   * API: browser gets some info about when you're logged in, eg user name [I missed this]
   * Also learns when you're logged out, also finds out the cookie or auth token that holds the logged-in state
   * How to prevent abuse, know user really thinks they are logged in?  Laundry list of ideas in the explainer.  WebAuthN, password manager, etc.  Want to balance acquiring a strong signal vs. ease of use for web devs
   * Want to look at how signal integrates with Federated Login flows, First Party Set, Storage Access API, etc
   * Can we support a "half state"?  What if user wants the site to remember their username but not all the other information?  Needs to be discussed as we develop the proposal
      * Q: Ben Savage: Re your motivation — even if you haven't visited a site for a long time, site might know personal things about you for a long time afterwards.  How does this help?  Information sent to a server means it persists even if browser cookies are cleared.
      * Melanie: There are a lot of other things to address.  I see this as being very helpful for users because storage clearing today is a very blunt tool.  This makes it possible for users to clear their storage except for stuff where they expect to stay logged in, avoid the pain of logging back into lots of sites
      * Ben: Is the space taken up by cookies really an urgent problem?
      * Melanie: That's not a core motivation.
      * Ben: If it doesn't solve storage of private information (since that's on the server) and doesn't solve a space problem, then what is the primary motivation?
      * Melanie: Applications like relaxing the restrictions that Safari has placed on storage lifetime, or prompting for uses of storage.
      * George Fletcher: I'm an identity guy; obviously have to worry about privacy too.  There are a huge range of use cases for how identity is used on the web.  maybe we should create a collected set of identity use cases and deployment.  As we protect against silent 3rd-party tracking, it's going to affect lots of other use cases, relying on identity and identity federation that are outside of tracking.  We should holistically look at how browser understands identity to protect users more generally.
      * Melanie: Agreed, should look at holistic idea of identity rather than one API at a time
      * Tanvi: Where is this holistic view happening?
      * George: Some of this conversation has happened in WebID in WICG — ues cases - enterprise, academic, federated login, etc.  We need to talk about all the use cases.  Should bring these together.
      * Mark Xue: Re. Ben's Q's about intent: Just because information stored in cookies is saved on a server doesn't mean that the linking keeps being possible after cookie clearing.  Site can't associate my activity from a year ago with my activity today, even if each one separately is saved
      * Ben: So is the intent to prevent first-party sites from correlating multiple visits from the same user, and so keep them from understanding that the same user is on their site on multiple occasions?
      * Mark: That Q assumes "the same user" means an identity
      * Ben: In the FPS discussion, we said the user has a strong notion of the first party = eTLD+1.  Sounds like Safari wants to intervene between a first party and the user who went to their site.
      * Melanie: We did research about requests to access powerful APIs, like geolocation.  We found user expectation was that the information wouldn't be retained by the site forever — that the site gets geolocation access for only one session at a time.  So just because a user goes to a first party over time, not necessarily the case that they want everything to be remembered over time.  We've been hyper-focused on first parties, but looking at first-party too is valuable
      * Ben: So you want to shrink the privacy boundary from eTLD+1 to eTLD+1-cross-session?
      * Melanie: Not necessarily, but give user that flexibility.  Could relax some restrictions with the isLoggedIn API.

<i>Out of time, sorry to the people left on the queue.  Please open issues on GitHub.</i>

<b>Attendees</b> (sign yourself in): ~64 Participants on zoom
* Melanie Richards (Microsoft)
* Sean Harrison (Google)
* Josh Karlin (Google)
* Chris Needham (BBC)
* Theresa O’Connor (Apple)
* Tanvi Vyas (Mozilla)
* Kaustubha Govind (Google)
* Michael Kleber (Google Chrome)
* Theodore Olsauskas-Warren (Google)
* Marijn Kruisselbrink (Google)
* Paul Jensen (Google)
* Andrew Knox (Facebook)
* Erik Anderson (Microsoft)
* Mike Lerra (Google)
* Steven Valdez (Google)
* Russ Hamilton (Google)
* Steven Englehardt (Mozilla)
* Arthur Edelstein (Mozilla)
* Mike O’Neill {Baycloud)
* Jason Nutter (Microsoft)
* Wendy Seltzer (W3C)
* James Hartig (Admiral)
* Jeffrey Yasskin (Google Chrome)
* Raj Belur (Amazon)
* Kate Cheney (Apple)
* Paul Bannister (CafeMedia)
* Diarmuid Gill (Criteo)
* Shigeki Ohtsu (Yahoo Japan)
* Shivan Sahib (Salesforce)
* Christine Runnegar (PING co-chair)
* George Fletcher (Verizon Media)
* Wendell Baker (Verizon Media)
* Eric Mwobobia (ARTICLE19)
* Shivani Sharma (Google Chrome)
* Jack Frankland
* Pranjal Jumde (Brave)
* Chris Wilson (Google)
* Eli Grey (Transcend)
* George London (Survata)
* David Benjamin (Google)
* Bartłomiej Romański (RTB House)
* Ben Savage (Facebook)
* Mark Xue (Apple)
* Lisa LeVasseur (Me2B Alliance)
* Devin Rousso (Apple)
* Jeddy Chen (Magnite)
* Sam Weiler (W3C/MIT)
* Nicolas Arciniega (Microsoft)
* Kris Chapman (Salesforce)
* Kushal Dave (Scroll)
* John Sabella (PubMatic)
* Jordan Mitchell (IAB Tech Lab)
* Andy Sharkey (CafeMedia)
* Christopher Cornett
* Cory Hammon
* David Dabbs
* Joel Odom
* Maud
* Michael Smith
* Przemyslaw Iwanczak
* Robert Stratton
* skocheri
