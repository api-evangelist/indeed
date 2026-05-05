---
title: "Speed Matters, But It Isn’t Everything"
url: "https://engineering.indeedblog.com/blog/2022/01/speed-matters/"
date: "Mon, 31 Jan 2022 21:43:37 +0000"
author: "Ben Cripps"
feed_url: "https://engineering.indeedblog.com/feed/"
---
<p><img alt="" class="aligncenter wp-image-4509 size-large" height="631" src="https://engineering.indeedblog.com/wp-content/uploads/2022/01/jonathan-chng-HgoKvtKpyHA-unsplash-1-1024x631.jpg" width="1024" /></p>
<p style="text-align: center;"><em>Photo by <a href="https://unsplash.com/@jon_chng">Jonathan Chng</a> on <a href="https://unsplash.com/s/photos/collaboration?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></em></p>
<p>Over the last few years at Indeed, we noticed our public-facing web applications were loading more slowly. We tested numerous ways to improve performance. Some were very successful, others were not.</p>
<p>We improved loading speeds by 40% but we also learned that speed is not always the most important factor for user experience.</p>
<h2>Performance metrics</h2>
<p>We measured loading speed using two key metrics:</p>
<ul>
<li><a href="https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint"><b>FirstContentfulPaint</b></a> – when the user sees the first content of the page</li>
<li><a href="https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigationTiming/domContentLoadedEventEnd"><b>DomContentLoadedEventEnd</b></a> – when the page becomes interactive, <a href="https://html.spec.whatwg.org/multipage/parsing.html#the-end">as measured by the moment where all critical CSS and JavaScript have been parsed and executed</a></li>
</ul>
<p>We chose a weighted average instead of a single metric. This provided a more accurate measure of perceived load time, and helped us answer two critical questions:</p>
<ul>
<li>How long did the user wait before the page seemed responsive?</li>
<li>How long did the user wait before they could interact with the page?</li>
</ul>
<p>Though these metrics came with tradeoffs, we decided to use them instead of Google Web Vitals because they gave the broadest coverage across our user base. After deciding on these metrics, we had simple, observable, and reportable data from hundreds of applications and across a variety of web browsers.</p>
<h2>Successful methods for improving speed</h2>
<p>While we tried many strategies, the following efforts provided the biggest increases in performance.</p>
<h3>Flushing &lt;Head/&gt; early</h3>
<p>Browsers generally use the most resources during page load when they are downloading and parsing static resources such as JS, CSS, and HTML files. To reduce this cost, we can send static content early, so the browser can begin to download and parse files even before those files are required. This eliminates much of the render-blocking time these resources introduce.</p>
<p>By flushing the HTML head early on multiple applications, we saw load time improvements of 5-10%.</p>
<p>This implementation comes with a few trade-offs, however, since flushing the HTML document in multiple chunks can result in confusing error modes. Once we’ve flushed the first part of the response, we’re no longer able to change parts of the response, such as status code or cookies. Even if an error occurs somewhere before the last part of the response, we can’t change these headers. We’ve implemented some common libraries that help with these complications.</p>
<h3>Reducing files on the critical path</h3>
<p>Apart from the total number of bytes, one of the most important aspects for page load time is the number of total resources – especially render-blocking resources – required on the critical path for rendering. In general, the more blocking files you request, the slower the page. For example, a 100kB page served with 5 files will be significantly faster than a 100kB page served with 10 files.</p>
<p>In an A/B test, we reduced the number of render-blocking files from 30 to 12, a 60% reduction. The total amount of bytes shipped during page load was roughly identical. This test provided a 2+ second improvement for <strong>domContentLoadedEventEnd</strong> at the 95th percentile for our desktop and mobile search pages, as well as significant improvements in <strong><a href="https://web.dev/lcp/">largestContentfulPaint</a></strong>.</p>
<p>To dive into this further, we explored the cost of a single extra CSS file. We ran a test on one of our highest trafficked pages to reduce the number of CSS files by 1. Page load times improved by a statistically significant amount, about 15ms at the 95th percentile.</p>
<h3>Improving the runtime cost of CSS-in-JS</h3>
<p>As more of our applications started using our newest component library, built on top of the <a href="https://emotion.sh/docs/introduction">Emotion library</a>, we noticed 40% slower page loads.</p>
<p>The Emotion library supports CSS-in-JS, a growing industry trend. We determined that rendering CSS-in-JS components added extra bytes to our JavaScript bundles. The runtime cost of this new rendering strategy – along with the added bytes – caused this slowdown. We built a webpack plugin that precompiled many of our most commonly used components, reducing their render costs and helped address the problem.</p>
<p>This strategy resulted in a massive improvement, decreasing the slowdown from 40% to about 5% in aggregate, at the 95 percentiles. However, the CSS-in-JS approach still incurred more runtime cost than more traditional rendering approaches.</p>
<h2>Factors outside our control</h2>
<p>Along with testing improvements, we analyzed the types of users, locales, and devices that had an impact on page speeds.</p>
<h3>Device type and operating system</h3>
<p>For Android devices, which are generally lower powered than their iOS counterparts, we saw 63% slower timings for <strong>firstContentfulPaint</strong>, and 107% slower timings for <strong>domContentLoadedEventEnd</strong>.</p>
<p>Windows users saw 26% slower timings for <strong>domContentLoadedEventEnd</strong> compared to their iOS counterparts. These results were somewhat expected, since Windows devices tend to be older.</p>
<p>This data provided important takeaways:</p>
<ul>
<li>The performance impact of features and additional code is non-linear: newer, robust devices can incur 100kB more code without an impact to performance, while older devices see a much bigger slowdown as a result.</li>
<li>Testing applications using real user metrics (RUM) is critical to understanding performance, since performance varies so widely based on device and the operating system’s capabilities.</li>
</ul>
<h3>Connection type and network latency</h3>
<p>We used the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Network_Information_API">Network Information API</a> to collect information about various connection types. The API is not supported in all browsers, making this data incomplete, however, it did allow us to make notable observations:</p>
<ul>
<li>4G connection types were 4 times faster than 3G, 10 times faster than 2G, and 20 times faster than connections that were less than 2G. Put another way, network latency accounts for a huge percent of our total latency.</li>
<li>For browsers that report connection type information, 4G connection types make up 95% of total traffic. Including all browser types drops this number closer to 50%.</li>
</ul>
<p>Networks vary greatly by country, and for some countries it takes over 20 seconds to load a page. By excluding expensive features such as big images or videos in certain regions, we deliver simpler, snappier experiences on slower networks.</p>
<p>This is by far the simplest way to improve performance, but it comes at the cost of complexity.</p>
<h2>Results of speed and other factors</h2>
<p>The impact of performance on the web varies. Companies such as Amazon have reported that <a href="https://www.fastcompany.com/1825005/how-one-second-could-cost-amazon-16-billion-sales">slowdowns of just 1 second could result in $1.6 billion in lost sales</a>. However, other case studies have reported a more muddled understanding of the impact of performance.</p>
<p>Over the course of our testing, we saw some increases in engagement based on performance improvements. But we’re not so sure they’re strongly correlated to performance improvements alone.</p>
<h3>Reliability vs speed</h3>
<p>Our current understanding of these increases in engagement is that they are based on increased reliability rather than an improvement in loading speed.</p>
<p>In tests where we moved our static assets to a content delivery network (CDN), we saw engagement improvements, but we also saw indications of greater reliability and availability. In tests that improved performance <strong>but not reliability</strong>, we did <strong>not</strong> see strong improvements in engagement.</p>
<h3>The impact of single, big improvements</h3>
<p>In tests where we improved performance by a second or more (without improving reliability), we saw no significant changes in our Key Performance Indicators.</p>
<p>Our data suggests that for non-commerce applications, small to medium changes in performance do not meaningfully improve engagement.</p>
<h3>Engagement vs performance</h3>
<p>Our observations reminded us not to equate performance with engagement when analyzing our metrics. One stark example of this point was the different performance metrics observed for mobile iOS users versus mobile Android users.</p>
<p>While Android users had nearly 2 times slower rendering, there was no observable drop in engagement when compared to iOS users.</p>
<h2>So when does speed matter?</h2>
<p>After a year of testing strategies to improve speed, we found some that are worth the effort to improve performance. While these improvements were measurable, they were not significant enough to drive changes to key performance indicators.</p>
<p>The bigger lesson is that while a certain level of speed is required, other factors matter too. The user&#8217;s device and connection play a large role in the overall experience. The silver lining is that knowing we cannot fully control all these factors, we can be open to architectural strategies not specifically designed for speed. Making minor trade-offs in speed for improvements in other areas can result in an overall better user experience.</p>
<p>&nbsp;</p>
<p><a href="https://medium.com/indeed-engineering/speed-matters-but-it-isnt-everything-879b4203fd9f">Cross-posted on Medium</a></p>
<p> </p>
