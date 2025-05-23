---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: DevOps Response Time Metrics
subtitle: Why Percentiles Win
info: |
  ## DevOps Response Time Metrics
  An interactive presentation on why percentiles are superior to averages for understanding performance.

  Inspired by Slidev features!
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
highlighter: shiki
---

# DevOps Response Time Metrics
## Why Percentiles Win

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---
transition: fade-out
level: 1
---

# A Tale of Misleading Averages

Imagine you're monitoring an API. You get 10 requests with these response times (in milliseconds):

<div v-click="1">
  <div class="mt-4">
```json
[100, 120, 110, 130, 115, 125, 105, 140, 150, 2000]
```
  </div>
</div>

<div v-click="2">
  <div class="mt-4">
    Average: 
    <code>(100 + 120 + 110 + 130 + 115 + 125 + 105 + 140 + 150 + 2000) / 10 =</code>
    <code v-click="3" v-mark.red.bold="3">399.5ms</code>
  </div>
</div>

<div v-click="4">
  <p class="mt-4">
    Sounds bad, right? But look closer...
    </p>
</div>
  <div v-click="5">
  Why this matters: One outlier (<code v-mark.orange="6">2000ms</code>) skews the average, making performance seem worse than it is. Let's dig deeper.
  </div>


<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
transition: slide-up
level: 1
---

# Sorting to See the Truth

Let's sort those response times:
<br>
<br>

<div v-click="1">

````md magic-move
```json {all|all}
// Unsorted
[120, 100, 110, 130, 115, 125, 105, 2000, 140, 150]
```

```json {all|all}
// Sorted
[100, 105, 110, 115, 120, 125, 130, 140, 150, 2000]
```
````
</div>

<div v-click="1" class="mt-4">
  Now, let's find the response time that 90% of users experience or better:
  <ol class="mt-2 ml-6 list-decimal">
    <li v-click="3">
     Sort the list.
    </li>
    <li v-click="4">
      Find the rank for the 90% mark. For 10 items:
    </li>
    </ol>
</div>

$$ {hide|hide|1|2|3|all}
\begin{aligned}
\text{Rank} &= n \times \frac{\text{percentage}}{100} \\
&= 10 \times \frac{90}{100} \\
&= 9
\end{aligned}
$$
<div v-click="9">
  The 9th value in our sorted list is <span v-click="10" v-mark.underline.blue="10">150ms</span>
</div>
  <div v-click="11" class="mt-2">
    This 150ms value? 
    <span v-click="12" >We call it </span> 
    <span v-click="13" v-mark.underline.blue="13">90th percentile</span> 
  </div>

<!-- <div v-click="7" class="mt-4">
Median (Middle Value): Average of <code v-mark.green="7">120</code> & <code v-mark.green="7">125</code> = **<code v-mark.green.bold="7">122.5ms</code>**.
</div>

<div v-click="8" class="mt-6 text-lg italic">
The takeaway: Sorting reveals typical user experiences by sidelining outliers.
</div> -->

<!--
This slide uses Magic Move to animate the sorting process!
Clicks:
1: Show P90 text
2: Highlight P90 value and explanation
3: Show Median text
4: Highlight Median values
5: Show "Why this rocks..."
-->
---
layout: default
level: 2
title: Example 1 - Bad Average, Good Percentiles
---

# When Averages Fail
## Bad Average, Good Percentiles

<div v-click="1">
Scenario: 10 requests: <code> 50, 60, 55, 65, 70, 75, 80, 85, 90, <span class="text-red-500 font-bold">5000</span></code>
</div>

<div class="grid grid-cols-3 gap-4 mt-4">
  <div v-click="2">
    <h3>Average:</h3>
    <pre><code class="language-ts"> = 563ms</code></pre>
    <span v-click="3" v-mark.red="3">Highly misleading!</span>
  </div>
 
  <div v-click="4">
    <h3>p90:</h3>
    9th value in sorted list
    <pre><code class="language-ts">= 90ms</code></pre>
    <span v-click="5" class="text-green-500" v-mark.green="5">Much better!</span>
  </div>
 <div v-click="7">
    <h3 v-click="7">p50 <span v-click="8" v-mark.highlight.orange="8">(Median):</span></h3>
    <pre><code class="language-ts">(70+75)/2 = 72.5ms</code></pre>
  </div>
  
