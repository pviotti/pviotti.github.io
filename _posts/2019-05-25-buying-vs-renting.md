---
layout: post
title: "Buying vs renting: a simple model"
date: 2019-05-25 00:00
categories: money
comments: true
---

I have moved to Ireland less than one year ago.
While Ireland hosts some of the most beautiful natural landscapes I've ever seen
and some of the most welcoming and friendly people I've ever met,
it is also currently enduring a severe [housing crisis][irish-housing-crisis].
As a somewhat privileged expat, this affects me as I try to settle down
and find a place suitable for my medium (or even long) term needs.
The incredibly high renting prices, when affordable at all,
make the alternative of buying a house more attractive.
This is why I set out to try to find a quantitative answer to the following
question:

> at which point in time will it become more convenient applying for a
> mortgage loan to buy a house rather than keeping on renting an apartment.

So I created a spreadsheet to make some simple simulations based on the
following inputs:

 - current rent
 - maximum annual rent increase rate
 - amount of money spent so far in rent over the years
 - current savings
 - estimated interest on the savings
 - future monthly savings
 - (fixed) mortgage interest rate
 - duration of the mortgage (in years)
 - cost of the house/apartment to purchase
 - legal and other one-off expenses involved in the purchase

The result is a graph that looks like this:

![Rent vs Buying graph](/assets/rent_vs_buy-graph.png)

The blue curve represents the amount of money spent in rent as the years pass,
while the orange curve is the total amount of money paid as interest
should you apply for a mortgage at that point in time.
Their intersection is therefore the point in time at which, under the aforementioned
assumptions and inputs data, it becomes more convenient buying than renting.

You can download, use and modify the spreadsheet by clicking [here](/assets/Buy_vs_Rent-blank.xlsx).

## Disclaimer

While this simple spreadsheet has helped me achieving a (fake?)
sense of confidence in the decisions I have to make in the coming months about
my housing situation, it is by no means a complete and fully reliable model.
Notably, it does not account for several important variables, such as a
variable mortgage rate, the variance and the possible losses incurred when
investing savings in a financial product, appreciation or depreciation of the
house to be purchased, etc.
Finally, I do not claim expertise in matter of money investment and housing market,
therefore any life-changing conclusion you might draw solely from using this
spreadsheet should be challenged and re-examined in the light of proper, more reliable models.

**Update 12/2020**: [this blog post][medium-post] proposes another somewhat arbitrary rule to help on the
rent vs buy decision.


 [irish-housing-crisis]: https://www.irishtimes.com/news/ireland/ireland-s-housing-crisis-in-numbers-1.3842183
 [medium-post]: https://medium.com/makingofamillionaire/the-5-rule-2a0af60d345c
