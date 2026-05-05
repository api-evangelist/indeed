---
title: "Normalized Entropy or Apply Rate? Evaluation Metrics for Online Modeling Experiments"
url: "https://engineering.indeedblog.com/blog/2025/11/normalized-entropy-or-apply-rate-evaluation-metrics-for-online-modeling-experiments/"
date: "Tue, 11 Nov 2025 06:16:53 +0000"
author: "Megan Chen"
feed_url: "https://engineering.indeedblog.com/feed/"
---
<h1>Introduction</h1>
<p><span style="font-weight: 400;">At Indeed, our mission is to help people get jobs. We connect job seekers with their next career opportunities and assist employers in finding the ideal candidates. This makes matching a fundamental problem in the products we develop. </span></p>
<p><span style="font-weight: 400;">The Ranking Models team is responsible for building Machine Learning models that drive matching between job seekers and employers. These models generate predictions that are used in the re-ranking phase of the matching pipeline serving three main use cases: ranking, bid-scaling, and score-thresholding.</span></p>
<p>&nbsp;</p>
<h1>The Problem</h1>
<p><span style="font-weight: 400;">Teams within Ranking Models have been using varying decision-making frameworks for online experiments, leading to some inconsistencies in determining model rollout – some teams prioritized model performance metrics, while others focused on product metrics. </span></p>
<p><span style="font-weight: 400;">This divergence led to a critical question: </span><b>Should model performance metrics or product metrics be the primary metric for success?</b><span style="font-weight: 400;"> All teams provided valid justifications for their current choices. So we decided to study this question more comprehensively.</span></p>
<p><span style="font-weight: 400;">To find an answer, we must first address two preliminary questions:</span></p>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">How well does the optimization of individual models align with business goals?</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">What metrics are important for modeling experiments?</span></li>
</ol>
<p><span style="color: #808080;"><i><span style="font-weight: 400;"><img alt="🍰" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f370.png" style="height: 1em;" /> We developed a parallel storyline of a dessert shop that hopefully provides more intuitions to the discussion: A dessert shop has recently been opened. It specializes in strawberry shortcakes. We are part of the team that’s responsible for strawberry purchases.</span></i></span></p>
<p>&nbsp;</p>
<h1>Preliminary Questions</h1>
<h3>How well does the optimization of individual models align with business goals?</h3>
<p><span style="color: #808080;"><span style="font-weight: 400;"><img alt="🍰" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f370.png" style="height: 1em;" /></span><i><span style="font-weight: 400;"> How much do investments in strawberries contribute to the dessert shop’s business goals?</span></i></span></p>
<p><span style="font-weight: 400;">To begin, we will review how individual models are used within our systems and define how optimizing these models relates to the optimization of their respective components. Our goal is to assess the alignment between individual model optimization and the overarching business objectives. </span></p>
<h3>Ranking</h3>
<p><span style="font-weight: 400;">Predicted scores for ranking targets are used to calculate utility scores for re-ranking. These targets are trained to optimize binary classification tasks. As a result, optimization of individual targets may not fully align with the optimization of the utility score [</span><a href="https://medium.com/pinterest-engineering/handling-online-offline-discrepancy-in-pinterest-ads-ranking-system-8fd662da4c2d"><span style="font-weight: 400;">1</span></a><span style="font-weight: 400;">]. The performance gain from individual targets may be diluted or omitted when used in the production system.</span></p>
<p><span style="font-weight: 400;">Further, the definition of utility may not always align with the business goals. For example, utility was once defined as total expected positive application outcomes for invite-to-apply emails while the product goal was to deliver more hires (which is a subset of positive application outcomes). Such misalignment further complicates translating performance gains from individual targets towards the business goals.</span></p>
<p><span style="font-weight: 400;">In summary, optimization of ranking models is </span><b>partially aligned</b><span style="font-weight: 400;"> with our business goals.</span></p>
<h3>Bid-scaling</h3>
<p><span style="font-weight: 400;">Predicted scores for bid-scaling targets determine the scaled bids: pacing bids are multiplied by the predicted scores to calculate the scaled bids. In some cases, additional business logic may be applied in the bid-scaling process. Such logic dilutes the impact of these models.</span></p>
<p><span style="font-weight: 400;">Scaled bids serve multiple functions in our system. </span></p>
<p><span style="font-weight: 400;">First, similar to ranking targets, the scaled bids are used to calculate utility scores for re-ranking. Therefore, for the same reason, the optimization of individual bid-scaling targets may not fully align with the optimization of the utility score.</span></p>
<p><span style="font-weight: 400;">Additionally, the scaled bids may be used to determine the charging price and in budget pacing algorithms. Ultimately, performance changes in individual bid-scaling targets could impact budget depletion and short-term revenue.</span></p>
<p><span style="font-weight: 400;">In summary, optimization of bid-scaling models is</span><b> partially aligned</b><span style="font-weight: 400;"> with our business goals.</span></p>
<h3>Score-thresholding</h3>
<p><span style="font-weight: 400;">Predicted scores for score-thresholding targets are used as filters within the matching pipeline. The matched candidates with scores that fall outside of the pre-determined threshold are filtered out. Similarly, these targets are trained to optimize binary classification tasks. As a result, the optimization of individual targets aligns fairly well with their usage.</span></p>
<p><span style="font-weight: 400;">In some cases, however, additional business logic may be applied during the thresholding process (e.g., dynamic thresholding), which may dilute the impact from score-thresholding models. </span></p>
<p><span style="font-weight: 400;">Further, the target definition may not always align with the business goals. For example, </span><i><span style="font-weight: 400;">p(Job Seeker Positive Response|Job Seeker Response) </span></i><span style="font-weight: 400;">model optimizes for positive interactions from job seekers. It may not be the most effective lever to drive job-to-profile relevance. Conversely, </span><i><span style="font-weight: 400;">p(Bad Match|Send)</span></i><span style="font-weight: 400;"> model optimizes for identifying “bad matches” based on job-to-profile relevance labeling, and it could be an effective lever to drive more relevant matches which was once a key focus for recommendation products.</span></p>
<p><span style="font-weight: 400;">In summary, optimization of score-thresholding models could be </span><b>well aligned or partially aligned</b><span style="font-weight: 400;"> with our business goals.</span></p>
<h2>What metrics are important for modeling experiments?</h2>
<p><span style="color: #808080;"><i><span style="font-weight: 400;"><img alt="🍰" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f370.png" style="height: 1em;" /> How do we assess a new strawberry supplier? </span></i></span></p>
<p><span style="font-weight: 400;">Let&#8217;s explore key metrics for evaluating online modeling experiments. Metrics are grouped into three categories: </span></p>
<ul>
<li style="font-weight: 400;"><span style="background-color: #d6f280;"><b>Model Performance</b></span><span style="font-weight: 400;">: measures the performance of a ML model across various tasks </span></li>
<li style="font-weight: 400;"><span style="background-color: #d2b4f0;"><b>Product</b></span><span style="font-weight: 400;">: measures user interactions or business performance</span></li>
<li style="font-weight: 400;"><span style="background-color: #ccffff;"><b>Overall Ranking Performance</b></span><span style="font-weight: 400;">: measures the performance of a system on the ranking task</span></li>
</ul>
<p><i><span style="font-weight: 400;">(You may find the mathematical definitions of model performance metrics in the </span></i><i><span style="font-weight: 400;">Appendix</span></i><i><span style="font-weight: 400;">.)</span></i></p>
<h3>Normalized Entropy</h3>
<p><span style="background-color: #d6f280;"><b>Model Performance</b></span></p>
<p><span style="font-weight: 400;">Normalized Entropy (NE) measures the </span><b>goodness of prediction</b><span style="font-weight: 400;"> for a binary classifier. In addition to </span><b>predictive performance</b><span style="font-weight: 400;">, it implicitly reflects </span><b>calibration</b><span style="font-weight: 400;"> [</span><a href="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf"><span style="font-weight: 400;">2</span></a><span style="font-weight: 400;">].</span></p>
<p><span style="font-weight: 400;">NE in isolation may not be informative enough to estimate predictive performance. For example, if a model predicts twice the value and we apply a global multiplier of 0.5 for calibration, the resulting NE will improve, although the predictive performance remains unchanged [</span><a href="https://quinonero.net/Publications/predicting-clicks-facebook.pdf"><span style="font-weight: 400;">3</span></a><span style="font-weight: 400;">].</span></p>
<p><span style="font-weight: 400;">Further, when measured online, we can only calculate NE based on the matches delivered or shown to the users. It may not align with the matches the model was scored on in the re-ranking stage.</span></p>
<h3>ROC-AUC</h3>
<p><span style="background-color: #d6f280;"><b>Model Performance</b></span></p>
<p><span style="font-weight: 400;">ROC-AUC is a good indicator of the </span><b>predictive performance</b><span style="font-weight: 400;"> for a binary classifier. It’s a reliable measure for evaluating </span><b>ranking quality</b><span style="font-weight: 400;"> without taking into account calibration [</span><a href="https://quinonero.net/Publications/predicting-clicks-facebook.pdf"><span style="font-weight: 400;">3</span></a><span style="font-weight: 400;">].</span></p>
<p><span style="font-weight: 400;">However, as calibration is not being accounted for by ROC-AUC, we may overlook the over- or under-prediction issues when measuring model performance solely with ROC-AUC. A model that is poorly fitted may overestimate or underestimate predictions, yet still demonstrate good discrimination power. Conversely, a well-fitted model might show poor discrimination if the probabilities for presence are only slightly higher than for absence [</span><a href="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf"><span style="font-weight: 400;">2</span></a><span style="font-weight: 400;">].</span></p>
<p><span style="font-weight: 400;">Similar to NE, when measured online, we can only calculate the ROC-AUC based on the matches delivered or shown to the users.</span></p>
<h3>nDCG</h3>
<p><span style="background-color: #d6f280;"><b>Model Performance</b></span> <span style="background-color: #d6f280;"><b><span style="background-color: #ccffff;">Overall Ranking Performance</span></b></span></p>
<p><span style="font-weight: 400;">nDCG measures </span><b>ranking quality</b><span style="font-weight: 400;"> by accounting for the positions of relevant items. It optimizes for ranking more relevant items at higher positions. It’s a common performance metric to evaluate ranking algorithms [</span><a href="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf"><span style="font-weight: 400;">2</span></a><span style="font-weight: 400;">]. </span></p>
<p><span style="font-weight: 400;">nDCG is normally calculated using a list of items sorted by rank scores (e.g., blended utility scores). Relevance labels could be defined using various approaches, e.g., offline relevance labeling, user funnel engagement signals, etc. Note that when we use offline labelings to define relevance labels, we can additionally measure nDCG on matches in the re-ranked list that were not delivered or shown to the users.</span></p>
<p><b>When model performance improves against its objective function, nDCG may or may not improve</b><span style="font-weight: 400;">. There are a few scenarios where we may observe discrepancies: </span></p>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">Mismatch between model targets and relevance label (e.g., model optimizes for job applications while relevance label is based on job-to-profile fit)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Diluted impact due to system design</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Model performance change is inconsistent across segments</span></li>
</ol>
<h3>Avg-Pred-to-Avg-Label</h3>
<p><span style="background-color: #d6f280;"><b>Model Performance</b></span></p>
<p><span style="font-weight: 400;">Avg-Pred-to-Avg-Label measures the </span><b>calibration performance</b><span style="font-weight: 400;"> for a binary classifier by comparing the average predicted score to average labels, where the ideal value is 1. It provides insight into whether the </span><b>mis-calibration</b><span style="font-weight: 400;"> is due to over- (when above 1) or under-prediction (when below 1). </span></p>
<p><span style="font-weight: 400;">The calibration error is measured in aggregate, which implies that the errors presented in a particular score range may be canceled out when errors are aggregated across score ranges. </span></p>
<p><span style="font-weight: 400;">The error is normalized against the baseline class probabilities, which allows us to infer the degree of mis-calibration in a relative scale (e.g., 20% over-prediction against the average label).</span></p>
<p><b>Calibration performance directly impacts Avg-Pred-to-Avg-Label. Predictive performance alone won&#8217;t improve it.</b></p>
<h3>Average/Expected Calibration Error</h3>
<p><span style="background-color: #d6f280;"><b>Model Performance</b></span></p>
<p><span style="font-weight: 400;">Calibration Error is an alternative measure for </span><b>calibration performance</b><span style="font-weight: 400;">. It measures the </span><b>reliability of the</b> <b>confidence of the score predictions</b><span style="font-weight: 400;">. Intuitively, for class predictions, calibration means that if a model assigns a class with 90% probability, that class should appear 90% of the time. </span></p>
<p><span style="font-weight: 400;">Average Calibration Error (ACE) and Expected Calibration Error (ECE) capture the difference between the average prediction and the average label across different score bins. ACE calculates the simple average of the errors of individual score bins, while ECE calculates the weighted average of the errors weighted by the number of predictions in the score bins. ACE could over-weight bins with only a few predictions.</span></p>
<p><span style="font-weight: 400;">Both metrics measure the absolute value of the errors, and the errors are captured on a more granular level compared to Avg-Pred-to-Avg-Label. Conversely, it could be difficult to interpret over- or under-prediction issues using the absolute value. Also, these metrics are not normalized against the baseline class probabilities.</span></p>
<p><span style="font-weight: 400;">Similar to Avg-Pred-to-Avg-Label, </span><b>calibration performance directly impacts Calibration Error. Predictive performance alone won&#8217;t improve it</b><span style="font-weight: 400;">.</span></p>
<h3>Job seeker positive engagement metrics</h3>
<p><span style="background-color: #d2b4f0;"><b>Product</b></span></p>
<p><span style="font-weight: 400;">Job seeker positive engagement metrics capture job seekers’ interactions with our products for the interactions that we generally consider to be</span><b> implicitly positive, </b><span style="font-weight: 400;">for example, clicking on a job post, submitting applications. The implicitness implies potential misalignments with users’ true preferences. For example, job seekers may click on a job when they see a novel job title.</span></p>
<p><b>When model performance improves against its objective function, job seeker positive engagement metrics may or may not improve.</b><span style="font-weight: 400;"> There are a few scenarios where we may observe discrepancies:</span></p>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">Misalignment between model targets and engagement metrics (e.g., ranking model optimized for application outcomes which negatively correlates with job seeker engagements)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Diluted impact due to system design</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Model improvement in the “less impactful” region (e.g., improvement on the ROC curve far from thresholding region)</span></li>
</ol>
<h3>Outcome metrics</h3>
<p><span style="background-color: #d2b4f0;"><b>Product</b></span></p>
<p><span style="font-weight: 400;">Outcome metrics measure the (expected) outcomes of job applications. The outcomes could be captured by employer interactions (e.g., employers’ feedback on the job applications, follow-ups with the candidates), survey responses (e.g., hires), or model predictions (e.g., expected hires model). </span></p>
<p><span style="font-weight: 400;">Employers’ feedback can be either implicit or explicit. When it is implicit, it again leaves room for possible misalignment with true preferences – for example, we’ve observed spammy employers who aggressively reach out to candidates regardless of their fit to the position. </span></p>
<p><span style="font-weight: 400;">Additionally, there are potential observability issues for outcome metrics when they are based on user interactions – not all post-apply interactions happen on Indeed, which could lead to two issues: bias (e.g., engagement confounded) and sparseness. </span></p>
<p><b>When model performance improves against its objective function, outcome metrics may or may not improve.</b><span style="font-weight: 400;"> There are a few scenarios where we may observe discrepancies: </span></p>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">Misalignment between model targets and product goal (e.g., one of the ranking model optimized for application outcomes while product specifically aims to deliver more hires)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Diluted impact due to system design</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Model performance change is inconsistent across segments (e.g., the model improved mostly in identifying the most preferred jobs, while not improving in differentiating the more preferred from the less preferred jobs, resulting in popular jobs being crowded out.)</span></li>
</ol>
<h3>User-provided relevance metrics</h3>
<p><span style="background-color: #d2b4f0;"><b>Product</b></span></p>
<p><span style="font-weight: 400;">User-provided relevance metrics capture match relevance based on user interactions on components that explicitly ask for feedback on relevance, for example, relevance ratings on invite-to-apply emails, dislikes on Homepage and Search.</span></p>
<p><span style="font-weight: 400;">User-provided relevance metrics often suffer from observability issues as well – feedback are optional in most scenarios and therefore sparseness and potential biases are two major drawbacks. </span></p>
<p><b>When model performance improves against its objective function, user-provided relevance metrics may or may not improve.</b><span style="font-weight: 400;"> For example, we may observe discrepancy when there’s misalignment between model targets and relevance metrics.</span></p>
<h3>Labeling-based relevance metrics</h3>
<p><span style="background-color: #d2b4f0;"><b>Product</b></span> <span style="background-color: #d6f280;"><b><span style="background-color: #ccffff;">Overall Ranking Performance</span></b></span></p>
<p><span style="font-weight: 400;">Labeling-based relevance metrics measure match relevance through a systematic labeling process. The labeling process may follow rule-based heuristics or leverage ML-based models.</span></p>
<p><span style="font-weight: 400;">The Relevance team at Indeed has developed a few match relevance metrics:</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">LLM-based labels: match quality labels generated by </span><b>model-based (LLM) </b><span style="font-weight: 400;">processes.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Rule-based labels: match quality labels generated by </span><b>rule-based</b><span style="font-weight: 400;"> processes.</span></li>
</ul>
<p><span style="font-weight: 400;">Similar to nDCG, we may also use labeling-based relevance metrics to assess </span><b>overall ranking performance</b><span style="font-weight: 400;">, e.g., GoodMatch rate@k, given the blended utility ranked lists.</span></p>
<p><b>When model performance improves against its objective function, labeling-based relevance metrics may or may not improve.</b><span style="font-weight: 400;"> We may observe discrepancies when there’s misalignment between model targets and relevance metrics.</span></p>
<h3>Revenue</h3>
<p><span style="background-color: #d2b4f0;"><b>Product</b></span></p>
<p><span style="font-weight: 400;">Revenue measures advertisers’ spending on sponsored ads. The spending could be triggered by different user actions depending on the pricing models, e.g., clicks, applies, etc.</span></p>
<p><span style="font-weight: 400;">Short-term revenue change is often driven by bidding and budget pacing algorithms, which ultimately influence the delivery and budget depletion. Long-term revenue change is additionally driven by user satisfaction and retention.</span></p>
<p><b>When model performance improves against its objective function, revenue may or may not improve.</b></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">For short-term revenue, bid-scaling models could impact delivery and ultimately budget depletion. However, the effect could be diluted due to system design, for example, when objectives for monetization have a trivial weight in the re-ranking utility formula, improvement to bid-scaling models may not have a meaningful impact on revenue..</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">For long-term revenue, we expect directionally positive correlation, though discrepancies could happen, e.g., when there’s misalignment between model targets and relevance, when impact is diluted due to system design.</span></li>
</ul>
<p>&nbsp;</p>
<h1>Evaluation Metrics for Online Modeling Experiments – Our Thoughts</h1>
<p><span style="color: #808080;"><i><span style="font-weight: 400;"><img alt="🍰" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f370.png" style="height: 1em;" /> Purchasing higher-quality, tastier strawberries may not always lead to more sales or happier customers. Consider a few scenarios:</span></i></span></p>
<ul>
<li><span style="color: #808080;"><i><span style="font-weight: 400;">The dessert shop started to develop a new series of core products featuring chocolates as the main ingredient. It becomes more important to find strawberries that offer a good balance in taste and texture with the chocolate.</span></i></span></li>
<li style="font-weight: 400;"><span style="color: #808080;"><i><span style="font-weight: 400;">The dessert shop started to develop a new series of fruit cakes. Strawberries are now only one of many fruits that are used.</span></i></span></li>
<li style="font-weight: 400;"><span style="color: #808080;"><i><span style="font-weight: 400;">There’s a recent trend in gelato cakes. The dessert shops decided to introduce a few gelato cakes that use much less strawberries in them. However, gelato hype may go away, and strawberry shortcake has always been our star product.</span></i></span></li>
<li style="font-weight: 400;"><span style="color: #808080;"><i><span style="font-weight: 400;">The dessert shop moved to a location which it’s much harder to find, losing significant regular customers.</span></i></span></li>
</ul>
<h2>Product Metrics vs. Model Performance Metrics</h2>
<p><span><span class="_2rkofajl _19pk1n1a _2hwxidpf _otyr1n1a _18u0idpf _bfhk1j28 _1e0c1o8l _s7n4nkob _v4pn1ule _tn8j166n _3naf166n _160jewfl _1theewfl _1kogh2mm _qyp0ewfl _nt751r31 _49pcglyw _1hvw1o36 _7ehi1vm4 _491113zc emoji-common-main-styles emoji-common-node emoji-common-emoji-image" title=":first_place:"><img alt="1st place medal" class="emoji" height="20" src="https://pf-emoji-service--cdn.us-east-1.prod.public.atl-paas.net/standard/ef8b0642-7523-4e13-9fd3-01b65648acf6/32x32/1f947.png" width="20" /></span></span> <strong>Top recommendation: Improvement over product metrics and guardrail on individual model performance metrics.</strong></p>
<p><span style="font-weight: 400;">As previously discussed, </span><b>optimizing individual models often doesn&#8217;t directly translate to achieving business goals</b><span style="font-weight: 400;">, and the relationship between the two can be complex. Therefore, making investment decisions based solely on improvements in model performance are likely ineffective.</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">When model targets and business goals are misaligned, it’s challenging to derive product impact from model performance impact. Making decisions based on product metric improvements ensures the impact is realized. </span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">When the model&#8217;s contribution is diluted due to system design, it prompts investment in bigger bets or alternatively in components that allow incremental impact to be realized more effectively. </span></li>
</ul>
<p><span><span class="_2rkofajl _19pk1n1a _2hwxidpf _otyr1n1a _18u0idpf _bfhk1j28 _1e0c1o8l _s7n4nkob _v4pn1ule _tn8j166n _3naf166n _160jewfl _1theewfl _1kogh2mm _qyp0ewfl _nt751r31 _49pcglyw _1hvw1o36 _7ehi1vm4 _491113zc emoji-common-main-styles emoji-common-node emoji-common-emoji-image" title=":second_place:"><img alt="2nd place medal" class="emoji" height="20" src="https://pf-emoji-service--cdn.us-east-1.prod.public.atl-paas.net/standard/ef8b0642-7523-4e13-9fd3-01b65648acf6/32x32/1f948.png" width="20" /></span></span> <strong>Secondary recommendation: Improvement over either product metrics or overall ranking performance metrics.</strong></p>
<p><span style="font-weight: 400;">Although optimizing individual models doesn&#8217;t always directly meet business goals, enhancing overall ranking performance through metrics like nDCG@k aligns better with business objectives. This approach also helps mitigate downstream dilution or biases, allowing us to concentrate on improving re-ranking performance more effectively. That said, when the downstream dilution is by design, we could be making ineffective investment decisions if simply ignoring their impact. </span></p>
<p><span style="font-weight: 400;">This approach may also be valuable</span><b> when the company temporarily focuses on short-term business goals. </b><span style="font-weight: 400;">It allows ranking to be less distracted and more focused on delivering high quality matches</span> <span style="font-weight: 400;">when products take temporary detours.</span><i></i></p>
<h2>Among Product Metrics</h2>
<p><span style="font-weight: 400;">Product metrics for experiment decision making should ultimately be driven by business goals and product strategy. We want to share a few thoughts on the usage of different types of product metrics:</span></p>
<p><span style="font-weight: 400;">User engagement metrics are </span><b>relatively easy to move in short-term experiments</b><span style="font-weight: 400;">. They are often a fair proxy for positive user feedback. However, we shall be mindful that they could have an </span><b>ambiguous relationship with long-term business goals </b><span style="font-weight: 400;">[</span><a href="https://www.kdd.org/kdd2016/papers/files/adf0853-dengA.pdf"><span style="font-weight: 400;">4</span></a><span style="font-weight: 400;">]. For example, clicks or applications are often considered as implicit positive feedback. However, it’s not very costly for job seekers to explore or even apply to jobs that they are not a great fit for. At the same time, exploring or applying to more jobs could be driven by bad user experiences (e.g., when they do not get satisfactory outcomes so far).</span></p>
<p><span style="font-weight: 400;">Relevance metrics, conversely, generally </span><b>align well with long-term business goals </b><span style="font-weight: 400;">[</span><a href="https://www.kdd.org/kdd2016/papers/files/adf0853-dengA.pdf"><span style="font-weight: 400;">4</span></a><span style="font-weight: 400;">]. Nevertheless, there are a few drawbacks: </span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">User-based relevance metrics could be hard to collect and measure in short-term online experiments.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Heuristic-based metrics may not have great accuracy.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Model-based metrics could be hard to explain and may carry inherent biases that are hard to detect.</span></li>
</ul>
<p><span style="font-weight: 400;">Therefore, we may consider leveraging a combination of user engagement metrics and relevance metrics to achieve a good balance in business goal alignment, observability, and interpretability.</span></p>
<p><span style="font-weight: 400;">Lastly, revenue is a </span><b>key performance indicator for the business in the long term</b><span style="font-weight: 400;">. However, short-term revenue may have an ambiguous relationship with long-term business goals as well</span> <span style="font-weight: 400;">[</span><a href="https://www.kdd.org/kdd2016/papers/files/adf0853-dengA.pdf"><span style="font-weight: 400;">4</span></a><span style="font-weight: 400;">]. We may drive more clicks or applications to increase spending in the short term, but if we are not bringing satisfactory outcomes to our users, they may not continue to use our product in the future. Hence, we recommend using revenue as a success metric </span><b>only when we are improving components within the bidding ecosystem</b><span style="font-weight: 400;">, where there are short-term objectives defined for the bidding algorithms to achieve. In all other cases, we may keep revenue as a monitoring metric to prevent unintended short-term harms.  </span></p>
<h2>Among Model Performance Metrics</h2>
<p><span style="font-weight: 400;">We recommend setting </span><b>guardrails on individual model performance with Normalize Entropy </b><span style="font-weight: 400;">— we don’t want to degrade either predictive performance or score calibration. In addition, monitor ROC-AUC to help with deep-dive analysis and debugging.</span></p>
<p><span style="font-weight: 400;">For bid-scaling models, we recommend we additionally monitor their calibration performance with Avg-Pred-to-Avg-Label. This allows for visibility into over- / under-predictions and scales the error to the baseline class probability.</span></p>
<p>&nbsp;</p>
<h1>References</h1>
<ol>
<li style="font-weight: 400;"><a href="https://medium.com/pinterest-engineering/handling-online-offline-discrepancy-in-pinterest-ads-ranking-system-8fd662da4c2d"><span style="font-weight: 400;">Handling Online-Offline Discrepancy in Pinterest Ads Ranking System</span></a><span style="font-weight: 400;"> </span></li>
<li style="font-weight: 400;"><a href="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf"><span style="font-weight: 400;">Predictive Model Performance: Offline and Online Evaluations</span></a><span style="font-weight: 400;"> </span></li>
<li style="font-weight: 400;"><a href="https://quinonero.net/Publications/predicting-clicks-facebook.pdf"><span style="font-weight: 400;">Practical Lessons from Predicting Clicks on Ads at Facebook</span></a><span style="font-weight: 400;"> </span></li>
<li style="font-weight: 400;"><a href="https://www.kdd.org/kdd2016/papers/files/adf0853-dengA.pdf"><span style="font-weight: 400;">Data-Driven Metric Development for Online Controlled Experiments: Seven Lessons Learned</span></a></li>
<li style="font-weight: 400;"><a href="https://link.springer.com/content/pdf/10.1007/s10994-009-5119-5.pdf"><span style="font-weight: 400;">Measuring classifier performance: a coherent alternative to the area under the ROC curve</span></a><span style="font-weight: 400;"> </span></li>
<li style="font-weight: 400;"><a href="https://assets.amazon.science/ed/07/7766149e4f8d80e4572af86c353b/how-well-do-offline-metrics-predict-online-performance-of-product-ranking-models.pdf"><span style="font-weight: 400;">How Well do Offline Metrics Predict Online Performance of Product Ranking Models? &#8211; Amazon Science</span></a><span style="font-weight: 400;"> </span></li>
<li style="font-weight: 400;"><a href="https://ora.ox.ac.uk/objects/uuid:2ba4b711-f8ca-43f9-be8b-15f61da891f5/download_file?file_format=application%2Fpdf&amp;safe_filename=neumann18c.pdf&amp;type_of_work=Conference+item"><span style="font-weight: 400;">Relaxed Softmax: Efficient Confidence Auto-Calibration for Safe Pedestrian Detection</span></a><span style="font-weight: 400;"> </span></li>
</ol>
<p>&nbsp;</p>
<h1>Appendix</h1>
<p id="Appendix"><strong style="font-size: 16px;">Normalized Entropy</strong></p>
<p>Normalized Entropy (NE) is defined as the following [<a class="_ymio1r31 _ypr0glyw _zcxs1o36 _mizu1p6i _1ah3dkaa _ra3xnqa1 _128mdkaa _1cvmnqa1 _4davt94y _4bfu1r31 _1hms8stv _ajmmnqa1 _vchhusvi _kqswh2mm _syaz14q2 _ect41gqc _1a3b1r31 _4fpr8stv _5goinqa1 _f8pj14q2 _9oik1r31 _1bnxglyw _jf4cnqa1 _30l314q2 _1nrm1r31 _c2waglyw _1iohnqa1 _9h8h16c2 _1053w7te _1ienw7te _n0fxw7te _1vhvg3x0" href="https://quinonero.net/Publications/predicting-clicks-facebook.pdf" title="https://quinonero.net/Publications/predicting-clicks-facebook.pdf"><u>3</u></a>]:<a href="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.29.56-PM.png"><img alt="" class="size-medium wp-image-4740 aligncenter" height="49" src="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.29.56-PM-300x49.png" width="300" /></a></p>
<p>where y_i is the true label, p_i is the predicted score, and p is the background average label.</p>
<p><em>Note: NE normalizes cross-entropy loss with the entropy of the background probability (average label). It’s equivalent to 1- Relative Information Gain (RIG) [</em><a class="_ymio1r31 _ypr0glyw _zcxs1o36 _mizu1p6i _1ah3dkaa _ra3xnqa1 _128mdkaa _1cvmnqa1 _4davt94y _4bfu1r31 _1hms8stv _ajmmnqa1 _vchhusvi _kqswh2mm _syaz14q2 _ect41gqc _1a3b1r31 _4fpr8stv _5goinqa1 _f8pj14q2 _9oik1r31 _1bnxglyw _jf4cnqa1 _30l314q2 _1nrm1r31 _c2waglyw _1iohnqa1 _9h8h16c2 _1053w7te _1ienw7te _n0fxw7te _1vhvg3x0" href="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf" title="https://chbrown.github.io/kdd-2013-usb/kdd/p1294.pdf"><em><u>2</u></em></a><em>]</em></p>
<p><strong>ROC-AUC</strong></p>
<p>The Receiver Operating Characteristic (ROC) curve plots true positive rate (TPR) against the false positive rate (FPR) at each threshold setting. ROC-AUC stands for Area under the ROC Curve.</p>
<p><em>Note: Given its definition, ROC-AUC could also be interpreted as the probability that a randomly drawn member of class 0 will have a score lower than the score of a randomly drawn member of class 1 [</em><a class="_ymio1r31 _ypr0glyw _zcxs1o36 _mizu1p6i _1ah3dkaa _ra3xnqa1 _128mdkaa _1cvmnqa1 _4davt94y _4bfu1r31 _1hms8stv _ajmmnqa1 _vchhusvi _kqswh2mm _syaz14q2 _ect41gqc _1a3b1r31 _4fpr8stv _5goinqa1 _f8pj14q2 _9oik1r31 _1bnxglyw _jf4cnqa1 _30l314q2 _1nrm1r31 _c2waglyw _1iohnqa1 _9h8h16c2 _1053w7te _1ienw7te _n0fxw7te _1vhvg3x0" href="https://link.springer.com/content/pdf/10.1007/s10994-009-5119-5.pdf" title="https://link.springer.com/content/pdf/10.1007/s10994-009-5119-5.pdf"><em><u>5</u></em></a><em>].</em></p>
<p><strong>nDCG</strong></p>
<p>nDCG stands for normalize Discounted Cumulative Gain. We define Discounted Cumulative Gain (DCG) at position k for a ranking list of query q_i as</p>
<div class="ak-renderer-extension ">
<div class="ak-renderer-extension-overflow-container">
<div class="_o5724jg8 _11rm1hrf _5xln4jg8 _1v0sze3t _tmbuze3t _og5autpp _oxx4utpp">
<div class="ap-container conf-macro output-block" id="ap-com.adaptavist.confluence.contentFormattingMacros__latex-formatting3575389327785151401">
<div class="ap-content " id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting3575389327785151401">
<div class="ap-iframe-container iframe-init" id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting__35c210b4" style="text-align: center;"><img alt="" class="size-full wp-image-4742 aligncenter" height="76" src="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.30.51-PM.png" width="269" /></div>
<div class="ap-iframe-container iframe-init" style="text-align: left;">y_i,j is the relevance label for j-th ranked item in query q_i. The gain function could be defined in different forms (e.g., linear form, exponential form).</div>
</div>
</div>
</div>
</div>
</div>
<p>Then, we normalize DCG to [0,1] for each query and define nDCG by summing the DCG values for all queries:</p>
<div class="ak-renderer-extension ">
<div class="ak-renderer-extension-overflow-container">
<div class="_o5724jg8 _11rm1hrf _5xln4jg8 _1v0sze3t _tmbuze3t _og5autpp _oxx4utpp">
<div class="ap-container conf-macro output-block" id="ap-com.adaptavist.confluence.contentFormattingMacros__latex-formatting6134312264106827989">
<div class="ap-content " id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting6134312264106827989">
<div class="ap-iframe-container iframe-init" id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting__f057812"><a href="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.31.20-PM.png"><img alt="" class="size-full wp-image-4744 aligncenter" height="79" src="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.31.20-PM.png" width="297" /></a></div>
</div>
</div>
</div>
</div>
</div>
<p>maxDCG is the DCG value of the ranking list obtained by sorting items in descending order of relevance [<a class="_ymio1r31 _ypr0glyw _zcxs1o36 _mizu1p6i _1ah3dkaa _ra3xnqa1 _128mdkaa _1cvmnqa1 _4davt94y _4bfu1r31 _1hms8stv _ajmmnqa1 _vchhusvi _kqswh2mm _syaz14q2 _ect41gqc _1a3b1r31 _4fpr8stv _5goinqa1 _f8pj14q2 _9oik1r31 _1bnxglyw _jf4cnqa1 _30l314q2 _1nrm1r31 _c2waglyw _1iohnqa1 _9h8h16c2 _1053w7te _1ienw7te _n0fxw7te _1vhvg3x0" href="https://assets.amazon.science/ed/07/7766149e4f8d80e4572af86c353b/how-well-do-offline-metrics-predict-online-performance-of-product-ranking-models.pdf" title="https://assets.amazon.science/ed/07/7766149e4f8d80e4572af86c353b/how-well-do-offline-metrics-predict-online-performance-of-product-ranking-models.pdf"><u>6</u></a>].</p>
<p><em>Note: “query” may not be relevant in all search ranking tasks. Based on the product&#8217;s design, we may replace it with suitable groupings. For example, for the homepage, we may group on “feed.”</em></p>
<p><strong>Avg-Pred-to-Avg-Label </strong></p>
<div class="ak-renderer-extension ">
<div class="ak-renderer-extension-overflow-container">
<div class="_o5724jg8 _11rm1hrf _5xln4jg8 _1v0sze3t _tmbuze3t _og5autpp _oxx4utpp">
<div class="ap-container conf-macro output-block" id="ap-com.adaptavist.confluence.contentFormattingMacros__latex-formatting2027803626901039227">
<div class="ap-content " id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting2027803626901039227">
<div class="ap-iframe-container iframe-init" id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting__300847ac"><a href="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.32.07-PM.png"><img alt="" class="size-full wp-image-4746 aligncenter" height="66" src="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.32.07-PM.png" width="287" /></a></div>
</div>
</div>
</div>
</div>
</div>
<p>where y_i is the true label, p_i is the predicted score.</p>
<p><em>Note: The percentage change in this value may not be fully informative since the ideal value is 1. To use it for experimental measurements, we may consider taking the Abs(actual &#8211; 1) or establishing alternative decision boundaries. </em></p>
<p><strong>Average/Expected Calibration Error </strong></p>
<p>Average Calibration Error and Expected Calibration Error are defined as the following [<a class="_ymio1r31 _ypr0glyw _zcxs1o36 _mizu1p6i _1ah3dkaa _ra3xnqa1 _128mdkaa _1cvmnqa1 _4davt94y _4bfu1r31 _1hms8stv _ajmmnqa1 _vchhusvi _kqswh2mm _syaz14q2 _ect41gqc _1a3b1r31 _4fpr8stv _5goinqa1 _f8pj14q2 _9oik1r31 _1bnxglyw _jf4cnqa1 _30l314q2 _1nrm1r31 _c2waglyw _1iohnqa1 _9h8h16c2 _1053w7te _1ienw7te _n0fxw7te _1vhvg3x0" href="https://ora.ox.ac.uk/objects/uuid:2ba4b711-f8ca-43f9-be8b-15f61da891f5/download_file?file_format=application%2Fpdf&amp;safe_filename=neumann18c.pdf&amp;type_of_work=Conference+item" title="https://ora.ox.ac.uk/objects/uuid:2ba4b711-f8ca-43f9-be8b-15f61da891f5/download_file?file_format=application%2Fpdf&amp;safe_filename=neumann18c.pdf&amp;type_of_work=Conference+item"><u>7</u></a>]:</p>
<div class="ak-renderer-extension ">
<div class="ak-renderer-extension-overflow-container">
<div class="_o5724jg8 _11rm1hrf _5xln4jg8 _1v0sze3t _tmbuze3t _og5autpp _oxx4utpp">
<div class="ap-container conf-macro output-block" id="ap-com.adaptavist.confluence.contentFormattingMacros__latex-formatting1788919645887834973">
<div class="ap-content " id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting1788919645887834973">
<div class="ap-iframe-container iframe-init" id="embedded-com.adaptavist.confluence.contentFormattingMacros__latex-formatting__39d50405"><img alt="" class="size-full wp-image-4748 aligncenter" height="125" src="https://engineering.indeedblog.com/wp-content/uploads/2025/11/Screenshot-2025-11-07-at-5.32.51-PM.png" width="255" />where M+ is the number of non-empty bins, S_m is the average score for bin m, A_m is the average label for bin m.</div>
</div>
</div>
</div>
</div>
</div>
<p><em>Note:</em></p>
<ul>
<li><em>Average calibration error is a simple average of calibration error across different score range bins</em></li>
<li><em>Expected calibration error is the weighted average of calibration error across different score range bins, weighted by the number of examples in the bin</em></li>
</ul>
<p> </p>
