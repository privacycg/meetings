# 27 October Privacy CG Meeting
* Chair: Martin Thomson
* Scribe: Sean Harrison

# Agenda
* [Consider building on top of Permissions instead of using the "storage access map"](https://github.com/privacycg/storage-access/issues/121)
* [Global Privacy Control](https://github.com/privacycg/proposals/issues/10)
* Any other business

## Notes
* [Consider building on top of Permissions instead of using the "storage access map"](https://github.com/privacycg/storage-access/issues/121)
    * [Ben] information update on the permission model of the storage access map, this is a closer representation of what is actually going on in firefox and chrome. And makes it easier to integrate into existing specs rather than adding something custom.
    * [Martin] Have you resolved on the issue of permissions query, is there the ability for sites to request the availability in the usual fashion
    * [Ben] Up in the air
    * [Jeffrey] On the question of query, anne filed a [bug (#388)](https://github.com/w3c/permissions/issues/388) in the permission API, UAs should have a way to respond prompt instead of denied when the user wants them to.
    * [Ben] This change would solve the issue
* [Global Privacy Control](https://github.com/privacycg/proposals/issues/10)
    * [Martin] Proponents of the spec have asked for some time to bring it into the Privacy CG, so we will go into what it is, and what the request looks like. Looking a bit into the future of the work and some things that are going on outside of the W3C (regulation, etc.). What we want out of this discussion, is this something we should be picking up, not digging into the weeds of the proposal. Things like the spec isn’t ready to pick up are fully valid.
    * [Aram] Quick overview of spec - Sharing screen of [[https://globalprivacycontrol.github.io/gpc-spec/](https://globalprivacycontrol.github.io/gpc-spec/) ]
        * This has a number of editors from a variety of different backgrounds, mainly want to talk through is the technical process this is going through.
        * Specify that the user can provide a signal to indicate they would like to exercise their privacy rights under a set of laws, e.g. the Do not sell preference in california. Preference is formalized into an HTTP header (sec header) The field has only 1 meaningful value, which is the number, which, if present, specifies the opt out preference of the user. This is intended to not to be defined with an extensibility mechanism. I.e this is what the value is and this is what you should expect. The works with a Javascript property (navigator level object) which can translate this header into a page level object. We considered just using the header, but after talking to people in the market, such as publishers (me). However there are times that these sites cannot read into headers in this way, so this Javascript property is needed. Value of the object can be read as a true/false
        * GPC support JSON file for the website to say they support GPC on their site. Last update to show the realities of the legal landscape at the time the JSON file was updated.
        * The spec is already in use at the level of some browsers and with some extensions, you can see them on the GPC website [link]. Already being read by a number of sites and publishers, it is being supported by a number of consent management platforms as well.
    * [Martin] What sort of adoption do you see for the well known resource? How much do we see the server side indication sent out
    * [Aram] We see this picked up in a number of location. One developer has set up a resource on twitter to tweet when sites adopt the well-known resource, had to be disabled due to volume of tweets. 
        * [https://twitter.com/gpcsup](https://twitter.com/gpcsup) 
        * [https://twitter.com/braedon/status/1576926046229737472](https://twitter.com/braedon/status/1576926046229737472) 
    * [Ralph] FYI - [https://well-known.dev/?q=resource:%22gpc%22#results](https://well-known.dev/?q=resource:%22gpc%22#results) 
    * [Justin] Some background -
        * Largely reminiscent of Do not track. That effort largely broke down to lack of consensus on it. However there are now a number of state laws that allow users to opt-out of data collection. But it is too much to expect users to scroll to the bottom of every site and figure out the opt-out. So there was a desire to find a way for users to represent this.
        * California's privacy law went into effect 3 /4 years ago. Do not sell should ~~not~~ be seen as a legally binding request to not sell the user’s data. Later this year they will start sending out letters to companies not respecting GPC from users. They have already done this to a company Sephora. 5 other states have adopted laws of this nature. Some have mentioned browser mechanisms, like universal opt out mechanisms and they need to be recognized as a legally binding out out from users (colorado). Colorado will host a registry of signals companies need to respond to, GPC would likely be one, but they could also bring back do not track. Other states neutral on universal signals. A number of bills from other states are popping up with Universal signal language. There is a value to standardize this so that there is an ease of implementation and compliance with these signals. So this is an effort to make it a smaller number of ways for users to express this.
        * Around the world things of this nature are also coming, Europe has Article 21 for opt out. Bermuda wants universal opt-out signals to be respected.
    * [Michael] On the topic of signal proliferation, and the spectre of a multitude of different signals. Admire your optimism that standardization helping, DNT vs GPC and browsers current implementation. If there is already one setting that already exists, why would this new one be the one that was respected by a large number of different legal jurisdictions. The Colorado registry fills me with dread rather than hope.
    * [Justin] Even if there was just 1 US standard that would help, but now there are 6 state laws so far. This is a degree of complexity that we are stuck with at this point. GPC was originally designed to work with California’s laws but is not California specific, Some states will decide it counts and others will decide it won’t. There is usually a directive for State AGs to provide parity with other laws. It took 20-30 years for states to get into this space, but they are here now. In regards to DNT, it has some legacy issues, currently just need to have it in privacy policy how you will handle it. It would make more sense for browsers to at least provide 1 signal to help with this. The registry may be long, but hopefully not. If things centered around a signal for the web that would help
    * [Aram] One main difference between this and DNT, is that this was built around a specific legal definition. This also has a much broader acknowledgement from major players. We have sign on from CMPs, which is the primary way that consent is managed on the web. We pushed this internally at the WP, and were one of the first to adopt it, because the simplicity and technical approach is a big requirement of what we want to support. There is another thing for privacy signals, but it is quite complicated, and has a delayed response as a result, and milliseconds cost us millions of dollars. GPC is very fast, there is no promise in the request. These are the reasons why we think it makes sense to advance it towards a standard and more broadly adopt it. The main thing here for us is that there a requirement for this sort of signal, not so in the past, and now that it exists we need some way to fulfill it with minimal impact on our business as possible.
    * [Ralph] FYI posted a [link](https://well-known.dev/?q=resource:%22gpc%22#results) in the notes to the well known scan. This is down the road, but what are the expectations around enforcement of this. Are you expecting companies to record the status of this information as they respond to them?
    * [Justin] Great question, but the answer largely depends on jurisdiction. We have asked colorado AG, we have asked for more specificity around this. For logout (optout?) do you need to propagate, how far. The language around opt-out mechanisms for Colorado is a couple sentences, so there is a bit of room. Sephora got fined, but the ruling didn’t specify how they might remediate their violation. Over time this is likely to expand absent a federal pre emptive bill
    * [Aram] Clear technical standards, the LSPA who is trying to release a second contract that covers all states in the US. 1 of 2 ways, a signal added to every piece of advertisement that anyone in any system could do. Or you would see a difference in the things that are called when an opted out users goes to your site. In theory both of these should be highly visible, but theoretically tools could be put in place to support this.
    * [Brian] Makes sense for this group to work on this, but what do we do in the case of a proliferation of signals? Do we get involved in each one? Or limit it?
    * [Ben] Mozilla is supportive of this as a work item for PrivacyCG, it seems to be moving forward, and it would make sense to have a unified browser understanding of this.
    * [Martin] You have chosen a specific framing around this, so in different jurisdictions it will have different practical effects for users. DNT had various technical issues, but there is a risk to users about making it clear to them what happens if they turn it on.
    * [Justin] If someone is in a state without these laws, it would have no impact and how to communicate this. Putting several years into DNT it would have been better if it worked (in having common agreement of what respecting a signal would mean across many jurisdictions), would have been better if the UN passed something, but this is where we are. This isn’t imposed by GPC, regulators imposed it, GPC is just trying to comply with it.
    * [Martin] Microsoft would try to take the maximum that California and make it a global level, so that they could have correct compliance, but does it make more sense to make it more distinct.
    * [Justin] Microsoft has the size and goodwill to try something like that, it is not a GPC issue. When do we have to comply the hardest, I don’t think this work item will make this more or less of an issue.
    * [Ari] Communicating to the user what GPC means to the user. It is mostly between the user and site, but what sort of burden per jurisdiction is being placed on the browser.
    * [Justin] For colorado, a general purpose user agent can’t turn on without users knowing about it. A service like duckduckgo or brave has said use is enough of a signal. DNT was meant to be a signal to AdTech, 3rd parties were best equipped to handle this. Regs now target publishers who are embedding these things, why the AG went after sephora rather than ad company, at least in california. This is something that has to be worked out by the state, complexity of world not my fault, california has sites display opt-out state, so will there be a cookie-esque banner to ignore opt out state in california. There could be a thing to show currently opted out to make sure it wasn’t a mis-click, but it depends on what the states decide to do.
    * [Peter] From a duckduckgo perspective we don’t want to deceive users about the GPC settings, we want to explain to users what this means and doesn’t mean, particularly that it depends on what jurisdiction you are in as to what the actual effect is.
    * [Martin] Thoughts on adopting the work? What concerns do you all have, and thoughts on managing them?
    * [Kaustubha] The only question is, do we have the right audience in this group, I don’t have much policy and legal expertise, a lot of eng background in this group. If this is around document loads and sub-resource loads, then this is the right place, but if there are other discussions to be had around jurisdictional information to websites. Do we have the right people from browser and site side.
    * [Aram] These legal questions are more about whether or not this is fulfilling something needed out in the wild. If this group agrees this is something needed out in the wild there isn’t much need to talk about other legal aspects of this, and just discuss the technical aspects of this spec. This is not a space we expect laws to happen
    * [Michael] I really have to disagree, if this signal we are sending have to respect some sort of users intent. If the UA is collecting the signal, then that means the UI we are putting up needs to have the user intent. I don’t think we as browsers can dodge the bullet about how this signal relates to laws
    * [Aram] There is some responsibility of browser UI, but that is not subject to this group. We are looking to specify an API and browsers decide what goes on one side of it, and websites what goes on the other, and we should not expand past that in this context.
    * [Brian] Similar concerns to Kaustubha, this is something that is extra browser and extends into many different domains. I think we should get a better idea of the limits around it. Do we just need GPC, or do we need more explicit interaction and consent from the user, implying we need UI, or like duckduckgo and brave do we take it. What does this signal mean, a la DNT.
    * [Justin] DNT broke down because there was some issue of what does DNT mean, this is being decided on a jurisdiction by jurisdiction basis, this is not something we need to deal with, it has already been decided, we just need to figure out the technical conveyance of this signal through the browser.
    * [Jeffrey] As justin and Aram said, we try to avoid specifying browser UI in the W3C, this isn’t an argument against adopting the spec as an incubation — incubations are a good place to experiment with specifying novel things. But with the laws, the browser needs to get user intent for these laws, which means the UI is involved, and if done wrong sites would ignore this because they would say it doesn’t hook into the laws correctly.
    * [Aram] I don’t agree
    * [Nick] We have some experience with browsers collecting user intent, far weightier intent, my browser can turn on my camera and reveal the inside of my home, or geolocation. Which in some ways is weightier. I think this is something that needs standardization and should be adopted
    * [Ben] Generally ditto to Jeffery, I would like to point to powerful permissions as a way that UI is defined in the W3C
    * [Allan] We have only been talking about browsers, is there any intent to expand to platforms.
    * [Aram] That is out of scope for this spec.
    * [Martin] Chairs will take this feedback and decide whether or not to adopt the work item. We will look also at where this could possibly move to in the future.
    * [Ralph] FYI an interesting gpc.json that explicitly says they (Money Talk News) do not support GPC [https://well-known.dev/resources/gpc/sites/moneytalksnews.com](https://well-known.dev/resources/gpc/sites/moneytalksnews.com) (at least today)


## Attendees
1. Ralph Brown (Brown Wolf Consulting)
2. Ben VanderSloot (Mozilla)
3. Brian May (dstillery)
4. Tim Huang (Mozilla)
5. Paul Zühlcke (Mozilla)
6. Ben Kelly (Google)
7. Ari Chivukula (Google Chrome)
8. Konrad Kollnig (Independent / University of Oxford)
9. Kale Smith (Roku)
10. Allan Spiegel (Adobe)
11. Jeffrey Yasskin (Google Chrome)
12. Steven Valdez (Google)
13. Sean Harrison (Google)
14. Kaustubha Govind (Google Chrome)
15. Matt Reichhoff (Google Chrome)
16. Justin Brookman (Consumer Reports)
17. Martin Thomson (Mozilla)
18. Eric Mwobobia (ARTICLE19)
19. Erik Anderson (Microsoft Edge)
20. Braedon Vickers
21. Sam Macbeth (DuckDuckGo)
22. Russell Stringham (Adobe)
23. Christine Runnegar (PING co-chair)
24. Sam Weiler (W3C)
25. Nick Doty (CDT)
26. Gaurav Chhawchharia (Yahoo)
27. Wendell Baker (Yahoo)
28. Aram Zucker-Scharff (The Washington Post)
29. Wendy Seltzer (W3C)
30. Michael Kleber (Google Chrome)
