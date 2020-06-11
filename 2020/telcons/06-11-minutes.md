# Privacy CG telecon, 11 June 2020

[Agenda link](https://github.com/privacycg/meetings/blob/master/2020/telcons/06-11-agenda.md)

* Chair: Tanvi
* Scribe: Anne

## Attendees
* Ben Savage (Facebook)
* Theresa O'Connor (Apple)
* Wendell Baker (Verizon Media)
* David Dabbs (Epsilon)
* George Fletcher (Verizon Media)
* Tanvi Vyas (Mozilla)
* Chris Needham (BBC)
* Hong Cai (BBC)
* Mike O'Neill (Baycloud)
* Max Gendler (NY Times)
* Sam Weiler (W3C/MIT)
* jsaraceno (Healthwise)
* Kris Chapman (Salesforce)
* Johann Hofmann (Mozilla)
* Przemyslaw Iwanczak (RTB House)
* Bartłomiej Romański (RTB House) 
* Andrzej Surkont (RTB House)
* Jeffrey Yasskin (Google Chrome)
* Shivan Sahib (Salesforce)
* Bill Densmore (ITEGA)
* Steven Englehardt (Mozilla)
* Anne van Kesteren (he/him; Mozilla)
* Lionel Basdevant (Criteo)
* Josh Karlin (Google)
* Paul Jensen (Google)
* Arthur Edelstein (Mozilla)
* Melanie Richards (Microsoft)
* Sebastian Zimmeck (Wesleyan University)
* Carlos Bracho (Axel Springer)
* Thomas Bergemann (Axel Springer)
* Jack Frankland
* Brad Lassey (Google)
* Peter Saint-Andre (Mozilla)
* Christine Runnegar (PING co-chair)
* Joel Odom (Salesforce)
* Scott Low (Microsoft)
* Mark Xue (Apple)
* Ian Truslove (White Ops)
* Kushal Dave (Scroll)
* Sam Tingleff (IAB Tech Lab)
* David Benjamin (Google)
* Vibha Sethi (Verizon Media)
* Erik Anderson (Microsoft)
* Don Marti
* Lisa LeVasseur (Me2B Alliance)
* Marshall Vale (Google)
* John Wilander (Apple)
* Kate Cheney (Apple)
* Chris Pedigo (DCN)
* Raj Belur (Amazon)
* Michael Kleber (Google)
* Wendy Seltzer (W3C)
* viraj
* Valentino Volonghi (NextRoll)
* Steven Valdez
* Shivani Sharma (Google)
* Shaun Gilmore (Apple)
* Sam Goto (Google)
* Salah
* Russell Stringham
* Russ Hamilton
* rbelur
* rbaudisc
* Marjin Kruisselbrink
* Kaustubha Govind (Google)
* Joel Meyer
* Jason Nutter (Microsoft)
* Jason Dorweiler
* George London
* Dan Veditz (Mozilla)
* Chris Wilson
* Arnaud Blanchard
* Aram Zucker-Scharff (The Washington Post)
* Andy Sharkey
* Mike Smith


## Face to Face Recap - Erik

Erik: \[Presents slides on recap of Privacy CG F2F.\]
...: 90 people. People wanted longer breaks.
...: CryptPad wasn't great. \[Doh. Still isn't.\]
...: Too many different collaboration tools.
...: Folks missed the hallway track.
...: Browser overview was useful. Scheduling went well.
...: Discussion didn't go deep enough.
...: Goals should be clearer during discussion.
...: Conversation was primarily browser folks. Would be good to hear from non-browsers.
...: If people have concrete ideas on how to run things better, please reach out. And thanks for filling out the survey!

Tanvi: TPAC is going to be virtual. We'll have another F2F TBD.

Lionel: We attended the F2F and we have two questions that we put on GitHub. 1) About advertising and privacy. Is it acceptable policy-wise to do personalized advertising in a privacy-preserving manner? 2) \[Scribe missed.\] Can data be shared with a third party?

Tess: Not clear if people are prepared to discuss this on the call. Please comment on GitHub:

