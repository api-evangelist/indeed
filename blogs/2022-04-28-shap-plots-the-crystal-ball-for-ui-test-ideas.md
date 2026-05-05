---
title: "SHAP Plots: The Crystal Ball for UI Test Ideas"
url: "https://engineering.indeedblog.com/blog/2022/04/shap-plot/"
date: "Thu, 28 Apr 2022 15:40:29 +0000"
author: "Vicky Zhang"
feed_url: "https://engineering.indeedblog.com/feed/"
---
<div class="wp-caption aligncenter" id="attachment_4573" style="width: 250px;"><img alt="" class="wp-image-4573 size-medium" height="300" src="https://engineering.indeedblog.com/wp-content/uploads/2022/04/crystal-ball-240x300.png" width="240" /><p class="wp-caption-text" id="caption-attachment-4573"><em>Photo by <a href="https://unsplash.com/@shambam">Sam</a> on <a href="https://unsplash.com/photos/SaFjBsJASOQ">Unsplash</a></em></p></div>
<p>&nbsp;</p>
<p>Have you ever wanted a crystal ball that would predict the best A/B test to boost your product’s growth, or identify which part of your UI drives a target metric?</p>
<p>With a statistical model and a <a href="https://towardsdatascience.com/introducing-shap-decision-plots-52ed3b4a1cba">SHAP decision plot</a>, you can identify impactful A/B test ideas in bulk. The Indeed Interview team used this methodology to generate optimal A/B tests, leading to a 5-10% increase in key business metrics.</p>
<h2 style="text-align: left;">Case study: Increasing interview invites</h2>
<p><a href="https://www.indeed.com/employers/interview?hl=en&amp;co=US">Indeed Interview</a> aims to make interviewing as seamless as possible for job seekers and employers. The Indeed Interview team has one goal: to increase the number of interviews happening on the platform. For this case study, we wanted UI test ideas that would help us boost the number of invitations sent by employers. To do this, we needed to analyze their behavior on the employer dashboard, and try to predict interview invitations.</p>
<h3><img alt="Employer using Indeed Interview to virtually interview a candidate." class="alignnone wp-image-4565 size-full" height="1130" src="https://engineering.indeedblog.com/wp-content/uploads/2022/04/Employer-using-Indeed-Interview-to-virtually-interview-a-candidate..png" width="1600" /></h3>
<h3>Convert UI elements into features</h3>
<p>The first step of understanding employer behavior was to create a dataset. We needed to predict the probability of sending interview invitations based on an employer’s clicks in the dashboard.</p>
<p>We organized the dataset so each cell represented the number of times an employer clicked a specific UI element. We then used these features to predict our targeted action: clicking the <strong>Set up interview</strong> button vs. not clicking on the button.</p>
<p><img alt="Set up interview button on the employer dashboard" class="alignnone wp-image-4569 size-full" height="175" src="https://engineering.indeedblog.com/wp-content/uploads/2022/04/Set-up-interview-button.png" width="631" /></p>
<h3>Train the model on the target variable</h3>
<p>The next step was to train a model to make predictions based on the dataset. We selected a tree-based model, <a href="https://catboost.ai/">CatBoost</a>, due to its overall superior performance and ability to detect interactions among features. And, just like any model, it works effectively with our interpretation tool – <a href="https://github.com/slundberg/shap">SHAP plot</a>.</p>
<p>We could have used correlation or logistic regression coefficients, but we chose SHAP plot combined with a tree-based model because it provides unique advantages for model interpretation tasks. Two features with similar correlation coefficients could have dramatically different interpretations in SHAP plot, which factors in feature importance. In addition, a tree-based model usually has better performance than logistic regression, leading to a more accurate model. Using SHAP plot combined with a tree-based model provides both performance and interpretability.</p>
<h3>Interpret SHAP results into positive and negative predictors</h3>
<p>Now that we have a dataset and trained model, we can interpret the SHAP plot generated from it. SHAP works by showing how much a certain feature can change the prediction value. In the SHAP plot below, each row is a feature, and the features are ranked based on descending importance: the ones at the top are the most important and have the highest influence (positive or negative) on our targeted action of clicking <strong>Set up interview</strong>.</p>
<p>The data for each feature is displayed with colors representing the scale of the feature. A red dot on the plot means the employer clicked a given UI element many times, and a blue dot means the employer clicked it only a few times. Each dot also has a SHAP value on the X axis, which signifies the type of influence, positive or negative, that the feature has on the target and the strength of its impact. The farther a dot is from the center, the stronger the influence.</p>
<p><a href="https://engineering.indeedblog.com/wp-content/uploads/2022/04/SHAP-plot.png"><img alt="SHAP plot displaying features A-O ranked by descending influence on the model (regardless of positive or negative). Each feature has red and blue dots (feature value) organized by SHAP value (impact on model output). Features outlined in red: A, B, D, F, H, I, K, L, and N. Features outlined in blue: E, G, M, and O." class="alignleft wp-image-4571 size-large" height="643" src="https://engineering.indeedblog.com/wp-content/uploads/2022/04/SHAP-plot-1024x643.png" width="1024" /></a></p>
<p class="image-caption" style="font-style: italic; text-align: center;">SHAP plot with features outlined in red for positive predictors, and blue for negative predictors</p>
<p>Based on the color and location of the dots, we categorized the features as positive or negative predictors.</p>
<ul>
<li><strong>Positive Predictor</strong> &#8211; A feature where red dots are to the right of the center.
<ul>
<li>They have positive SHAP value: usage of this feature predicts the employer will <strong>send</strong> an interview invitation.</li>
<li>In the SHAP plot above, Feature B is a good example.</li>
</ul>
</li>
<li><strong>Negative Predictor</strong> &#8211; A feature where red dots are to the left of the center.
<ul>
<li>They have negative SHAP value: usage of this feature predicts the employer will <strong>not send</strong> an interview invitation.</li>
<li>Feature G is a good example of this.</li>
</ul>
</li>
</ul>
<p>Red dots on both sides of the center are more complex and need further investigation, using tools such as dependency plots (also in SHAP package).</p>
<p>Note that this relationship between feature and target is not causal yet. A model can only claim causality when it assumes all confounding variables have been included, which is a strong assumption. While the relationships <em>could</em> be causal, we don’t know for certain until they are verified in A/B tests.</p>
<h3>Generate test ideas</h3>
<p>Our SHAP plot contains 9 positive predictors and 4 negative predictors, and <strong>each one is a potential A/B test hypothesis</strong> of the relationship between the UI element and the target. We hypothesize that positive predictors boost target usage, and negative predictors hinder target usage.</p>
<p>To verify these hypotheses, we can test ways to <strong>make positive predictors more prominent</strong>, and direct the employer’s attention to them. After the employer clicks on the feature, we can direct attention to the target, in order to boost its usage. Another option is to test ways to <strong>divert the employer&#8217;s attention away from negative predictors</strong>. We can add good friction, making them less easy to access and see if usage of the target increases.</p>
<h4>Boost positive predictors</h4>
<p>We tested changes to the positive predictors from our SHAP plot to make them more prominent in our UI. We made Feature B more prominent on the dashboard, and directed the employer’s attention to it. After the employer clicked Feature B, we showed a redesigned UI with improved visuals to make the <strong>Set up interview</strong> button more attractive.</p>
<p>The results were a 6% increase in clicking to set up an interview.</p>
<h4>Divert away from negative predictors</h4>
<p>We also tested changes to the negative predictors from our SHAP plot in the hopes of increasing usage of the target. We ran a test to divert employer attention away from Feature G by placing it close to the <strong>Set up interview</strong> button on the dashboard. This way it was easier for the employer to choose setting up an interview instead.</p>
<p>This change boosted clicks to send interview invitations by 5%.</p>
<h2>Gaze into your own crystal ball</h2>
<p>A SHAP plot may not be an actual crystal ball. When used with a statistical model, however, it can generate UI A/B test ideas in bulk and boost target metrics for many products. You might find it especially suitable for products with a complex and nonlinear UI, such as user dashboards. The methodology also provides a glimpse of which UI elements drive the target metrics the most, allowing you to focus on testing features that have the most impact. So, what are you waiting for? Start using this method and good fortune will follow.</p>
<p>&nbsp;</p>
<p>Cross-posted on <a href="https://medium.com/indeed-engineering/shap-plots-the-crystal-ball-for-ui-test-ideas-121838d917f5">Medium</a></p>
<p> </p>
