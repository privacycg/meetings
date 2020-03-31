# Privacy CG telcon - 26 March 2020
* Chair: Erik
* Scribe: npdoty

## Attendees (sign yourself in)
* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* Pete Snyder (Brave)
* Christine Runnegar (PING, co-chair)
* John Wilander (Apple)
* Sam Weiler (W3C)
* Thomas Bergemann (Axel Springer)
* René Baudisch (Axel Springer)
* Carlos Bracho (Axel Springer)
* Peter Saint-Andre (Mozilla)
* Anuvrat Singh (Amazon)
* Nick Doty (UC Berkeley)
* Wendy Seltzer (W3C)
* Mark Xue (Apple)
* David Senecal (Akamai)
* Eric Lawrence (Microsoft)
* Kristen Chapman (Salesforce)
* Steven Englehardt (Mozilla)
* Melanie Richards (Microsoft)
* Brandon Maslen (Microsoft)
* Jack Frankland
* Don Marti
* Michael Ten-Pow (Amazon)
* Nicolas Arciniega (Microsoft)
* Diptendu Bhowmick (Amazon)
* Bill Densmore (Information Trust Exchange Governing Association)
* James Hartig (Admiral)
* Pranjal Jumde (Brave)
* Martin Meyer (Akamai)
* Bobby Holley (Mozilla)
* Aram Zucker-Scharff (The Washington Post)
* Wendell Baker (Verizon Media)
* Kushal Dave (Scroll)
* Ketan Deshpande (Amazon)
* Cathie Chen, Igalia
* Brian Kardell, Igalia
* Zach Edwards (Victory Medium)
* Joey Salazar (ARTICLE 19)


## Agenda
* Introductions

