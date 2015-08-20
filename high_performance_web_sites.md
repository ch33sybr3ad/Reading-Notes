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



