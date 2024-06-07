# 2024-05-23 Privacy CG meeting
* Chair: Erik
* Scribe: Kaustubha Govind

# Agenda
* Partitioned Popins
* Any other business

# Notes


## [Partitioned Popins](https://github.com/privacycg/proposals/issues/47) 

[Proposal](https://github.com/arichiv/partitioned-popins), Presentation (Ari -)
* Partitioned Popins (PP) - usecase is that of white label identity. Not Login with Google/Facebook, not SSO. Account that is unique to their website, but don’t want to manage the identity stuff themselves, have an identity provider provide this service. Impacted by third-party cookie deprecation since the cookie is written in a popup, and needs to be available in the iframe.
* &lt;image in slide deck showing interaction of web documents for use-case>
* In terms of UX; thinking of a few requirements
    * UX of opener isn’t available until the popin is dismissed
    * Only one popin for a given tab at a time
    * When switching tabs, popin disappears (i.e. still present in the other tab that opened it)
    * Page opened needs to opt-in to being opened in a popin
    * URL of popin cannot be edited by user (concern being that if user navigates the popup to navigate to other origin, can enabling browsing history collection across new contexts).
* Other requirements:
    * postMessage - want to align with cross-origin isolation. Still trying to figure this out.
    * New permission policy. Opened page needs to opt-in.
    * Will provide APIs to allow documents to query if they’ve been opened in a popin context.
* Questions?
    * Nick Doty: Could this be done with an iframe with new policies (i.e. visible UI)?
        * Ari: Definitely could. The reasons that the usecases we’re looking at don’t want to do that; is that they don’t want a login page where user enters credentials to be embedded on a potentially untrusted top-level page.
        * Nick: Great, really clarifying the situation. Is the idea that I type in a “not partitioned” username and password from the IdP; and the IdP provides partitioned identity? Or do we just want to provide another system to allow usernames/passwords for that site’s partition.
        * Ari: There would be a 1:1 relationship with that identity and that site. That identity wouldn’t be usable on another site. Login is only used that the site that opened it.
        * Nick: Then, wouldn’t iam.example have fewer security concerns. For example, this isn’t your google username/password being used on a random site.
        * Ari: True that if you used a password manager. The feedback we got on the feature request was that they want to do it on a new page.
        * Ben: Why is clickjacking a concern; when you can use CSP to limit where you can embed it.
        * Ari: Good point. The way I’ve been thinking about it is. Not sure if
        * Kaustubha: Want to give context on what we’ve seen. Okta as an example. Millions of customers. To “why is it so security sensitive if 1:1 relationship?” their B2B model doesn’t, I think, want their customers becoming vulnerable to a security attack to in turn affect their own service. They don’t necessarily hold sway over their business customers’ security posture. Might have one customer who has one attack with a script injection attack; malware in top-level script and can read everything typed into iframe. Think they’re trying to deal with a not completely secure supply chain. Not sure if that answers your earlier question, Nick. To Ben’s question– I don’t know if I fully understand it, but good to understand use case… CSP I’ve not been too involved, but canonical use case is Okta (but we haven’t talked to Okta), but we’re thinking about other non-Okta folks to use it, secure by default, have necessary protections in place that may not be 1:1 and may be many:1. For popins UX constraints, hopefully not used for more than 1:1.
        * Johann: I don’t think iframes can be a viable alternative to popins. Besides the security aspects; iframes don’t offer the right composability. Much nicer UX experience for users with popins. Can Ben repeat the CSP question?
        * Ben: CSP can ensure 1:1 relationship; so why have additional requirements? But I think Kaustubha answered that by describing the usecase.
        * Johann: …
        * Nick: I thought the point of iframes was composability.
        * Johann: That is true, except when there is a need to enter credentials.
        * Nick: I thought main reason was that you don’t Okta/Google credentials to be typed into random sites.
        * Johann: 1:1 doesn’t imply trust from the web security perspective.
        * Erik: Possible multi-layer iframe scenarios where this is helpful as well. You may trust your own iframe (e.g. within a Teams app). Being able to pop-out a trustworthy address bar where the user understands the context. This is why popups have been prevalent among IdPs.
        * Erik: Are we thinking this is primarily identity flows? What about a modal popin concept for other cases where a partitioned iframe could be popped out into an iframe to persist for longer. Probably a host of UX concerns. Maybe you’re constrained because of how difficult the UX is? Some similarity with sandbox iframes, the popup inherits sandboxing of the iframe, which the users can’t obviously connect. Are you envisioning keeping this constraint?
        * Ari: Login is a thing we’re thinking about now. Web app in an iframe, can imagine popin for a printing operation. The key thing is to make sure is user understands. A flexibility extension could be to make this an unpartitioned context.
        * Kaustubha: There is a usecase that came up on the CHIPS repo that wants to pop-out a bank statement/other document in a new tab which gets authenticated in a popin/white label IdP.
        * Erik: There are developer understanding issues as well. Microsoft internally has a training site with many nested, sandboxed iframes. When a popup was launched to load a PDF, the sandbox attributes created issues with how the browser loaded it in a new window (which didn’t happen with a nav in a new tab). If the UX isn’t very visibly different, devs have a hard time understanding who broke what.
        * BenV: “UX spoofing, password managers, and a new navigation primitive where iframe hardening would do”
            * UX spoofing - Not married to “line of death” - but what is paintable by the origin, especially a URL bar, is a security UX question.
            * Password managers - would expect this to be partitioned by the top-level site. But if you start using this for many:1 cases, then you’re teaching users the wrong thing. 
            * Maybe we need some sort of iframe hardening, clickjacking protections. Send keystrokes to parents, etc. Don’t know that we have the expertise on this call for this. Maybe the ??? call that’s happening.
        * Ari: To respond to the UX spoofing concern, the mocks might be deceiving. The hope is that it is a separate window in the windowing system. Should have the same properties as a popup. Just that you can’t go back to the opener tab until you’ve dismissed the popin.
            * Ben; Okay, I didn’t catch that from the mock.
            * Johann: I think I have seen sites mock the popin behavior; definitely a thing. I don’t think the implementation of this would add any attack surface. We could improve things with this proposal though.
        * Ben Kelly: Over time, it would be good to brainstorm a good top-level partitioned contexts (see [https://github.com/privacycg/nav-tracking-mitigations/issues/42](https://github.com/privacycg/nav-tracking-mitigations/issues/42)). For nav tracking, we’ve allowed sites to have a partitioned redirect flow, but not good UX. I think there is a long-tail opportunity here of leveraging this work for other use cases/scenarios in the future.
            * Johann: Problem is that if you redirect in a partitioned tab; and then have the same origin open in a regular tab; there might be user confusion.
            * Ben: Not a slam dunk, but opportunity that we can figure out.
        * Ari: The other issue that came up was that if you’re running a popular aggregator, you could partition all the outbound links to be partitioned by the opener. So now you could do tracking across that partition.
        * BenV: That’s actually a really good privacy point. UI difference - if you drag a popin out of a window (and eventually navigates within that), wouldn’t it need a top-level UX anyway?
            * Ari: One of the notes for UX requirements, this would not be allowed. Haven’t figured out all of the UX issues, but in general, really trying to restrict.
    * Covered Popin Tradeoffs slide from deck.
        * Not a good solution for federated login, but could take away from adoption of other APIs.
* Ari: Question from me is whether there is any objection to incubating this within PrivacyCG?
    * Erik: Will need to be async over GitHub issue. Need to hear from other implementers if they want to tackle this space.

# Attendees
* Erik Anderson (Microsoft Edge)
* Ben Kelly (Google Chrome)
* Walter Rudametkin (URennes / Inria)
* Ari Chivukula (Google Chrome)
* Kaustubha Govind (Google Chrome) 
* Johann Hofmann (Google Chrome)
* Christian Biesinger (Google Chrome)
* Benjamin VanderSloot (Mozilla)
* Chris Fredrickson (Google Chrome)
* Yi Gu (Google Chrome)
* Jeffrey Yasskin (Google Chrome)
* Nicolás Peña Moreno (Google Chrome)
* Christine Runnegar (PING co-chair)
* Nick Doty (CDT)
* Paul Zuehlcke (Mozilla)
* Mike O’Neill (baycloud)
* Shannon Roddy (self, UC Berkeley)
* Wendy Seltzer
* George Fletcher (Capital One)
* Kosei Akama (Keio University)
