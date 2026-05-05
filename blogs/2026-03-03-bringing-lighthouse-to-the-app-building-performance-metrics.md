---
title: "Bringing Lighthouse to the App: Building Performance Metrics for React Native"
url: "https://engineering.indeedblog.com/blog/2026/03/bringing-lighthouse-to-the-app-building-performance-metrics-for-react-native/"
date: "Tue, 03 Mar 2026 19:13:53 +0000"
author: "Ben Cripps"
feed_url: "https://engineering.indeedblog.com/feed/"
---
<p><a href="https://engineering.indeedblog.com/wp-content/uploads/2026/03/newestthing-1.png"><img alt="" class="alignnone size-full wp-image-4764" height="900" src="https://engineering.indeedblog.com/wp-content/uploads/2026/03/newestthing-1.png" width="1600" /></a></p>
<p>At Indeed we’ve open sourced a new React Native repository which makes it simple to measure Lighthouse scores in your mobile apps. We think it will help other organizations better measure their app performance, especially for companies similar to Indeed who are transitioning from a web-first to an app-first approach.</p>
<p><a href="https://github.com/indeedeng/react-native-lighthouse">You can check out the code here</a>, and read on for more details.</p>
<h2>The Challenge</h2>
<p>Indeed had traditionally been a web company. Site speed wasn’t just a nice-to-have — it was fundamental to how we built systems. We believed good software was always fast, and for many years now, we had relied on <a href="https://lighthouse-metrics.com/">Lighthouse</a> to keep us honest. In the past <a href="https://engineering.indeedblog.com/blog/2024/01/composite-web-performance-metric/">we’ve written in depth on this topic</a>, but as we’ve transitioned to a Mobile App first company, we needed a way of bringing the same performance rigor to our native code.</p>
<p>As React Native proliferated across our most critical pages—ViewJob, SERP, Homepage—we found ourselves flying blind. We had no standardized way to measure whether our mobile performance was improving, degrading, or holding steady. We needed answers to fundamental questions: How fast did our screens load? When could users actually interact with them? Were we maintaining the performance standards that Indeed was known for?</p>
<h2>The Solution: Core Web Vitals for React Native</h2>
<p>Rather than reinvent the wheel, we looked to the industry standards that had proven effective on the web: Core Web Vitals. These metrics — designed by Google to capture the essence of user experience — translated remarkably well to mobile apps. We just needed to adapt them for React Native’s unique threading model and lifecycle.</p>
<h2>The Metrics That Matter</h2>
<ol>
<li><strong>Time to First Frame (TTFF) — When users saw content</strong><br />
Our analog to Largest Contentful Paint (LCP). It measures how quickly users see meaningful content after a component starts mounting. In a native app context, this needs to be fast — there’s no network request to fetch the document, no HTML parsing, no CSS cascade. Code is pre-bundled. Users expect instant visual feedback.<br />
<em>Threshold:</em> &lt; 300ms is good, &gt; 800ms is poor.</li>
<li><strong>Time to Interactive (TTI) — When users could actually do something</strong><br />
The most critical metric for mobile apps. We measure when a component transitions from “loading” to “ready for interaction.” Unlike the web, where TTI was algorithmically determined, we let components self-report when they’re truly interactive — when data is loaded, UI is rendered, and touch handlers are ready. While not ideal in every case, we’ve found algorithmic TTI (e.g., <a href="https://github.com/GoogleChromeLabs/tti-polyfill">TTI Polyfill</a>) can also be inaccurate.<br />
<em>Threshold:</em> &lt; 500ms is good, &gt; 1500ms is poor.</li>
<li><strong>First Input Delay (FID) — How responsive the app felt</strong><br />
Captures the delay between a user’s first touch and when the app responds. On mobile, touch interactions should feel instantaneous. Any perceptible lag breaks the illusion of direct manipulation that makes mobile apps feel native.<br />
<em>Threshold:</em> &lt; 50ms is good, &gt; 150ms is poor.</li>
</ol>
<h3>Why These Thresholds?</h3>
<p>Our thresholds are significantly stricter than Core Web Vitals (roughly 40% tighter). This was intentional. Native apps need to be faster than web apps:</p>
<ul>
<li><img alt="✅" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" style="height: 1em;" /> No network requests for initial render</li>
<li><img alt="✅" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" style="height: 1em;" /> Code is pre-bundled in the app</li>
<li><img alt="✅" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" style="height: 1em;" /> No HTML/CSS/JS parsing overhead</li>
<li><img alt="✅" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" style="height: 1em;" /> Users expect native app speed</li>
</ul>
<p>For context, Core Web Vitals consider LCP &lt; 2.5s as “good.” We consider TTFF &gt; 800ms as “poor” — about 6× stricter. Mobile users have different expectations, and our thresholds reflect that reality.</p>
<h2>Integration: Dead Simple</h2>
<p>The entire system is packaged as a single React hook. Integration takes three steps:</p>
<pre><code class="language-tsx">function MyComponent(): JSX.Element {
  // 1. Add the hook
  const { markInteractive, panResponder } = usePerformanceMeasurement({
    provider: 'myapp',
    componentName: 'MyComponent' as const,
  });

  // 2. Mark when interactive
  useEffect(() =&gt; {
    if (dataLoaded) {
      markInteractive();
    }
  }, [dataLoaded]);

  // 3. Attach pan responder to root view
  return (
    &lt;View {...panResponder.panHandlers}&gt;
      {/* Your component */}
    &lt;/View&gt;
  );
}
</code></pre>
<p>That’s it. No configuration files, no complex setup, virtually no performance overhead in production. The hook handles everything: timing, measurement, logging, and cleanup.</p>
<h2>Technical Implementation</h2>
<h3>Architecture Overview</h3>
<p>The measurement system follows a component’s lifecycle from mount to interaction:</p>
<p>Component Mount → TTFF Measurement → TTI Marking → FID Capture → Logging</p>
<h3>1. Measuring Time to First Frame</h3>
<p>React Native’s <code>InteractionManager</code> is key. It lets us run code after the current frame finishes rendering — the perfect hook for measuring TTFF:</p>
<pre><code>useEffect(() =&gt; {
  const handle = InteractionManager.runAfterInteractions(() =&gt; {
    const ttff = Date.now() - mountStartTime;
    // TTFF captured after first frame renders
  });
  return () =&gt; handle.cancel();
}, []);
</code></pre>
<h3>2. Marking Time to Interactive</h3>
<p>Components know best when they’re truly interactive. Rather than trying to algorithmically determine this (as Lighthouse does for web), we provide a <code>markInteractive()</code> callback that components call when they’re ready:</p>
<pre><code>const { markInteractive } = usePerformanceMeasurement({
  provider: 'viewjob',
  componentName: 'ViewJobMainContent'
});

