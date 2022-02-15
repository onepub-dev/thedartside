# The hidden epidemic in coding

Coding is hard, expensive and time consuming.

So why do developers just love to do everything twice?

The epidemic of duplicated code is everywhere, it's in the codebase you are writing now and it's a problem that we have been trying to solve since the very beginnings of IT.

The very first programmers realised that they were having to write the same piece of code multiple times.  The very first attempts at code reuse were really just 'goto' statements in assembler. Store the current address in memory, jump to a piece of code and have the code return to the original jump site stored in memory.

The concept of a stack soon followed and the beginings of modern functions the earliest form of which was essentially a 'gosub'.

Yet still, some 60+ years later we are still writing the same code over and again.

I believe that  the reason why we are still writing the same code over and again can be summarised as:

* Information Asymmetry
* Complexity

Complexity can be considered a form of Information Asymmetry so perhaps we can summarise the problem down to one cause: Information Asymmetry.

So what do we mean by Information Asymmetry (IA)?

IA is a term that comes out of the field of economics and refers to situations where one entity has information that another entity doesn't.

An example of this is a Buyer and a Seller. A seller may be able to take advantage of a buyer if the seller has information that the buyer doesn't. This can also work in the other direction.

{% hint style="info" %}
Another name for Information Asymmetry is Insider Trading or IT for short ;)
{% endhint %}

IA can occur between teams in your organisation or even with your past self. Its common to see code written by a single developer having duplicate code. If the code base is large enough or developed over a long enough period you simply forget that you have written something.

{% hint style="info" %}
Your past self knows things that you don't know.
{% endhint %}

We can see the problems that IA cause every day in a project. &#x20;

Code duplication is so common we have terms for:

* Not invented here syndrome
* Re-inventing the wheel
* copy and paste programming
* scrounging

