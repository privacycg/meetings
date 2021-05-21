### Privacy CG Virtual Face to Face, May 19-20, 2021

*   Chair: Tess
*   Scribe: Nick Doty

## [Agenda](https://github.com/privacycg/meetings/tree/main/2021/05-virtual)

**Day 2**

*   8:00 - 8:45 [First-Party Sets](https://github.com/privacycg/first-party-sets)
    *   Use-cases and specific applications (SameParty cookies, cookie partitioning, etc.). 
*   ~~8:45 - 9:00~~ 9:00 - 9:15 Break
*   ~~9:00 - 9:30~~ 9:15 - 9:45  [RSA Blind Signatures](https://datatracker.ietf.org/doc/draft-wood-cfrg-rsa-blind-signatures/) Presentation
*   ~~9:30 - 10:15~~ 9:45 - 10:30  [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    *   [Fraud prevention](https://github.com/privacycg/private-click-measurement/issues/41) 
    *   Advertiser control over reports going to publishers ([https://github.com/privacycg/private-click-measurement/issues/65](https://github.com/privacycg/private-click-measurement/issues/65))
    *   JS API vs same-site pixel API ([https://github.com/privacycg/private-click-measurement/issues/71](https://github.com/privacycg/private-click-measurement/issues/71)) 
*   ~~10:15 - 10:45~~ 10:30 - 10:50 Break
*   10:50 - 11:20 Work Item Status
    *   Status of various work items and where we are with each. Which work items look like they are converging enough to move to standards bodies?
*   11:20 - 12:00 Closing Remarks

## Notes

### [First-Party Sets](https://github.com/privacycg/first-party-sets) (Kaustubha [Google], Scribe: Wendy Seltzer)

*   Kaustubha: [[slides](https://docs.google.com/presentation/d/1hMEjM-SqKRo99mhh28rLqxksD6-47mczfPFrJ2WANFI/edit?usp=sharing)]
*   Use-cases and specific applications. To address questions we’ve gotten in slack, “can you explain what this will be used for?”. General discussion, not a specific issue.
*   Principles. Chrome’s privacy model. Pointing to michaelkleber/privacy-model
    *   Identity partitioned by first party site. No entity should be able to cross-link a user, say from NY Times is the same user as one browsing Washington Post
    *   Third parties can be allowed access to a first-party identity, as delegated by the first party
    *   Any cross-site information exchanged between first parties should be small amounts (differential privacy, aggregation, etc.)
*   FPS tries to define what we mean by “First Party Site” in principle 1. 
*   Today, nothing better than registrable domain, so that’s what lots of browsers are using to block cross-domain cookies. Is domain really the right answer?
*   [slide: a look at other privacy models] 
    *   Exchange of information about the user exchanged across parties.
    *   Common language or terminology used across tracking policies
*   [slide: a look at other platforms]
    *   iOS app store re tracking. “Linking …  other companies’ apps, websites, or offline properties”
    *   IDFV, “same vendor”
*   [Q&A]
*   James Rosewell [51 degrees]: great to see ref to other data sources. Yesterday, [UK regulators produced a statement](https://github.com/privacycg/first-party-sets/issues/41) I’ll put into the minutes. I’d like to see that put into the discussion re competition and privacy. 
*   Kaustubha: Thanks, haven’t yet gotten to digest all of it. FPS shouldn’t prevent orgs from fulfilling their legal obligations
*   James: or hinder other orgs in fulfilling theirs
*   Kaustubha: FPS isn’t in itself limiting other information transfer
*   Michael Lysak [Carbon]: Question about browser itself engaging in tracking activities for users cross-site. Is there a plan to deal with sync? What will prevent browsers from doing cross-site identification of user if the first party doesn’t consent?
*   Erik A [Microsoft,Chair]: is that a question about FPS? Typically browser logic isn’t in scope of web standards
*   Michael L: concerned if browsers have capabilities others don’t. References presenter’s earlier point that no entity should do this, and concerns that browsers can.
*   Robin [NY Times]: suggest we discuss that in another conversation
*   Erik A: consider PING privacy threat model for that discussion
*   Erik Taubeneck [facebook]: FPS is trying to take multiple sites and map to one entity; opposite problem in [PSL](https://publicsuffix.org/) right now, multiple entities as subdomains of one domain; PCM and conversion measurement  issue, as they all ping to one ETLD+1. Could FPS consider that use case for splitting domain usage? 
*   Kaustubha: aware of that pressure, maybe discuss at end.
*   John-Marcus Phillips: appreciate the framing. How does first principle of chrome’s model align with iOS definition of tracking?
*   Erik A: think this wasn’t claiming complete alignment, but identifying similar themes
*   Kaustubha: agree
*   James Rosewell: Domain names are a poor proxy for data processors and data controllers.  Consider revisiting that equation. 
*   Kaustubha: [slide: use cases]
    *   Tried to categorize use cases: App domains, could be a single application, e.g. outlook.com, live.com. Single app that uses multiple domains.
    *   Brand domains. Single org with multiple brands
    *   Country-specific domains to enable localization
    *   Common eTLD, e.g. gov.uk with many gov agencies as subdomains. Using the PSL to create security boundaries for registrable domains. But some services like consent management, with shared services across subdomains. 
    *   Service domains. Users don't directly interact with. Some use cases might be solved by partitioned cookies, but also serve multiple app domains, brand domains.
    *   Sandbox domains, where sites want to separate into mult domains, e.g. for user-uploaded content. E.g. codepen.io, cdpn.io. Don’t want untrusted content to access cookies, credentials.
*   [Q&A]
*   Brian May [dstillery]: using domains to try to manage disparate resources not necessarily aligned with domains. FPS may be extending a model that isn’t working well. Consider, is there another mechanism we can use to distinguish?
*   Kaustubha: domain is the best we have today. Trying to talk about the cases where that isn’t sufficient.
*   Chris Needham [BBC]: from BBC, our use case, one website with two domains: bbc.com and bbc.co.uk. We want to offer a single, consistent, signed-in experience across those two domains, i.e. synchronize cookie state between the two, it’s becoming more and more difficult. It’s all within one org but happens to be between 2 domains. E.g. bounce-tracking prevention may block. Set and refresh cookies across both domains; OIDC sign-in flow between the two. We’re currently redirecting between the sites. Can we continue to use our existing OIDC flow or do we need to move to WebID or something similar?
*   Kaustubha: could add that use case
*   [slide: Proposed applications]
    *   Intention is to be used with new tracking prevention features
    *   Same-party cookies, for cookies marked with this attribute, 
    *   CHIPS. Keying partitioned cookies from top-level. 
    *   WebID long-term thinking, the RP tracking threat, directed identifiers, sharded by site. Could be keyed by FPS instead of registrable domain. E.g to unify payment behind uber, ubereats. 
    *   Privacy budget anti-fingerprinting, FPS gives us an entity across which to apply entropy limits. Important that entropy limits not be limited to registrable domain. 
    *   From yesterday’s discussion, consideration whether FPS could help in generating blocklists/allowlists for bounce tracking mitigation. 
*   I had another slide on PSL, can skip for now. 
*   [slide: prior art][slide: reliance on PSL]
*   [Q&A]
*   AnneVK: 2 points, one about comparison of web with curated platforms, I don’t think that applies, there’s no contractual relationship as there is with an app store owner. I’d hate to move the web to curated platform. 
*   Struck by lack of UI and attention to what the user might want. As a user, if I visit foo.com, I’d expect bar.com visited later not to know about prior visit.
*   Kaustubha: for this conversation, scoping to use cases and applications to answer that question. Certainly want to talk about UX. 
*   James Rosewall: key word “sets”. Domains part of the same legal entity, third-party providers under contract, creates a set under privacy policy. Instead of domain names, look at legal relations. Is there an appetite from browsers, TAG, to look at that possibility? Addressing these problems might become simpler if there is an appetite to have such a conversation.
*   Jason Nutter: Quick question about TAG review, that found FPS harmful. Have you made any changes in response? 
*   Kaustubha: We’ve met with the TAG and are working through that feedback. Some of their concerns were that it would be used as security boundary. We’re talking about privacy. Responding by being much more explicit on scope, not impinging on same-origin policy. Re policy piece, acceptance process. Re Anne’s concern. Could this be done be purely technically? There’s room for people to raise that concern. \
[See TAG minutes from these sessions here: [https://github.com/w3ctag/meetings/blob/gh-pages/2021/telcons/04-05-minutes.md](https://github.com/w3ctag/meetings/blob/gh-pages/2021/telcons/04-05-minutes.md) and here [https://github.com/w3ctag/meetings/blob/gh-pages/2021/telcons/04-12-miunutes.md](https://github.com/w3ctag/meetings/blob/gh-pages/2021/telcons/04-12-miunutes.md).]
*   Michael L: what’s the correct forum to raise cross-proposal issues? E.g. combination of WebID and FPS could address BBC issues; what about detrimental combinations? 
*   Erik A: reasonable to comment on “please be aware”, but might get the response, “we’re aware, and still want to proceed”
*   John Wilander: think it would be helpful for FPS to distinguish 2 potential buckets of use cases: relaxing policy, because 2 domains relate to same entity; unblocking from more restrictions - the bounce tracking case. If we had a trustworthy source for knowing domains were part of same org, could unblock from shipping bounce-tracking protections. 
*   Kaustubha: what do you think the delta is? 
*   JohnW: [scribe needs a break]

### [RSA Blind Signatures](https://datatracker.ietf.org/doc/draft-wood-cfrg-rsa-blind-signatures/) Presentation (Chris Wood and Frederic Jacobs, Scribe: Sean Harrison)

*   Link to presentation - [https://lists.w3.org/Archives/Public/www-archive/2021May/att-0001/W3C-BlindSignaturesForPCMFraudPrevention.pdf](https://lists.w3.org/Archives/Public/www-archive/2021May/att-0001/W3C-BlindSignaturesForPCMFraudPrevention.pdf)
*   [Frederic Jacobs] - Proposed flow from Jonathan on github for fraud prevention in PCM, we have user, user visit websites, click source returns attributes from user clicking on links. Browser fetches token public key, and client side runs blinding operation based on public key retrieved. Post the blinded message to endpoint doing the signing, blinded message is signed by the private key of the clicksource. Additional validation can be done by clicksource ad link. After this the click source returns the blind signature. Client does final step which yields unblinded signature, if signature is correct, sotres sig. Triggering event happens and client fetches key and validates key matches one from click source. The attribution report is sent to the click nonce and the token sig. Clients are able to verify uniqueness of clients nonce. Click destination is able to verify based on well known source of private key.
    *   Req for cryptographic scheme
        *   Unlinkable between the token and source nonce
        *   Public verifiability for both click source and destination to verify
        *   Resistance against “one-more-forger” attacks
    *   Attack considerations
        *   Efficiency
            *   Computation, reasonable
            *   Bandwidth efficient
        *   Ease of implementation
        *   Post-Quantum sec
        *   Cryptographic scheme to be versioned
    *   RSABSSA
        *   Publicly verifiable RSA-PSS sigs
            *   Widespread library support
            *   Resistance to one-more-forgers
        *   Unlinkability against classical and quantum attackers
            *   Forgery by quantum possible
        *   Straightforward imp using current libs supporting RSA
    *   Blind RSA protocol
        *   Outputs blind message and the inverse for the end blinding
        *   Blind message is sent that takes the private key and the blinded message and produces the blind signature
        *   Client can do the final operation with the public key and plaintext message, blind message, and inverse for the unblinding with a valid signature
            *   Alleviates some tracking concerns
    *   Alternatives considered
        *   ECVOPRFs
            *   More efficient with smaller keys, but not publicly verifiable
        *   Blind Schnorr Sig
            *   Smaller keys more efficient, but 3 messages and recent ROS attacks (not much peer review yet)
        *   Blind BLS
            *   Smaller key and more efficient, but signing and verification are expensive without widely supported pairing support
        *   Abe
            *   Polynomial concurrent security and seems unaffected by ROS attacks, but needs three messages with larger sigs
    *   This concludes presentation chris is on call who co-authored standard
*   [Erik Taubeneck] This is slightly different than what is described in issue 41. There is a flow missing from the presentation from 41.
*   [] what are the bugs?
*   [Erik Taubeneck] described it on 81, what you called nonce, is the source token doesn’t get generated earlier, mostly clarification
*   [John Wilander] I believe you [erik] are correct, that once something goes in the explainer it should be right
*   [Erik Taubeneck] want to make sure we have it written down correctly, what is called a nonce is doing the work of a csrf token, so we should call it a csrf token for clarity
*   [John Wilander] don’t think it is only a ccerf token, it is providing exact context of the click, this is the exact context at click time
*   [Erik Taubeneck] isn’t that the same way you would you a csrf token
*   [John Wilander] they are closely related, I think the csrf is more open ended, you can produce them for everything you do
*   [Erik Taubeneck] context token, I think the nonce is a loaded term
*   [John Wilander] you could have a csrf token for the loaded page, it depends on the click source, supposed to be unique per click.
*   [Andrew Knox] thank you, facebook, my concern here is around the public viability and the fraud motives. I would content the destination is more critical to sign, depends on what sort of fraud is the main concern. The fraud is happening against the flow of money. If everyone is a bad actor, ad frauds middleware, everyone frauds person paying them. Where you are singing source, this doesn't provide meaningful fraud prevention, publisher is trying to take credit for purchases that didn’t happen. The publisher is the one defrauding, not being defrauded. The protection being provided is not against the click. The way that it is written publisher can generate clicks, no sig on dest, generate reports off the cuff generate clicks say are linked to purchases when they didn’t happen.
*   [John Wilander] we are going into the next issue on the list, more broadly about fraud prevention, we ack that the destination side should sign. PCM original design the report only goes to the click source not the advertiser, would like to but not atm. 2 years ago a lot of merchants are not able to change quickly (even in years). We were asked so that no changes were needed merchant side. That is why it is leaning on click source, funded by advertising. Opening up more possibilities for destination to protect against fraud. We are very interested and going to go that route, but why we started there. This exact design was presented to that group, the worry was that was nothing for the legitimate click, send a curl to click source with fraud data. Express concern from click source, we need to know this is a trustworthy source, click source sig solves this. If we assume the click source isn’t defrauding, it started trustworthy
*   [Andrew Knox] it protects against random trolling. It does not protect against fraud from party with highest incentive to commit fraud
*   [John Wilander] if you go back to original assumptions about merchant update on crypto signing of tokens. Click sources are far more incentivised
*   [Andrew Knox] you are correct in the assessment of speed. But if they are seeing massive support fraud they will update. There is no prevention avenue in this scheme. It is a very challenging thing. Because this doesn’t protect against the most common form of fraud there isn’t much benefit
*   [tanvi] talk about the crypto in particular, redirect. Let’s start with the crypto
*   [chris] not crypto related
*   [tanvi] any crypto
*   [Andrew Knox] one comment about the multiround sigs, jason/jacob number of rounds based on bandwidth issues. There are other issues around other types of concurrency issues, but does provide a richer suite of sig schemes. Could someone talk about the req for number of rounds, is it really just bandwidth or privacy or etc?
*   [Frederic] I can talk about the concern, there are some concurrency concerns with the blind schnorr sigs, but we are looking at proposals that address the concurrency issues, but there is nothing that we’ve reviewed that is ready for production on a short timeframe meeting these requirements. For now it would add some round trips that would add delay, but not blocking on user action. Some server considerations on servers needing to keep state in case the user comes off, affects load projections
*   [Andrew Knox]the main reason I ask about the blinding proposal blinding to the bits, that’s attractive with a partial blind sig, then the fraudster can’t change the bits. Came up with a dumb solution that binds to those bits so that different destinatino values can mean very different things. If you can change a sign up to a purchase that is attractive fraud
*   [Frederic] it is something that can be done with blinded rsa, but it has the problem of non-deterministic performance, because adding the keys to the metadata adds something to the exponent that can affect the performance. RSA blind sig scheme allows this sort of extension
*   [Erik Taubeneck] one thing that has been discussed, might need own issue, is key rotation. We are checking the key is the same, but using previous key and current key could be a tracking vector, we don’t want the same key forever
*   [John Wilander] just talked about this, in the slides, get public keys plural, current imp is get public key but open issue to move to signing key windows. How is this gonna work, 7 day click window, need that to keep working, actually 9 days. You have the fact this needs to be publicly verifiable, if the cycling of keys is too fast then how do you verify, public dept. Should we specify this, first the source later the destination. It should suffice to have the two keys, I used to use this one and now I use this one. Need to handle revoked keys. On the tail end of that, we are interested in looking into multi conversion. Similar to google conversion reporting api. Do one conversion and report another one up to I think 30 days later. Need to now handle the keys over a longer period of time and can still pull from server
*   [Erik Taubeneck] sounds great, glad to see we are thinking about these things.
*   [Chris Wood] just wanted to follow up on 2 things with multi round concerns and operation concerns. State management on the server is tricky, harder than multiple round trips and size. The security of some of these schemes relying on multiple keys will be hard to implement for some implementers. Second, it’s desirable to have the public metadata bound to the report, I have concerns that the report can’t be verified from the client side, they don’t know if it is unique to them, can this be used as a tracking vector? Taking a step back, a requirement coming from this being the public metadata verifiable or generated from client, I don’t see how we mitigate tracking vectors, outside of limiting the number of bits
*   [Andrew Knox] public part is the part the client already knows, part everyone agrees on is unblinded.

### [Private Click Measurement](https://github.com/privacycg/private-click-measurement) (John Wilander, Scribe: Kaustubha)

John: (Context setting) Opening up the broader conversation. Thank you for the RSA conversation. For fraud prevention, I’d like to start with the smaller. A year ago, in the F2F we proposed this idea which is now implemented in WebKit. We will fix the flaws pointed out by Erik in v2 of PCM. But there are some small things we could fix quickly:

*   We could rename the nonce in requests
*   Add a “scheme” in the payload to capture the crypto scheme being used
*   Be more precise on the naming of this. Fraud comes in many flavors. Once this is done, there doesn’t mean other forms of fraud come up in the future. Need help coming up with more precise wording. Maybe “click fraud”?

Larger future things:

*   Destination signing. Especially, assuming destination doesn’t want to use 3p JS. We could use the pixel API, but could be tricky. FB says they’d like to tie this to “sites”.
*   Public keys, windows, having a set of tokens be signed because you could have multiple conversions per click.

#### [#41 Fraud prevention](https://github.com/privacycg/private-click-measurement/issues/41)

*   Chris Wood: Going back to the destination signing. It seems like the threat model isn’t well articulated, or an agreement on it. It may be useful as we think through future versions, identify the threat model and type of fraud to be prevented.
    *   John: Valid. Has been identified in the issue as well. All for working on that and including it in the explainer/spec. Important to recognize that the way it is has evolved, there is an extra requirement that the merchant shouldn’t have to change anything. That has made us focus on the click source.
    *   Chris: Not saying what we have is wrong, but just feedback for the future.
*   AramZA: Click Fraud is probably what you want to use, that’s what this style of fraud is called. Sources of fraud: publisher fraud, but could happen at any step of the adtech transaction. Landing page is also incentivized to have some level of fraud “arbitrage fraud”, where they’re looking to buy users. Ad exchanges have their own purchasing/selling of “clicks”, as well as various third-party systems. I imagine publishers might be interested in them as well. But possibility for “click fraud” to occur on both sides of transaction.
*   David Van Cleve: Ben on GitHub has a high-level qualitative discussion . Want to add more weight to earlier discussion about partial blinding + click destination designing - both of these seem like valuable improvements. Separately, when we think about how the click source producing signatures, heard an earlier mention of only handing them out to genuine click. People who provide anti-abuse solution go to great lengths to avoid oracles on what constitutes genuineness. Might be important to account for that: if you have to provide an oracle, what’s the impact? On the other hand, if you have to always hand out signatures to known-”bad” impressions, to avoid providing an oracle, what’s the impact?
    *   John: Click source doesn’t have to authenticate the whole thing. The way it’s designed, the browser will reach out to the click source after the click+nav happens with a context token. At that point, the browser assumes the click has happened. Intention is that if anything existing accounts for it… still applies ???
*   Michael Lysak: Part of my concern on fraud involves issue #65, but I also have questions about #41. (Chair: Yes, let’s start with 41). Click tracking for pubs is interesting in today’s environment. Every pub that I’ve spoken to is very concerned about it. CompanyA & CompanyB might have some business relationships. And CompanyB with CompanyC. CompanyA could potentially pay for bots that visit CompanyB, click around to provide CompanyC. B and C could have bot detection and build in some buffers for bots. Company C might say that bots were involved, but B might be agree. The concept that signatures being optional may not be feasible, and may now need to part of the contract. Pubs need the click, and whether or not it is fraudulent. Sometimes, B,C and maybe even A talk about these numbers. Sometimes there are server failures. It’s important to have every single click, both parties on both sides need to have it and be able to determine if the click was legitimate or not.
    *   John: Not clear if you are proposing a change to PCM, or that destination signature isn’t good, doesn’t solve the problem, or we need something new.
        *   Michael: Was trying to restate the usecase in the context of what Chris & others brought up
        *   John: Let’s start an issue on PCM Fraud Threat Model, and we arrive on something we agree on, we can add it to explainer.
        *   Michael: Should accidental fraud (e.g. server goes down; happens relatively commonly) be separate or connected?
            *   John: Keep separate until we decide that they’re the same. Can merge issues.
*   Charlie Harrison: 
    *   Naming comment: Think about what is the capability that this system gives. One potential name is “authentication”, as in a private auth system that has some notion of verification of click. 
    *   Brought up previously, but saying it again. Fraud detection is within the goals of the Attribution Reporting API from Chrome (previously Conversion Measurement), which involves adding noise. Some of these systems, like partially blind signatures make that really hard. Either we give you an oracle saying that the browser added noise. Or we break functionality by adding noise.
        *   Tension: These systems give you a noiseless system. Or a binary system. In other sophisticated systems, ???
*   Andrew Knox:  Agree with Aram that fraud comes from both sides. People can claim some stuff is fraud when it isn’t, so you don't have to pay out. This goes to the point of it not being optional. I really like the point about the valid oracle because it increases the problem of the fraudsters of coming up with a way to frustrate the fraud detection system (effort can go from hours -> months). One of the top things that can affect things in real  life. Destination signing could be a stopper on the whole fraud ecosystem.
*   Mark Lee: Secondary problem of going down the chain. OpenRTB as a supply chain example - from client to click src to advertiser, everyone else in the exchange. With the metadata being signed by each player in the ecosystem. Even though it doesn’t show the initiator, show the generalized src of the information.
    *   John: There’s nothing like that today. Only thing implement is the click src first-party to sign, and then same thing for destination. Haven’t thought about additional signatures/proofs. Definitely interested, if you have ideas on how this could work, please file an issue. Have not captured the oracle in the repo discussion. I think the attribution reporting api has a way to sign so that a fraudster can sign but not ??? 
        *   Charlie: Yes, similar idea also in Trust Tokens.

#### [#65 Advertiser control over reports going to publishers](https://github.com/privacycg/private-click-measurement/issues/65)

*   John: We have implemented, but not shipping: attribution reports go both to click source and destination with same information (public keys). An advertiser reached out and ask if the click src could “not be told” about their data since it could have proprietary info and potentially exposed to a competitor. They said: please send it only to me, don’t want it to leak to competitors. The other side is, when I think of pubs, I am thinking of news, etc. who are also not with an ad tracking business or other services. That notion of publisher doesn’t create that tension. The notion that the destination …. But of course, pubs want to know how well they’re driving advertising business. There are pubs who also want to optimize ad placement based on how much revenue they are generating. There are adtech folks saying that don’t want to share data, but there are also folks saying that both sides need it. We could say: Maybe you can noisier data, have smaller windows, etc. if you want access to this info.
    *   Michael Lysak: Pubs I spoke to were most worried about this issue (purposes you mentioned). Right now, pubs have all the data about every single time someone click on an ad on their site. Once you get to the landing page, pub already has 100% info on what occurred. But if the attribution is an event on the landing page, it gets tricky because the landing page has to fire an event back. It depends on the situation, whether it’s typical or not. This is very worrisome, losing the info they already have is very worrisome, as well as the info they could theoretically have. If there is a power imbalance, pubs tend to lose.
        *   John: Thank you. Has been brought up others in this group before. What I’m hearing is that there would be a risk that the pubs would lose info that the user clicked. They would not lose that information since it happened on their site. What we’re talking about is the conversion on the destination (e.g. click happened on a sale)
            *   MichaelL: Thank you, wasn’t aware of it. In that case, nothing else to say.
        *   John: , Aram, and others does this change your feedback?
            *   Aram: No
    *   Aram: Conversion tracking is a methodology by which people are billed. Therefore everyone in the chain should have equal knowledge. People use the term “kill zone”, if you have an email product and they want to advertise with say Google, who has a competitor product; they might be concerned about giving them data about prospects … If you want to market with someone… maybe taking away conversion tracking is one way to do this. An active FBI investigation using buyers with clawback techniques… would require payback from pubs. That is a power balance that never falls on the side of pubs. As long as there is an imbalance, there is potential for fraud. It has to be all or nothing. Either both sides get it, or none. In the case of the advertiser is concerned, they should turn off conversion tracking, or not be advertising there. Part of thing we’re not saying is how it can change how advertising on the web works. Another real life situation: NYT runs subscription ads that can show up on WaPo. Both our ads teams would have this concern, and I think both would block those ads. So the options are to either block those ads, or turn off conversion tracking. Imbalance of reporting (whether accurate or not) will cause fraud.
    *   Kris: This is heavily oriented towards paid media. It is the idea that there is a pub showing the ads.. But there is also earned media and owned media that are trying to do conversion tracking. Web mail tracking (advertiser is sending an email), might be links to something like Amazon. In such case, the advertiser has to go to the source/destination get it. They probably don’t want conversion report sent to Google/GMail, because Google isn’t sending them advertising space. The way this is done today through bounce tracking. If PCM could be extended to support those use cases, or change the destination to be different types of channels, could be useful.
        *   John: We do have an issue for this use case, please chime in
        *   Kris: Can also happen on social media.
    *   Robin Berjon: Wanted to say exactly what Aram said.

#### [#71 JS API vs same-site pixel API](https://github.com/privacycg/private-click-measurement/issues/71)

*   Didn’t get to this topic.

### Work Item Status (Tess, Scribe: Nick Doty)

*   Tess: Status of various work items and where we are with each. Which work items look like they are converging enough to move to standards bodies?
    *   Previous items covered the current status. Nobody being forced out of the nest. But want to avoid lingering in incubation indefinitely without a clear path or goal.
    *   Client-side Storage Partitioning, more coordination than spec work; not expected to have a specific standards-track deliverable, but lots of pull requests and issues in others. When are we done?
        *   Anne: we have an overview and some partial standardization done. Local Storage, broadcast channel. How does it tie in with the Storage Access API? Lots of overlap with Storage Access.
    *   First-Party Sets: maturity of the document and timeframe to move into standardization?
        *   Kaustubha: open issues but may need a more focused discussion on it as a policy rather than a specific technical spec.
        *   Tess: worth a dedicated call. Happy to host, may include inviting people outside privacycg. Expected destination?
        *   Kaustubha: does fit within the charter of WebAppSec, cookies and site data accessed across sites. Can email WebAppSec chairs to confirm.
        *   Tess: chairs will follow-up on scheduling a meeting.
        *   Kaustubha: what constitutes whether it’s ready to move on?
        *   Tess: up to the editors to determine ready for migration. Chairs will work with editors and the destination chairs. That’s the point of incubation!
        *   Wendy: if it’s in-scope for an existing WG, talk with that WG’s chairs about it, and WGs like to hear about work items with dedicated editors already. If it’s not in-scope, could require chartering or re-chartering.
        *    W3C team is here ready to help.
    *   IsLoggedIn: WebAppSec does make sense as a potential destination. When would you be ready to move? Possible solution-space may be pretty open-ended still. What issues are blocking?
        *   Melanie: fairly early on. Agree WebAppSec would be the natural home. Try to resolve on core use cases and fitting the API to them.
        *   JohnW: furthest from ready to move. Do we need implementations prior to moving to standardization?
        *   Tess: implementer interest is relevant for adopting a work item, in order to have potential wide adoption on the Web. Don’t need multiple implementations prior to migration to standardization, but will depend on the venue’s standards for accepting it. PRs to HTML, for example, requires test coverage and multiple implementers.
    *   Private Click Measurement: fraud prevention seems to have been the big question. Other issues needed before migration?
        *   JohnW: still expect a few more iterations, including alignment with attribution reporting API proposal from Google. Right people with issues are showing up at PrivacyCG, so happy to continue here.
        *   Tess: increasing the size of the common surface will make it easier to accept PRs to HTML and shrink the external specs required. Assumed HTML spec.
        *   JohnW: would prefer dedicated WG on privacy-preserving [] technologies. Permanent home for iteration, rather than getting lost in HTML discussion.
        *   Tess: W3C sounds like a willing home
    *   Storage Access API: furthest along work item. Work seems pretty close to ready to move.
        *   Johann: somewhat blocking is treatment of different storage types. Or just a decision about agnosticism or connection to cookies, etc.
        *   Tess: +1 on more of a decision. Timeline? Summer/fall?
        *   Johann: would have to come up with a proposal and maybe make a decision by summer.
        *   JohnW: iteration would continue. Potential alignment with Fenced Frames from Google?
        *   Tess: HTML and Storage as the main destinations.
        *   JohnW: should it be mentioned in IETF Cookie spec?
        *   Tess: should include HTTPWG, or maybe just WHATWG
        *   Anne: Fetch / HTML might handle that. Trying to help refactor the IETF spec as well.
    *   Thanks all for the feedback.
*   Wendy: if we wanted to start a Privacy Enhancing Technologies Working Group, that would require drafting a charter and reviewing by the Advisory Committee. Work in the privacycg would help with scoping. Privacycg could also be a useful home for the charter drafting process.
*   James Rosewell: would like a privacy baseline, don’t want to create a group that is pro-privacy but anti-choice. Lessons to be learned from Do Not Track experience (paper of interest: https://core.ac.uk/download/pdf/234109764.pdf). Would rather have a constructive discussion rather than dismissing industries. Rather than existing WGs about to have charters renewed.
*   ErikT: Facebook would be supportive of a privacy WG, and would help with volunteering chairs.
*   Tess: having a common baseline is definitely useful for any group. PING as a group that helps with horizontal review on privacy questions, and could be a home for discussing conceptual framework. WG/charter would have to rely partly on PING for that. I’ve been reluctant about a privacy WG, because we should only have a WG with particular work to do, in order to scope and be concrete.
*   Aram: +1 would need more work items in process, but interested in a WG and would help with charter etc.
*   Wendell: +1 for a privacy technology WG because the technology is fun! Charter is unclear. Want to avoid activist axis [sp?]. WebAdv has split into interdiction and business continuity. Privacy technologies could work well as business continuity processes. A lot of other groups outside W3C, including trade groups and lobbying, some of which are anathema to ???.
*   James: reminder of TPAC session from 2020 - [TPAC 2020: Breakout schedule (w3.org)](https://www.w3.org/2020/10/TPAC/breakout-schedule.html#privacy-baseline) could effect a real change for business continuity and privacy. Could ask W3C Team to negotiate a baseline agreement.
*   Weiler: +1 for fun. Fine to have a WG with a single work item, because narrow scope can be successful (WebAuthn as an example). 
*   Tess: +1 for tightly scoped.
*   Michael Lysak: would like one place to discuss cross privacy proposal issues. Proposing that each group define privacy in their own way to help the charter discussion
*   Tess: sounds like there is enthusiasm for a privacy technology WG. Will talk with chairs and Team.

## Closing Remarks (Erik)

*   Logistical - Work item issues we didn’t get to that were agenda+f2f’ed - please mark them agenda+.
*   Scheduled for May 27th meeting - chairs will let you know early next week if it is on or cancelled.
*   Survey - [https://forms.office.com/r/2YiHPNqeaz](https://forms.office.com/r/2YiHPNqeaz) - Please take these extra minutes to respond. 
*   Thank you!!!

## Attendees

1. Al McLean (Carbon AI)
2. Allan Spiegel
3. Andrew Knox (Facebook)
4. Angela Ballard (CJ Affiliate)
5. Aram Zucker-Scharff (he/him; The Washington Post)
6. Arthur Edelstein (Mozilla)
7. Ashkan Soltani (GPC/Independent Consultant)
8. Anne van Kesteren
9. Ben Kelly
10. Benjamin Case (Facebook)
11. Brian May (dstillery)
12. Cezar
13. Charlie Harrison (Google)
14. Chetna Bindra
15. Chris Needham (BBC)
16. Christy Harris (Future of Privacy Forum)
17. Craig Spiezle
18. Daniel Appelquist (Samsung)
19. Daniel Smedley (Booking.com)
20. David Van Cleve (Google)
21.  (AirGrid)
22. Devin Rousso (Apple)
23. Don Marti (CafeMedia)
24. Eric Mwobobia (ARTICLE19) 
25. Ericka Wright (Intuit)
26. Erik Anderson (Microsoft)
27. Erik Tauubeneck (Facebook)
28. Frederic Jacobs (Apple)
29. Grant Nelson (TripleLift)
30. Heather Flanagan (Spherical Cow Consulting)
31. Hirsch Singhal (Microsoft) - Identity
32. Hitesh Lad (CJ Affiliate) 
33. Jack Frankland
34. Jade Kessler (Google)
35. James Hartig (Admiral)
36. James Rosewell (51Degrees) - left 1:25 back 2:45
37. Jason Kint (DCN)
38. Jason Nutter (Microsoft) - Azure Identity
39. Jeegar Shah (CJ Affiliate)
40. Joel Odom (Salesforce)
41. Johann Hofmann (Mozilla)
42. John Bryson
43. John Delaney (Google Chrome)
44. John Wilander (Apple WebKit)
45. John-Marcus Phillips (Nielsen)
46. Kate Cheney (Apple)
47. Kaustubha Govind (Google)
48. Kelda Anderson (Microsoft)
49. Kris Chapman (Salesforce)
50. Laszlo Gombos (Samsung)
51. Lee Graber (Tableau / Salesforce)
52. Lionel Basdevant (Criteo)
53. Liz Hurley (CJ Affiliate)
54. Lola Odelola (Samsung Internet)
55. Mallory Knodel (CDT)
56. Mark Lee (Carbon)
57. Mark Xue (Apple)
58. Max Gendler (The New York Times)
59. Melanie Richards (Microsoft)
60. Michael Kleber (Google Chrome)
61. Michael Lysak (Carbon)
62. Mike O’Neill (Baycloud)
63. Naïma Conton (Sirdata)
64. Newton (Magnite)
65. Nick Doty
66. Paul Bannister (CafeMedia)
67. Peter Lowe
68. Peter Saint-Andre (Mozilla)
69. Phil Eligio (EPC)
70. Robin Berjon (The New York Times)
71. Russell Stringham (Adobe)
72. Sam Weiler (W3C/MIT)
73. Sam Goto
74. Sean Harrison (Google)
75. Shaun Gilmore
76. Shigeki Ohtsu (Yahoo JAPAN)
77. Steven Valdez (Google)
78. Tanvi Vyas (Mozilla, co-chair)
79. Theodore Olsauskas-Warren (Google)
80. Tim Cappalli
81. Viraj Awati (Amazon)
82. Vishnu
83. Wendell Baker (Verizon Media)
84. Wendy Seltzer (W3C)
85. Anna Lysyanskaya (Brown, guest)
