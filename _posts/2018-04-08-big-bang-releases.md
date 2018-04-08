---
layout: post
title: Big bang releases
description: When replacing legacy software, a common strategy is the so called big bang release. As we saw with Argenta, this can literally lead to a big bang. Is there any alternative?
image: /public/images/big_bang_releases/websnippet.jpg
title_image: /public/images/big_bang_releases/title_big_bang_release.jpg
---

When replacing legacy software, a common strategy is the so called big bang release. As we saw with Argenta, this can literally lead to a big bang. In a single weekend they replaced a core legacy system with a new one. It failed miserably, resulting in customers not being able to access their bank accounts. The release at Argenta was a very public failure, but they are not alone. A lot of companies have tried and failed replacing their legacy systems in a single go. Quite often multi-million euro projects don't even reach production. Years of development have been thrown in the trash attempting a replacement. Is there an alternative strategy?

A more agile approach to releasing consists of starting with a small release. A first minimal viable product is released in production within a couple of months. It might not have a lot of features or serve all your customers, but atleast it is live. It allows you to gather early feedback and continuously monitor the behavior. Once this version is released, you keep on extending. Every sprint you release a new increment. The smaller you can make the releases, the more you reduce the risk. Some companies release multiple times a day, in a fully automated fashion. Their releases are utterly boring, not the result of [military planning](https://www.linkedin.com/feed/update/urn:li:activity:6386286352039768064) or cause for celebration.

It is tricky to apply this strategy to replacing legacy systems. A first release without all the features of the legacy system is a hard sell. The solution? Don't replace, strangle.

# ![Strangulation](/public/images/big_bang_releases/title_strangulation.jpg)

When you apply [strangulation](https://www.martinfowler.com/bliki/StranglerApplication.html), you start with a release that just takes over one responsibility of the legacy system. Start with something that actually hurts, where you can create new business value.

![Step 1](/public/images/big_bang_releases/step1_strangulation.jpg)

Deploy this release _next_ to the existing legacy system. When strangling a legacy system, the legacy system keeps running until we are sure the new system took over all responsibilities and is performing well.

We don't want to bother the user with figuring out if he should use the new system or the legacy system. Instead we introduce a composite ui, which will show either an old legacy application screen or a new screen. Users will perceive a single integrated system, even if they notice the occasional difference between an old-fashioned legacy screen and a more modern screen of the new system.

![Step 2](/public/images/big_bang_releases/step2_strangulation.jpg)

Avoid implementing anything new in the legacy system. Instead prefer extending the new system, taking over responsibilities of the legacy system. Keep on releasing and monitor the effects and behavior. The smaller the release, the less risk there is. Note that you can prioritise on business value. You don't need to wait until everything is replaced before you deliver something new to your customers. You can do this continuously.

If you are replacing a big legacy monolith, consider splitting it up in a microservice architecture in the new system. It will allow you to scale the number of developers working on the new system, and give you a better handle on business complexity. Apply [Domain Driven Design](http://domainlanguage.com/ddd/) and [TDD](https://en.wikipedia.org/wiki/Test-driven_development) to avoid creating a new legacy system.

![Step 3](/public/images/big_bang_releases/step3_strangulation.jpg)

A big part of strangling the legacy system, is syncing data. When you change something in the new system, you will probably need to update something in the legacy system. Vice versa, if a user updates something in the legacy system, you'll need to notify the new system. In the next section we'll dive deeper in this aspect.

Avoid cutting inside the legacy system itself. That's where the dragons and unexpected behaviors are. Stay around the edges. As far as the legacy system is concerned, the new system is just another user. It doesn't even need to "know" it has less responsibilities. Some legacy screens might no longer be in use, but you don't need to spend the effort deleting them.

![Step 4](/public/images/big_bang_releases/step4_strangulation.jpg)

After a while the new system has taken over all responsibilities. It has proven itself by running in production continuously. Once you are there, turn off the legacy system and break out the champagne.

The same principle applies if your legacy system provides an API to other applications.

![Step 1 API gateway](/public/images/big_bang_releases/step1_api_strangulation.jpg)

 Introduce an API gateway providing the same legacy API. Gradually reimplement the legacy API by having it call the new API. New consumers of your API can directly call the new api, and take advantage of all the new features of your system. Legacy consumers shouldn't notice anything changed.

![Step 2 API gateway](/public/images/big_bang_releases/step2_api_strangulation.jpg)

You may never get rid of the legacy API itself (except if you have some control over _all_ your api consumers). That shouldn't stop you from turning off the old system.

# ![Event capture](/public/images/big_bang_releases/title_event_capture.jpg)

An important part of strangulation is being able to sync data with the legacy system. Old systems might not provide a nice API for everything, or they might not notify you of all the interesting events.

You should avoid adding logic all over in the new system that handles these limitations of the legacy system. Always keep in mind the legacy system is disappearing, so you should be able to easily remove any logic that is "legacy-specific".

![Anti corruption](/public/images/big_bang_releases/anti_corruption_layer.jpg)

Instead create an anti-corruption layer around the legacy system. It should pick up any events from the new system, and turn it into legacy commands. It should also pick up any data changes in the legacy system, and publish them as nice events on your messaging bus.

![Event capture flow](/public/images/big_bang_releases/event_flow_cdc.jpg)

If your legacy system doesn't publish events, you can deduce them from tracking changes in the database. There are some solutions on the market for doing this, they will for example integrate with the transaction log of your database. Google [Change Data Capture](https://en.wikipedia.org/wiki/Change_data_capture "CDC") for a variety of patterns and solutions.

# ![Master of data](/public/images/big_bang_releases/title_master_of_data.jpg)

The hard thing about syncing in a system is dealing with conflicts. If one system thinks `X = 1`, while another thinks `X = 2`, then who is right? Always consider who the _master_ is of a piece of data. There should only be one system that is allowed to update a piece of data. They are the "master of that data". Any other system might contain a copy of the data to do its work, but should ask the other system if they want to update it.

![Asset capture](/public/images/big_bang_releases/asset_capture.jpg)

Initially when strangling a legacy system, the legacy system will be master of all its data. Gradually the new system will take over this role. Figure out what kind of assets the legacy system has. Step by step, asset per asset have the new system take over.

# ![Conclusion](/public/images/big_bang_releases/title_conclusion.jpg)

In some cases thousands of man-years went into the development of a legacy system. You can't simply replace it in a single go without major risks. Applying the strangulation pattern allows you to reduce the risk and actually deliver business value sooner rather than later.