</div>
 
 <br>
 <br>
 
<div v-click="9">
  <p class="mt-6 text-lg italic">
    Takeaway: One terrible request (<span class="text-red-500" v-mark.red="9">5000ms</span>) inflates the average, but 90% of users got &lt;90ms. Percentiles tell the true story.
  </p>
</div>

---
layout: two-cols
layoutClass: gap-8
level: 1
---
# Why Percentiles Beat Averages

<div v-click="1" class="flex items-start gap-3 mb-4">
  <div class="text-blue-500 text-xl">👥</div>
  <div>
    <h3 class="text-lg font-semibold mb-1">Real User Experience</h3>
    <p class="text-gray-600">Percentiles show what most users actually experience, not rare edge cases</p>
  </div>
</div>

<div v-click="2" class="flex items-start gap-3 mb-4">
  <div class="text-green-500 text-xl">🎯</div>
  <div>
    <h3 class="text-lg font-semibold mb-1">Perfect for SLOs</h3>
    <p class="text-gray-600">Set targets like "95% of requests under 200ms" — percentiles measure this directly</p>
  </div>
</div>

<div v-click="3" class="flex items-start gap-3">
  <div class="text-purple-500 text-xl">🛡️</div>
  <div>
    <h3 class="text-lg font-semibold mb-1">Outlier Resistant</h3>
    <p class="text-gray-600">One 30-second timeout won't destroy your metrics</p>
    <p class="text-sm text-purple-700 mt-1">Averages would spike, percentiles stay stable</p>
  </div>
</div>

::right::

<div v-click="4" class="mt-4">
  <h3 class="text-xl font-bold mb-6 text-center">Same Data, Different Stories</h3>
  

  <div class="bg-red-50 border-l-4 border-red-500 p-4 mb-4 rounded-r">
    <div class="flex items-center justify-between mb-2">
      <span class="font-semibold text-red-800">Average</span>
      <code v-mark.red.bold="5" class="text-3xl font-bold text-red-600">399ms</code>
    </div>
    <div class="text-red-700 text-sm font-medium">
      😰 "Our app is slow! Users are suffering!"
    </div>
  </div>

  <div class="bg-green-50 border-l-4 border-green-500 p-4 mb-4 rounded-r">
    <div class="flex items-center justify-between mb-2">
      <span class="font-semibold text-green-800">P90</span>
      <code v-mark.green.bold="5" class="text-3xl font-bold text-green-600">150ms</code>
    </div>
    <div class="text-green-700 text-sm font-medium">
      😊 "90% of users get sub-150ms response!"
    </div>
  </div>
</div>

<div v-click="6" class="mt-6 p-4 bg-blue-50 rounded-lg">
  <p class="text-blue-800 font-medium text-center">
    💡 <strong>Key Insight:</strong> Most users are happy, only a few outliers are slow
  </p>
</div>

---
level: 2
title: Good Average, Bad Percentiles
---

# The Hidden Trap
## Example: Good Average, Bad Percentiles

<div class="mb-6 mt-3">
  <div class="font-mono text-xl bg-gray-900/80 p-4 rounded-lg inline-block border border-gray-600/30">
    <span class="text-green-300">50, 50, 50, 50, 50,</span>
    <span class="text-red-300 font-semibold">400, 400, 400, 400, 400</span>
  </div>
</div>

