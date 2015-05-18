Web development is tricky. There is a myriad of goals to strive for, like an effective user experience, unique features and components, nifty buttons and impressive overlays, a responsive/adaptive layout, browser/device compatibility, SEO, accessibility, style guide consistency... No matter the consequence, businesses always want to have the latest and greatest for their sites. 

Since websites have such diffuse goals, it is very challenging to ensure optimum performance. Just think about how much code gets downloaded to the browser - as the code for all those goals is combined.

Delivering all your code during the page load can be very cumbersome for your page performance, and can spike your **Speed Index** way up (which is **bad**: the higher your Speed Index is, the worse your page is performing). If you are not familiar with the Speed Index, think of it like a consistent score of combined measurements to assess the performance of a webpage. For more information, check out the [WebPageTest.org Documentation](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index).

The good news is that optimizing such scenarios is not rocket science. All you need to do is be savvy about how to optimize the Critical Rendering Path. Whaat? Let me explain.

## The Critical Rendering Path

Putting it simply, the Critical Rendering Path (or CRP) is the sequence of steps the browser takes to render the critical content of a webpage (we will go through the definition of critical content soon). In other terms, the CRP is the browser's own workflow to process all the critical HTML, CSS, JavaScript, images, and media content.

One of the biggest challenges in web development is keeping your page's CRP as minimal as possible. This is your best bet to ensure a good page load time and to achieve a better Speed Index. In the next sections of this article, we will discuss some ways to minimize the CRP.

