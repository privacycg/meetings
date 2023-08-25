# 2023-08-24 Privacy CG Meeting
* Chair: Erik
* Scribe: Nick & Martin


# Agenda
* [Login Status API](https://github.com/privacycg/is-logged-in)
    * [Should FedCM use the Login Status API? #53](https://github.com/privacycg/is-logged-in/issues/53)
* [Global Privacy Control (GPC)](https://github.com/privacycg/gpc-spec)
    * [Private mode/incognito #34](https://github.com/privacycg/gpc-spec/issues/34)
* [Tentative TPAC agenda discussion](https://github.com/privacycg/meetings/blob/main/2023/tpac/agenda.md)
* Any other business


## Notes
First meeting in a while!

[Should FedCM use the Login Status API? #53](https://github.com/privacycg/is-logged-in/issues/53)

Context from Sam: JohnW had an isLoggedIn proposal well before FedCM, signal that developers can use to signal to the browser that the user is logged in to their site. FedCM had some intersection. Shared the intuition that user’s logged in status as a first-class citizen could help the browser take actions on behalf of the user – clearing storage, freeing up space, etc.

Sam: FedCM was developed, and Ben (Mozilla) saw a privacy challenge of timing attacks, could use a signal from the identity provider to determine whether the user was logged in or not. Designed API to look a lot like Login Status API; Chrome is running origin trials with a fedcm-version of login-status. Exposed as both javascript object and HTTP header. Deployment experience with IdPs, self-declaring when users are logged in or logged out. That helps mitigate timing attack. Deployment experience makes us confident in the utility.



1. Keep it as a fedcm-specific signal. Less broad, but requires less discussion.
2. Merge with Login Status API to also serve the FedCM use case. … could also be useful for the Storage Access API, and so a general-purpose API would make sense.

In April, provided a proposal, working on explainer. Chrome and Firefox seem generally aligned on architecture and use. Looking to hear from John / Login Status API. Issue has been open for a while, want to move on this API sooner, before deprecating third-party cookies in Chrome.

Johann: Storage Access API had two interests: 1) prompt users when the site doesn’t know if the user is logged in or not; 2) use login status as a trust signal to gate access to Storage Access flows that don’t rely on iframes.

John W: thanks for engagement. If a big change, like change of direction, would prefer a natural language discussion, and then get to a PR text change later (more code review style). Breakout session or similar at TPAC. This is aligned with how the proposal came about. Trustworthiness of the signal: Started with: could we find something for web applications that could be more like an install state? If the browser knows that the user is logged in, that’s a strong signal to the browser that the user has a trust relationship. Intelligent Tracking Prevention blocked cross-site tracking and also deleted data, and don’t want to delete user login data. So Safari used first-party interaction as a proxy signal. Storage Access API also has a requirement for interaction, for authenticated embeds. But it’s a poor signal for login status, because users interact with many sites even without logging in. And there are other sites that a user may be logged into even if the user doesn’t visit that domain, like SSO. Also wanted to standardize the success step of an authentication: HTTP Basic Auth maybe has that, but none of the other authentication mechanisms have that. We could formalize that step. But all of these uses rely on sites not being able to willy-nilly indicate that the user is logged in. Others will want to build on it, as something to check. If we make it trustworthy, can unlock a lot of things for the web platform.

Sam: trustworthiness is the philosophical basis. Game theoretically, there is no reason for the IdP to lie to the browser about the signal. [why?] FedCM is to be trusted, because if the IdP lied, it would disrupt its login experience. Extensibility points so that any other site could use it, even if it’s not an IdP. Can change the economics so that there is no incentive for the site to lie to the browser. [?] Just a question of sequencing towards what we need to get to that vision.

Johann: long-term vision is that we have this good system that works even when there are many different kind of authentication methods. Would have to make sure that it can’t be abused or faked. Independent from this long-term vision, we need the signal not to have any incentives for being faked. Shouldn’t block API design on that in the meantime, because it could take a long time. But our proposal is for everyone to be able to set the signal for the moment.

Aram: in support of having this signal in order to do the sorts of things we talked about, but it needs to be trustworthy. There will be incentives to cheat/lie.  Lots of sites want authentication, that might fit with this well. I like the idea of logged in users having greater access to browser features, which seems to be a key piece of this.  WebAuthn has been suggested as the key, but it doesn’t have the adoption that would be required to make it the basis.  Maybe if this required WebAuthn it would push adoption of WAn. Incentives and trust … I am pro the site being in control of this (? not clear on the point).  In terms of trust, that might help.  If isloggedin is used to unlike site features, that incentivizes the site to take more responsibility over it. Challenges for sites that lie to access capabilities.

TimC: The challenge with any signal/codification is that every site is going to have a different idea.  Entry of an email address might count; there are rich and simple policies at IdPs.  FedCM can be observed, but the browser can’t observe that someone has typed an address.

Chris P: THe boolean loses the fidelity (is it MFA, a session, or does an account exist).  How do we make it trustworthy also depends on being able to identify what it means.  Sites have different expectations.  Risk of comingling the notion of a session; in some contexts, it’s longer term, some have a much shorter notion.

John: One way to get unblocked is to explore whether we can encode trustworthiness in the signal. Allow sites to set it, but having other more powerful capabilities depend on stronger signals.

… What we agreed on was to say that browser-mediated logins would be automatically considered “trustworthy”.  Not just WebAuthn, but extensions or OS.  The browser would then be able to decide based on what it understands to have happened.  User confirmation would be a fallback for these bespoke schemes (like capability links in email), or just visibility to the user that the site indicates that you’re logged in. Need incentive for sites to log users out, because they might hang on to it.

…. This cannot carry auth tokens, so that you can know precise details about the login process.  The browser has no idea about what the sites think.

Brian: I’m concerned about talking about generally available, highly trusted in that people might use it for other purposes than what was intended.  There is value in a bespoke signal; if a signal is highly trustworthy, there is an incentive to bar access to services without that signal. [Equity issue].  We need to be careful where we create an environment where providing credentials is a precondition for access to services.

Sam G: +1 stuff!



    * +1 on “mediated login” (WebAuthn, FedCM and Password autocompletes) on setting this bit
    * +1 on encoding trustworthiness in the API
    * +1 on logging out not being thought out enough

… this has been open for about a year, with lots of prototypes.  API is in production, running.  Want to have the API before we block cookies, which is October 3rd, one week after TPAC.  We want a resolution to the topic before we conclude at TPAC.  Happy to work on this.

[Private mode/incognito #34](https://github.com/privacycg/gpc-spec/issues/34)

Next, a GPC topic, Aram to provide overview; Martin opened the issue.

Aram: discussion, should we formalize how GPC work in private or incognito mode? The user might choose to have different profiles or different GPC settings; not something we typically include in a spec. Seems to be out of scope for a specification, up to different UA implementers. Browser and extension developers have better insight into the user’s intent, and it could differ between browsers. Browsers have different messaging on their private modes. Editors open to feedback on this topic.

Martin: not just about private/incognito mode, but how the expectations are formed for how the browser decides that the user has that intent. Good answers from Aram that should be detailed, that browsers work hard to understand context and user intent and should use that. Defaults should be addressed to some extent. Effect of DNT being enabled by default in some implementations led to some different interpretation of what the signal meant or not. Based on understanding of a particular audience and their understanding of how the audience behaves.

Nick: We haven’t standardized private browsing, but we have text in different places where expectations are documented. The initial work that explored this quickly got into the different threat models that apply to private modes, but they don’t necessarily cover these things.  If the browser has a mode about not wanting data sold or shared, then maybe this is OK, but if the mode is about clearing local state, then maybe not.  I don’t know if this needs to be in a spec.  That this is about the agent and interpreting intent is most useful.  There is some language on this point, but maybe we need a PR about that.

Sebastian: Agree with all.  The default is very important in my view, but ultimately it is the regulator/law making that decides. In CA, under CCPA, when the browser is marked as privacy-protective that is OK.  A state might say that some modes are OK to have GPC on in that context.

Aram: follow-up on messaging that could be added to be more descriptive. Would non-normative language on defaults/intent be enough? Or looking for something stronger? 

Martin: +1 to what Aram/Sebastian/Nick said. Current text is just a little brief. Just note that it’s important that different implementations will choose different ways, for different users. The decision on the actual legal import will be determined elsewhere, but can be affected by presentation to the user.

Ben V: Varies by jurisdiction, some [do/might?] require that it not be on by default. Colorado specifically.

Jeffrey: Some regulators might say that different browsers get different treatment based on how they ask/decide.  Browser makers might need to consult with regulators.  Maybe we can include advice based on those learnings/discussions.  Maybe not now, but eventually as we learn more.

Justin: have talked to regulators, who don’t want to pre-bless particular implementations, but have provided some detailed guidance. There is also a broader [issue](https://github.com/privacycg/gpc-spec/issues/52) around defaults and UI. Agree with discussion about a little more detail, but can’t be so specific.

Nick: Normally we do specifications, we don’t publish legal opinions.  Keep it that way.  But it might be useful to do some sort of summary of what people have done or learned.  That makes it easier to use specifications even if we aren’t lawyers writing legal opinions.  That’s new to us, but we seem to have some expertise looking to help out in this area.  Without trying to interpret laws or anything like that ourselves.

Aram: Putting some more meat on the section on when to turn the feature on sounds necessary (making notes on the issue).  Regulator feedback is something that we might want to avoid doing in any detail in the specification, but there is a sort of idea of building explainers for specifications, which might be better place to do that sort of documentation.  Having a list of resources, pointers to how people have decided.  That might follow the W3C tradition of explainers and supporting material for specifications.

[Tentative TPAC agenda discussion](https://github.com/privacycg/meetings/blob/main/2023/tpac/agenda.md)

TPAC coming up. Tentative agenda linked to from this meeting agenda. Can mark issues  \
Agenda+f2f”

If there are breakout sessions to note, that’s also useful to mark.

Johann had some suggestions about priority from Google.

W3C Slack probably best, mailing list and offline discussion also possible.

Johann: not dictating schedule, but tried to estimate how much we have to discuss and what things need to be discussed face-to-face. Don’t have enough time to cover everything.

Kaustubha: a couple topics on anti-fingerprinting, and the most vexing issue has been what to do with anti-fraud use cases, not just ad fraud, but also account takeover, payment fraud, ddos. Anti-Fraud CG has a lot of domain experts with insight into particular attacks and capabilities that they’re interested in for the platform. A lot of philosophical questions in that group about how it can be compatible with privacy. Some browsers are using blocklists. In AFCG, should we have allowlists of sites that can continue to do fingerprinting? Not sure that’s a great long-term solution. AFCG already has some time allocated, so maybe enable privacycg participants not to have a conflict so they can attend.


## Attendees
* Justin Brookman (Consumer Reports)
* Erik Anderson (Microsoft Edge)
* Tom Van Goethem (Google Chrome)
* Chris Needham (BBC)
* Nick Doty (CDT)
* Don Marti (Raptive)
* Zachary Tan (Google Chrome)
* Christian Biesinger (Google Chrome)
* Russell Stringham (Adobe)
* John Wilander (Apple WebKit)
* Johann Hofmann (Google Chrome)
* Sam Goto (Google Chrome)
* Nicolas Pena Moreno (Google Chrome)
* Benjamin VanderSloot (Mozilla)
* Sam Weiler, W3C
* Aram Zucker-Scharff (The Washington Post)
* Sebastian Zimmeck (Wesleyan University, GPC team)
* Brian May (dstillery)
* Eric Mwobobia 
* Ari Chivukula (Google)
* Chris Phillips (CANARIE)
* Ben Kelly (Google)
* Eyal Abadi (P&G)
* Martin Thomson (Mozilla)
* Pete Snyder (Brave, PING)
* Tim Cappalli (Microsoft Identity)
* Craig Erickson (PrivacyPortfolio)
* Kaustubha Govind (Google Privacy Sandbox)
* Tara Whalen (Cloudflare)
* James Hartig (Admiral)
* Christine Runnegar (PING co-chair)
* Jeffrey Yasskin (Google Chrome)