useEffect(() =&gt; {
  if (dataLoaded &amp;&amp; uiReady) {
    markInteractive(); // Component decides when it's interactive
  }
}, [dataLoaded, uiReady]);
</code></pre>
<h3>3. Capturing First Input Delay</h3>
<p>React Native’s <code>PanResponder</code> gives us comprehensive input capture across all touch types. We measure the delay between touch start and when the main thread can process it:</p>
<pre><code>const panResponder = PanResponder.create({
  onStartShouldSetPanResponder: () =&gt; {
    const inputTime = Date.now();
    setImmediate(() =&gt; {
      const processingTime = Date.now();
      const fid = processingTime - inputTime; // Main thread delay
    });
    return false; // Don't capture the gesture
  }
});
</code></pre>
<p>The <code>setImmediate</code> is crucial — it ensures we measure the actual main thread processing delay, not just the touch handler execution time.</p>
<h3>4. Smart Logging Strategy</h3>
<ul>
<li>Wait for FID: Delay logging until first user interaction</li>
<li>Timeout fallback: Log after 5 seconds even without interaction</li>
<li>Single event: All metrics logged together for easier analysis</li>
</ul>
<p>This approach gives complete performance profiles while avoiding metric fragmentation.</p>
<h2>Real-World Results</h2>
<p>We first integrated this system into ViewJob, one of Indeed’s highest-traffic pages. Here’s what we learned:</p>
<h3>Console Output (Development)</h3>
<pre><code>[Performance-Debug] TTI marked: 172ms
[Performance-Debug] TTFF captured: 347ms
[Performance] viewjob/ViewJobMainContent: {
  TTFF_ms: 347,
  TTI_ms: 172,
  FID_ms: 0,
  FID_type: "touch"
}
</code></pre>
<h3>The Lighthouse Score Equivalent</h3>
<p>To make performance actionable, we created a composite score (0–100) that mirrors Lighthouse scoring:</p>
<pre><code>const PERFORMANCE_WEIGHTS = {
  TTFF: 0.25, // Visual loading
  TTI: 0.45,  // Interactivity (most critical)
  FID: 0.30   // Responsiveness
};
</code></pre>
<p>TTI gets the highest weight (45%) because mobile users expect immediate interactivity. Visual loading and responsiveness are important, but nothing frustrates users more than tapping a button that doesn’t respond.</p>
<p><strong>ViewJob Performance:</strong><br />
• Average score: 81 (Good)<br />
• P75 score: 95 (Excellent)</p>
<p>These scores give us a single number to track over time, making it easy to spot regressions and measure improvements.</p>
<h2>What We Learned</h2>
<h3>1. Native Apps Should Be Faster</h3>
<p>Our initial thresholds were too lenient — we started with web-based Core Web Vitals and quickly realized native apps should perform better. The absence of network latency and parsing overhead means users rightfully expect faster experiences.</p>
<h3>2. Components Know Best</h3>
<p>Letting components self-report interactivity (<code>markInteractive()</code>) proved more accurate than algorithmic detection. Components understand their own loading states, data dependencies, and UI readiness in ways that external observers cannot.</p>
<h3>3. Complete Profiles Matter</h3>
<p>Waiting to log all metrics together (rather than logging each individually) made analysis significantly easier. It’s much simpler to query for “sessions with TTI &gt; 500ms” than to join three separate metric events.</p>
<h2>Looking Forward</h2>
<p>This measurement system is now our foundation for mobile performance at Indeed. We’re expanding it beyond ViewJob to SERP, Homepage, and other React Native surfaces. Each integration gives us more data, more insights, and more confidence that we’re maintaining the performance standards Indeed is known for.</p>
<p>But measurement is just the beginning. The real value comes from what we do with the data:</p>
<ul>
<li>Automated alerts when performance degrades</li>
<li>Performance budgets enforced in CI/CD</li>
<li>A/B testing to validate that optimizations actually improve user experience</li>
<li>Correlation analysis between performance and business metrics</li>
</ul>
<p>We’re no longer flying blind in the mobile world. We have the metrics, the thresholds, and the tooling to ensure that as Indeed becomes app-first, we remain performance-first.</p>
<h2>Get Involved</h2>
<p>At Indeed we’ve open sourced this repository because we think it will help other organizations better measure their app performance, especially for companies similar to Indeed who are transitioning from a web-first to an app-first approach. To contribute, please see the details in our contribution guidelines: <a href="https://github.com/indeedeng/react-native-lighthouse/blob/main/CONTRIBUTING.md">CONTRIBUTING.md</a>.</p>
<p> </p>