For more information about the CRP, check out [Patrick Sexton's article](https://www.feedthebot.com/pagespeed/critical-render-path.html) and the thorough [Ilya Grigorik's Website Performance Optimization course at Udacity](https://www.udacity.com/course/website-performance-optimization--ud884).

In order to discuss what is accounted for in the **critical** content of the page, I would like to present to you some concepts:

### 1. Above the fold vs Below the fold

Think about a newspaper, when you have just bought it. It comes folded, and the most important headlines appear above the fold. This is meant to catch your attention before you open the newspaper, in order to convince you to buy it. 

In this way, the content above the fold has the absolutely necessary information to be advertised about today's edition, which is defined as critical. Everything else will appear the below the fold and will only be revealed after you have bought and opened the newspaper.

Web pages are no different. The fold is now the bottom line of your browser. When you open a page, the page fold will separate the content that gets displayed right away from the content below which would require scrolling down in order to be revealed. 

Likewise, the above the fold page content is considered more critical, for which the browser will spend all its processing power during the CRP processing: downloading files, parsing HTML, CSS and JavaScript and then immediately rendering the markup (images and media content as well). Hence, the below the fold content stands as less critical, since the browser will just download it and parse it but not render it.

**TL;DR**:

  - Above the fold content: more critical (download, parse and render)
  - Below the fold content: less critical (download and parse)

### 2. Page loading vs Lazy loading

Now, think about a quite complex website with massive content you interact with frequently, such as Facebook or Amazon. Those sites always have more content to show as you scroll down the page. And they do not appear sluggish at all. How come?

The key here is lazy loading. These sites will keep the page load to the minimum, and will lazily load more content as the user interacts with it, like scrolling down, pressing a button, or clicking on a tab.

Pretty much any content can be lazy loaded: HTML, JavaScript, CSS, images and media. The more content gets lazy loaded, the less content is downloaded on page load time, which will certainly help with the CRP and keep your Speed Index low (remember, a low Speed Index is **good**).

Once any page-loaded content has to pay the high rent (in order to belong to the CRP private club), only the necessary content should be on page load. In this way, any page-loaded content is potentially critical while any lazy-loaded content can't be critical.

**TL;DR**:

  - Page-loaded content: potentially critical
  - Lazy-loaded content: non-critical

### 3. Server-side rendering vs Client-side rendering

In the old generation of the web, every website would process all its templates on the server-side and deliver an HTML response to the browser. Nowadays websites may render some (or most) templates on the client-side instead, from a JSON response.

One of the main reasons behind this shift is that a JSON response containing the data needed to render a feature is generally much smaller than the actual HTML content for the same feature. From an HTTP response containing just JSON, the same HTML content can be rendered on the client-side, and this HTTP response can be downloaded in considerably less time.

Something to keep in mind is that doing client-side rendering on a feature may negatively impact on its SEO value, since the content doesn't belong to the first page response and hence it won't be available to most search engine bots. Besides there's been an year since [Google bots officially started to process JavaScript](http://googlewebmastercentral.blogspot.com/2014/05/understanding-web-pages-better.html), still your feature's SEO value is likely to be impacted, as  Google might not be able to execute highly complex or arcane JavaScripts, and we should always remember that Google isn't the only big search engine around. More on that on [Santiago Esteva's article](http://ng-learn.org/2014/05/SEO-Google-crawl-JavaScript).

**TL;DR**:

  - Server-side-rendered content: more critical and more SEO value
  - Client-side-rendered content: less critical and less or none SEO value

## The tipping point of performance

Quick recap: 

- **More critical content** is related with above the fold, page-loaded and server-side rendered. 
- **Less critical content** is related with below the fold, lazy-loaded and client-side rendered. 

In order to optimize the CRP, think about this: everything that isn't critical enough should be taken off the CRP for a better page performance and lower Speed Index. Some ways you can do that:

### 1. Moving content to below the fold

**Search for less important HTML/images/media content which is above the fold and move it to below the fold.**

For instance: let's say your page has a 100 KB tall vertical banner on the left side, but only half of it appears above the fold, which makes it somewhat useless for the above the fold experience. By pushing it a little bit down (so that none of it appears above the fold for a typical 1024x768 resolution) you will have effectively moved it to below the fold, which means the browser no longer needs to wait for this image download to be finished in order to render the above the fold content. The browser will still download it, but not during the above the fold rendering. This means that now the critical content of the page isn't depending on that banner anymore.

### 2. Lazy-loading features

**Search for lazy-loadable features on the page load and replace their immediate loading with hooks/listeners that will load them upon some condition.**

For instance: your page might have an overlay that is only displayed after the user presses a button. Instead of downloading the HTML, CSS or JavaScript for the overlay on the page-load time, you can just add a click listener on this button that will download the HTML, CSS or JavaScript for the feature only after the user presses this button for the first time. This can be accomplished in a few different ways, but I particularly recommend [Webpack](http://webpack.github.io).

P.S.: If you are developing a responsive page, this step is very important since your code should suppress any feature that isn't compatible with the user's device. By suppressing I mean not even downloading the HTML, CSS and JavaScript for the feature - once "simply hiding the extra content" will still download and parse it.

### 3. Rendering non-SEO-critical content on the client-side

**Search for non-SEO-critical content which is being rendered server-side during the page load, and remove it from the first page response. Now, add a JavaScript listener after the DOM is ready that will render the feature on the client-side.**

For instance: your page might have some section for user comments that you don't want to be indexed by search engine bots. Instead of including the HTML comments on the first page response, you may just embed a JSON array with the comments data on the page, under a ```<script id="commentsData" type="application/json">``` tag. This technique is also known as JSON bootstrapping.

Alternatively, this JSON array may be obtained from an asynchronous call to the server, if you don't want to bootstrap the JSON on the first page response. Keep in mind, though, that an extra roundtrip to the server during the page load may increase your total load time.

Now, you just need to provide a JavaScript listener that, once the DOM is ready, will retrieve this JSON array from the tag, parse it and render the comments markup using the data from it.

## Be a savvy front-end developer

This is just the very beggining about CRP optimization. There is a lot to go through from here, and many other optimization techniques may also positively affect the CRP. I have previously written about the [mind-boggling JavaScript modules](https://www.airpair.com/javascript/posts/the-mind-boggling-universe-of-javascript-modules), which can give your page a boost when well structured, specially if your previous code was based on global variables.

Hopefully, now you will be able to tell if the next feature asked by your business is going to impact on your page's CRP, and if so, you should be able to set up an action plan!
