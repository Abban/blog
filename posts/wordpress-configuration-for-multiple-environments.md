<!--%

Title: Wordpress Configuration for Multiple Environments
Author: Abban Dunne
Category: Notebook
Status: Published
Date: 29th March 2012

%-->

No matter how small your business is, if you make websites you work in multiple environments. If you're a freelancer working alone chances are that for each project you work on you have at least two environments, possibly three. One on your local machine, one for client previews, and maybe a further one for production.

If you work as part of a team it can get even more complicated. In my office we can have up to three people contributing on a job, each with their own local environment settings. Plus, depending on the needs of the client, there could be up to four server environments set up. Throw source control into the mix (and you really should be using source control) and you end up with a small headache just keeping track of whats going on.

In this post I'm documenting a method of working with WordPress and multiple environments but the same concepts can be applied across a range of other content management systems such as ExpressionEngine and Joomla or frameworks like CodeIgniter.

### Overview of the different environments

1.  The development environment is for testing settings on your server. It helps to test for problems which would be server specific. This is only for you or your team, the client never sees it.
2.  The preview environment is for allowing your client to preview the current work in progress without having to give them access to the source code.
3.  Staging is for testing before deployment to production and is hosted on the clients servers. You put the site up here towards the end of the project when you are happy letting them have access to the source code. If you have an ongoing retainer with the client you might ditch the preview environment and only use this.
4.  Production is the live environment. The site is deployed here when it has been fully tested and is ready to be seen by the world.

### The development cycle

1.  The developer works on a feature in their local environment.
2.  When the feature is ready, the developer commits it to the source control and deploys it to development server to test it.
3.  When the feature is reviewed and tested it is deployed to preview server.
4.  The client then previews and you go through a number of revisions.
5.  When the client is happy they sign off and it is deployed to staging.
6.  Then on launch day, the site is deployed to production.

### The problem with this setup and WordPress

The problem with the way WordPress is set up is that it has only one configuration file (wp-config.php) which allows you to specify your database details and a few other options for a single environment.

Say you're finished working on a feature and you want to put it onto a preview site and allow the client to look at it. You create the webspace and deploy the latest revision from your source control. The wp-config file that gets uploaded to your preview server will still have the details for connecting to your local database meaning it won't work. Changing these details to point to the servers database obviously means that then your local site won't work. You can get around this by deploying, logging into the server through FTP and changing the settings manually but this isn't really an ideal solution as theres still the chance it could be overwritten by accident through a future deployment.

### Configuration differences across environments

1.  Database configuration.
2.  Error reporting, it should be turned on on development servers and off on preview, staging and production.
3.  Robots need to be blocked from development, preview and staging environments. Having Google display links to your work in progress is not just embarassing, duplicate content will hurt your clients SEO.
4.  Keeping unauthorised people from accessing the development and preview environments.

## The Solution

Enter the solution, the multi-environment [wp-config bootstrap](http://github.com/Abban/WordPress-Config-Bootstrap). This is a custom file that replaces the wp-config file in your installation and helps you configure your different environments.

### How it works

This works by accessing the `$_SERVER['SERVER_NAME']` variable which gives us the server name. The environments each have a unique URL which gives them their own unique itentifier that will allow us differentiate between them, something similar to this_

*   **Local**: http_//localhost/project ("localhost")
*   **Development**: http_//dev.yoursite.com ("dev.")
*   **Preview**: http_//preview.yoursite.com ("preview.")
*   **Staging**: http_//stage.clientsite.com ("stage.")
*   **Production**: http_//clientsite.com (none of the above, which is also unique!)
Then we define a constant variable giving us an easy way to check the environment throughout the application. This all works like this_

<script src="https://gist.github.com/Abban/5816521.js?file=environments.php"></script>

### Adding your database connection details

Now all we need to do to add the different database configuration details. I'm using a switch here but an if statement will work just as well.

<script src="https://gist.github.com/Abban/5816521.js?file=databases.php"></script>

You should create different databases for each environment. However, if you want to use the same database for multiple ones you can define the `WP_SITEURL` and `WP_HOME constants` for each one inside the switch. This will hardcode the site URL meaning all your links will work.

Just be careful because if for example you're using the same database for development and preview, any changes you make to content in development will then be reflected in the preview.

You could also use the same database and customise the table prefix for the environments which would effectively add multiple WordPress installations to the one database.

Lastly it can be set it up to turn on debugging automatically for the local and development environments and off for the others making sure you never accidentally leave it on on your production site.

### Other usage examples

Now that we have the `ENVIRONMENT` variable set up we can add a few handy functions to the theme.

#### is_production()

A function to check if the current environment is production or not. Add this into your themes functions.php file:

<script src="https://gist.github.com/Abban/5816521.js?file=is-production.php"></script>

#### Using Less or Sass

If you like to use Less or Sass and only compile the CSS for the production you can add this snippet into your themes header_

<script src="https://gist.github.com/Abban/5816521.js?file=less-sass.html"></script>

Just make sure you compile the CSS before deploying to production!

#### Making the site accessible to only logged in users

If you want to make sure only people who are logged in can see the site you can reirect all unlogged in requests to the WordPress login page. Handy for keeping clients from nosing around your development environment. Add this to functions.php:

<script src="https://gist.github.com/Abban/5816521.js?file=pw.php"></script>

#### Adding a cookie check

A simple way of adding a cookie check to your preview environment that only lets people with a preview cookie view it. The cookie is created by adding ?preview to the URL like this `http://preview.yoursite.com?preview`

<script src="https://gist.github.com/Abban/5816521.js?file=cookie.php"></script>

That will keep the general public out of the site while giving the client an easy way to preview it.

#### Blocking robots

As explained earlier, keeping crawlers out of your development, preview and staging sites is very important. You can make sure this is done automatically by adding the following snippet_

<script src="https://gist.github.com/Abban/5816521.js?file=robots.php"></script>

And there you have it, a nice professional way to manage your WordPress sites across your environments. This was inspired by [Michael Flanagan's](http://twitter.com/micflan) CodeIgniter setup and Leevi Grahams [Config Bootstrap](http://ee-garage.com/nsm-config-bootstrap) for ExpressionEngine. You can download the wp-config file by clicking the link below and if you have any suggestions please leave a comment.

[Download from Github](http://github.com/Abban/WordPress-Config-Bootstrap)