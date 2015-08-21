# High Performance Web Sites <br/> by Steve Souders

Start: August 19th, 2015 <br/>
Finished: 

## Chapter 1: Make Fewer HTTP Requests
- **Image Maps:**
  allow you to associate multiple URLS with a single image. The destination URL is chosen based on where the user clicks on the image. This allows you to replace multiple HTTP requests with just one.<br/>
- **CSS Sprites:**
  allow you to combine images, but they’re much more flexible than image maps. Each image is wrapped in a span that has a different class, you specify the distance between each image using background-position property.
  - combined images tend to be smaller than the sum of separate images, resulting from reducing amount of image overhead (color tables, formatting information, etc.) <br/>
- **Inline Images:**
  allows you to include images in your webpage without any additional HTTP requests using data: URL scheme. data: URLs are embedded into the page, so they won't be cached across different pages.<br/>
- **Combined Scripts and Stylesheets:**
  you should modularize your code and write your JavaScript and CSS in external scripts and stylesheet files. In the ideal situation, there would be no more than one script and one stylesheet. Combining your scripts and stylesheets leads to a 38% increase in load time. 

## Chapter 2: Use a Content Delivery Network
- **Content Delivery Network:**
  - If your website starts getting larger and needs to scale, do not attempt to redesign your web application to work in a distributed architecture FIRST. Remember the *Performance Golden Rule* 
    > Only 10–20% of the end user response time is spent downloading the HTML document. The other 80–90% is spent downloading all the components in the page.
  If component web servers are closer to the user many HTTP requests response times are improved. 
  - CDN: collection of web servers distributed across multiple locations to deliver content to users more efficiently. Their services include backups, extended storage capacity, and caching. A CDN can also help absorb spikes in traffic, for example, during times of peak weather or financial news, or during popular sporting or entertainment events.
  - Souders tested that website with components on CDN loaded 18% faster than the page with all components hosted from a single web server. It's very easy to move all static components to CDN.

## Chapter 3: Add an Expires Header
  - **Expires Header:**
    Using an expires header allows your components to be cacheable by telling the web client that it can use the current copy of a component until the specified time. This avoids unnecessary HTTP requests on subsequent page views. 57% reduction in response time. 
  - **Max-Age and mod_expires:** These two are alternatives to the expires header, which has a stricter clock synchronization requirements betweenserverandclient. 
    - Max-age directive specifies how long a component is cached. (Cache-Control: max-age=315360000_
    - Mod_expires Apache module lets you use an Expires header that sets the date in a relative fashion similar to max-age. It sends both an expires header and a Cache-Control max-age header in the response. 
  - **Empty-Cache vs. Primed-Cache:**
    - Empty-cache: The browser cache is considered empty when none of your page's components are in the cache. Recently cleared, or first itme visiting website.
    - Primed-cache: when all of your page’s cacheable components are in the cache.
    - Caching only impacts your web performance only if the user hits your page with a primed cache. 
  - **More Than Just Images:**
    It's fairly common to use the far future Expires header on images, but we should extend this best practice to other components that are changed infrequently as well. This will help to optimize the experience for users with a primed cache. 
  - **Revving File Names:**
    Refers to changing the file name of cached files in order to allows users to get updates when you change certain cached components. 
    The browser will not check for any changes until the expiration date of the expires header, so that completed new representations will be loaded fresh from the origin server. 

## Chapter 4: Gzip Components
  - **How Compression Works:**
    We have been using the same file compression for decades for reducing file sizes in emails and to deliver compressed web pages in browsers. By reducing the actual size of HTTP responses, we can reduce response times. Gzip is the most popular and effective compression method.
    > An "Accept-Encoding: gzip, deflate header" indicates support for compression in the web client. 
    > Web server notifies web client of the use of compression using "Content-Encoding: gzip". 
  - **What to Compress:**
    - Most servers are configured to compress their HTML files, but it's also worthwhile to compress your scripts, stylesheets, or any text response including XML and JSON. You should not compress Image and PDF files because they are already compressed. 
    - Compression utilizes additional CPU cyles on the server and it may not worthwhile to compress every file. However a general rule of thumb is to compress files that are greater than 1 or 2K.
  - **The Savings:** 
    Gzipping generally reduces the response rate by 70%. 
  - **Proxy Caching:** 
    Uzing compression would make your proxy caching more complicated. For example, if your proxy server initially cached a uncompressed file from a request that did not support gzip, it would subsequently return the uncompressed version for every following request. You're missing out on a lot of savings if following HTTP requests do support gzip. It's a lot worse if the situation is reversed and a browser that does not support gzip receives a compressed file. 
    > You can solve this problem using a Vary: Accept-Encoding header. THe proxy server will not cache multiple versions of the response. 
  - **Edge Cases:** 
    There are several web browesers who state that they support compression, but with some known bugs. It's generally safer to use a browser whitelist approach and only allow proven browsers to receive compressed files. 

## Chapter 5: Put Stylesheets at the Top
  - **Progressive Rendering:**
    refers to how browser to display whatever content it has as soon as possible. It is also important to give users visual feedback in terms of a progress indicator. Putting stylesheets on the bottom prohibits rendering in many browsers. Browsers block rendering to avoid having to redraw elements of the page if their styles change. Because of this browsers will typically wait for the stylesheets before showing visible components on the page. 
  - **LINK tag vs @import rule:**
    Those are only two ways you can include a stylesheet in your document. @import rule must always be at the top and causes an unexpected ordering of how the components are downloaded. It is preferable to always use the LINK tag over the @import rule.
  - **Flash of Unstyled Content:**
    The blank white screen phenomenon is due to browser behavior.
    > If stylesheets are still loading, it is wasteful to construct the rendering tree, since you don’t want to paint anything at all until all stylesheets have been loaded and parsed. Otherwise you’ll run into a problem called FOUC (the flash of unstyled content prob- lem), where you show content before it’s ready.

## Chapter 6: Put Scripts at the Bottom
  - **Problems with Scripts:**
    Progressive rendering is blocked for all content below the script.Moving scripts lower in the page means more content is rendered progressively.
  - **Parallel Downloads:**
    The biggest impact on response time is the number of components in the page. HTTP/1.1 specifications suggests that browsers download only two components in parallel per host name. If a web page evenly distributed its components accross two hostnames, the overall response time would be about twice as fast. Running too many computations in parrallel has its costs, Yahoo! Research has shown that splitting components accross two hostnames leads to better performance. 
  - **Scripts Block Downloads:**
    - Parallel download has clear benefits; however, it is disabled while a script is downloading. The browser will not start any other downloads, even if it's from a different hostname. If scripts are put at the top of the page, everything is blocked from loading until the script is finished loading. 
    - DEFER attribute: indicates that the script does not contain document.write, and is a clue to browsers that they can continue rendering. However, if a script can be deferred, it should just be moved to the bottom of the page. 

## Chapter 6: Put Scripts at the Bottom











