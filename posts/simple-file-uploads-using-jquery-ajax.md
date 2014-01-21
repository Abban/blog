<!--%

Title: Simple File Uploads Using jQuery & AJAX
Summary: A method for handling file uploads on a form with AJAX using jQuery.
Author: Abban Dunne
Category: Notebook
Status: Published
Date: 14th October 2013

%-->

**Update (20, Nov, 2013):** I made a [plugin](https://github.com/Abban/jQuery-Ajax-File-Upload) to handle this.

## As you probably know file uploads over AJAX are a nightmare

I recently had a client ask for file uploads on an AJAX form at the last minute. Scrambling to get it working before deadline I stumbled onto the following method of handling them:

## 1. Grab file data from the file field
The first thing to do is bind a function to the change event on your file field and a function for grabbing the file data:

<script src="https://gist.github.com/Abban/6977334.js?file=prepareUpload.js"></script>

This saves the file data to a file variable for later use.

## 2. Handle the file upload on submit
When the form is submitted you need to handle the file upload in its own AJAX request. Add the following binding and function:

<script src="https://gist.github.com/Abban/6977334.js?file=uploadFiles.js"></script>

What this function does is create a new formData object and appends each file to it. It then passes that data as a request to the server. 2 attributes need to be set to false:

* __processData__ - Because jQuery will convert the files arrays into strings and the server can't pick it up.
* __contentType__ - Set this to false because jQuery defaults to `application/x-www-form-urlencoded` and doesn't send the files. Also setting it to `multipart/form-data` doesn't seem to work either.

## 3. Upload the files
Quick and dirty php script to upload the files and pass back some info:

<script src="https://gist.github.com/Abban/6977334.js?file=handleUpload.php"></script>

Don't use this, write your own.

## 4. Handle the form submit
The success method of the upload function passes the data sent back from the server to the submit function. You can then pass that to the server as part of your post:

<script src="https://gist.github.com/Abban/6977334.js?file=submitForm.js"></script>

## Final note
This script is an example only, you'll need to handle both server and client side validation and some way to notify users that the file upload is happening. I made a project for it on [Github](https://github.com/Abban/jQueryFileUpload) if you want to see it working.