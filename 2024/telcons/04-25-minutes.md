# 2024-04-25 Privacy CG meeting
* Chair: Erik
* Scribe: Nick

# Agenda
* [Support FedCM-based grants #196](https://github.com/privacycg/storage-access/issues/196)
* Any other business

# Notes
### [Support FedCM-based grants #196](https://github.com/privacycg/storage-access/issues/196)
* Johann: proposal primarily from Chrome, discussion with Mozilla. Proposing new work item for privacycg, integrating FedCM and the Storage Access API. 
    * FedCM is a browser API that allows mediating federated log-in, access to a cross-site credential, including showing the user a login-oriented permission prompt, after which an identity provider can pass a high-entropy token/identifier to the relying party.
    * User accepts breaking the privacy boundary between these two entities. FedCM is well-suited to a lot of log-in use cases around the web.
    * Some providers have indicated that it doesn’t work very well for additional functionality for the IdP on the RP website: token refresh in an iframe on the RP page; facilitated login and more complicated network requests when both IdP and RP are operated by the same entity. So what they want is requests with third-party cookies.
    * Storage Access API would provide that if the user approved the permission. But would require two prompts: a login and storage access.
    * Given that the user has already agreed to the same privacy-wise capability, don’t need to show the storage access prompt separately. Why don’t we auto-grant Storage Access when the user has just granted FedCM permission?
    * One distinct difference in the privacy model: FedCM prevents IDP-tracking across different relying parties around the web. In this case, the RP would have to separately indicate disallowing that tracking by the IdP. Propose tying this new functionality to the FedCM permission policy.N
    * [https://github.com/explainers-by-googlers/storage-access-for-fedcm](https://github.com/explainers-by-googlers/storage-access-for-fedcm)
    * Would love to incubate this in PrivacyCG.
    * Chrome is prototyping this right now.
* Brian May: what is the degradation if the additional access isn’t provided?
    * Johann: proposal is for no additional user prompt. Without this, IdP has to give guidance to the RP to pass on additional tokens.
    * Brian May: should the user get a different question – do you also want to grant storage access?
* Ben: this idea is worth working on. prompt from FedCM is clear, and already breaks the privacy wall. the user has opted in to the un-private state, so make it more ergonomic.
* Achim: how much effort is there for an IdP to leverage this API? would make it much easier to adopt these kind of APIs.
* Johann: navigator.credentials returns a signed cross-site identifier token, but IdP doesn’t get any unpartitioned storage. Erik: is this opening it broader than it needs to be? Johann: token is accessible in the top-level context and anywhere the Permission Policy allows it. might as well give the capability to centrally manage their cookies, good for logout. [scribe is missing some of the back and forth here]
* Johann: security implications as well as privacy implications of third-party cookie access, attacks like CSRF and clickjacking. Any storage access should have opt-in by the IdP/third party.
* Johann: there are complicated enough use cases that the IdP needs
* Nick: From the short description, it sounds like you’re saying, “the user’s already functionally agreed and so we should just make it if the IDP requests storage access that we just grant it.” The side-effect is that enables the IDP to track that user across different relying parties. Currently they could do that today if the RPs collude which certainly does happen some of the time. But I’m not clear if what you’re suggesting is if the RP has to opt into this or by default the IDPs will have an easier way to get storage access and tracking capability.
* Johann: Sorry if that wasn’t clear from the talk just now. I’m suggesting I think the former of what you’re saying; I want to keep the current model of what FedCM has where only collusion, or opt-in– collusion is difficult to define– but the RP needs to opt into it and that’s what I want to preserve. Previous proposal to “just grant access to them” but we thought about that and it doesn’t make sense because it doesn’t meet privacy property of FedCM that you’re describing. Clever way to restrict it, instead of just SAA permission policy, also tie the grant to the FedCM permissions policy. That works by the RP setting it explicitly to delegate responsibility to third party.
* Nick: Remaining question– whether we want to provide an ergonomic path to do that or if we want to say, “hey, third-party cookies and this kind of collusion even if you could have done it is not what we want to encourage.” Situation we could have is not just where IDP auto gets this capability. IDP could have documentation that says “copy this to enable login” and it could include the bit to do tracking. RPs say, “great, this works the way we want to, we get 3p cookie access.” Maybe not what we want the platform to do.
* Johann: Understand, but already the case. They’re giving script tags today. As soon as you can compel them to copy-paste stuff onto their sites and get users to click the “cross the boundary” prompt, then it’s game over and we lost. We can’t prevent this scheme from happening broadly, think we’ve gotten enough feedback that this is something people need. We can push FedCM adoption, better model, want to use for login, we can push adoption by making it broadly more useful for developers and this is part of that. That’s why I say I understand where you’re coming from, but we’re not violating privacy boundaries and should make it more useful for people.
* Ben: one point of concern is requestStorageAccessFor – a hypothetical feature. and would create a situation where you don’t have the explicit permission policy step.
* Johann: storage access headers would also have a similar sort of problem. it does sound useful to enable some top-level storage access. is there a clever way to make an opt-in mechanism for the RP?
* Brian: there is a straightforward way and a more involved way. if we make it easier to do it, that might encourage people to do it, or make it harder to detect when people do it. making it more involved might discourage it or make it more obvious.
* Johann: showing a prompt to the user is very visible. more overt might be two permission prompts, but for a motivated attacker, it is trivial to pass the token and then set a partitioned cookie but for the positive security team it isn’t trivial.
* Brian: a lot of people on the Web may be interested in re-gaining access to a cross-site identity, and might become commonplace.
* Johann: a simple shortcut for the attacker if they convince the RP to send along the token. same visibility – prompt to the user but no additional warning. researcher can trace which called requestStorageAccess. Ben: the attack (colluding RP) would be server-side, so harder to observe in the browser.

Erik: relevant to the group. establish sufficient implementer interest – can figure that out offline.

# Attendees
* Joey Stanford (Platform.sh)
* Erik Anderson (Microsoft Edge)
* Nick Doty (CDT)
* Brian May (Dstillery)
* Benjamin VanderSloot (Mozilla)
* Johann Hofmann (Google Chrome)
* Christian Biesinger (Google Chrome)
* Wendy Seltzer (Tucows)
* Walter Rudametkin (University of Rennes / Inria)
* Heather Flanagan (Spherical Cow Consulting)
* Nicolás Peña Moreno (Google Chrome)
* Achim Schlosser (Bertelsmann)
* Chris Fredrickson (Google Chrome)
* Tim Cappalli (Okta)
* Shannon Roddy (individual/UC Berkeley)
* George Fletcher (Capital One)
* Brian Campbell
* Michael Knowles
* Miguel Morales
* Ruby Rose
* Yi Gu
* Zachary Tan
* Arthur Coleman (IDPrivacy/ThinkMedium)
