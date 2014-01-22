<!--%

Title: Get Anchor CMS working with PHP 5.3.0+
Author: Abban Dunne
Category: Notebook
Status: Published
Date: 20th June 2013

%-->

##Step 1. Install Anchor as normal.

##Step 2. Create the environments file.

Create a new php file called environments.php save it into the anchor folder and add this content:

<script src="https://gist.github.com/Abban/5820865.js?file=environments.php"></script>

##Step 3. Include it in the initilisation.

Open the root index.php and just above where it says `require SYS . 'start' . EXT;` around line 33 add `require APP .'environments' .EXT;`

##Step 4. Edit the database connection.

Open anchor/config/db.php and in place of the database connection info paste the following:

<script src="https://gist.github.com/Abban/5820865.js?file=db.php"></script>

Remember to save your current database connection and paste it into he correct place in the switch. You can now add the info for multiple database connections depending on the environment. 