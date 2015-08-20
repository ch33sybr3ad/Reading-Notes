# High Performance Web Sites Notes, by Steve Souders

Start: August 19th, 2015
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
  
