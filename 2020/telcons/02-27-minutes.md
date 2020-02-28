# Privacy CG telcon - 27 February 2020

* Chair: Tanvi
* Scribe: Scott Low

## Attendees (sign yourself in)

* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* Scott Low (Microsoft)
* Pete Snyder (Brave)
* John Wilander (Apple)
* Brandon Maslen (Microsoft)
* Pranjal Jumde (Brave)
* Kushal Dave (Scroll)
* Arthur Edelstein (Mozilla)
* Ehsan Akhgari (Mozilla)
* Jack Frankland
* Francois Marier (Brave)
* Don Marti ()
* Diptendu Bhowmick (Amazon)
* Sam Macbeth (Cliqz)
* Nicolas Arciniega (Microsoft)
* James Hartig (Admiral)
* Wendy Seltzer (W3C)
* Christy Harris (Future of Privacy Forum)
* Steven Englehardt (Mozilla)
* Ketan Deshpande (Amazon)
* Joey Salazar (ARTICLE19)
* Konrad Dzwinel (DuckDuckGo)
* Aram Zucker-Scharff (The Washington Post)
* Wendell Baker (Verizon Media)
* Zach Edwards
* Bill Densmore, Information Trust Exchange Govng Assn
* David Dabbs
* Hannes Kuhl
* Sam Tingleff (IAB)
* Diptendu Bhowmick (Amazon)


## Agenda

* Introductions

