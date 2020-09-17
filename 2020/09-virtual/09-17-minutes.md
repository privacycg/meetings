# Privacy CG F2F, 17 September 2020

[Agenda link](https://github.com/privacycg/meetings/tree/master/2020/09-virtual)

*   Chair: varies throughout the day

### [IsKnown Proposal](https://github.com/privacycg/proposals/issues/20)

Discussion lead: Aram Zucker-Scharff

Scribe: Kaustubha Govind

*   Aram (Washington Post): This proposal came out of isLoggedIn discussion. We’re looking for a more private way to recognize a returning user. This could be used to present a different UI (e.g. login paywall). The goal is to … and to significantly decrease the incentive to fingerprint people, and preserve functionality in a more privacy-preserving way. 
    *   There is a signal at the browser that can be read/set by site. Basic binary (?) is that it is presented within a time period.
    *   3p cookies used by federated login providers can be made available. But would force users to login.
    *   The goal is to provide a middle-ground between not recognizing user at all vs. forcing them to login
*   Aram: There is a counter that is incremented every time the user visits; but after a period of time, the timer is reset. Melanie suggested that the site shouldn’t be able to read the counter, which is a good mitigation to prevent fingerprinting.
*   Inclination is to set it per page, but if there are any embedders who would like to use this feature, we could consider making it available to them.
*   Q&A:
    *   Ben Savage (Facebook): I don’t understand why we need this, when 1p cookies are available to have this functionality.
    *   Aram: The same is true for logging in. This is often a role delegated to 3ps, and while large sites may handle their own recognition of users; smaller sites delegate to 3ps, this sort of mechanism could be useful. Could also be useful to lock/unlock the framework as being discussed within the context of isLoggedIn.
    *   Ben Savage: But 3ps can set 1p cookies, why not do that?
    *   Aram: My understanding is that this would replace whatever the use for isLoggedIn is intended to be.
        *   Valentino Volonghi (over chat) : isLoggedIn blocks anyone's ability to place a 1st party cookie. Hence even my question yesterday on first party analytics and First Party Sets
    *   James Hartig (Admiral Adblock Publisher Solutions): We provide paywall services as a 3p. Part of my concern is that what’s being outlined is not going to be verbose enough for our purposes. For example, publishers want the wall to show up on every other page. Some people want to user different kinds of paywalls (e.g. email, other info). There would have to be multiple isKnown counters to keep track of multiple different states.
        *   Aram: Verbosity is an interesting question. The number of visits needs to available, the amount of time since you’ve last set it is what we’re trying to avoid revealing. Potentially there’s other signals that could allow sites to partition isKnown. Seems like a common usecase that we need to account for.
    *   James Hartig: How does this relate to Trust Tokens?
        *   Aram: This is for when users don’t have Trust Tokens. I like TT, and that is another way to solve this problem. I see TT as a type of potential login, even though it’s not necessarily a full login.
    *   Mike O’Neill: We need to get consent before being able to use cookies. Anything being stored on the browser that is giving information about the user that the user may not want to be known. Since this is equivalent to a 1p cookie, there needs to be a way for users to consent/opt-out. You can do something similar to an extent in a low-entropy privacy-preserving way with existing cache headers. The site may know that the user has visited before . But I quite like the idea of something like this.
        *   Aram: Yeah, browsers should gate on user consent. Re: the cache headers mechanism I wonder if that will be available in the future with browsers clamping down.
    *   Arthur Edelstein: Seems like this is something we should treat similar to cookies (e.g. when users asks to clear storage), but I wonder if it affects the usefulness/efficacy if users can clear state.
        *   Aram: It’s reasonable that this would be cleared when user clears site data, but this would be outside of a standard cookie to use in a focused way.
        *   Aram: Someone asked if this would work in normal incognito. It would probably violate user expectations, and also browsers would not want to allow it.
    *   Charlie Harrison: We have another use-case to use something like this as a view counter, or to run A/B experiments to change behavior for some fraction of users. If those 3ps are operating in an environment when 1p state for some users is periodically cleared, those frameworks would not be able to function properly. From my understanding, this is a popular 3p offering, so might be good to keep in mind. Framing this problem as a more focused 1p storage for the purpose of not being intervened as much as the full 1p cookie that browsers would want to intervene may be the right way to think about this.
        *   Aram: Yeah, it would be interested to hear how the A/B case would want to modify this; but not sure that having an embedded 3p have access to this state is reasonable.
        *   Charlie: Interested to hear from John Wilander if ITP would not intervene on something like this. Because if it’s going to be treated just like cookies, I don’t know how useful it would be.
        *   Aram: Browser could intervene differently that it would for a cookie.
    *   John Wilander: In my mind this is all about 1p data. In Safari, 3p cookies are blocked and storage is ephemeral+partitioned. In this case, the 1p is managing the relationship with the user, and that could happen in cooperation with 3ps. What I’m hearing from Aram is that you want to operate login paywalls beyond 7 days. This goes back to the isLogged proposal. This should be framed as a small amount of data that persists longer than other kind of data. 3p case would immediately get into them building up a user identifier via a series of bits. It’s the 1p that should set the bit on whether the user is logged. I would think that everyone is familiar with clearing cookies / incognito mode. Are we going into a space where we think users will just accept the paywall.
        *   Aram: A decent size do try to avoid the paywall, but don’t have the numbers right now. The first time you hit a page, you hit a login wall. I’m hoping that this prevents that from happening every time a user visits the site. If we don’t have a known state outside of login, I don’t think it’s healthy (speaking as a newspaper publisher). The other thing that pushes people to bypass paywalls/registration walls is that we need to study what triggers users to do this.
        *   John: Regarding A/B or multivariate testing. Other usecases are: (a) Have I already asked consent from this uer?(b) Is this user in bucket A or B.
            *   These are all low-entropy that are being impacted by clearing of high-entropy. Would be good to explore the low-entropy vs. high-entropy angle.
            *   Aram: Others where users may be offered a discount deal. Drive-by users get a different kind of deal than returning ones.
    *   Brian Campbell (Ping Identity): I wanted to share an observation that I think is being overlooked. LoggedIn is not a binary state, there is a continuum of identifying/authentication/assurance about the user. When we consider storage privileges to something like isLoggedIn, that strikes to the heart of the need for something like isKnown. It seems concerning that the discussion has trickled out to another API. It makes me wonder if logged-in state is a good signal to consider when to clear state. Login state is complex, and trying to sync that up with what the browser thinks about preserving state seems like a sign of the mess of complexity involved with that.
        *   Aram: I’m not a fan of asking user to install, but Kushal’s proposal on [issue #21](https://github.com/privacycg/proposals/issues/21) about the high-level trust signal, and I think he mirrors your concern that the relationship of a user with a site is a spectrum. isLoggedIn is a binary, and isKnown sits in the middle. We can consider where the right spaces are in that spectrum. You have an excellent point that we need to put more thought into that.
    *   Achim Schlosser: I want to second that at that level it gets a bit nitty-gritty. Brian was saying that an identity provider maintains a lot of privacy signals out-of-band to the browser. We’ve had a bunch of discussions with the Google team on WebID, and there is an idea called session state opacity. An IDP would for example be able to send a binary signal saying that the user is logged in, for example. It’s obviously useful for publishers to know if users are already authenticated with them, and then there’s a whole swathe of ?? that APIs like WebID could provide as a signal that the browser can observe.
        *   Aram: the WebID proposal is a factor in all of this as well. You’re right that the logged-in signal is of very high importance to the publisher, and consent is a fairly wild set of signals as well.

From zoom chat:
*   Valentino Volonghi: isLoggedIn blocks anyone's ability to place a 1st party cookie. Hence even my question yesterday on first party analytics and First Party Sets
*   Sam: Sounds like a chicken and egg problem.  What stops the user from getting multiple sets of trust tokens?
*   James Hartig: Yeah that's a good point, Sam. Haven't fully thought through that just thought there were some similarities
*   James Hartig: Users more commonly just use Incognito to get around various walls
*   Ben Savage: That doesn't ever work for me... I assume due to fingerprinting =)
*   Sam:  @Ben: Try turning off javascript (for the misbehaving site)
*   James Hartig: I have success just clicking Stop in the browser as soon as the page loads. Timing is tricky but can stop the third-party from loading
*   Basile Leparmentier: There are some website that by default block incognito If I remeber well I think the NYT does that
*   Sam: And those are often using javascript-based tests to detect incognito mode
*   Basile Leparmentier: Thanks to difference in storage in normal vs incognito mode. Maybe it has changed Ok thanks sam, didn't know that! Didn't know going to privacy CG would be about having tips on how to bypass paywall
*   Charlie Harrison: One interesting thing about A/B testing is the "bits" can just be a read-only seed I believe
*   Ben Savage, Valentino Volonghi, Brad Rodriguez, tim - All +1 Brian. Welcome Brian!


### IsLoggedIn Part 1
Lead: Melanie/John

Scribe: Wendy for most of it, Jeffrey Yasskin for the tail end

*   [Should we standardize how we expect browsers to use this signal?](https://github.com/privacycg/is-logged-in/issues/15)
    *   Melanie: we’ve been discussing ways the signal could be used: relax restrictions, UX, etc. How much should we standardize: consistent user expectations vs innovation?
    *   Michael Kleber: past half-hour suggests we need to specify something about implications of being in logged-in state. All the discussion of what’s an appropriate use of 1p cookies vs needing new API deeply intertwined with implications of being “loggedIn” and what browser does. Those limits need to be spec’d.
    *   Andrew Knox: +1 to Kleber. What’s really important to the conversation is how it’s going to be used (several issues, 14, 9, etc.). If you’re talking about a specific harm, e.g. bounce tracking as John mentioned, start with purpose and then discuss why isLoggedIn is best solution to that problem, rather than adding effort and risk without that premise.
    *   John Wilander: Other way around. Webkit team notes that to solve cross-site tracking long-term, we need to put a cap on website data. Can’t give 15-yr cookies or lifetime indexdb. That’s where we start. We could start with general data cap, for all sites. Then to, offer to browser, this site has user logged in, please relax your data restrictions. Browser could then choose to respect that request, for the benefit of the user. Then, if you have isLoggedIn signal, you can start innovating new web features, e.g. understanding where you’re logged in, with which account. Convo started from a problem: “logged in by default” as we refer in the explainer. You go to a site, it stores things in cookies or indexdb virtually forever.
    *   James Rosewell: seems we’re talking about a spectrum associated with trust. One end you have no idea,. Depending on relationships, different conceptions of logging in. eg., bank has different standards for identification and trust. Different parties: browser, domain, supply chain. Different aspects of the problem, eg. need to manage storage; tracking and privacy. Until we can answer these, need to define policy wrt trust spectrum. Only once we define policy can we address engineering questions. 
    *   Tess: challenge the assertion that browser is third party 
    *   Arthur Edelstein: makes sense to get more signals from the site when they’re asking for storage. Confusion based on the name isLoggedIn vs something to do with storage. Perhaps that’s scope creep. Tie it more closely to storage. There’s a persist API call on the storage manager. Perhaps use that. Couched as a permission request from the website, vs assertion of truth. Center things around the user, who’s in charge, operating a user agent. Don’t think we want guarantee that storage persists because site asserts user is logged in. Make the naming more precise. 
    *   Melanie Richards: perhaps change naming. Re user-centric, users do understand logged-in state, could be a pivot to user control. I saw a comment from a user asking for control on clear-site-data for sites where they weren’t logged in. So a useful signal to users. There should be some user participation, so it’s not just assumptions. 
    *   Kushal Dave: basic question: would like to understand connection between first-party state and cross-site tracking. 
    *   John Wilander: We don’t like virtually forever state for first parties. You got a link from a friend, that shouldn’t mean the data persists for years. That’s not a good platform. The web should forget. Use a state users are familiar with to connect that. 
    *   Kushal: so it’s about both preventing tracking and limiting 1st P state
    *   Ben Savage: if we were starting from problem-solution basis, we have two problem statements from John: making all 1p state for non-logged-in states ephemeral -- something on which we don’t have consensus and are backing into. Secondly, that cross-site tracking is a problem we all want to solve and the solution is to make all storage ephemeral. Would disagree with that premise too. 
        *   ...Based on this poor logic, we then create a problem where users are getting logged out, and building complexity to solve that problem we created. 
        *   ...We at Facebook are generally opposed to browser makers who also have sign-on services and other 1st party products outlining policies that could benefit those products disproportionately. That includes setting “User Agent Policies” about what constitutes a “valid login.” 
        *   ...It is possible to achieve the goal of preventing “bounce tracking" that is highlighted in the proposal, without the web browser needing to know where a user is logged in. 
        *   ...A different response is Mozilla’s bounce-tracking protection. They have rolled a solution for “bounce tracking” out into production already. This is an existence proof that there are alternative solutions that do not require knowledge of where the user is logged in.
        *   ...If your goal is to stop bounce-tracking, you could gate storage access on session length, for example. 
        *   ...There are multiple solutions to preventing cross-site tracking. This proposal doesn’t stop bounce-tracking for logged in websites. Instead policy seems to offer disproportionate benefit to platform provider. 
        *   ...Notes that on iOS platform policy requires app developers who offer social login to also offer “Sign in with Apple”. Notes similarity to UA policy on what constitutes a valid login, and the possibility of using this to advantage “Sign in with Apple” for the web.
        *   ...The isLoggedIn explainer also speaks about “Browser-mediated logins”, and says that for websites where the developer does not want to use that, it would have to take the user through “a sequence of steps that the browser understands”. This could set up a situation where web developers either go with a smooth, low-friction “Sign in with Apple” that the UA accepts as a valid login, or otherwise be forced to go through a high-friction set of steps, all in the name of “defending against abuse”.
        *   ...Let’s start from the problem and aim at solutions for that problem.
    *   John Wilander: we have a session on bounce tracking later today. ITP shipped in 2018. Moz… none of those solutions solve the whole problem. Browser-mediated includes third-party password managers. It's not just the browser vendor. No one has said all 1p state ephemeral. Some lifetime cap. 
    *   Ben: what’s the difference?
    *   John: ephemeral means purged when you quit the browser. Capped means window, could be rolling if you interact within the period.
        *   (Comments on Zoom chat that not everyone uses "ephemeral" to mean this specific thing)
*   /scribe -> Jeffrey Yasskin
    *   Brian Campbell: Mention of jumping to solution prior to understanding and agreeing on the problem, really resonates. I think that's happening here. There are problem statements, but "logged in by default" as a problem is erroneously conflating storage access with logged in status. That jumps to isLoggedIn and presumes that'll be used for storage access/expiration. The problem may have been written down, but presupposes the solution.
    *   Michael Kleber: Wonder if solution space could be made less contentious by discussing what time limits we're actually talking about for persistence. John has mentioned 15 years or unlimited storage a bunch of times. As a browser maker, I understand why giving unlimited-length storage is a risk of resources, and something we want to avoid for privacy & user expectation reasons. Huge difference between 24h or 7d or 30d. Maybe if we pick a number to indicate what we're actually thinking about, folks who are opposed to limits would be happier.  \
For the paywall use case, I personally haven't encountered a paywall where the time horizon is longer than a month. Would everyone be satisfied if we said first party storage is temporary, but expiration time is ~30 days? If you haven't visited in 30 days, maybe storage goes away or hidden-by-default-with-user-action-to-get-it-back. 24h and 30d are very different from each other, and are much shorter than the 15y situation that I agree with wanting to avoid.
    *   Basile Leparmentier: Afraid of unintended consequences. If you push people to log in, then website gets a more-cross-site identifier, like an email address. This creates cross-site tracking. More concerned about website having my email than having a tracking id. Cure might be worse than the problem. 1 day limit still risks this.
    *   John Wilander: 
        *   Comments from Melanie and others: Logging in and logging out is one of the few things that users recognize on the web. Would be powerful to inform the browser about that state (is the site informing the browser). Would get the site, user, and browser closer to a shared understanding.
        *   One use case we haven't brought up in the explainer, pending more good adoption. If we can assume websites are using the signal, browsers can focus security resources better. I fear the next thing similar to Spectre, which scrambles the same-origin policy. CPU-per-site? Good to reason about the sites, maybe under 10, that the user has personal data on. We use scarce processing resources to put extra security resources on these. 
        *   Michael's request for more concrete limits: 15 years isn't just arbitrary, it's one of the dominant bounce trackers that sets 15 year cookies. ITP has a 30-day-of-browser-use expiry for classified domains, but lots of sites that get logins fall into that category. Brad Hill of Facebook @ TPAC said something like (check the notes) "this hasn't affected Facebook logins in a meaningful way." FB users re-engage on a less-than-30-day cadence, so they keep cookies. More recent changes delete script-writable data after 7 days of browser use. In-memory-by-default may be an end state: when you quit the browser, the browser forgets what you've done except where you're logged in.
        *   Re Basile: Agree that if we're too tight, it could push to increasing the amount of logging in with personal data.
*   From zoom chat on issue 15:
    *   Achim Schlosser: For the record, in the federated login case the signals are mainly to inform how a user is most conveniently addressed by a publisher. Did he already authenticate on a device ?-> if so pubs know that they can offer a user to sign in with an existing SSO account
    *   Mike O'Neill: Agree with Arthur, it should be a permission rather than an assertion
    *   Basile Leparmentier: isn't there a 13 month expiry by law at elast in europe?
    *   Achim Schlosser: Its a recommended practise by some DPAs
    *   Mike O'Neill: Non-consensual tracking is already a well recognised problem
    *   Valentino Volonghi to Everyone: Ben isn't saying that it's not a problem, just that the solution may not be appropriate 
    *   Mike O'Neill: French DPA says 6 months now
    *   Michael Kleber: (Not everyone uses "ephemeral" to mean that thing, of course!)
    *   Ben Savage: Yeah, that was not my understanding of the word "ephemeral"
    *   Mike O'Neill: number of days is too much for many. I would say 24 hours, with user ability to change default To get an exemption under ePrivacy it would probably have to be &lt;24hrs, with no ability to regenerate


### IsLoggedIn Part 2
Discussion leads: John Wilander, Melanie Richards
Scribe: Jeffrey Yasskin

*   [Support multi-login and multiple user profiles](https://github.com/privacycg/is-logged-in/issues/21)
    *   John Wilander: We've mostly talked about this in the explainer as, the API takes a username. We probably have to take a unique identifier for the account instead. isLoggedIn could support multiple logins that you could switch between. Or you could be just logged in to one at a time, but remember the existence of the other accounts. isLoggedIn is just about informing the browser about logged-in state. Not the login mechanism itself. But may want to inform the browser about these more detailed states.
    *   James Hartig: Having multiple logins is useful. gmail and Google docs, I can log into multiple accounts, like company + personal. Letting user see that and switch it in the browser is useful. But also don't want to create a tracking id. 
    *   John Wilander: Personally want browser UI to switch user. But that really changes the scope of the API significantly. It's just the site telling the browser that a user is logged in. If the browser can switch the user, browser would need to call back to the site to tell it what to do and listen to result.
    *   James Hartig: If they're just telling the browser that they're logged in, there's not much effect of sending a username.
    *   John: If we support that, we could have the browser reveal the trust token for the new username and indicate if they were logged in before (remember me).
    *   Arthur Edelstein: Accidental complexity: Opinion of the website and opinion of the user whether they're logged in. If we're adding a new API for this, less complex to align that with the API that does the login negotiation. They need to be in sync, and if we use separate APIs, they can disagree with each other.
    *   Melanie: Good thing to pull out. We've been thinking about it as something that can work alongside a privacy-preserving login flow. Some proposals in that space chase federated auth, which doesn't cover all login flavors. Do we need this new signal to cover those other login types?
    *   John: So many ways that sites decide a user is logged in. We're not trying to tell them which way is right or wrong. Some of them try to prevent use of password managers, which I don't like, but it's a choice they make. Some use WebAuthn, some have specific hardware tokens. Don't think we can tie a signal that can serve all websites to specific ways of logging in. Want to allow every website to get the isLoggedIn state, as long as they can convince the browser, which might be up to a prompt.
*   [Support "Remember me" functionality](https://github.com/privacycg/is-logged-in/issues/9)
    *   Melanie: Lots of activity on this one. Couple different questions, and some of the other proposals spun out of this one. Some sites log users out aggressively and want to present user challenges, but if the user has logged in previously, they relax some of that. The next time a site sets the bit, they could notice the user has logged in before and skip some challenges. \
Maciej pointed out that there's also the literal "remember me" box: is that something the API can cover? \
And is there a way to hide some state for next time instead of completely clearing it.
    *   John: Current flows supporting "remember me" mostly front-load the remember-me functionality. At load, they have a cookie, maybe show the username up front. Those sites will have to change. We only reveal the state after the site has set isLoggedIn again. Is that tough for sites, so isLoggedIn just won't let sites work the way they want?
    *   Kushal: Opportunity for the browser to store a hash, and let the site guess if the value matches?
    *   John: Interesting, have to think about it. Wouldn't be able to let them search. Say the browser clears all data except where I'm logged in, but remember me where I've asked to be remember. Come back to website, no cookies. Remember me token is stored but not revealed. Could that be solved by hashing?
    *   Kushal: If you're rate limiting, is there a reason to prevent this from being set prior to isLoggedIn?
    *   John: Intention is to be able to expire website data. If user doesn't re-engage, delete after 30 days. Could hold onto the remember-me token, and if the user logs in again, then resurrect the data, which convinces site the user's trusted. I'd assume browsers wouldn't delete data quite as aggressively as would cause problems for banks that aggressively log out, but users could still delete, and maybe would have a button to except sites that remember them.
    *   Jeffrey: Wanted to talk about existing flows like the bank and google sign where you effectively log in once using some really secure mechanism; 2fa or personal data questions.  Semantically you are logged into the site for as long as possible after that.  Site often pretend you logged out to ask you for a password, but still logged in.  just reauthenticating.  I would guess that these sites are not going to say that you are logged out until whatever happens to cause them to clear your 2factor data.  That demonstrates that this device is known.  That is going to collide a little bit with the UI.  The browser will say you are logged in because your 2factor stuff is remembered, and the site will say you are logged out.  But will use that to get the right data storage and the Ui that they want.
    *   John: would you agree that this feature could resolve that and give the user both things.  If we allow this to be a fully unique token generated by site could have unique id for the user.  String; set login [missed a bit], and that will give you all the trust you need.
    *   Jeffrey: Google multi login shows the list of the accounts logged in and you click on and ask you for password.  Banks remember username and ask just for password.  Won’t work with isloggedin, if tell user logged out, and give secret token back to get the state back.  Tell a white lie to the API and say user is still logged in, even though going to ask for reauthentication password.
    *   John: that would show.  Browser would show avatar somewhere.  And tap on it will show that site knows you are “Jeffrey”
    *   Jeffrey: for bank or google that would be the right UI.  But would start saying please reauthenticate rather than please re login again
    *   John: yes see that.  Reauth.  Reveals truth to the user - site may ask to confirm your password, but it already knows you. Avatar on your browser is there and it knows you are “Jeffrey”.  Don't think that is bad.
    *   Jeffrey: may be better, site more honest about why you are asking for the password
    *   Michael Kleber: Feel like the "remember me" case is unnecessary if we choose a flow like we were talking about before: sites remember for 30 days and after that, data is hidden with browser flow to resurrect the data. If you come back inside 30 days, it still knows you. If you come back later site+browser could ask if you're a returning user, and click here to remember who you are. David Benjamin said on Slack "do you want to remember data from last time you were here" is asking the user a question at the moment it makes sense for them to think about the answer. Asking about "do you want to remember this in the future" is asking about future feelings, which is hard, but "do you want to remember now" is asking about their current feelings. (Want once-a-month to work, so remember 35 days, not 30.) This state resurrection is a simpler mechanism than much that we're currently talking about.
    *   John: Interesting thought. Worried as a browser engineer about storing the full state, potentially for multiple accounts, and then switching it out in all active tabs at once. Replacing existing data: deleting it or keeping it as logged-out state? Could get weird and complicated to switch out the whole sessions.
    *   Michael: You're reading about our comments on requestStorageAccess(). ;-)
    *   John: There's a reason in Safari that cookies are either blocked or available. Cookies are enough for the site to pull up any state they need. If we go with a remember-me token, it's up to the site owner to handle that, but they're also empowered to handle that. Worried if user sets up a complicated shopping cart and then logs in, and it replaces their state.
    *   Valentino Volonghi: Feel the comments: prompting the user, "was this a real login". Worried about user experience. In latest MacOS, people complain about all the permission prompts. Unless a prompt is necessary for the user's steps, adding them doesn't improve people's experience of the web. 2) I want to figure out ways to have the browser take a more user-centric approach. But having the browser turn into an opaque container of what has happened in the past: hiding the relationship between the site and browser seems less secure. Attackers can install a bad browser that overrides logged-in experience. Amazon doesn't ask you to log in again unless you're doing something sensitive. If the browser makes that too opaque, attacker could create a browser shell that violates those specific areas.
    *   Melanie: Agree with worry about prompt fatigue. We need to find a good balance between putting user in control and spamming them.
*   [Cater for existing logins, a.k.a. upgrade to IsLoggedIn](https://github.com/privacycg/is-logged-in/issues/18)
    *   John: Posted the menu of options. Maybe this is a good time to talk about prompt fatigue. The upgrade is similar to non-mediated logins: no WebAuthn, credential-manager, etc. Login might have happened a long time ago or just now without involving the browser. Could prompt here, or could do something else. If this is tied to specific times for website data expiry and isn't providing other powerful features. Or only provides positive extra powers, then could say that it's a free thing to set w/o a prompt as long as it's within the time where the data would have expired anyway. If the site sets logged in for 14 days, and we'd have kept data for 30 days anyway, we could provide the browser-side logged-in UI, e.g. an avatar, but don't need a prompt. Would only need a prompt for unmediated logins that ask for more time than the default. The prompt could explain the expiration time too.
    *   Brian Campbell: Agree prompt fatigue is a real problem. But concerned about prompts that are indistinguishable from the consent being prompted on. If we're gating storage access on login, asking the user if they're logged in just after they think they logged in, it'll be confusing and not even get into the heart of storage access. Complexity of figuring out what login means: can't imagine a site asking for a year at once. Sites associate a risk metric to the login status to decide how much to prompt. If we prompt, it should be about the particular permissions being requested.
    *   John: Heard this request before, that maybe this should be a prompt for long-term storage. Always willing to hear those arguments and think about it again. But think that users actually know and understand the idea of logging in.
    *   Brian: True to some extent, but users have a simplistic mental model that doesn't match what's actually going on [what sites actually do?]. Don't think users actually know that. Many states of login, synchronizing client understanding with server understanding. Adding another component further complicates things.
    *   John: Envision a prompt about storage, very few people understand what that means or the consequences. As for syncing between servers and clients: have login cookies in the browser, but the user has used another browser to log out all devices. Browser with the login cookie has no way to know, and have to go back to the site. \
There are other uses for this. Storage is just one benign use. Browser can show an avatar. Signal is "this site knows who you are, and this is the username." It's useful for users to realize they're still logged into a place they didn't expect.
    *   Ben Savage: Cart is before the horse. The need to figure out what's a valid login is created by the threat to break user experiences if a site doesn't conform to the browser's expectation. Want to go back to figure out if there's consensus about the problem. Way ahead of ourselves. You're saying we could do cool things with an isLoggedIn signal, but I want to talk more about what problem we're solving.
    *   John: Hear your feedback, want to formalize the signal of whether a user is logged in or logged out.
    *   James Hartig: Re Ben: use case around requestStorageAccess: don't know if a user is logged in or not, and what UI to show user, and whether to try to requestStorageAccess(). Want a binary signal within a 3p widget that the user is logged in to the 1p of the same site, to drive the widget's UI.
    *   John: Developers asked for this. The problem, from Michael Kleber, is that that's also a user identifier if you get a bunch of isLoggedIn bits. Lots of ideas thrown out. "only 5 iframes can ask"? But does that encourage bad actors to exhaust the budget? I don't have good ideas here.
    *   James Rosewell: Re Ben and James Hartig: problem is storage access api. Problem is relationship between parties. Browser takes a view of what can and can't be achieved based on current state. Until we can find a policy around relationship between parties, we're not going to find a solution.
    *   Sam Weiler: Complaints about browser vendors using market positions to enforce things. From the user's perspective, if the browser isn't doing this for me, I have to manipulate the browser to do it for me: plugins, settings, etc. Less blameful to say that users are doing things that break things we like, so maybe we can give them things they'll like more. Instead of blaming browser vendors, could look at how to help the users.
    *   Melanie: Will open new issues on a bunch of these meta points.
*   [Prevent 1p websites from fingerprinting IsLoggedIn state in 3p iframes](https://github.com/privacycg/is-logged-in/issues/13)
    *   Punt to github, out of time.
*   [3p access to isLoggedIn](https://github.com/privacycg/is-logged-in/issues/19)
    *   Punt to github, out of time.
*   [Why expose IsLoggedIn state directly?](https://github.com/privacycg/is-logged-in/issues/14)
    *   Punt to github, out of time.

### Attendees
1. Achim Schlosser (European netID Foundation)
1. Andrew Knox (Facebook)
1. Aram Zucker-Scharff (Washington Post)
1. Arthur Edelstein (Mozilla)
1. Abhinav Sinha (PubMatic)
1. Allan Spiegel
1. Anne van Kesteren
1. Basile Leparmentier (Criteo)
1. Ben Savage (Facebook)
1. Brad Rodriguez (Magnite)
1. Brian Campbell (Ping Identity) 
1. Charlie Harrison
1. Chelsea Gong
1. Chris Needham (BBC) - W3C AC Rep for BBC, interested in impact of proposals on our website (across several domain names)
1. Chris Pedigo (DCN)
1. Christy Harris (Future of Privacy Forum)
1. David Benjamin (Google Chrome)
1. David Waite (Ping Identity) 
1. Erik Anderson (Microsoft)
1. Eric Mwobobia (ARTICLE 19)
1. Harshad Mane
1. Jack Frankland
1. Jade Kessler
1. James Hartig (Admiral)
1. James Rosewell (51Degrees)
1. Joe Lukas (Magnite)
1. John Sabella (PubMatic)
1. John Wilander (Apple WebKit)
1. Josh Karlin
1. Kate Cheney (Apple)
1. Kaustubha Govind (Google Chrome)
1. Kushal Dave (Scroll)
1. Marjin Kruisselbrink
1. Mark Xue (Apple)
1. Marshall Vale (Google)
1. Marçal Serrate (Hybrid Theory)
1. Melanie Richards (Microsoft)
1. Michael Kleber (Google Chrome)
1. Mike Lerra (Google)
1. Mike O’Neill (Baycloud)
1. Paul Bannister (CafeMedia)
1. Paul Marcilhacy
1. Raj 
1. Russ Hamilton (Google)
1. Sam Macbeth (Cliqz/Ghostery)
1. Sam Weiler
1. Scott Low (Microsoft)
1. Shigeki Ohtsu (Yahoo JAPAN)
1. Soujanya Kocheri(Magnite)
1. Steven Englehardt (Moilla)
1. Steven Valdez (Google)
1. Tanvi Vyas (Mozilla)
1. Theodore Olsauskas-Warren (Google)
1. Theresa O’Connor (Apple)
1. Thomas Steiner (Google)
1. Tim Cappalli (Microsoft Identity)
1. Valentino Volonghi (NextRoll) 
1. Wendy Seltzer (W3C)
1. Apascoe
1. Hannes
1. Jeddy
1. Joey Salazar (ARTICLE 19)  
1. Christine Runnegar (PING co-chair) (part meeting)
1. Henry Lau (Privolta) 
1. Sean Bedford (Facebook)
1. Jeffrey Yasskin (Google Chrome)
