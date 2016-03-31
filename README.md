# Umbraco Deployment Checklist

Inspired by [Alexander Haneng's](https://twitter.com/ahaneng) (a former colleague) [deployment checklist for EPiServer](http://world.episerver.com/blogs/Alexander-Haneng/Dates/2011/9/EPiServer-CMS-Checklist-Deployment-Checklist/), I wanted to create one for [Umbraco](http://umbraco.com/). Mainly as a reminder to myself and hopefully the list can get some additions from the community. 
Most of the points are not specific to Umbraco, but general to all ASP.NET MVC deployments.
The list is not sorted or prioritized in any particular way, but I have grouped the items in a way that seems logical to me.

## Server

| Title | Description |
| :--- | :--- | 
| Separate drive | You do not want the system drive to run out of free space, so make sure the website, logging (both IIS and Umbraco), Umbraco media folder, search index, caching etc are stored on a separate drive  |
| Backup and monitoring | Ensure that both  web server and SQL Server are constantly monitored and regularly backed up  |

## Code

| Title | Description |
| :--- | :--- | 
| Release mode | All DLL's in production environments should be built in Release mode |
| Minified CSS and JS | If possible, all JavaScript and CSS files should be bundled and minified. I use [Mindscape Web Workbench](http://www.mindscapehq.com/products/web-workbench) but there are loads of options |
| Remove debug stuff | Search through your entire project for lines that you usually use for testing/debug and remove the ones that will display in your html, css or javascript. "console.log", "todo", "lorem ipsum", "test" etc.  |
| Google Analytics | If you use Google Analytics, make sure the tracking code is correct  |
| Caching | Make sure the code leverages caching offered by the framework. Look [here](http://stefantsov.com/umbraco-7-mvc-performance/) and [here](http://www.abstractmethod.co.uk/blog/2015/8/optimize-your-mvc-umbraco-site/) |

## Configurations

| Title | Description |
| :--- | :--- | 
| Debug = false | Seems like the most obvious thing in the world, but I still stumble upon sites with compilation debug set to true in production environments  |
| Force WWW | To keep consistent URLs on my site, I like to force my website to be visited with the WWW prefix. One way to ensure this is using a simple [URL rewrite rule](http://stackoverflow.com/questions/10153670/microsoft-rewriting-module-force-www-on-url-or-remove-www-from-url)
| Verify all paths | Search for ":\" and "\\" in all config files and check that all paths are correct  |
| Verify appsettings and connectionstring | make sure they have the correct values for production environment |
| Encrypt connectionstrings | Don't keep connectionstrings in plaintext in the [demilitarized zone](https://en.wikipedia.org/wiki/DMZ_(computing)). [Separate the connectionstring section] (http://stackoverflow.com/questions/6582970/separate-connectionstrings-and-mailsettings-from-web-config-possible) and [encrypt it on the web server](http://www.codeproject.com/Tips/795135/Encrypt-ConnectionString-in-Web-Config) (Remember: You cannot encrypt the file on one server and transfer it to another. You must copy the file in plaintext and do the encryption on each server) |
| Security headers | Analyze security headers at  [Securityheaders.io](https://securityheaders.io/) and add the missing ones |

## IIS

| Title | Description |
| :--- | :--- | 
| SSL | No excuses. Get a certificate and make sure that information sent between your site and the users are encrypted. Analyze your website at [SSLLABS](https://www.ssllabs.com/ssltest/analyze.html) and try to get an A. [Read Sebastiaans great blog post about securing Umbraco](https://cultiv.nl/blog/so-you-want-to-secure-your-umbraco-site/) Remember to add ```<add key="umbracoUseSSL" value="true" />``` in web.config! |
| No logging until needed | Turn off logging (both IIS and Umbraco) until needed. (Not everyone will agree, and if you have a low-traffic website you can ignore this part. I usually do like this: Keep logging on for 2 weeks. If everything runs smooth I turn it off until things get shaky) |
| App pool recycle | Create a separate Application Pool for the site and  [schedule a nightly recycle](https://technet.microsoft.com/nb-no/library/cc754494(v=ws.10).aspx) |
| Robots.txt | Do you need search engines to ignore parts of your page? |
| 404 | Do you handle 404's in a decent way?  |
| 500 | Do you handle 500's in a decent way? [Custom error page](https://msdn.microsoft.com/en-us/library/h0hfz6fc(v=vs.85).aspx)  |
| Print.css | Sounds crazy, but some people actually print web pages |

## Umbraco

| Title | Description |
| :--- | :--- | 
| Latest version | Update to latest (stable) version |
| Files and images | Verify that files and images can be uploaded to the site |
| Scheduled tasks | Verify that schedule tasks are running |
| Search | Test some search queries |
| Access to backoffice | Do you want to expose the ".../Umbraco" URL to everyone or limit it to your local network? [Check out this blog post to restrict access to backend by IP address](http://staheri.com/my-blog/2014/may/restrict-umbraco-cms-by-ip-address/) |
| Redirect old URLs | Are you replacing an old site with new URLs? Go through analytics data and set up 301 redirects for the most popular pages. The [301 URL Tracker](https://our.umbraco.org/projects/developer-tools/301-url-tracker/) makes it easy, and is planned implemeted in the core for [7.5.0](https://our.umbraco.org/contribute/releases/750) |
| Forms | Using Umbraco Forms? Make sure it's the correct license and try to submit a test form |
| Cold boot | Delete cache and search index. Restart site. [Republish site] (https://our.umbraco.org/wiki/reference/api-cheatsheet/publishing-and-republishing/) Reindex  |

## SEO

| Title | Description |
| :--- | :--- | 
| Page titles | One h1 tag on each page? |
| Meta data | [HTML meta tags](http://www.w3schools.com/tags/tag_meta.asp) |
| Alt text | Images have alt text? |
| Broken links | Check site for broken links. I Use [LinkChecker](https://wummel.github.io/linkchecker/) | 
| Favicon | Add favicon |

## Load balancing

TODO!
