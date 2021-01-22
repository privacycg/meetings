# Privacy CG Teleconference, 10 December

* [Agenda](telcons/12-10-agenda.md)
* Chair: Erik Anderson
* Scribe: Michael Kleber for main call and part of GPC break-out session, Arthur Edelstein for remaining portion.

### Agenda
*   [First-Party Sets](https://github.com/privacycg/first-party-sets)
    *   #32 Proposed SameParty cookie attribute ([link to explainer](https://github.com/cfredric/sameparty)) to annotate cookies intended for contexts within the same FPS
        *   Lily Chen (Google):
            *   SameParty builds on First Party Sets, to allow cookies to be accessed in a same-party context.  "Same party" = browsing context where the current context and all of its iframe ancestors, up to the top window, are part of the same First Party Set.
            *   Lots of pictures here illustrating specific examples: [https://github.com/cfredric/sameparty#sameparty-cookies-on-http-requests-cookie-header-rules](https://github.com/cfredric/sameparty#sameparty-cookies-on-http-requests-cookie-header-rules)
            *   Note in particular the negative examples, that show the chain of same-partiness being broken.
            *   New cookie attribute SameParty, which is just as stand-alone attribute, unlike the key-value SameSite=xxx variants.
            *   SameParty does require Secure attribute.  Can coexist with SameSite — table in the design doc that explains the enforcement mode when both appear; basically SameParty overrides, whenever possible.
            *   SameParty cookie accessible from JS if it's inside a SameParty context, same as when the frame loaded.
            *   SameParty is its own attribute, rather than another value for SameSite, because it's about the privacy boundary, not the security boundary that SameSite enforces.  Also our compatibility experiences with attempting to launch the SameSite=None attribute taught us that it's difficult to add new values; a new attribute will be easier to launch.
            *   SameParty makes sense even for site not in a First Party Set; then it just turns into SameSite=Lax+Post.
        *   Arthur Edelstein:
            *   Mozilla position: FPS is not the right approach, not intuitive to users, we think there are better alternatives including Storage Access API.
            *   Specifically on SameParty name, concern that it confuses because of the previous definitions of first-party vs third-party.
            *   Lily: We did consider SameOwner, but felt the analogy to SameSite, and incorporating FirstPartySets.  Happy to talk about names.
        *   John Wilander:
            *   Also concerned about FPS in general.  We also want ways to understand why cookies are being shared.  This attribute for cookies would not lead to any automatic access, e.g. it could be a way to signal that a third-party domain is "the login domain for" the first-party web site, and then browser could appropriately modify the text in a prompt, the duration that access lasts for, etc.  Then could associate the changed permissions with a specific cookie, rather than all cookies for a domain.
            *   Lily: Useful signal about what cookie is supposed to be used for
        *   James Rosewell: How many domains in a First Party Set?
            *   Kaustubha: We've talked about size limits previously — range of answers (5, 10, 50, etc).  We're hoping to get more feedback.  Considering placing entropy and size limits (e.g. shared storage limit) that adds some counter-pressure to dis-incentivize FPS sets from being too large, but no concrete number yet.
        *   Kaustubha Govind:
            *   Responding to Arthur re. Storage Access API: Will user understand the prompt in most cases?  Login prompt is a clear use case, but other uses are less obvious to user; hard for them to understand what the button means.  If we do the right thing wrt policy and user expectations, then we can avoid showing users these prompts that they don't understand just to keep sites functioning properly.
        *   Dan Applequist:
            *   When we [reviewed this in TAG in May](https://github.com/w3ctag/design-reviews/issues/342#issuecomment-634733391), we noted there was a big debate in community, lack of consensus.  What I'm hearing today is Chrome people talking about First Party Sets and other implementers saying they don't believe in this approach.  I'd encourage people, in some venue here or elsewhere, to find a way to reach consensus instead of talking across purposes.  Need to bring these threads together; damaging if Chrome goes off in one direction with FPS and other implementers shut the door to it.  Bad for developer community.
            *   Lily: As far as SameParty goes, in the cookie spec there is a line indicating that you should just ignore an unknown attribute, so for interoperability purposes, having the SameParty as a separate attribute should prevent dev problems with compatibility.
            *   Kaustubha: We'd be willing to work in another venue.  There have been other vendors who have expressed interest and developers who have expressed interest.  There is support, and we are willing to work together.  Will reach out.
            *   Dan: Happy to play whatever role that TAG can to facilitate.  [We want cross-browser, cross-OS consistency](https://www.w3.org/2001/tag/doc/ethical-web-principles/#multi)
            *   Erik: Microsoft is generally supportive of the effort.
            *   Lisa LeVasseur: Thinking about exactly how cookie notice will change could be helpful.  I remain concerned about whether people will understand this.  The legal relationships established between people as they use web sites are muddied by these changes.  Support Dan on wanting to work through divergence.
            *   Lily: Re. cookie consent notices — I'm not sure that relates to SameParty.  But we certainly need to work on how to make this clear to  users inside the browser.
            *   Kris Chapman: Salesforce supports the direction.  We're a very large company (25K engineers); use domains to identify which team is working on things.  When you interact with a single Salesforce product, it draws on multiple domains, which are important to us for understanding and tracking down problems.  Asking users to give permissions for every domain would be very annoying to users.  We want to be transparent to users, but remain useful to us as developers.
            *   Steve Englehardt: We Moz. appreciate Dan's point, and are more than willing to continue to engage and reach consensus.  It's not about cookie-line parsing, it's about sites breaking.
            *   Lily: This design allow sites to fall back to SameSite=Lax, in an attempt to mitigate any cross-browser compatibility problems.
*   [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    *   [#53 Send report to both click source and destination](https://github.com/privacycg/private-click-measurement/issues/53)
        *   John Wilander: Mostly positive reaction to this so far.  We're heard from advertisers that this will help them be confident in data coming from their third-party partners.  Also we're not creating a situation where we're setting up barriers about who owns this data.
        *   Ben Savage: FB is Supportive
    *   [#54 Require description of the click content and if it is an ad, information on who purchased it](https://github.com/privacycg/private-click-measurement/issues/54)
        *   John Wilander:
            *   Hasn't made much progress.  We're interested in requiring a short description (say 100 char) of what the ad is and of who purchased the ad, purely for on-device or in-browser transparency features, or ever when you're about to buy something you could see what ads you'd seen were related to it.
            *   There is concern over potential misuse, but we think the big players will make an honest effort to get this right.  No huge opposition to being transparent, though there could well be cases where people misuse.  Of course browser needs to be careful any time it displays strings that come from a web site.
        *   Ben Savage:	
            *   I'm not sure what kind of information you're hoping this string would include.  We have a description and link text in the ad, is that what you want?
            *   Wilander: There are two things: Description and Purchaser.  I'm not envisioning description being a technical thing like a link, more like something human-readable, to help them recall after the fact what ad they saw or clicked on.
            *   Ben: Where would that come from?  Something we'd ask all advertisers to include?
            *   Wilander: Yes — would come from the advertiser, along with the creative.
            *   Ben: "Purchaser" seems like a link to the purchaser's FB page would be more useful than a string that's just the name of the purchaser — e.g. could include how to contact them.
            *   Wilander: True, but there is risk to visiting a URL.  Makes sense, this is super-early.
            *   Ben: Is there a risk of the browser doing some learning on these images and strings etc.  Can we know what all the downstream uses are?
            *   Wilander: Hadn't thought about this.  Could be useful to users, but are you worried about the browser leveraging this?
            *   Ben: Sure, browser or an extension.  Who could use this at scale, what could they do with it?
            *   Wilander: Haven';t thought about this.
        *   Alex Cone:
            *   Have you thought about all the different formats for ads?  How this parameter would be appended?  There's a lot of variation.  Maybe I should just raise issues.
            *   Wilander: I've heard about a 3rd parameter, that the ad needs to state its type (video, banner, interactive,...).
            *   Alex: I'd be interested in engagement from advertising side.  I was more thinking whether the ad itself is delivered via a URL (VAST), or as an arbitrary HTML blob, or etc.  Ads delivery is very un-structured, and each type of ad might need a different way to set this parameter.
            *   Wilander: I've been in conversations with people who want to standardize some kind of ad element, but there are downsides, e.g. ad blockers.  We're just looking at the case of a click, where a click takes the user from one site to another.  Could apply to any ads that have links, and that's it.  Some of the TAG feedback on these proposals has been to try to detach these things from advertising lingo and terminology — browser doesn't know it's an ad, but does know it's a cross-site navigation.  No longer talking about "conversions" or "campaigns", now about "trigger" and "source".
        *   Mike O'Neill:
            *   This could be useful when there are legal requirements for having some information about ads, e.g. who's paying for a political ad.  Could some of this metadata meet legal requirements?
        *   Wendall Baker:
            *   +1.  We would appreciate more formalized interfaces in the ways that pages are mashed up.  Those of us who have to do legal compliance would be in a better position if there were formalized ways to pass along data.  Could probably find more neutral language than "advertisement", but would be good to indicate commercially-placed content, etc.  There are lots of ugly hairballs now; formalizing composition into the browser would help us.
        *   Charlie Harrison:
            *   I mentioned concerns on GitHub — allowing untrusted entities to provide information that appears in a trusted context, etc.  The fraud case is worrying also: in PCM we talked about "How can we make it so that you won't receive fraudulent reports?"  Here we need to worry about the surrounding page maybe being able to change the context string, where lots of parties get to write script that run in the same context; could lead to bad actors adding offensive content that looks like / claims it came from someone else.
    *   [#57 Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)
        *   John Wilander:
            *   I tried to explain our thinking about why we don't think  we can send reports to third parties or to subdomains.  Interested folks, please join in the conversation in the Issue.
        *   Chair Erik Anderson: Let's pick it up on a future call
    *   [WICG Conversion Measurement call](https://github.com/WICG/conversion-measurement-api/issues/80) recap
        *   Charlie Harrison:
            *   We're kicking off live discussions in WICG about conversion measurement work.  First one was last Monday.  Lots of concern and discussion about how to make reports sufficiently anonymous that we don't need to ask for specific consent for legal compliance.
            *   Next meeting is next Monday, will post link for scheduling and upcoming topics.  Next week (Mon 4pm GMT): SPURFOWL proposal for on-device processing followed by aggregated reporting.
            *   [https://github.com/WICG/conversion-measurement-api/issues/80](https://github.com/WICG/conversion-measurement-api/issues/80)
*   Any other business
    *   No meeting on December 24th (Christmas Eve); next meeting January 14th

# Global Privacy Control Ad-Hoc Meeting
*   [Global Privacy Control](https://github.com/privacycg/proposals/issues/10). Also see [scheduled ad-hoc meeting](https://github.com/privacycg/meetings/issues/9) following this one.
    *   WEBSITE:[ https://globalprivacycontrol.org](https://globalprivacycontrol.org)
    *   PROPOSED SPEC:[ https://globalprivacycontrol.github.io/gpc-spec/](https://globalprivacycontrol.github.io/gpc-spec/)
    *   FAQ:[ https://globalprivacycontrol.org/faq](https://globalprivacycontrol.org/faq)
    *   W3C OPEN ISSUE:[ https://github.com/privacycg/proposals/issues/10](https://github.com/privacycg/proposals/issues/10)
    *   We'll start this discussion now, and continue on into the ad-hoc meeting
    *   Sebastian Zimmeck:
        *   Great discussion on GitHub issue.  Wanted to provide a forum and hear what people are thinking.
    *   Ash S:
        *   Goal is for people to express privacy preferences, specifically addressing CCPA's language about using a browser privacy setting.  We hope spec would eventually evolve into other legal jurisdictions.  Focus here is on preference expression, not the policy side.
        *   We've worked with interested parties (Brave, DuckDuckGo) to give users ways to invoke those rights.
        *   We briefed 40 premium publishers yesterday and received positive feedback, and likewise from California AG as well
        *   We're working on a contributor document to better align with W3C
        *   If you're a California consumer, most sites you visit over a certain size will let you click on a link "Do not sell my information".  Opt-out under GDPR vs opt-in.  Goal is to provide a robot that clicks those links for people — this invokes the right automatically.  Three of the top CMPs support this also.
        *   Do Not Sell under CA law: any transfer for value or consideration.  Transfer for advertising is explicitly called out in the regs.  New law will go into even more detail about what is covered or permitted.
        *   Under GDPR could use the same mechanism , e.g. an Article 21.5 objection.  Alternatively it could evolve into invoking other rights.  Do you overload Do Not Sell, or offer other possibilities?
    *   Robin Berjon:
        *   We're here to answer questions.  NYTimes supports this in production now, in CA and in GDPR or GDPR-like jurisdictions.  It's a forward-thinking and web-friendly approach to privacy.
    *   Kris Chapman:
        *   Evolution of DoNotSell and DoNotShare: Anything else that you've considered beyond these?
        *   Ash: When we first announced this, we received unsolicited blog post from privacy commissioner of Bermuda(!) indicating that the mechanism could apply under their GDPR-like PIPA (?) mechanism.  Nevada in US has a similar mechanism to California and they are interested as well, though CA law allows pseudonymous users while Nevada requires user to be known, so maybe would apply only to users with an account.
    *   Mike O'Neill:
        *   Re GDPR, ePrivacy DPAs indicated that there is an exemption for analytics, and they referred to a right to object.  Then discussion didn't happen.  There should be some way for someone who doesn't want to be tracked for any purpose
        *   If you've gotten a consent override, there should be a standard way for it to be defined, rather than everyone doing it in a proprietary way.
        *   Q re California act: you wouldn't need to put a link on the site if it supports the GPC.
        *   Ash: Under the new CPRA law (if it's not preempted before 2023 when it takes effect), there's an explicit requirement for the AG to specify the control mechanism
        *   When you click the link, the web site can ask you to reconsider, and there are requirements about how often you can do that.  Up to the regulator
        *   Law says that if you don't pop up another dialogue, then you don't need to show a link, you can just do the spec.
        *   ePrivacy: still ongoing revision, don't know if this will relate.  As this gets more cemented, maybe more people will look do it.
    *   Sebastian:
        *   We're just trying to provide a mechanism; signal can have different meaning depending on what regulators decide.  We don't try to decide on meaning, just mechanism.
        *   People complaining that GPC is all-or-nothing: user could specify consent at whichever level they want.  There is an extension that lets you select who you send the signal to and who you don't.
    *   Dan Applequist (Samsung Hat):
        *   We support, though don't know the timeline at the moment
        *   Support continuing the work and technical discussion in this venue
        *   Would like a wider and more diverse set of parties to be engaged through this group.  Want to address all the edge cases.
    *   James Rosewell:
        *   Seems like this is about a technical standard for capturing consent information.  This could transmit other information as well.  One decision that should be made: is this about this one particular signal or could it be extended to others?
        *   This is the first time I've seen a proposal that refers to a certain law.  Somewhere in the spec it needs to indicate what it means in different jurisdictions.  Can't be vague, lawyers need to know what it means.  Spec needs to handle those unambiguously.  What happens if I don't wish to respect the signal, fulfil my legal obligations in a different way?  Not an open standard if we're forcing a person to comply with it in a specific way.  Is there any precedent?  Seems like it will be highly confusing for anyone trying to figure out how to implement.
        *   Ash: Under California law, it isn't a consent mechanism, it's an objection you're invoking.  User is expressing intent to invoke their privacy rights.
        *   What those rights are?  Would be hard if you tried to dictate what the consent language had to be.  We're just creating the transport layer.  Whether the signal adequately represents the user's intent is up to the lawmaker; maybe they would come back to W3C and ask for another signal.
    *   _&lt;scribe switch from Kleber to Arthur Edelstein>_
    *   Robin: I want to strengthen that point from our perspective. It’s not a standards place to dictate law, it’s to indicate user intent. As a publisher, you probably know which  jurisdictions you are operating in and what to do when you receive a clear statement of user intent. If you’re facing a Californian, what do you need to do? If you’re a European, etc.? The lawyers don’t seem to struggle at all with this notion. The standard provides a better way for users to effect control and for users to express their preferences.
    *   Paul Bannister, CafeMedia: We think this is really interesting. Don is on our team and thinks it’s a very good idea. What we do is we represent thousands of mid-size publishers who want to outsource more and more of the management of their businesses to agencies like us, including compliance. One of our challenges is, how do we explain to a smaller less savvy or one-person company what the benefits of this is? From their perspective privacy is a hassle, such as popups, and something they’re not naturally happy to do more of. How do we explain this to them, so they can understand why it’s good.
    *   Ash: It’s required in law is the key. The law requires that fall under the law respond to a GPC mechanism. As of the current law, it doesn’t specify what the mechanism is. In the next iteration the AG is required to specify. If you argue the mechanism is widely used, and there could be liability, and you get different standards that companies are required to abide by. Lastly there could be standards that are not privacy focused or undermine. For example, there are proposals that would embed a unique identifier and be sent to third parties. In order to invoke the law, consumers would have to reveal their identity. The ideal is to come to an open standard good faith solution that receives the blessing of regulators. As far as to Robin’s point, potentially we’ll see interest and support from other regulators such that the technical spec can evolve. I know there is a proposal from WebKit will do something similar. I don’t think anyone here is tied to GPC being the end-all be-all solution. I think there are other possible solutions and the group is open to evolving it. The idea was to keep it simple, satisfy the base requirements and then evolve it how it needs to. We pulled from DNT as people can see, maybe an existing framework that works well. If you look at GPC, it looks very similar in terms of simplicity to DNT.
    *   Sebastian: Apart from the enforcement point, it would also make the process easier. I came across a blog post that says how expensive it is to handle opt-out requests manually. Proposals like GPC can make the process easier. You have these many smaller publishers and they wonder, how do I comply? Do I even fall under this law, and if so how do I do it? These mechanisms could help with that.
    *   Robin: As a publisher who has switched it on, this is by far the easiest mechanism across the board. From where I’m sitting, that’s a big win for my team.
    *   Lisa LeVasseur:
        *   Wanted to get clarity on where the spec work is going to be done. Dan mentioned here, but we’re a community group?
        *   Ash: We’re open to discussion. We’ve invited everyone to provide feedback in the github repo, and we’re happy to do it in the community group or ad hoc group. A good question for here.
        *   Re individual per-site signals, in the early release it was maybe the Brave browser that had a unilateral enabling of the do-not-sell signal and no user interface to change that around. Was an interesting decision. I get it, but I felt, ugh. It’s up to the implementation to figure out how to enable and disable the signal
        *   Last thing I am puzzled by -- the signal protocol is fine, and makes sense. The thing we’re not talking about the default setting. The default setting of DNS is set to disabled last I saw. I understand this to be an opt-out. I understand in at least a couple of european jurisdictions they have made quite clear that opting out is not OK from a human perspective. How do we reconcile that bit of it? The default is the linchpin of this whole discussion
        *   Ash: Great question. People are going to have different feelings and it is going to be hard to come to consensus. We will allow the regulators in different jurisdictions to weigh in. In CA law, the mechanism can be on by default if it’s a privacy-by-design tool. We refer to this point in the FAQ. That might change in Europe, and a regulator there would say yes, if it’s on by default or interface expresses the user intent in this way. That would guide not just the tech spec, but the guidance for implementers. If the goal is to build the pipes to convey the signal, and let the innovation be free on the browser side.
        *   Lisa: I’ve never seen a spec that didn’t have a default setting specified
        *   Ash: We say what the defaults are in the FAQ.
        *   Lisa: It’s default to disabled, correct?
        *   Ash: No, the mechanisms should convey the user’s intent. In CA law, the mechanism can be on by default if the user expects it to. Brave is marketed as a privacy-by-design browser, they turn up shields to 10. They AG would argue that that is the expectation of the user. If it’s a general purpose tool, in contrast, maybe not. It might vary in a European context, but it depends on what right you’re trying to map it to in Europe. That’s why this is a simple attempt to get a 1.0 version for the spec, and then evolve and take into account these complexities.
    *   Tanvi: I want to talk about the default as well and I want to make sure we learn from the lessons of DNT. Many of us aim to provide privacy by default for users, but we have to think about improving privacy by default overall and how decisions we make here affect privacy in the long run. I worry this is too soon, before we have more laws and regulations, then sites won’t adopt and adhere to the header. GPC is only effective if it’s adhered to. If we think about this more strategically, it may make sense to wait for other jurisdictions to implement before we set it on by default.
    *   Ash: I think that’s an important point. One of the goals -- as the specification evolves, publishers can support a specific version. There’s room to support the evolution of the standard as well as a technical perspective and regulatory. [https://globalprivacycontrol.org/faq#Default](https://globalprivacycontrol.org/faq#Default)
    *   Jason Kint: Re what Robin said in his last comment, I can’t speak on behalf of many premium publishers.  Beyond subscription focused businesses, advertising businesses large and small, publishers getting closer to intent of user is what they want, and this absolutely gets closer to the intent of users, in the sense that we have an efficient signal that is clearer on delivering on those expectations, and that is why we have been very positive about this initiative. If any individual regulator (eg CA) says this is the signal we intended in our law. This gets us a step closer to consumers' intentions. Anyone else who is a service provider needs to honor the signal.
    *   Ben: Wondering if anyone from Webkit cares to comment in terms of comparing and contrasting GPC with their consent API. How is that progressing, what are the plans?
    *   Tess: The [centralized consent API](https://github.com/kcheney1/explainers/blob/3521fef2e059c1a5287b4925e7dddb89a5984336/CentralizedConsentAPI/README.md) is probably fairly unrelated. It’s trying to solve a problem in the same-ish space, but it's not the same problem it’s trying to solve. Trying to reduce the number of times you see existing consent banners by providing an API that sites can use to figure out if they should show the banner and if so it provides them storage to store the results of the user’s choice. It’s still very early and obviously the explainer is up and if people want to read it and think about it and comment on it, then that would be great. It’s not something ready to bring to the community group. I’d rather not distract from the GPC folks, and I think we should keep talking about that. We can do the webkit proposal some other day.
    *   Robin: To second what Tess said, a lot of people have been talking about how to solve the cookie problem. In design space that’s quite orthogonal . We’re relying on the fact that these are different issues.
    *   Kris Chapman: I’m wondering if there’s been discussions about including a jurisdiction signal. IP blindness and trying to get away from that info. You get the chicken and egg problem - you don’t know who the user is. I’m wondering if there is to be a signal from browsers that says I am an EU or California citizen. Are there discussions that way?
    *   Ash: There was discussion around it, but to Robin’s point around consent banners, different publishers and businesses will use different ways to get origin, such as IP or sign-in info. Aside from the legal concern, the fingerprinting concern, as you add bits to the user to declare I am in this geo, PING and others would take issue. The idea is to square that circle to provide meaningful information. It has been considered and I think it’s important information.
    *   Kris: I totally get the fingerprinting aspect, but I was literally thinking, let me no longer have the IP address. I’m stepping back from having as much as I currently do.
    *   Robin: If we have a way for publishers to not have the IP Address, but that’s still an unsolved problem. At least at the kind of scale that we need to have it. This would essentially add to the information that is already exposed by the IP address. One point that Ashkan mentioned in passing that this would put a lot of liability on the browser. Hey -- you told us this is someone from North Dakota, but the person was visiting Germany at the time. It kicks the can down the road, but I’m not sure it solves the problem.
    *   Sebastian: Want to add one point, a little bit related to what Kris said: what we also have is a sort of response mechanism actually. Previously we were talking about a signal from client, but on the publisher's side you can have a response and the way we did this in the draft standard is there is a .well-known URL that is available for every site and you can post the information there. You can say, OK, I detect the user is from a certain state depending on the IP address. Because this is not an IP address from CA, that’s why I do not apply the Do Not Sell right to this request. There is some scope for the recipient of the signal to respond.
    *   Sam: Wanted to appreciate Ash’s answer to James Rosewell’s point on the need to include law in our standards. In the telephone system we have an opt-out consent signal in the form of the Do Not Call signal. There are all kinds of regulatory exceptions on who it applies to. If I give a scammer that signal, I am sure they have local attorneys to tell them whether to abide by the signal. I don’t think we can include regulations in our standard. What we can do is convey a signal. We don’t need to rewrite legislation
    *   David Singer: Wanted to ask about verifiability. If there’s a data breach or something, I have no way of verifying what goes into their database. A lot of us are leery of specifications where we don’t obey a mandate. As a user can I verify what they’ve said they’re doing is what they’re actually doing? Or for researchers and regulators. Normally you can verify a spec is working.
    *   Ash: The spec for example, you can verify if the signal was received in our test reference implementation (test page). For some companies like the Times, they don’t integrate third party calls. They tailor the ad partners they use to those that don’t sell info. The Times could turn around and without the user being able to see, transfers the data to some other company. It’s a question of enforcement, in CA for the AG to enforce. For the CPRA, the Data Protection Agency in CA has audit rights to show up at a company and verify their data practices if they have cause. The company could still obfuscate a la Volkswagen. We can verify if the signal is sent.
    *   Robin: That spoke well for me
    *   Aram: I wanted to add two things -- the WashPo is in the process of implementation. We’re interested in seeing this go forward and the standard in general. From the point of view of a publisher, and we also work in ad tech, one of the things that pops out is how the proposal is limited in scope and has a lot of speed. milliseconds can mean millions of dollars. That’s why we’re very interested in this and like the simplicity.
    *   James: To clarify: I certainly wasn’t saying earlier that laws should be part of tech standards. The precise opposite. Also, in relation to David’s point it’s very interesting how different jurisdictions have different rules. In the UK, you get a letter from the info commissioner’s office, you have to pay a fee. Everyone has the legal right to ask you to tell me what personal info you have got. If you are a plumber and have people’s addresses, you have to report it. In the UK, if to verify if someone’s right is being respected there are mechanisms to do that. You would be fined as a business and it is enshrined in the law. Perhaps that speaks to different jurisdictions and different rights that people have. I wonder if the title of this proposal is right, transmitting preferences seems to be a good thing. Loved Aram’s point about removed friction. Maybe this should be generic and preferences and consent rather than focusing specifically on one particular signal. As a tech standard it does have a specific legal issue associated with it, it’s just a pipe and what goes through that pipe is a separate question. It seems to me there are two choices: is there a registry of signals together with their meaning/jurisdictions that a lawyer could pick up. Perhaps less of an issue for NYT than other lawyers. Maybe there is a registry, or it becomes part of the info that the website sends back in response: “I treat this signal this way” and the user can use that signal. If that then happens and we have a group of one or more consent signals, then that’s more or less like the credit card/payment wallet and you get a section of credit cards. Perhaps you might have different personas for different purposes. One set of preferences, and another set of preferences. For that modification, it solves a multitude of problems, deals with different laws, allows extensibility.
    *   Ash: Thanks or feedback. I think it’s precisely because of those issues that we chose not to go that route. Focus on a very narrow place where we have a law, we have a requirement, that is specifically focused on privacy, with GPC/DNS. This could evolve into another right under the privacy rubric, but I’m reticent to add a lot of complexity or make it so generic or have no meaning or purpose. Goal is to have a legal requirement under at least one jurisdiction. Otherwise folks might remember the I want a pony header, in the DNT debate. We’re back to signalling I want a pony, while no one supports the right to have a pony.
    *   James: Seems to me that if it is focusing on a specific law, that is going to be problematic for a standards body.
    *   Sebastian: To clarify, this is an excellent comment. What you said is what we’re doing. Essentially we have one binary signal, but it can be interpreted differently depending on who receives it. There is the flexibility in that signal, on the other side, the recipient of the signal also has the flexibility they can say in their .well-known site, this is how I treat signals. Or if the signal does not apply to me. The point you said with the registry, I think that’s a great idea. When regulators are saying “this means that” in our jurisdiction, I can imagine having a site where this is all shown and somebody can look that up. Would be very useful indeed.
    *   Peter Dolanjski: DDG very much supports GPC in fact we have rolled it out in our mobile browsers and desktop extension. We recognize this is quite early for a lot of jurisdictions. On the default question, it’s clear in CA regs, default elsewhere may not be as we figure out how GPC maps to those jurisdictions. We might consider default for CA, if not allowed elsewhere it’s something we’ll have to address and respond to.
    *   Ben Savage: Want to recap what I understand. There is a header which is binary (present and value is 1 or not present
    *   Sebastian: Also a DOM property
    *   Ben: Expresses same thing. Binary signal that is called GPC. As a website operator, I observe GPC is set, or I observe GPC is not set. That is all. This does not tell me how I ought to interpret jurisdictions, that is up to me as a website operator to determine. Does not tell me what laws exist in each jurisdiction. Doesn’t tell me if GPC = 1 maps to something or means something. Doesn’t tell me what I have to do as a result. What it may map to may map to radically different things depending on the jurisdiction. Not an expression of a user’s anything. Doesn’t allow customization or control. Users is able to say “privacy” or “no privacy” that maps to unknown things.
    *   Robin: Good point, but not a correct interpretation. There’s privacy, and inside of privacy. GPC only addresses one, which is the right to not sell and express an opt-out of sale of your data. Could map to different jurisdictions. But under the GDPR there are very similar provisions. Withdrawing consent to the sharing of third-party data controllers. Or rejecting the legitimacy of information transfer to third-party controllers. This conveys a clear user intent. Made clear by the browser UI if it’s not by default or if the user is using some system that says privacy turned to 11 everywhere. As a website operator, you receive that signal, you can say “this is a right I want to afford users globally”. You have a very simple thing to do is provide your data to third parties that may make independent use of it. If you want to be specific, you have to use the same mechanism as a global publisher. You're either in scope for CCPA or GDPR or not. If we know you have a billing address in CA and something in Europe, we have to apply the CCPA and GDPR to you. GPC reduces the number of affordances. Tied to simple rights that don’t have wildly different interpretations but are broadly aligned.
    *   Ben: Global Privacy Control specifically controls refer to the sale of data?
    *   Ash: In this initial implementation
    *   Sebastian: Under GDPR, there is the right to object to the processing of personal data. In the CCPA it’s called Do Not Sell. I looked up different privacy laws from Africa and China. They also talk about the objection to the processing of personal data. Generally the right to object to the processing of personal data.
    [Sebastian clarification 12/12/2020: my suggestion for an objection to processing in the GDPR context was to mirror what the “do not sell” signal signifies in California: objection to processing by anyone other than the controller]
    *   Ben: Processing refers to everything, not sharing or selling.
    *   Ash: What you’re asking about what the legal regime is. I would take a look at FB’s current position on the 
    *   Ben: This spec is very unclear what it maps to or what is the default.
    *   Ash: FB: We currently interpret GPC as the CA Do Not Sell. And the AG would say this is an adequate mechanism for invoking your right. A company can also say we volunteer to accept the signal as the requirements in GDPR. That can evolve: we want a right to have ponies in Europe. Then the next GPC version could include a privacy pony right. The language in the UI would indicate that right. The initial right because there is a legal regime in CA.
    *   Steve: I think it seems very difficult to decide how to show this in the UI as a browser vendor. In Europe, the signal might mean one thing, in CA another. If the user is in North Dakota, what do we show them? Do we geolocate the user? That has its flaws. Do we show all regimes? Operationally seems very difficult to design the UI.
    *   Ash: Right now it applies specifically to CA. I would expect in 2021 the regulators are required to issue what that standard is. The regulator might endorse or bless this as a standard at least in CA. 
    *   Peter: The way we’re addressing that in DDG, we’re trying to be upfront about it, but we say most websites don’t recognize it yet. We’re pretty upfront about it.
    *   Ash: Absolutely the spec looks like DNT. We had intended for DNT to be the mechanism in the CCPA, but because in CA there is a law saying DNT doesn’t have to be respected by companies. Because the law says you do have to respect a GPC, in this one jurisdiction you have a legal basis.
    *   Johann: Comment to Steve and Ben’s points. Something that could be helpful here is if GPC were more clear on what specific jurisdiction is applying and being sent along, and could be chosen by the user. I don’t want to show an input field, that says enter your specific legal code, but I think if it were more verbose, it could help the publishers and the people sending the signals. Also: something I have perceived with GDPR personally is sites that have partial functionality, because you haven’t given us the right to share with third parties. It seems like this may apply to sharing as well as selling. If we give a voluntary signal that is backed by some law, then websites will have more of this pattern where functionality is impacted by your lack of consent. You have an opt-in screen and an upgrade that websites can claim they can show under most legal entities, that say we need your consent to do this real cool functionality. And thus we end up with the same thing that we had, just another signal with DNT. Websites will get what they want because they can gate it behind some functionality.
    *   Robin: IT’s important to note that this kind of thing is illegal in some jurisdictions. Sites can always choose to do that, just as they can say sign over your entire life savings. At some point it’s illegal for them to do that. Whether or not that will be the effect I don’t know, but one of the advantages is that it moves the decision away from publishers and back into the user’s hands where it belongs. All publishers give away your data at the very least to Google and FB. This provides a way for users to express rights that allows users.
    *   Johann: I trust that it might be illegal, but most German publishers have a pretty concrete data sharing wall. Either sign up as a subscriber, or consent to a lot of data sharing.
    *   Robin: You're right that Europe has an enforcement problem and that data authorities aren’t doing their job.
    *   Ash: These are excellent points, but you’re getting to key nuances where there is not a consens. You're talking about normative and legal questions in jurisdictions where we have no influence and control. There will be a variety of different interpretations. The flip is -- we have this one jurisdiction (CA) which sets the norm for a lot of the US. MS and others have agreed to respect CA across the US and not just for Californians. We have this opportunity to put a stake in the ground at least for one jurisdiction. Opportunity to solve that narrow question and evolve these additional questions, what other rights. We would evolve that discussion and informing policymakers on what the right outcomes and potential outcomes are in the hopes of convergence or additional user choice. The whole purpose of inviting folks in this forum is to identify that this is a good idea, and support for it, and evolve it over time in the hopes of providing a technical mechanism for users to invoke their privacy rights.
    *   James: It seems to me there are 2 ways forward. One is to take all the legal issues out, and simply have it as a tech proposal. The other is to have the legal issues stay in, and it’s still called Do Not Sell, then I would love to see a session with 3 or more lawyers having the discussion and see what they have to say about the issues. It seems very complicated how you overlay this signal on the contract.
    *   Ash: Sebastian is a lawyer. Model you proposed that essentially binding into a contract. There’s also a voluntary contract that “we will honor this as X”. Rather than us do it, and dictate language that every browser needs to have and publisher, we are producing pipes that convey the signal. Browsers can indicate the rights are being invoked. AG and others can vet whether those or valid or not. Bless or not blessed by public opinions or by rulemaking.
    *   Robin: Contract law does not allow you to do things that are otherwise illegal. There were ToS that I could kill my grandmother, because I was obligated by a contract. Similarly here, law takes precedence over this. Any terms of service of privacy policy that goes against that is evidently void.
    *   James: I would certainly like to see a multiple of legal opinions. 
    *   Ash: Hopefully in this forum and other forums we can address some of the harder issues. Please provide feedback, email us, we can schedule another ad hoc conversation. I want to thank everyone for your time.

## Attendees:
Note: Attendee list this week includes both the main teleconference and the ad hoc afterwards.  Some folks were only in one or the other.  About 80 participants in teleconference and 66 in the adhoc.
1. Alan Toner
2. Alex Cone (IAB Tech Lab)
3. Allah Spiegel (Adobe)
4. Anthony Molinaro
5. Aram Zucker-Scharff (The Washington Post)
6. Arnold rw
7. Arthur Edelstein (Mozilla)
8. Ashkan Soltani
9. Ashok Chandra
10. Ben Savage (Facebook)
11. Bennett
12. Brian May (bmay, dstillery)
13. Bill Densmore (ITEGA)
14. Boaz Sender (Bocoup)
15. Brain Campbell (Ping)
16. Brian Campbell
17. Brooke Pearson
18. Charan Mann (ForgeRock) 
19. Charlie Harrison (Google)
20. Chris Fredrickson
21. Chris Needham (BBC)
22. Chris Pedigo (DCN)
23. Christine Runnegar
24. Christy Harris (Future of Privacy Forum)
25. Craig Spiezle
26. Dave Harbage (DuckDuckGo)
27. Dan Appelquist (Samsung)
28. Daniel Gale (Simplifi)
29. David Dabbs
30. Davind Singer
31. Devin Rousso
32. Doc Searls
33. Don Marti
34. Edward Brawer (Utreon)
35. Eric Mwobobia (ARTICLE19)
36. Ericka Wright 
37. Erik Anderson
38. fantasai
39. George Fletcher (Verizon Media)
40. Harneet
41. Henry Lau (Privolta)
42. Jack Frankland
43. James Rosewell (51Degrees)
44. Jason Kint (DCN)
45. Jason Nutter (Microsoft)
46. Jeddy
47. Joel Meyer
48. Joel Odom
49. Joey Salazar
50. Johann Hofmann (Mozilla)
51. John Wilander (Apple WebKit)
52. Jordan Mitchell (IAB Tech Lab)
53. joycesearls
54. Kaustubha Govind (Google Chrome)
55. Kelda
56. Konrad Dzwinel
57. Kris Chapman (Salesforce)
58. Lily Chen (Google)
59. Lionel Basdevant (Criteo)
60. Lisa LeVasseur (Me2B Alliance)
61. Liz Hurley (CJ Affiliate/Epsilon)
62. Lionel Basdevant
63. Marcal Serrate
64. Mary hodder
65. Marshall Vale (Google)
66. Maud
67. Max Gendler (NY Times)
68. Melanie Richards (Microsoft) 
69. Michael Kleber (Google Chrome)
70. Mike Lerra (Google)
71. Mike O’Neill (Baycloud)
72. Nathan Hagen
73. Paul Bannister (CafeMedia)
74. Peter Dolanjski (Duck Duck Go)
75. Phil Armstrong
76. Pi
77. Raj
78. Rob van Eijk
79. Robin Berjon (NYT)
80. Rowan Merewood (Google)
81. rstratto
82. Russ Hamilton (Google)
83. Russell Stringham (Adobe)
84. Ryan Avecilla (Neustar)
85. Sam Weiler (MIT/W3C)
86. Scott Mace
87. Sean Harrison (Google)
88.  (Wesleyan University)
89. Shaun Spalding (New Media Rights)
90. Steven Englehardt (Mozilla)
91. Steven Valdez (Google)
92. Slvrspoons fone
93. Stefano Leucci
94. T Bell
95. Tanvi Vyas (Mozilla)
96. Theresa O’Connor
97. Thomas Müller (Utreon)
98. Tim Cappalli
99. Tony Resendiz (CJ Affiliate)
100. Valentino Volonghi (NextRoll)
101. viraj
102. Wendell Baker (Verizon Media)
103. Wendy Seltzer (W3C)
104. Zubair Shafiq