* Reminders:
    * Please apply the agenda+ tag to issues you would like to discuss in an upcoming telecon
         * Permissions issues?: please let the chairs know so we can help debug.
    * [Virtual F2F availability survey](https://github.com/privacycg/meetings/issues/3)
* Charter updates
    * [Charter now links to our security and privacy vulnerability reporting policy](https://github.com/privacycg/admin/issues/11)
* Discuss Safari's [changes announced 3/24](https://webkit.org/blog/10218/full-third-party-cookie-blocking-and-more/)
* Storage Access API
    * [SSO and autologin](https://github.com/privacycg/storage-access/issues/16)
    * [Opt-in mechanism for shared storage access](https://github.com/privacycg/storage-access/issues/17)
* AOB

## Introductions

new folks:
* Bobby Holley, Mozilla
* Cathie Chen, Igalia
* Brian Kardell, Igalia
* Eric Lawrence, Microsoft, privacy and networking, cookies
* Rene Baudisch, Axel Springer

## Reminders
### agenda+ tag

everyone should have access to add this tag, but if you don't, let the chairs know.

###  [Virtual F2F availability survey](https://github.com/privacycg/meetings/issues/3)

planning a virtual f2f, even though we would have preferred in person. take a look at the link, register what dates/times work for you. try to announce a date sometime next week.

#### JS Membranes call after this call
separate call planned for the Membranes proposal. 

* pes: way of isolating untrusted scripts from trusted scripts; trusted scripts can mediate the access that an untrusted script gets. - https://github.com/privacycg/js-membranes

there's already a repo and active discussion on github. separate call will use this zoom room after this one.

## Charter updates

trying to be more formal about security and privacy vulnerabilities that you want to report to the community. some feedback this morning (thanks npdoty).

## Discuss Safari's [changes announced 3/24](https://webkit.org/blog/10218/full-third-party-cookie-blocking-and-more/)

* wilander:  easiest to read the blog post to get a summary of the changes. 3rd-party cookies are blocked without exceptions. new expiry of non-cookie website data, based on 7-days of non-use in Safari. shipped protection against delayed bounce tracking.

* tanvi:  this change seems great. ITP-related attacks not possible any more because all cookies are blocked, but what about redirect chains or click IDs?

* wilander:  should not be any observable state in ITP now, but if there is, please file a bug. ITP is lazy, whereas cookie blocking in the past was stateful and immediate. referrer downgrades were also observable (as highlighted by google), which is no longer stateful.

* npdoty: you noted in the post that there would be automatic expiry of data; is expiring on a timeline a feature that sites would be interested in? (maybe [clear site data](https: //developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Clear-Site-Data) was along that line)

* wilander:  clear site data is part of it, but that means the site needs to be loaded so it can tell the browser when to expire things; but a user could casually visit a site and not go back. This is different than cookies, and there's no mechanism for it right now. We have talked about such things in the isLoggedIn proposal (cookies expire but other storage forms don't). It's a problem that sites can't do this, and that data can sit around for the life of the device.

* Kris: statistics on: 1) what percentage of 3rd-party cookies are still being set? (what fraction is this new feature blocking?). 2) localstorage used for caching:  what might happen to page-load times for visitors who return after a week?
* wilander:  don't capture a lot of telemetry down to URLs. lots of 3rd-parties are speculatively trying to set cookies all the time, even if it's not working. and some users will turn off ITP.
* wilander:  don't have performance data/benchmarks. page-controlled cache a very particular use case, not something you rely on but hope to.

* pes:  there are proposed standards from Web Performance to read the size of the cookies that are requested, which could reveal some data on @@ from the third party. I can try to keep the groups in sync.
* Aram: Web Performance cookie size proposal:  we should put links into the minutes here.
  * Resource Timing API: https://w3c.github.io/resource-timing/ <- can recover cookie size from transferSize - encodedBodySize
  * Relevant PING review issues: https://github.com/w3c/resource-timing/issues/222, https://github.com/w3c/resource-timing/issues/223
* pes: Would only be relevant to platforms that implement both Storage Access API and the relevant end points in Resource Timing API, so Gecko and Blink

* James:  news re PWAs? thoughts on storage management .persist, or explicit indicators from sites on offline-only apps.
* wilander:  absolutely interested in alternatives. want to expire storage when users don't intend the longer term storage. could have a prompt, although afraid of prompt fatigue. logging in to a website provides some intent, expectation about a transaction going to a logged in state. so isLoggedIn could be an indicator for long-term persistence of storage, and wouldn't need a separate permission. also updated the blog post to note that web apps installed to the home screen won't have the same expiration rules.
* James:  should Storage Access API include a parameter for lifetime?
* wilander:  Storage Access API is about third-parties asking for access to their first-party storage, and using that API would update the timestamp. but since Storage Access is about third-parties and expiration is more a concern about a first-party page that wants to retain data for a longer time.
* James:  maybe a first party could use the same API call to request longer storage?
* wilander:  there may also be a Storage permission spec in WHATWG (?)

* englehardt:  share your experiences with groups like this. but is there a better way to follow along to see whether sites are broken by referrer drop or storage expiration.
* wilander:  not sure we can share a list of websites, maybe we can ask the websites if they're okay with sharing. this feature has been in betas/previews for 2.5 months. got fewer issues filed than expected (~10). safari has blocked many third-party cookie sets already, so there weren't that many cases left. a good starting point is only allowing cookies set by those who already have them. we did reach out to developers when it seemed like a systemic problem. response from developers was largely positive, that they planned to make the migration anyway. likely to get different kinds of reports from different user groups (power users, consumers, corporate users).
* tanvi:  sites with breakage, were they popular or less popular? specific to a particular region? can we find the bugs without sites reporting it directly.
* wilander:  not sure what details I can reveal now.

* Kris:  Salesforce is a vendor used by clients on their sites. many clients don't test on previews, but report bugs back to Salesforce once features are deployed to a public channel. do people see lots of advance testing?
* wilander:  a long-standing concern for browser vendors that when we deprecate or change functionality, it's hard to communicate to the entire world of developers and users about an upcoming change. even TLS, with some coordination from browsers, it's been hard to communicate. should we start prompting users? log in console or developer tools about upcoming deprecation? very open to suggestions about how we should deal with this. could have longer previews/betas to get bug reports, but of course if people aren't using the betas then you won't get the signal.
* erik:  a common problem. Chrome and Edge sometimes use enterprise policies to give enterprises an opportunity to deploy a quick mitigation and then hopefully remove them after time to address a problem.

* briankardell:  is there data about when data evictions (7-day expiration) happen? there have been critiques of 7-day expiration because it's so different. what data went into that timeframe decision?
* wilander: based on cap from cookie storage, which was 7-days, set 1 year ago. (also set on link decoration). 7 day timeframe based on what we think people expect:  casual website visits and what people expect from their regular lives (like going into a retail store), what seems reasonable about how long someone would remember you from a store.
* briankardell:  is there data about how often data is removed from storage? a generic count about how often it happens?
* wilander:  don't have telemetry on specific URLs. don't have a total count.
* erik:  not aware of others, but an interesting discussion if that functionality would be deployed by other vendors. could try to evaluate how often people visit a site after a particular timeframe.
* briankardell:  or how frequently data is removed because of memory pressure?

* aram: what works for warning developers: console-level warnings have been really appreciated by our team at WaPo. we've had some success reaching out to partners and vendors about upcoming changes, asking them if they're prepared for it. a problem that people report a problem after the fact, could definitely help to do proactive outreach so that people are aware ahead of changes. we'd also be willing to help out with providing feedback, from advertisers/etc. maybe a beta version of a browser should emit warnings in non-developer-speak (Kris:  +1)

* Kris:  thanks, and we do try to proactively notify clients who use our services. many of our clients aren't web developers and might not understand even if they're notified ahead of time. Salesforce would be happy to work with web developers on giving more feedback, especially for non-developer sites.

* Peter Saint-Andre:  we (Mozilla) proactively reach out to partners and blog about them, but everyone's bandwidth is limited. I have a short list of people we work with closely, and happy to add people to that list.

* aram:  would be glad to work closely with Mozilla on changes and translate conversations we have to you or any browser. 

* wilander:  in addition to notice to developers, we also have active adversaries, developers using features specifically to track end users, and when providing advance notice, those attackers can use the time to prepare alternatives, may be using that against privacy on the Web.

## Storage Access API

### [SSO and autologin](https://github.com/privacycg/storage-access/issues/16)

issue filer not present

* wilander:  can continue in the github issue.

### [Opt-in mechanism for shared storage access](https://github.com/privacycg/storage-access/issues/17)

* wilander:  opt-in mechanism for all iframes at once? what happens to other iframes if all their storage is changed? offering a 3rd-party to say in its iframes that if another frame from my origin gets new storage access, I would like my frame's access to change. this would help developers to be prepared. could resolve some of the back-and-forth on requesting-iframe vs all-iframes.

* Erik:  any immediate thoughts?

* englehardt:  well-summarized. trying to decide. one option would be providing a notification to frames that they can get first-party storage access now.

## Any other business

* reminder:  ad hoc membranes proposal in this zoom at the top of the hour.

* tanvi:  next call, if there's a specific topic you want to discuss, please add the agenda+ tag.
