<!--%

Title: Add A Current Class To wp_get_archives.
Author: Abban Dunne
Category: Notebook
Status: Published
Date: 16th June 2011

%-->

Sometimes I like to use wp_get_archives or the archive widget for a secondary navigation. This lets you list posts organised by month. The only problem with it is it doesn't highlight the current active item. To overcome this I wrote this little jQuery function:

<script src="https://gist.github.com/Abban/5816625.js"></script>

So what's happening here is its I'm setting a variable called date and setting it with a WordPress function called get_the_date(); This sets it to the current template date. It then looks through your widget for the item with the title attribute set to that date. Just remember that I wrap my widgets in a div with a class widget so if you use a different class you need to change it to the appropriate one.