* https://github.com/privacycg/admin/issues/15
* https://github.com/privacycg/admin/issues/16


## Specs to Working Groups

Dan Veditz: I'm the co-chair of the WebAppSec WG. If your specifications are more finished we could perhaps help get them done.
...: I think some proposals fit in our charter and might save some hassle in terms of IPR.
...: Rechartering is an option to allow for more proposals.

Tess: None of us enjoy rechartering. When does your current charter expire?

Sam: March 2021.

## Proposals:
### [The IsLoggedIn API](https://github.com/privacycg/proposals/issues/16) - John Wilander

John: We pitched this before TPAC last year and pitched it at the WebAppSec WG. Took a while before it moved to the webkit GitHub repository. Looking for interested browsers/stakeholders.
...: The purpose is to inform browsers that the user is logged in.
...: If we can make that work we think we could build a lot of things on top of that.
...: E.g., without knowing this it's hard to clear cookies/site data as we might log users out.

George: Identity architect at Verizon Media. Recognition is important. People don't authenticate each other, they recognize each other. E.g., if you go to a new device and login to Google, password might not be sufficient. Today, this is accomplished with cookies. It would be great if that kind of data could be identified by the system/site and allowed to flow for identity flows only.

John: That's up to the site.

George: The key here is that this security data is often managed by cookies/localStorage today and the life-time of this data is changing but it's needed to ensure the user gets a good experience. E.g., I would like to use SameSite=Strict, but cannot in a federated world. I think we need to think more holistically about this and include protocol design.

John: I'm not sure this fits with isLoggedIn, but I'm pretty flexible.

David Dabbs: Publishers need to store user decisions long term, like consent after permissions prompt responses.

John: If there was a dedicated API we could consider that.

David Dabbs: I see.

John: Cookies can be used to track users, so there would have to be some other capability to handle consent.

Chris Needham: What could the other features this enables be?

John: See the explainer. If we get good adoption, for instance, we could allow clearing of site data except for sites where you are logged in. We could also gate "powerful" features on this, such as showing list of sites you're logged into in the UI.

Sam W: https://github.com/WebKit/explainers/issues/44
...: Do you need to transmit the signal of being logged in?

John: We talked about that. At the heart of it the site decides logged in/out. I think that's why we want this signal. Cookies formatted in a specific way might convey this signal. And perhaps this needs to be part of the HTTP protocol.

Steven Englehardt: To what extent do you want to standardize the things this might be linked to?

John: My intention is to keep this as small as possible. The further use cases are exploration and motivation for doing this. We're open to standardize such things.

Johann Hofmann: The abuse criteria are rather handwavy. And at the same time, they might be too strict for some use cases. E.g., a chat site where you only need a username.

John: There's indeed a spectrum and what decision we make here has implications for the use cases it enables. And I'm open to discussion this full spectrum of possibilities.

David Benjamin: If the signal is weak (whatever the site said) everyone would set it. If the signal is stronger then we might as well use those heuristics (password manager, WebAuthn, etc.) directly.

John: Some of this was already addressed. UI signals - user continuously know this site claims that you are logged in.  incentive to not set the big; properly log them out.  all very relevant questions.  and maybe 5 months from now we decide we can’t solve the problem. Sad but okay.  could be outcome.  think it would be good to separate logged in state from other state.

Valentino Volonghi: The more anti-abuse requirements there are, the more likely sites will use a third-party service. And sites might end up requiring users to login more.

John: Today if you're redirected through a site they get to store data for years and that's problematic. And I would like to differentiate that from sites where users have meaningful interactions with.

### [First Party Sets](https://github.com/privacycg/proposals/issues/17) - Kaustubha Govind
 
Kaustubha: Proposal is in the WICG now. I also proposed it to this CG as you know.
...: Partitioning network state might be a use case. SameSite cookies is another. 
...: Preventing abuse was a big theme.
...: No concrete proposal for a policy yet, but we are working on it. Are happy to standardize it and would like it to be consistent across browsers.
...: Domains should exhibit common ownership, i.e., same organization.
...: Could even use origins and move away from Public Suffix List.
...: Could also have a unified privacy policy and surface that through browser UI.
...: A common user journey across the sites in a set. 
...: Show a reason why cookies are shared across a set.