<div class="grid grid-cols-3 gap-6 mt-8">
  
  <div v-click="1" class="bg-blue-900/40 p-6 rounded-xl border border-blue-400/30 hover:bg-blue-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-blue-300">Average</h3>
    <div class="font-mono text-sm mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-gray-200">(50×5 + 400×5)/10</div>
      <div class="text-blue-200 font-semibold">= 225ms</div>
    </div>
    <div  class="text-sm mt-3">
      <span class="px-3 py-1 bg-yellow-600/30 rounded-full text-yellow-200 border border-yellow-400/40">
        Seems okay...
      </span>
    </div>
  </div>

  <div v-click="2" class="bg-purple-900/40 p-6 rounded-xl border border-purple-400/30 hover:bg-purple-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-purple-300">Median</h3>
    <div class="text-xs mb-2 text-gray-300 font-medium">Sorted values:</div>
    <div class="font-mono text-xs mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-purple-200 mt-2 font-semibold">(50+400)/2 = 225ms</div>
    </div>
  </div>

  <div v-click="3" class="bg-red-900/40 p-6 rounded-xl border border-red-400/30 hover:bg-red-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-red-300">P90</h3>
    <div class="text-xs mb-2 text-gray-300 font-medium">9th value in sorted list:</div>
    <div class="font-mono text-sm mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-red-200 text-2xl font-bold">400ms</div>
    </div>
    <div class="text-sm mt-3">
      <span class="px-3 py-1 bg-red-600/40 rounded-full text-red-200 font-semibold border border-red-400/50">
        ⚠️ Alarming!
      </span>
    </div>
  </div>
  
</div>

<!--
Clicks:
1: Show Average calculation
2: Highlight "Seems okay..."
3: Show Median calculation
4: Show P90 calculation
5: Highlight "Alarming!"
6: Show Takeaway text
7: Highlight issue in takeaway
-->

---
level: 2
title: Averages and Percentiles Align
---

# When Averages and Percentiles Align

<div class="mb-6 mt-3">
  <div class="font-mono text-xl bg-gray-900/80 p-4 rounded-lg inline-block border border-gray-600/30">
    <span class="text-green-300">100, 102, 104, 106, 108, 110, 112, 114, 116, 118</span>
  </div>
</div>

<div grid="~ cols-3 gap-4" class="mt-8">
  <div v-click="1" class="bg-blue-900/40 p-6 rounded-xl border border-blue-400/30 hover:bg-blue-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-blue-300">Average</h3>
    <div class="font-mono text-sm mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-blue-200 font-semibold">= 109ms</div>
    </div>
  </div>
  <div v-click="2" class="bg-purple-900/40 p-6 rounded-xl border border-purple-400/30 hover:bg-purple-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-purple-300">Median</h3>
    <div class="font-mono text-sm mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-purple-200 font-semibold">(108+110)/2 = 109ms</div>
    </div>
  </div>
  <div v-click="3" class="bg-green-900/40 p-6 rounded-xl border border-green-400/30 hover:bg-green-900/50 transition-all">
    <h3 class="text-xl font-bold mb-4 text-green-300">P90</h3>
    <div class="font-mono text-sm mb-3 bg-gray-900/60 p-3 rounded-md border border-gray-600/40">
      <div class="text-green-200 font-semibold">9th value = 116ms</div>
    </div>
  </div>
</div>

<div
  v-motion
  :initial="{ x: 100, opacity: 0 }"
  :enter="{ x: 0, opacity: 1, transition: { delay: 1000 } }"
  class="mt-12 p-4 bg-teal-900/40 border border-teal-400/30 rounded-xl text-teal-100"
>
  This scenario represents a very consistent system performance with no outliers, which is ideal but rarely seen in real systems.
</div>

---
layout: default
level: 1
---

# Actionable Takeaways for DevOps

<div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-6">
  <div v-click="1" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-emerald-600">🎯 Use Percentiles</h3>
    Use percentiles (p90, p95, p99) for monitoring and SLOs. They reflect actual user experience.
  </div>

  <div v-click="2" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-amber-600">⚠️ Avoid Averages (Mostly)</h3>
    Avoid averages for response times unless data is very consistent (rare in production).
  </div>

  <div v-click="3" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-sky-600">📈 Example SLO</h3>
    Set an SLO: "95% of API requests < 200ms." Track p95 to ensure you meet it.
    <br>
    <span class="text-sm text-gray-600">e.g. $\\text{P}_{95}(\\text{response time}) < 200\\text{ms}$</span>
  </div>

  <div v-click="4" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-purple-600">🛠️ Tool Tip</h3>
    Use Prometheus, Grafana, or Datadog to track percentiles easily. They have built-in functions!
    <br>
    <span class="text-sm text-gray-600">(e.g., `histogram_quantile()` in PromQL)</span>
  </div>
