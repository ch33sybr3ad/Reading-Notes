High Performance Web Sites Notes

1) Make Fewer HTTP Requests
     using image maps, CSS sprites, inline images, and combined script and stylesheets 
     Image Maps: allow you to associate multiple URLS with a single image. The destination URL is chosen based on where the user clicks on the image. This allows you to replace multiple HTTP requests with just one. 
     CSS Sprites: allow you to combine images, but they’re much more flexible than image maps. Each image is wrapped in a span that has a different class, you specify the distance between each image using background-position property.
          - combined images tend to be smaller than the sum of separate images, resulting from reducing amount of image overhead (color tables, formatting information, etc.)
     Inline Images: 