Ben: facebook and instagram.  Not obvious things can point to there.  ex: launched product called shops; shopping experience across fb and isntagram.  interop use case as intending to build, does it count as a user journey?  common chats and shopping carts across them.

Kaustubha: Do you have state on those CDNs?

Ben: Not sure. I'm interested in the user journey aspect. We want to track things across Facebook and Instagram, but we don't direct users from one to the other.

Kaustubha: Journey there but maybe more disjointed then continuous, but good feedback.

Steven: Would like to reiterate concerns. Since this is such a big thing and it's not clear it's needed, use cases need to be a lot more detailed. Entities list does not count as precedent because it's about not asking sites to make changes, while adopting FPS would be a change.

Kaustubha: Would like to hear from John and Maciej on there thoughts on use cases.

Kaustubha: new york times and wirecutter.  Bsuiness get acquired; get sold.  are we teaching users to pay attention to subdomain instead of domain. chrome security does a lot of work to educate users around domain see in the url bar.  and not put in credentials for bankofamerica.evil.com.  motivation.  Also, talking to other companies who have multiple domains ex: verizons, salesforce.  talked about it being useful.  Sites and businesses that are interested and useful

Aram Zucker-Scharff: see both sides of this.  problem with lots of publishers with multiple domains and want to share login.  There are good reasons to represent these things as multiple domains vs subdomains because logically separate entities in eyes of folks consuming them.  also see downsides of how can be used to join data together in negative ways.  how sites can set it up, what qualifies as first party-ness and what doesn’t.  Ben talked about joint privacy policy.  maybe that provides opportunity to level out this problem.  the issue is that when we look at how a corporate entity might organize a set of websites, potentially a lot of websites not necessarily relevant to that.  relationship with corporate entity.  data joins can make it on the backend anyway.  making it harder for user to login without this first party consent.  On the other side, needs to be clarity to users about the relationships of these entities to the extent that we can.  ex: verizon, if using huffpost and know its on verizon.com or that its owned by verizon doesn't tell you anything as a user; but it does make your user experience more difficult if you have to login to every one separately.  legitimate concerns about competitiveness around entities that have visible brand names across their products.  ex: google.  youtube.google.com that wouldn't necessarily alarm or confuse their users. but huffpost.verizon.com confusing to users in a significant way.  That doesn't mean First Party-ness can be one solution, but it is a problem we should be concerned about.  how this creates after effects in creating competitive landscapes on the web.  favor one side over another. without way to federate login in the way

Kaustubha: Aram seems supportive that there is a problem that they would like to solve.  

Aram: yes

Chris Pedigo: Supportive of the concept of FPS. Tackled a bit of this in the Do Not Track standard. We tried to map it back to consumer expectations. Privacy Policy and domain ownership makes sense. Common branding was a thing we looked at as it might be clearer to the user they are the same.
...: Not clear what happens if you have a Facebook page from the Washington Post. Who owns the data in that case? Similar with AMP. Could be good to look at common branding.

Kaustubha: Does that mean the domains have to use common branding?

Chris: Yes. The privacy policy would discuss the brands as well. And it might be machine readable as well.

John: Our position is in the thread. Largely agree with Mozilla, but if it's going to be worked upon we'd like to see it being done in the Privacy CG. Branding is a huge aspect of this that we'd like to see explored. A powerful feature could be clearing data across a whole "set".

Kaustubha: I'll take that feedback on branding. Please do reach out if there are other concerns.

Anne: if becomes work item here, move from wicg or be in both?

Chris Wilson: expect it would move as co-chair of wicg

##  Deferred to next time:
* [Implementing privacy rights](https://github.com/privacycg/proposals/issues/10)
* Storage Access API - [Promptless storage access with constrained communication capabilities](https://github.com/privacycg/storage-access/issues/41)