</div>

<div v-click="5">
  <p class="mt-12 text-2xl text-center font-semibold text-gray-700">
Final word: <span class="text-green-500">Percentiles empower you</span> to focus on what matters—<span class="text-blue-500">your users!</span>
  </p>
</div>

<!--
Clicks:
1: Show "Use Percentiles" card
2: Show "Avoid Averages" card
3: Show "Example SLO" card
4: Show "Tool Tip" card
5: Show "Final word"
-->

---
title: Table of Contents
level: 1
---

# Table of Contents

You can use the `Toc` component to generate a table of contents for your slides.
The `level` in each slide's frontmatter determines its depth in the ToC.

```html
<Toc minDepth="1" maxDepth="2" />
```

<Toc class="mt-4" minDepth="1" maxDepth="2" />

<!--
This slide demonstrates the Table of Contents feature.
- Make sure slides that should appear in ToC have `level` set in their frontmatter.
No click animations on this slide.
-->

---
layout: two-cols
title: More Slidev Features
dragPos:
  logo: 0,-118,0,0
---

# Bonus: Slidev's Powerhouse Features

Slidev offers much more! Here are a few from the sample:

- **LaTeX**: For beautiful math equations.
  Inline: $\\sqrt{3x-1}+(1+x)^2$
  Block:
  $$
  \\begin{aligned}
  \\nabla \\cdot \\vec{E} &= \\frac{\\rho}{\\varepsilon_0} \\\\
  \\nabla \\cdot \\vec{B} &= 0 \\\\
  \\nabla \\times \\vec{E} &= -\\frac{\\partial\\vec{B}}{\\partial t} \\\\
  \\nabla \\times \\vec{B} &= \\mu_0\\vec{J} + \\mu_0\\varepsilon_0\\frac{\\partial\\vec{E}}{\\partial t}
  \\end{aligned}
  $$

- **Diagrams**: Create diagrams from text.
  ```mermaid
  graph TD
      A[Start] --> B{Decision};
      B -- Yes --> C[Good Percentiles!];
      B -- No --> D[Check Outliers!];
  ```

::right::

- **Motions**: Powered by `@vueuse/motion`.
  ```html
  <div v-motion :initial="{ x: -80 }" :enter="{ x: 0 }">
    Animated Text!
  </div>
  ```
  <div
    v-motion
    :initial="{ x: -80, opacity: 0 }"
    :enter="{ x: 0, opacity: 1, transition: { delay: 500 } }"
    class="text-2xl text-blue-500 mt-4"
  >
    Animated Text!
  </div>

- **Draggable Elements**: (Double-click to move)
  ```md
  <img v-drag="'logo'" src="https://sli.dev/logo.png" class="w-20">
  ```
  <img v-drag="'logo'" src="https://sli.dev/logo.png" class="w-20 cursor-grab active:cursor-grabbing">

- **Monaco Editor**: For live coding (add `{monaco}` to code block).
  ```ts {monaco}
  // Try editing this code!
  function greet(name: string) {
    console.log(\`Hello, \${name}!\`);
  }
  greet('Developer');
  ```

<!--
This slide showcases some extra Slidev features as requested.
- LaTeX for math.
- Mermaid for diagrams.
- v-motion for animations.
- v-drag for draggable elements.
- Monaco editor for interactive code blocks.
No sequential v-click animations on this slide.
-->

---
layout: center
class: text-center
---

# Learn More About Slidev

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

Explore themes, create custom components, and much more!

<PoweredBySlidev mt-10 />

<!--
Final "Learn More" slide.
- Links to official Slidev resources.
- PoweredBySlidev component.
No click animations on this slide.
-->