* Proposals
  * [JS Isolation via Origin Labels and Membranes](https://github.com/privacycg/proposals/issues/3)
  * [Bounce Tracking Protection](https://github.com/privacycg/proposals/issues/6)
  * [Pop-up Tracking Protection](https://github.com/privacycg/proposals/issues/7)
  * [Fingerprinting resistance mode](https://github.com/privacycg/proposals/issues/8)
  * [Debugging mode](https://github.com/privacycg/proposals/issues/9)

* Any other business

## Introductions

* New members from Amazon, Article 19, Brave, Epsilon, IAB Tech Lab and the Future of Privacy Forum

## Proposals: 

### [JS Isolation via Origin Labels and Membranes](https://github.com/privacycg/proposals/issues/3)

* Pete: Goal here is to figure out a solution for trusted parties (browser, page, extension, etc.) to isolate scripts that get included.
    * Goal here is not to isolate script elements from each other (solved via closures, etc.) but to give a more trusted party access controls. This seems like an unsolved problem on the web platform, so this is a proposal to help solve that.
    * Would love feedback and thoughts and any guidance proposing new web standards
* Tanvi: Some conversation here on the issue. We spun up a repo for this discussion because there's been a fair bit of traction. From here, we can see if issues are filed and we can discuss some items within these issues. (Missed this due to mic noise)... would be great to create a forum to discuss this
* Pete: Would be great!
* Tanvi: Any disucssions that folks want to have on this topic right now?
* James (Admiral): Curious how Pete imagined getting opt into this. CSP (but publishers haven't used it today). How might we get opt in here? Especially since it seems like a lot of work for them to do here
* Pete: Proposal is meant to make opt in easier than CSP.
    * Can be enabled extension/vendor side
    * Facilitate shared policies - an EasyList style mechanism could be maintained. Sites could then ship this without needing to change the functionality of the page. Library authors could also ship a policy alongside their library and consumers could #include both of these.
* John: I brought this up with our team and discussed with JS Core folks. We're interested in this. I wanted to ask Brave if there is any working prototype. Do you intend to implement in V8? Where would we start working on this? How should we think about sharing test cases?
* Pete: I can't speak to committed resources. We're interested in implementing this at some point (perhaps mid summer, but TBD). If there are larger browser vendors interested in this, that would be a fantastic collaboration opportunity. This has mostly been in research so far.
* John: If you were to implement, do you expect this to be open source?
* Pete: Yes.

### [Bounce Tracking Protection](https://github.com/privacycg/proposals/issues/6)

* John: We're interested in seeing if other browsers are interested in tackling this. I know Brave has expressed interest on Twitter. Interested in knowing if any other browser vendor is interested in this and how we might move forwards.
* Tanvi: Existing proposals are interesting. Would be interested in collaborating
* Erik: From Microsoft are we saying implementor support to move this to a work item? Or is this just getting early interest/feedback?
* Tess: Either way would be fine. We can continue discussing as a proposal or move to a work item. We have interest from Brave and Apple, so that gets us over that bar
* Erik: We're certainly interested in understanding privacy improvements that can be done in a way that maintains compat and/or eliminates breakage. If other browsers are interested in shipping this, we'd love to provide feedback
* Pete: Brave has a reserach project that is mentioned in the issue. We look for instances of bounce tracking. This summer, we'll have a project spun up that will identify bounce tracking in another way. We also do some bounce tracking prevention through known parameters through known parameters that push data throuhg 1P origins. We'll share this data, but we're also interested in dynamic solutions and would be interested in collaborating on those
* John: Question for Pete. Did you test with various UA strings?
* Pete: Nope. We used the default Chromium UA. It would be easy to spoof the UA, but it would be harder to repeat in a different actual UA given that the crawling infrastructure isn't super portable
  [pes: would be extemely happy to share the existing measurement machinery, though it would not be out of the box compat with Safari / WebDriver]
* John: It would be interesting to try this in a browser where developers know that they can't reliably set 3P cookies (i.e. Safari)
  * We are seeing this today. This isn't a theoretical item that we'd like to discuss over months. If we (browsers with tracking prevention) want to prevent this, we need to move quickly on this
* Sam: Cliqz is interested in this. We can help the Mozilla folks on this since we're a fork of them
  [englehardt: We'd love that. We have recently landed cookie purging code (https://bugzilla.mozilla.org/show_bug.cgi?id=1594226)]

### [Pop-up Tracking Protection](https://github.com/privacycg/proposals/issues/7)

* Jack: This is related to bounce tracking. It's a way for third parties to gain access to their 1P storage. Instead of using `window.location` they use popups instead. `window.opener` enables this.
  * This has been brought up in other channels. Most of the proposals are considering removing access to `window.opener`, but this has had little appetite since it could break compatbility with OAuth.
  * The proposal I put is hopefull a little softer. If the popup has access to `window.opener` and it's not the same TLD, then it should be treated the same as a 3P iframe
* Erik: Curious if others have thoughts on cases where `window.open` is used as a regular browsing context. Is it reasonable for users to understand that this is running in a different mode? Feels like the sandbox attribute on iframes. Have other browser vendors looked at this? Does this need a UX affordance?
* Ehsan: Two issues
  * This proposal modifies the user experience that the browser presents to the user in that the popup that opens in a use case like login/payments does not have access to the 1P. If the user is logged into the provider, that provider website in the popup context will appear as logged out. This is a change from what browsers typically implement today
  * Browsers (at least FF) have been trying to remove the set of situations where opener references are granted to popups by accident. For example, adding the explicit opener argument to `window.open`. Also trying to remove `opener` reference from `target=_blank` windows. Motivation here is related to various efforts around setting up multi-process model for isolating web content into its own process depending on the origin it comes from. In the future, there will be fewer cases where the `opener` reference is just going to be there when the website has not asked for it. If we are going to make the heuristic depend on the opener reference, we should keep in mind that the set of situations where the opener reference is not going to be there will continually increase in the future and we should design the heuristic with that thought in mind
* Pete: I had a little bit of difficulty following the last comments due to audio issues. I wonder if it makes sense to segment based on who opened the popup.
* John: I think we discussed this. WebKit does not partition for windows that have been opened. The 1P in the window gets 1P storage access. I think this is a good issue that has been filed. I know Maciej has already expressed interest in working on the issue. We are going to have a look at this and see if we can reuse the compat fix we discussed two weeks ago (Storage Access API access after user interaction) to see if we can prevent these popups from communicating back to their parent window until a user has interacted with them
* Jack: Common use case is that a publisher embeds our 3P script. Publishers show a model asking users to subscribe. Pops a window that lets folks subscribe. `window.opener` is used to communicate back the subscription status once the popup is closed. We plan on rewriting this, but just a data point
* Aram: I have seen a number of OAuth flows that do this today without any user intereaction. This would be a concern. Is the end goal to change that behavior so that the user would have to interact with these popups? I don't think that's a bad idea, but something worth thinking about
* John: Our initial version of compat fix allowed the flashing of the popup without user interaction. We had already seen this happen and understood it's not good. We did that only to make sure that we shipped without breaking federated login flows. We reached out to the ones we knew about and told them we were going to remove the ability for them to flash popups and they complied with adding user interaction. Perhaps they only did this for Safari, which would be unfortunate, however, I don't think it would be a lot of work to get them to change this behavior because they understand that it looks shady
* Jack: I agree with the issues Ehsan raised. Moving towards a model where window.opener declines is preferable. Partitioning could be a nice stepping stone in providing a better UX. In conjunction with the Storage Access API, the implementor could ask to get access to their storage if it wasn't the case that only a user interaction would suffice. 

### [Fingerprinting resistance mode](https://github.com/privacycg/proposals/issues/8)

* Pete: Two purposes
  * To be a place to start incubating concrete fingerprinting preventions so that they can be moved to the correct working groups. This has been difficult in the past. We may have more success in the community by incubating these within interested browsers before moving them to standards
  * To see if there is value in a toggle where an implementor could enable all anti-fingerprinting mechanisms that it currently implements
* Maciej: I think this is good, but I'm not clear why it should be a mode. Fingerprinting resistance is a thing that should always happen. I don't know if a mode is a core aspect of this; I'd like to see evidence that a mode is needed
* Pete: Mode is less critical in my mind. Primary goal is to have a place where the community can incubate ideas before moving them to working groups
* Ehsan: I wanted to mention on experience in Firefox that tracking prevention provided this. We tried to advertise this to users. Cost benefit ratio is that if you turn the feature on, you get some privacy benefit, but you also encounter some breakage. There has not been a significant amount of user opt-in. I question the value of working on an opt-in mode around a privacy feature such as this. 
* Pete: Mode was partially strategic as there are some browsers who always want to do the most privacy-preserving thing, but there are other browsers who may want to ship these changes in say private browsing mode. I don't know if that's a good idea, but it seems worth having the flexibility.
* Pete: If it seems like there is more agreement on the idea for incubating mitigations, should this be one issue/deliverable? Or should each one be spun into its own?
  * Tanvi: That's a good question. Tess, do you have ideas?
  * Tess: I think it's reasonable to have a work item for in general. If there's a sufficiently complex one, then it may make sense to have one for that issue. It may make sense to start with a general one and then to spin things out of that if they are complex.
  * Tanvi: Does that work for you, Pete?
* Pete: Yes
* Tess: If you want to have a work item for general proposals, feel free. Then we can go from there.

### [Debugging mode](https://github.com/privacycg/proposals/issues/9)

* Pete: Proposal here comes out of PING conflicts with specs where spec authors would like to have some functionality with some functionality that will help them debug. These serve a useful purpose, but also come with non-trivial privacy risk. Can we solve both use cases with a not-standard mode that could be enabled say for automation or users who want to help improve the site?
  * In the same way that WebDriver is not a mode that users are in often, it may be useful to have this functionality standardized
* Erik: I'd be surprised if we could get universal agreement that certain items could not be allowed by default. It would be interesting to see if this debug mode should be detetable if other browsers wanted to offer different defaults. Does GREASE-ing make sense here? Say you're a major browser who wants to expose WebRTC debugging by default. Would GREASing help here so that it's only exposed for a small number?
  * Browser could also ask the user
* Pete: My initial reaction is that there's no reason to hide this. Having the browser say that I'm about to send back analytics information seems just fine. Permission also could work, though I haven't thought about this deeply.
* Tanvi: I had a question about this. Is this a debugging mode for widespread users to turn on? Or is this an E2E mode for debugging within a corporation? Or is this both?
* Pete: My personal opinion is that this would solve both use cases. The real goal here is to keep things that are only analytics based out of the common path
* Maciej: This is perhaps trivial, but it might explain the intent better. Perhaps we should call this Analytics or Telemetry mode. That way vendors who already believe analytics/telemetry are opt in can also ask users to opt into sending analytics/telemetry on the web.
  [pes: that sounds good to me]
* Pete: If this sounds like not a terrible idea, I'd be happy to do any of the following to move forward:
  * Build a list of functionality that PING has come across that would likely fall into this category
  * Missed this due to mic noise... enabling and disabling functionality?

### [isLoggedIn()](https://github.com/WebKit/explainers/tree/master/IsLoggedIn)

* John: https://github.com/WebKit/explainers/tree/master/IsLoggedIn
* Would be great to get feedback and collaborate on this further. (Lots of mic noise) 

### [Storage Access API](https://github.com/privacycg/storage-access)

* (Lots of mic noise) Need to follow up with Christine how to mute everyone. 
* John: There was one interesting thing towards the end of the last call. Ehsan said that Mozilla would revisit their decision to go page-wide scope once I made it clear that we do it page-wide for compatibility fix but not for the Storage Access API. I wanted to ask if there has been any progress here. What is Microsoft thinking here?
* Ehsan: We have yet to make any progress here unfortunately.
[Scott: I have no mic]
* Brandon: We're still looking to see if it makes sense to go page-wide. 
* John: Would it be useful for us to add some information to the explainer to outline how our compat exception works?
* Aram: Can we link to documentation? [Scott: Done]

### F2F
* Pete: Very interested in a F2F. Has there been progress here?
* Tanvi: Yes. The chairs are discussing a venue and a time for the F2F. We were thinking mid-April and are discussing venues. We'll get back to folks soon. If anyone wants to host, please feel free to let us know publicly. Once we have an idea of a venue, we'll send a Doodle to collect dates.

### [Client-side Storage Partitioning Proposal](https://github.com/privacycg/proposals/issues/4)
* Tanvi: Does anyone want to be an editor here?
* Tess: I'll ensure that Apple provides at least one editor for  the goals of W3C website redethis. Exact victim tbd :)
* Tanvi: Others please take a look and let us know if there is interest in being a co-editor

### Ad-hoc calls for specific topics

* Tanvi: We'd like to give experts a space to collaborate, discuss problems, and come up with solutions (even in the pre-proposal state)
    * We were thinking folks could file an issue requesting an ad hoc meeting and the chairs can help gather stakeholders and facilitate the discussion. Of course, we can also have F2Fs, but those only happen so frequently. This could be a good stop-gap solution. We don't have anything formal written for this yet, but for now, just filing an issue in the meetings repo sounds like a good next step. If there's another mechanism to do this, we'll let you know; we'll also send a mail with more details.

## Any other business

* Tanvi: Browser vendors speak up a lot in these calls. I'd like to encourage other folks to keep coming to calls and contributing as this helps us get an understanding of real-world impact
* Joey Salazar: Who should we talk to about wanting to be an editor?
* Tanvi: Please comment on the GitHub issue

