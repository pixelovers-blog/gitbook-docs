---
description: >-
  Frontity framework and its extensions will help save you a lot of development
  time while enjoying of all of the latest technology trends, already configured
  for you.
---

# ⚛ Frontity features

Here's a list of the main features included in Frontity's core:

### **Frontity features**

* [Zero setup development](./#zero-setup-development)
* [Lightning-fast loading](./#lightning-fast-loading)
* [Instant in-app navigation](./#instant-in-app-navigation)
* [Server Side Rendering](./#server-side-rendering)
* [Extensible](./#less-than-greater-than-extensible)
* [Best Lighthouse score](./#best-lighthouse-score)
* [Perfect accessibility](./#perfect-accessibility)
* [Battle-tested](./#battle-tested-framework)
* [Serverless and horizontal scaling](./#serverless-and-horizontal-scaling)
* [First class TypeScript support](./#first-class-typescript-support)
* [Support for ES6 in modern browsers](./#support-for-es6-in-modern-browsers)
* [Support for Wordpress.com & WordPress.org](./#support-for-wordpress-com-and-wordpress-org)
* [Support for multiple sites with a single installation](./#support-for-multiple-sites-with-a-single-installation)
* [Code Splitting](./#code-splitting)
* [Smallest React bundle possible](./#smallest-react-bundle-possible)
* [Ready for React Concurrent and Suspense](./#ready-for-react-concurrent-and-suspense)

### 🔜 Coming soon

* [PWA and offline support](./#pwa-and-offline-support)
* [Google AMP with the same codebase](./#google-amp-support-with-the-same-codebase)

## Frontity features

### ⚙ Zero setup development

Everything is already wired up you can focus on building your site: React, Webpack, Babel, SSR, Routing, CSS-in-JS, WP REST API, TypeScript, Linting, Testing, and so on.

### 🚀 **Lightning-fast loading**

Frontity sends an HTML that is ready to start navigating the site, so the initial load feels almost instant. No extra assets or round trips are necessary.

This HTML is fully **functional** and **navigable** without Javascript. Once React loads, it takes control of the app and users don’t notice any change, it is 100% transparent.

### ⚡️ Instant in-app navigation

Once React has loaded, our router prefetches other routes and data automatically. Users never have to wait when they navigate inside the app.

### 🗄 Server Side Rendering

Frontity responds with a fully populated HTML file generated with React. This reduces the time required for the first contentful paint and ensures that the **SEO** is **not harmed**.

The content is retrieved using the WordPress REST API. Once React is loaded in the browser, it takes control of the page and does its magic.

### &lt;&gt; Extensible

One of the most amazing things about Frontity is its extensibility: it allows you to easily add new features to your theme via **extensions** and **NPM packages** without having to create them from scratch.

We are working on a lot of Frontity extensions which will be available soon. You can check them out [here](extensions.md). Some examples are Yoast SEO, AdSense, SmartAds, DoubleClick for Publishers, OneSignal Push Notifications, Disqus, Google Analytics, Google Tag Manager, or ComScore.

Apart from these extensions, there are many other **interface tools** specifically created for Frontity: context routing, swipe navigation, infinite scrolling, html-to-react, gutenberg-to-react, etc.

Our themes can also use any of the 80.000 React packages currently available in NPM.

### 💯 Best Lighthouse score

Frontity is optimized to get the maximum score in Lighthouse, including performance, SEO and accessibility. Theme developers start with **100/100** and they just need to maintain it while they add features to their theme.

### 🌎 Perfect accessibility

As part of our mission to make building websites with WordPress and React easier and more accessible, we also want to develop the framework focused on this aspect. Frontity is perfectly **accessible by default** and will provide tools that let the developers know if they break it. 

### 🎖 Battle-tested framework

We’re open sourcing the internal framework we’ve been using to power big WordPress news sites during the last year. Used by million readers, Frontity is proven and ideal for building engaging frontend experiences.

### 📈 Serverless and horizontal scaling

The Frontity server is so small it suits perfectly the serverless requirements. That means infinite scaling for the front-end.

All the server code is bundled in one file, ready to work with serverless services like [Now](https://zeit.co/now) or [AWS Lambda](https://aws.amazon.com/es/lambda/). Frontity is also prepared to scale horizontally in any Node server.

### {  } First class TypeScript support

Frontity has amazing TypeScript support. Actually, we like it so much that Frontity itself is built using TypeScript. But don’t worry, it’s ****absolutely optional: if you don’t know or don’t want to learn it you can use regular JavaScript without problems!

### **💻 Support for ES6 in modern browsers**

Frontity generates two bundles of JavaScript:

* One in ES6 without transpilation or polyfills so it’s as small and fast as possible. 
* The other in ES5 for the old browsers that don’t support ES6.

Modern browsers that support ES6 modules will request the ES6 bundle, translating into a **reduced bundle size** and **shorter evaluation time** in the browser. This guarantees that performance is not harmed in the modern browsers while ensuring backwards compatibility with the old ones.

### 🔗 Support for Wordpress.com & WordPress.org

Frontity can work with different “source” extensions. The first beta version includes a “wp-source” which works with the **REST API** of any [wordpress.com](https://developer.wordpress.com/docs/api/) or [wordpress.org](https://developer.wordpress.org/rest-api/) website. This way, whether you have a self-hosted site or it is hosted by Automattic, Frontity will suit your needs.

Frontity has been designed so it can support **other sources** in the future \(like the [GraphQL API for WordPress](https://www.wpgraphql.com/)\). Actually, we are discussing possible future sources here in [the community](https://community.frontity.org/t/potential-supported-sources/18/3). Feel free to join the conversation and share any ideas you might have.

### ☝️ Support for multiple sites with a single installation

This is something similar to WordPress multisite: Frontity allows you to serve any number of sites with just one installation. This can be really useful for users who manage different clients or those who want to create a network.

### 🕸 Code Splitting

Frontity uses webpack to split the code and send the minimum code required for the app to work. It also allows developers to dynamically load components with the help of loadable-components.

### **🌱 Smallest React bundle possible**

Frontity helps build websites which are fast to deliver better user experiences. That's the reason why we have struggled to make the core smaller. It has finally been reduced by 60% and only weights 60kb \(gzipped\).

### **✅ Ready for React Concurrent and Suspense**

The React team is working hard to release an async, “no-CPU-blocking” version and Frontity will be compatible with it. It is expected for Q2 2019. Once it is released, we expect to see a rise in the use of the React animation libraries available that will get the user experience to the next level.



## 🔜 Coming soon

Frontity is continuously looking to improve, so we are already working on some major \(and awesome\) features which will be available very soon.

### 📱 Google AMP support with the same codebase

Themes made with Frontity are able to render an AMP compatible version with the same React code and CSS used for the HTML version.

### ✨ PWA and offline support

Our themes work with the WordPress manifest to get full PWA compatibility out of the box. Frontity also uses **service workers** for caching assets, which allows your users to access previously visited content even when they are disconnected from the internet **without** any **extra configuration**.

{% hint style="warning" %}
🛠 Feature still under construction. [Feel free to ask](https://community.frontity.org/) any questions you might have.
{% endhint %}

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}

