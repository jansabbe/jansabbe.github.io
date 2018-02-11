---
layout: post
title: Product roadmaps relaunched
title_image: /public/images/product_roadmap_relaunched/product_roadmap_title.png
---

I recently read an interesting book [Product Roadmaps Relaunched](https://www.amazon.com/Product-Roadmaps-Relaunched-Direction-Uncertainty/dp/149197172X). In most companies, there is a desire to create a long-term roadmap. Quite often this results in quite a few conflicts with principles of agile software development. There is little point in creating a backlog of user stories for the next 3 years and planning them sprint-per-sprint. This book describes a better way of handling things. In this blogpost, I'll provide a short summary.

A product roadmap is fairly simple, consisting of a few elements. It is a great way to create alignment and focus within your company accross all divisions. Developers know _why_ they are creating something. Sales knows what we can promise, and what we can't. Product owners have an easier time prioritizing stories.

![Elements of a product roadmap](/public/images/product_roadmap_relaunched/example_product_roadmap.png)

Let's go over each element in a bit more detail.

## ![Product vision](/public/images/product_roadmap_relaunched/product_vision_title.png)

A product vision essentially explains why the product exist. It can be summarised in one sentence:

<b>For</b> (customer)<br>
<b>Who</b> (needs)<br>
<b>The</b> (product name)<br>
<b>Is a</b> (product category)<br>
<b>That</b> (benefit)<br>
<b>Unlike</b> (competitor)<br>
<b>Our product</b> (differentiates).<br>

It is a simple sentence, but it requires you to figure out who your target customer is, what they need, how your product will solve that need. It also requires you to figure out where your product fits in the market, who your competitors are and how you plan to differentiate from them.

## ![Objectives](/public/images/product_roadmap_relaunched/objectives_title.png)

You might be on the road to your ideal product vision, but how do you know you are driving in the right direction? Objectives are the goals of your product. An objective should help you measure the progress you are making towards your vision. If you release a new version of your software, what will be measurably different?

The book refers to the OKR (Objectives & Key Results) framework as a way to set these goals.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/mJB83EZtAjc?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Example:
* _Objective_: Lower costs
* _Key result_: Software hosting cost is reduced by 30%
* _Key result_: IT Operations cost is reduced by 90%

Note how we are focusing on the outcomes, not on the output. You might accomplish this objective by moving your software from your own datacenter to Azure and setting up a deployment pipeline using Visual Studio Online. The same could also be accomplished by moving to AWS and using Jenkins or AWS CodePipeline.

It's also interesting to look at it from your customer perspective. How would you reduce _their_ costs? For a data-entry application, your key result might be _end-user can enter 90% of the new data in less than 2 minutes_. Writing your objectives from the perspective of your customer,  enables you to share the roadmap with them.

## ![Themes](/public/images/product_roadmap_relaunched/themes_title.png)

Themes describe the high-level customer/system needs and problems you are trying to address. Themes should relate back to a certain objective(s). Instead of focussing on *what* you are going to do, you focus on the *why*. The *what* can wait until you are actually ready to tackle the problem.

![Future](/public/images/product_roadmap_relaunched/future_less_detailed.png)

For the current quarter, you would create a concrete backlog to actually solve a theme. Themes are split up in subthemes, and subthemes are split up in stories. Because stories are related to themes, and themes are related to objectives, we can prioritize them a little easier. For each story we can estimate what impact it will have on each business objective. It's hard to do this using absolute numbers, but t-shirt sizing works well here. The goal is to prioritise the stories relatively to eachother.

![T-Shirt Sizing](/public/images/product_roadmap_relaunched/tshirt_sizing.png)

By estimating effort and risk in a similar way, you can do a simple cost-benefit analysis. This results in a nicely prioritized list of stories.

![Priorities based on objectives and effort](/public/images/product_roadmap_relaunched/priorities_based_on_objectives_and_effort.png)

After releasing your user stories to production, you can measure how you are progressing on those key results. Maybe that minimal-viable-product was more than enough to achieve those objectives.

## ![Timeframe](/public/images/product_roadmap_relaunched/timeframe_title.png)

Don't try to figure out what you will release on 23th June in 2038. The further in the future you look, the more uncertainty you will encounter. Will your vision remain the same? Your objectives? The needs of your customer might change, competitors might come and go. All these things will have an impact on what you will need to do.

![Timeframe](/public/images/product_roadmap_relaunched/timeframes.png)

Make sure your roadmap reflects this inherent uncertainty. You can detail what you are doing this quarter. You can describe what problems you'll tackle the rest of the year, and what high-level issues or opportunities you see for the future.

A great way to figure out what themes to tackle first, is to create a [customer journey map](https://uxmastery.com/how-to-create-a-customer-journey-map/).

![Customer journey maps](/public/images/product_roadmap_relaunched/customer_journey.png)

How does your customer interact with your organisation or your product? Where does it hurt the most? This is the kind of excercise that can help shape themes, timeframes and even customer objectives.

## ![Disclaimer](/public/images/product_roadmap_relaunched/disclaimer_title.png)

It should be clear a product roadmap isn't something you can create once. You should revisit it regularly. Did your production releases have the expected result on your business objectives? Are those objectives still relevant? Were your effort/business value estimates realistic?

There are a lot of reasons why your product roadmap should be changed. So if you share your roadmap with your customers and stakeholders, do yourself a favor. Add a little _* subject to change_ at the bottom.

## Conclusion

[Product Roadmaps Relaunched](https://www.amazon.com/Product-Roadmaps-Relaunched-Direction-Uncertainty/dp/149197172X) is an excellent book. It is a fairly short read and gives some great practical advice on how to create a product roadmap in an agile world.