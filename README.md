# HRLO-Hierarchical-Refinement-Logit-Optimization

Author: Alexander Ratajczak Age: 15 Date: 03/05/2026

This file serves as proof of my authorship, I will go over a brief explanation of HRLO in my own words, I'll also give a context as to what lead me to this idea.

Context:

The first thought that started this cascade of thoughts that eventually led to the creation of HRLO was the observation of the fact that most tokens weighed are completely useless thus wasting lots of precious computing power. I then thought how could I prevent such an awful amount of operations, I figured that it would have to be something with predicting what tokens should and should not be, but singular predictions of singular tokens would be even slower than the prior weighing. Then I realised that token vectors are grouped based on similarity, effectively meaning that I could theoretically somehow predict a group of tokens that should be avoided. That was the starting point of HRLO.

Brief explanation:

HRLO - Hierarchical Refinement Logit Optimization is based on checking which tokens most definitely shouldn't be considered. That's why I propose a cascade of filters allowing smart filtering without weighing each token. We have n filters where M0 is the base model, each layer predicts the one beneath it, M1 predicts M0, M2 predicts M1 etc. This allows for simple prediction without a massive computing cost. Next while running we look at the context and we take a value out of it which allows the first filter to create a key, a very broad estimation of where the tokens should be in the vector space, each filter makes this direction more and more precise. Finally after all the filters filter out unnecessary candidates we are left with K best candidates (K can be set heuristically). And the base model (M0) checks which of these candidates is the best token which becomes the output. But we come to 2 core problems: 1. What if Mn hallucinates and gives a completely wrong direction (common during inference), this is why I propose a veto system which uses a buffer (which is set heuristically) and based on the cost of the ideal token - buffer, we check if a value equal or bigger is achievable among the candidates. If not we use the veto system as feedback to gently nudge Mn which produces the key to a better direction. 2. What about rare tokens? Rare tokens are further away from the ideal common tokens, making them practically invisible to the filters, that's why we invent a variable of the scale of niche tokens, which will allow for rare tokens to be taken as seriously as common ones.

IMPORTANT:
The paper is mostly AI written, while the core concept remains mine, I needed AI to formalize the concept both via text and via mathematical documentation. I am currently studying the underlying mathematics and seeking academic collaboration for empirical validation.
