# 2023-10-12 Privacy CG
* Chair: Tess
* Scribe: Jeffrey

# Agenda
* Keying of "CHIPS" cookies should align with other state
* Fragment Directives API
* Extending Storage Access API (SAA) to non-cookie storage
* Third-party Cookie Access Heuristics explainer
* Opener Storage Partitioning
* Any other business

# Notes

## [Keying of "CHIPS" cookies should align with other state](https://github.com/privacycg/CHIPS/issues/40)
* [Link to presentation](https://docs.google.com/presentation/d/1X-duZz8YB09czfIgpqTWoAvCXqQjXuVh2lKBL3AzHo8/edit?usp=sharing)
* Aaron: Talking about ABA embeds == cross-site embeds with a same-site ancestor.
* Use case: 3p authenticatioSame-Site Embeds with Cross-Site Ancestors for CHIPSn like MS Authentication inside Salesforce inside MS Teams
* CHIPS are only available on the top-level site where they're created. Prevents joining across top-level domains.
* Cookies are currently douple-keyed on their host + partition key. We're proposing a bit of a change.
* 2 reasons: Security: without the ancestor bit, the ABA embed can clickjack.
* Aligning with other forms of partitioning.
* Discussion on privacycg/CHIPS#40.
* Want to add an ancestor bit to CHIPS. Now 3 parts of the key: host, top-level site, ancestor bit. Similar to partitioned storage, HTTP cache, and network partitioning.
* Implications for early adopters:
* Requires a migration of existing partition keys. This blocks use in ABA embeds but doesn't touch them otherwise.
* Needs a new implementation, but it's similar to another widely-used pattern.
* Developers in ABA scenarios should use Storage Access for the inner embed.
* Erik: Storage access requires script to be running, and sometimes cookies are set in the response. Did you consider another cookie attribute?
* Kaustubha: We consulted some Chrome security folks. they weren't happy enough with an attribute. We argued that Same-Site=None is that security opt-in. They wanted document-level security opt-ins. Cookie header isn't granular enough. Maybe some sites need it in some cross-site contexts, like ABA isn't ok, but C is trusted so ACA is ok. 
* There's a separate conversation about cross-site cookie standardization, which also covers this outside of CHIPS.
* There, requestStorageAccess will be automatically allowed, but it's how a site opts in for security purposes.
* Usually requestStorageAccess requires heuristics or user opt in, but in this case it's just a security protection.
* Erik: If there were a new Same-Site attribute, maybe that's enough? Or is the cookie attribute just not enough developer intent?
* Kaustubha: If you only set it while the cookie is set, that's not close enough to the actual use. requestStorageAccess() has more context.
* Lee: Why are we inventing a new flag, when for clickjacking we have CSP frame-ancestors, source-restrict, etc. We have ways to prevent embedding inside evil.com. Why isn't that enough?
* Kaustubha: I'm not as familiar with frame-ancestors. We'll take this back to our security folks. 
* Lee: I want to be in that follow-up discussion. It's very useful, and sometimes this group's solutions are ignoring that capability that already exists.
* Lee: Earlier it was mentioned that other browsers should look at this. Is Apple, especially iPhone, going to adopt CHIPS?
* Tess: Letting Anne expand, but short version is that we don't talk about upcoming releases.
* Anne: We don't have a position yet. We're concerned about the memory overhead. Want to reduce the limit to 1k. Might make us positive, but I need to double-check.
* Lee: We've been testing CHIPS, but for us it's a weird spot that on non-iPhone we can just say "don't use Safari", but on iPhone, the lack of support makes standards like this difficult. We can talk offline.
* Johann: Re frame-ancestors: We've written some of this up in a document that assessed the storage access security model. Artur Janc and I wrote it. There are some ok-ish restrictions for iframes. They don't always have a way to prevent the request from being sent. You can make CORS decisions based on the request coming in. Anything like this needs a preflight. Either you're already loaded and call requestStorageAccess, or we could send a separate preflight. Make sense?
* Lee: Not totally. Let's chat. Everything I've read about clickjacking is that frame-ancestors is the best protection. Before some of the work here on blocking 3p cookies, some auth providers added ways to set domains that could embed. At times we're going away from what has been recommended. I haven't seen anything that explains why that doesn't work.
* Anne: frame-ancestors doesn't protect against the outgoing request. frame-ancestors only works after the response comes back, so the initial request can still leak things.
* Lee: Not seeing the larger security benefit against the additional overhead.
* Johann: We need to weigh that. We thought the utility benefit was greater, but in discussing it with Safari and Mozilla, we got the impression we needed to change our position and not leave these vulnerabilities open. In the ABA case, dev might be surprised that the B sites can embed images, and that leaks login state. Might not protect against that.
* Lee: I want to read the writeup.
* Johann: It talks about the storage access security model. [Improving the Storage Access API security model](https://docs.google.com/document/d/1AsrETl-7XvnZNbG81Zy9BcZfKbqACQYBSrjM3VsIpjY/edit)
* Ben: From Firefox, we're supportive of adding this bit to the keying. Any lesson you have about the migration, we want to hear it. We'll have similar issues.

## [Fragment Directives API](https://github.com/privacycg/proposals/issues/40)
* Eli: A bunch of browsers have implemented the scroll-to-text-fragment spec. This kinda changed the URL standard to hide the text fragment. I propose to continue hiding the text directive but open up the rest of the directives. Want web apps to be able to access this part of the URL. Open to feedback on the good shape of this API. It's a good area. Intended use described by scroll-to-text is how I use it. We make a pseudo-user-agent, and this is a good place to put commands. I'll show how we use it.
* Eli: Consent manager. Lets you specify some fragment directives. Doesn't interfere with the rest of the customer's site.
* Eli: I suspect we should keep protecting the text directive, but maybe not even that.
* Tess: This is neat. Thanks for showing this. My initial instinct (with many things) is anxiety. We've only spec'ed one fragment directive, and in that case, we decided it shouldn't be exposed. Our track record is that 100% aren't exposed to sites. I'm worried that if we make this change to expose all new ones, we're tying our hands. Odds are that if we add more, we'll want to add more private ones.
* Eli: Use a prefix for the private ones. And also text.
* Jeffrey: Imitate Fetch's Sec- prefix?
* Eli: Not the most ergonomic, but it's probably fine.

## [Extending Storage Access API (SAA) to non-cookie storage](https://github.com/privacycg/proposals/issues/41)
* Ari: Discussed this at TPAC. requestStorageAccess currently deals with cookies. 3p context can call it, and if the origin. (For Chrome) in the same RWS, it's automatically granted, but other wise permission.
* Proposal is that you can also ask for storage, and you get a handle back which gives you the 1p storage bucket. Give sites an easier way to access data, with the same privacy protections as the existing thing.
* Ari: Right now it's an explainer, but we can write a PR.
* Lee: In the writeup, it mentions 1st-party sets, but I thought those were tabled.
* Ari: As far as I know, related-website sets are moving forward for the storage access API. Proposing to use the existing permission flow for requestStorageAccess().
* Johann: Chrome shipped FPS->RWS, and we're using it to grant requestStorageAccess() calls. But not pursuing the Same-Party cookie attribute. As Ari mentioned, this proposal works with whatever mechanism a browser uses to grant requestStorageAccess.
* Ben: Seems worth discussing in the group, if adoption as a work item is the question. There's no privacy loss from adding storages, over just cookies. Adding that flexibility lets developers avoid exposing things to servers. Downside is looping in the storage, especially local storage because it's sync. The way the API is structured to make it a developer opt-in is nice and mitigates some of that concern.
* Eli: I don't have any critiques, but this is wonderful for my use cases. For private data you want to share across websites and you have your own consent model. We had a prototype with FPS. This makes it possible to share the data without breaking the law. With cookie-only sharing, it has to hit the network, you can't share personal data. But with this you can store it locally. I think this is a privacy improvement. You can go to a site with our consent manager, if it's in a RWS, your choices will be shared within the set even if you haven't consented to share them yet.
* Johann: This use case isn't iffy at all.

## [Third-party Cookie Access Heuristics explainer](https://github.com/privacycg/proposals/issues/42)
* Johann: Anton presented this at TPAC. I'm not super prepared to present today. We're trying to standardize the cookie access heuristics that are in 2 browsers, and Chrome's prototyping them and trying to ship them. Want to document what's happening. Want it to be adopted by Privacy CG. Other browsers' opinions?
* Tess: My understanding from the relevant WebKit engineers & folk at Mozilla is that while there are such heuristics, those are seen as temporary compatibility shims, and we'd like them to go away over time. There's a risk in specifying them that we'll ossify them. Want others to chime in.
* Johann: One of our intents in standardizing this is to align on a way to get rid of them. They break some of the privacy and security goals from 3p cookie deprecation. They were necessary, but we want to get away from them eventually. It's massively adopted already. Websites have adopted this pattern, not just because it existed before, but they're writing documentation for how to build your website so it works in Safari. That's not a great situation. Writing up how it works and the fact that browsers want to get away from it. 3-year plan to take it away from the web seems preferable.
* Ben: I agree with Tess's statement that these were compat measures, and we don't want them long term. There's a risk in standardizing them that they'll ossify. If it's done well and with the right messaging, ti could be clear that it's temporary. Documenting how it'll go away is better than "here's how the heuristics should work." Pushing back on Johann, it's not that websites build for these heuristics; the heuristics were needed to get rid of 3p cookies in most contexts while a major browser hasn't gotten rid of 3p cookies.
* Johann: It turns out that the major browser also needs to keep websites working. We have to work together on this. 
* Ben: It's gone from a transition measure for us, and now Chrome needs a transition measure. We need to plan how it should work so we don't get stuck. 
* Johann: We're thinking about the Web Compat spec as a potential standardization target. Could go directly there, but some work in Privacy CG could help. Going directly to Compat means it doesn't have a home for discussion.
* Lee: Reiterating, from the perspective of people who own iframes, standardizing on heuristics isn't what we want. We want explicit APIs where we can guarantee seamless SSO. PArent site, frame, and auth provider are all known, and they can define relationships that consumers never have to worry about. As we work through the heuristics, want to know that an iframe that needs authentication won't be broken. Whatever's put forward also needs to have a non-heuristic approach.
* Brian: When we talk about ossification, there's the implication that a browser vendor might be signing up to support something longer than they want to. Don't know if that's a concern.

## [Opener Storage Partitioning](https://github.com/privacycg/proposals/issues/43)
* Ari: Presented at TPAC. Much less certain proposal: lot's up for grabs. The basic idea is that, from a Chrome perspective, now that there's storage partitioning, we want to think about other avenues of storage pollution. Say you have a 3p iframe that opens a 1p frame for its own origin. It can postMessage, do sync scripting. This is critical for login flows and payments, so we don't want to disable it entirely. But mitigate the ways it could be used for tracking. 2 categories:
* 1) Times you can sever the opening. When there's a cross-origin navigation in the opening frame, then does it make sense to preserve the opener relationship?
* 2) Looking at partitioning the storage for opened windows, which is quite difficult from a user experience perspective. Popups might have a different partition. Or start with the opener severed, and you have an API to re-join, which could create a prompt.
* Not proposing any particular plan, but want to work on it in the Privacy CG.
* Ben: Opener partitioning is something we've thought about in the past. It's a hard problem, and this takes some steps toward solving some of the issues we had when we first thought about tackling it. Haven't thought about it enough to have firm thoughts, but we should think about it.
* Ari: I don't mean to say that nothing is being proposed. No-one is wedded to the possible solutions here, but there are some solutions here.
* Ben: This is a good problem particularly related to navigational tracking tie-ins. It's in the wheelhouse of what we work on here.
* Tess: Would it be useful for us to ask folks from the other engines to socialize the possibilities here with colleagues, and see what people think is more promising, and bring that back for a future call?
* Ari: Yes, that makes sense. Want to prototype this in Chrome in the next month, and I'll open requests for position on Firefox + Safari.
* Tess: Everyone should chime in on github with preferred approaches.

# Attendees
* Erik Anderson (Microsoft Edge)
* Theresa O’Connor (Apple, TAG)
* Brian May (dstillery)
* Ari Chivukula (Google Chrome)
* Jeffrey Yasskin (Google Chrome)
* Russell Stringham (Adobe)
* Benjamin VanderSloot (Mozilla)
* Tim Huang (Mozilla)
* Chris Needham (BBC)
* Lee Graber (Tableau / Salesforce)
* Johann Hofmann (Google Chrome)
* Dylan Cutler (Google Chrome)
* Aaron Selya (Google Chrome)
* Kaustubha Govind (Google Chrome)
* Chris Fredrickson (Google Chrome)
* Eric Mwobobia (IS)
* Paul Zuehlcke (Mozilla)
* Tom Van Goethem (Google Chrome)
* Eli Grey (Transcend)
* Anne van Kesteren (Apple)
