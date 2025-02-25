---
date   2015-05-10 09:00:00
---

Nobody likes estimating software projects, but it's a fact of life. While the <a href="https://twitter.com/search?q=%23NoEstimates&src=tyah" target="_blank">#NoEstimates</a> movement is gaining traction, the majority of software developers are still required to estimate tasks on a regular basis.

With this in mind, I'd like to explore a little known affect called 'anchoring', and how it affects our estimates.

# How old was Gandhi?

When we make estimates, we subconsciously tend to start with an initial value and make adjustments from there. If our initial 'anchor' is inaccurate, this adjustment is often not sufficient to bring the estimate 'back on track'.

This effect has been demonstrated in a number of experiments.

In one, <a href="http://soco.uni-koeln.de/files/jpsp73.pdf" target="_blank">Strack and Mussweiler (1997)</a> asked people to answer a series of questions in relation to some implausible 'anchors', for example:

> Was Gandhi older or younger than 9 years old?

or 

> Was Gandhi older or younger than 140 years old?

Although the answer to both of these questions are obvious, when subsequently asked for Gandhi's *real* age, those who were asked the first question estimated Gandhi's age significantly lower than those who were asked the second question!

# How long will this feature take?

This also affects us software developers.

There are a number of good studies that prove this, but <a href="http://www.cs.toronto.edu/~sme/papers/2005/ESEC-FSE-05-Aranda.pdf" target="_blank">Anchoring and Adjustment in Software Estimation</a> is a good one to read.

Software developers were given a project description document and asked to estimate the time it would take to complete the project.

The documents were identical, with the exception of one sentence buried on page 2 of the document. This was either a control, or one of the following:

>I admit I have no experience with software projects, but I guess this will take about **2 months** to finish.

>I admit I have no experience with software projects, but I guess this will take about **20 months** to finish.

The idea was that this sentence was not prominent enough to *consciously* influence the estimate.

Those that were shown the document with the **20 months** anchor estimated significatly higher than those given the **2 month** anchor.

The lower anchor caused estimates closer to the control group, but they were still lower.


# How to ask for estimates

If you're **pre-estimating stories in your backlog** before sprint planning.. you are anchoring, and will affect the estimate.

If you're a senior developer who is **asking for an estimate like this**:

> I've done this before so it would take me 2 days, but it could take you longer

.. then you are anchoring, and will affect the estimate.

Anchoring is why some people recommend 'planning poker'. This is where everybody around the table secretly picks their estimate for a given task from a selection of cards. 

The team then reveal their estimates at once and discuss the results to agree on a realistic number. That way one developer's estimate is not influenced by anybody else's.

<br>
If we are going to insist on estimating development tasks (and make planning decisions based on those estimates), we have to take great care not to accidentally influence those estimates.

