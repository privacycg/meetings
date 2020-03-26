# Privacy CG [JS Membranes](https://github.com/privacycg/js-membranes) meeting - 26 March 2020

Convener: Pete

Scribe: Tess

## Attendees

* Aram Zucker-Scharff
* Bill Densmore
* Bobby Holley
* Brian Kardell
* Carlos Bracho
* Cathie Chen
* Christine Runnegar
* Deian Stefan
* Erik Anderson
* harneet
* Jack Frankland
* James Hartig
* Michael Ten-Pow
* Pete Snyder
* Pranjal
* rbaudisc
* Sam Weiler
* Steven Englehardt
* Tanvi Vyas
* Tess O'Connor
* Thomas Bergemann
* Wendy Seltzer
* Zach Edwards

## Are there competing or related proposals out there?

pete: what are folks doing to contain third party scripts? or is this the only approach on the table these days

bobby: on github i proposed using a separate global

...: i have implementibility concerns with the proposal as written

james: we're a third party, but deal with a lot of first parties. i don't see them doing much to protect against third party scripts. they often have auditing teams that periodically look, but nothing real-time / active

pete: there's [cowl](https://www.w3.org/TR/COWL/) from Deian, but i don't think it's being actively pursued

deian: ended up going to iframes

...: there were patches to chromium, but [sribe missed]

...: label iframes or scripts and cowl restricts data transfer between labeled things

pete: i'm not aware of anything being actively pursued

aram: we do some of this. we've had a lot of security issues with adds in our mobile app, so we no-op a bunch of browser APIs in the ad iframes, we try to intercept privileges that are often available to ads. this has been pretty successful. often hacks, the ads are using javascript badly. we've had a lot fewer complaints from our users. we've been considering expanding this.

zach: i was really interested in COWL and have been reviewing it. working on a privacy schema. how did you submit COWL to Chromium? how/when did it stall out?

deian: it was personal communication with some folks. we still have the patches. they're not hard; it would be possible to rebase and fix them. it's just adding some new web apis and tweaks to what you can do when the label is a particular value. lots of the infrastructure is shared with CSP. can talk to you offline.

zach: i'll reach out, thanks

## Tracing lineage of function calls / code units

pete: how important is this? is any of this stuff useful if you don't have tracing information? i think there's still value, but i'm curious about what others think

bobby: i agree they're different concerns, though it depends a lot on what the actual membrane policy is.

...: the scope of that determines how many vectors there are for script to be passed around

pete: if you can get the labeling correct, you can carry the policy forward at run-time.

aram: it can be useful, but not necessarily required.

bobby: when you say "managing at runtime" what do you mean? when i think of lineage tracking, that's at runtime

Pete: if the membrane can see that a script is injecting a script, then the existing approach may allow for the membrane to carry forward the policy to that new code

Bobby: in the membrane proxy 

Deian: i think it gets a bit weird to track this in a single context if you're not passing things around. there are nodejs libraries that do async queueing. if you have a list of thunks to run later, it's hard to think about who you're running them on behalf of. i'm worried about stuffing too much into the policy.

pete: read of the room, it would be useful to have this if you only have the first-order code, but it would be more useful if you had more information

bobby: depends on the implementation (same / different global etc). it's difficult to say without talking about that.

## Issue #1: [Same or separate globals](https://github.com/privacycg/js-membranes/issues/1)

pete: i think we're talking about the same thing with different words.

bobby: my concern with the proposal is that it'll be difficult to implement correctly and performantly in an engine. the idea is that the untrusted scripts are run in the same global as the trusted script, but the membrane would be interposed, both between the script and its own object, as well as built-ins

...: we've dealt with things like this befoer, with windowproxy and our compartment infrastruture, where we separated all globals and we have an ES6 proxy layer between each of them

...: each of these was a very invasive change in the engine

...: in terms of making these guarantees from the engine, it requires advanced, invasive changes in the engine, that's quite difficult.

...: much easier to introduce those barriers between globals—that's what we do today

...: a more tractable version of this would be to re-use the browser's existing separation between globals

...: the first proposal requires the lots of injection etc. at various points in the engine

...: the second proposal would not require engine changes




pete: is there a fundamental capability difference between the two? or is it just an ease of implementation concern?

bobby: If you take assumption as you do do the interposition. could zoom out to a level where equivalent. but that interposition - i don’t know even where to start defining where all of those would be. difficult to see if equivalent or not.

deian: i think they're equivalent, but there's a major difference. I’m on the side of creating a fresh global. Edge cases are super hard to get right. fresh global can be populated with exactly what you want. the other way you need to remove the things you don't want. allowlist v. denylist kind of thing. a lot comes down to did you get the right thing in the membrane and did you populate the right thing. I don’t think much harder; hard part in the membrane. what you populate it with is the hard bit. then have to go run it on real code that does knarly things like monkey patch. without a fresh global, you'd spend years playign whack-a-mole. 

...: beyond everything needing to be wrapped, you also need to have invariants around handing objects between globals. is it just for the object, or its prototype chain, or...

brian: when i was reading the proposal, it seemed related to Realms but that word isn't used in it. can someone explain the relationship?

pete: two reasons it's not in there: realms aren't a done thing, and my intuition that if you define these things in terms of realms, it'll be hard to deal with things like the old prototype.js library, where things muck with lots of prototypes.



brian: if i include an ad module, and that module uses prototype, you want to limit that use to there and not affect the global

pete: the main requirement is to not rewrite code

...: have to impose policies on code without changing it

brian: in that case, it feels weird if they muck with the prototypes for everyone else

pete: the policy script would be able to say "yes, you can modify this prototype" as part of the policy

brian: if you wanted to make it local, the realms thing would also allow that

pete: i think they scrape down to the same thing, but it sounds like one would be much harder than the other

bobby: there's the proposal text, then there's this long github issue called questions, which has modified the proposal

...: boils down to this question of same v. different global

...: are these things equivalent?

...: there are three interesting cases: 1. allow, 2. deny, or 3. allow locally (so the script sees it but the rest of the page doesn't, two separate prototypes). clear differences between same & different globals in that third case.

...: if you have these things running in the same context and you rely on the proxy handler, if the untrusted script declares a new thing, you can emulate this with a proxy handler, but a separate global would get that for you for free. what are the default semantics that we want.

pete: is this a thing other folks would be interested in implementing?

bobby: we'd (mozilla) would probably be uninterested in implementing current proposal as stated. complexity way too high to make it work. happy to keep discussing. there's space for us to get realms and piggy-backing on that could be easier to implement cross-vendor

pete: are other vendors looking to solve this problem?

bobby: it's an interesting problem to solve and there's tremendous value there. can you design policy that is permissive enough to work but prescriptive enough? excited to see something there. leaving aside implementation concerns, i'm a blank slate and would be really excited to see progress in this space.
