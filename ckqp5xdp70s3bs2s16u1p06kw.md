## How to Hard Refresh the Web Browser?

Recently I was working on a small web development project where after modifying one of the JavaScript file I was not seeing the updates on webpage. I have tried restarting the browser, restarted the Flask app but no luck. It was frustrating then thought to do a hard refresh and voila the updated content reflected on the page. Sometimes, a website does not behave as expected or seems stuck showing outdated information. To fix this, it’s easy to force your browser to completely reload its local copy of the page (cache) using a simple keyboard shortcut. Today's article is all about how to do web browser hard refresh to by pass the cache.

### What Is a Browser Cache?

To speed up browsing, web browsers save copies of website data to your computer as a set of files called a cache. When you load a website, you are often viewing a local copy of elements from the site (such as images) pulled from your cache.

Normally, if the browser loads a website and detects a change, it will fetch a new version of the site from the remote web server and replace the cache. But the process is not perfect, and sometimes your browser may end up with a local copy of the website data in your browser cache that doesn’t match the latest version on the server. As a result, a web page may look incorrect or not function properly.

To fix this, we need to force the web browser to discard what it already has in the cache and to download the latest version of the site. Many people call this a “hard refresh.”

### How to Perform a Hard Refresh in Your Browser?

In most browsers on PC and Mac, you can perform a simple action to force a hard refresh. Hold down the Shift key on your keyboard and click on the reload icon on your browser’s toolbar.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625399765923/JfT0uCEln.png)

### Keyboard shortcuts

- There are also keyboard shortcuts to perform the equivalent hard refresh. Because there are multiple ways to do the same action,  listed below:

- Chrome, Firefox, or Edge for Windows: Press Ctrl+F5 (If that doesn’t work, try Shift+F5 or Ctrl+Shift+R).

- Chrome or Firefox for Mac: Press Shift+Command+R.

- Safari for Mac: There is no simple keyboard shortcut to force a hard refresh. Instead, press Command+Option+E to empty the cache, then hold down Shift and click Reload in the toolbar.

- Safari for iPhone and iPad: There is no shortcut to do a cache refresh. You’ll have to  [dig into settings](https://www.howtogeek.com/205653/how-to-clear-history-cache-and-cookies-in-safari-on-iphone-or-ipad/) to erase your browser’s cache.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625401039904/8jR-mKHv-.png)

After the hard refresh, the web page will go blank for a second, and the reloading process will take little bit longer than usual because the browser is redownloading all of the data and images on the site. Now you should see the updated content on webpage.

Let me know if you find it helpful. Happy learning!
