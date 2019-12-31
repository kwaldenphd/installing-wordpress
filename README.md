# WordPress installation lab

This tutorial was written by [Lindsay K. Mattock](http://lindsaymattock.net) and adapted by [Katherine Walden](https://github.com/kwaldenphd). 

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

In this lab, we will install WordPress on our Raspberry Pi Webserver. 

WordPress has two versions: 
1. a hosted version Wordpress.com where you create a login and have web access to the administrative end of the software so that you can create and modify a website
2. a self-hosted version from Wordpress.org that you install on your server and manage the WordPress instance locally yourself. The self-hosted version allows you to make many more customizations to your site and the software and have full control of the site. 

With the hosted version, you are accessing the software on another server maintained by the developer of the software. With the self-hosted version, you are installing and accessing the software on your own server – in this case, our Raspberry Pi. The other option is to use a third-party host like https://reclaimhosting.com/. With this option a third-party maintains the server (the computer), but you have full access at the Terminal just like you do on your Pi. You can download any software that you’d like and work with the system at the command line. You are essentially leasing space on another's server.

## Lab Objectives

By the end of this lab you will be able to:
-	Explain the differences between hosted and self-hosted software
-	Describe how the elements of the LAMP stack work together with self-hosted software
-	Install and configure an FTP client on your Pi
-	Build a website using WordPress

## Acknowledgements

This lab is based on the "Project 6: WordPress" project materials developed by [Lindsay K. Mattock](http://lindsaymattock.net/) for the the [SLIS 5020 Computing Foundations course](http://lindsaymattock.net/computingfoundations.html). 

# Table of Contents
- [Getting Started With WordPress](#getting-started-with-wordpress)
- [Step 1: Download the WordPress Package](#step-1-download-wordpress-package)
- [Step 2: Create a database for WordPress](#step-2-create-a-database-for-wordpress)
- [Step 3: (Optional) Find and rename wp-config-sample.php to wp-config.php](#step-3-optional-find-and-rename-wp-config-samplephp-to-wp-configphp)
- [Step 4: Upload the WordPress files to the desired location on your webserver](#step-4-upload-the-wordpress-files-to-the-desired-location-on-your-webserver)
  * [Setting a Permission Scheme for Wordpress](#setting-a-permission-scheme-for-wordpress)
- [Step 5: Run the WordPress installation script by accessing the URL in a web browser](#step-5-run-the-wordpress-installation-script-by-accessing-the-url-in-a-web-browser)
  * [Using WordPress](#using-wordpress)
  * [FTP for WordPress](#ftp-for-wordpress)
- [Create a WordPress Site](#create-a-wordpress-site)
- [Putting It All Together](#putting-it-all-together)
- [Lab Notebook Questions](#lab-notebook-questions)


# Getting Started With WordPress

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/image_1.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/image_1.jpg?raw=true" /></a></p>

1. Let’s take a look at WordPress. The full WordPress documentation can be found at https://codex.wordpress.org/Main_Page. 

2. You should start with “Installing WordPress” https://codex.wordpress.org/Installing_WordPress. First, take a look at “Things to Know Before Installing WordPress.”

3. First, the instructions tell you that you need access to your web server. This is your Pi. 

4. Second, you need a text editor. We’ve been using `nano` at the Terminal. We don’t have an FTP client, but this isn’t essential for the install, so we’ll take a look at this in a bit (see step 7). 

5. And finally, you need a web browser. We have the GUI browser and `lynx` at the command line.

<blockquote>FTP and Shell: In the documentation you see a reference to FTP and Shell. FTP stands for File Transfer Protocol and Shell is short for SSH or Secure Shell. Think back to when we installed the firewall – these were two additional protocols or rules that we could add to our firewall. Both methods are used to access a server, usually offsite. That is, if you are using a hosting service you would use FTP or SSH to log into your server remotely. We’ll take a look at FTP later, as this is the method that WordPress uses to load themes and plugins.</blockquote>

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_2.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_2.jpg?raw=true" /></a></p>

6. Look at the system requirements to get a sense of what the minimum software requirements for WordPress. Here we see some of the LAMP stack components we installed in the LAMP stack project.

<blockquote>Nginx: (pronounced Engine X) is another web server http://nginx.org/. You can use Nginx as an alternative to Apache in your stack.</blockquote>

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/image_3.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/image_3.png?raw=true" /></a></p>

7. Now we are ready to install. Let’s look at the instructions for WordPress's “Famous 5-Minute Install” https://codex.wordpress.org/Installing_WordPress#Famous_5-Minute_Install.

8. If you are feeling brave, try working just from the 5-Minute Install Instructions on WordPress's website.

9. Steps for the Famous 5-Minute Install are listed below with a few hints. It may take you more than 5 Minutes – DON’T PANIC! For most of you this will be your first install of self-hosted software and it’s OK if you need to take your time. 

# Step 1: Download the WordPress package

1. Navigate to https://wordpress.org/download/ in a web browser.

2. Download the installation package.

# Step 2: Create a database for WordPress

3. We'll a database for WordPress on your web server, as well as a MySQL user who has all privileges for accessing and modifying it.

4. When we installed MySQL a few weeks ago, we created a database called csc2020 and a user cs_user. All we need to do is repeat this step to create a database for WordPress to use.

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_4.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_4.jpg?raw=true" /></a></p>

5. Open a Terminal session and log in to MySQL with the command `mysql –u root –p;` This command is requesting to log in to MySQL as the root user. You’ll be prompted for your root password. 
  * Remember the cursor will not move as you enter your password.

<blockquote>IMPORTANT: Remember the cursor will not move as you enter your password.</blockquote>

<blockquote>IMPORTANT: the semicolons (;) at the end of each of the following lines are crucial for ending the commands. If you press <enter> before typing the ; the command will not execute. Simply type the ; on the next line to execute the command.</blockquote>
  
6. You will now see the mysql prompt `mysql>`. Remember, this means that we are now working within MySQL and not the Bash Shell.

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_5.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_5.jpg?raw=true" /></a></p>
 
7. Now we can create a new database and user for WordPress. You can call your database anything that you’d like. Here I am going to call my database `wp2020.` Create a new database using `sudo create database wp2020;`.

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_6.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_6.jpg?raw=true" /></a></p>

8. Now you need to create a user and password. Again, you can call your user whatever you’d like. I’m using `wp_user`. Replace the word `password` with your password. `grant all on wp2020.* to ‘wp_user’ identified by ‘password’;`

9. Type `quit` to quit MySQL and return to the Bash Shell.

# Step 3: (Optional) Find and rename wp-config-sample.php to wp-config.php.

10. Find and rename wp-config-sample.php to wp-config.php, then edit the file and add your database information. This step is optional, so we are going to skip it for now. We’ll return to this step in a few minutes.

# Step 4: Upload the WordPress files to the desired location on your webserver.

11. At the moment, the wordpress.zip file is still sitting in our Downloads folder. We haven’t unzipped it and we haven’t moved it into place. 

12. We’ll do that now using the command line to unzip and move this file.

13. Navigate to `Downloads` in the Terminal with the `cd` command. Use `ls` to see the contents.

14. Use `unzip` to unzip or “inflate” the compressed file.

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_7.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_7.jpg?raw=true" /></a></p>

15. Use the `ls` command again. You should now see a `wordpress` directory in blue text.

16. Now we need to put our `wordpress` folder in our `public_html` directory, where all of our web files are located for our website. 

18. We have two options:
  a. “integrating WordPress into the root of our domain” – this means that when we visit `mysite.dev`, the home page will default to our WordPress site. We can use the `mv` command to move all of the contents of the folder (each individual file within the wordpress directory) to the `public_html` folder. `sudo mv ~/Downloads/wordpress/* /var/www/mysite.dev/public_html`

  b. The second option is to install WordPress in a subdirectory. This means that we will move the wordpress folder to our `public_html` directory. We will access WordPress by navigating to http://mysite.dev/wordpress in the browser. We can accomplish this using `mv wordpress /var/www/mysite.dev/public_html`

19. You can also rename your folder if you’d like. For example, in the WordPress instructions the wordpress directory has been renamed “blog” so that you would navigate to http://mysite.dev/blog. To rename the folder you also use the move command: `mv wordpress newname`. For example, `sudo mv wordpress blog` to rename the folder to `blog`.

**THE INSTRUCTIONS ARE WRITTEN FOR OPTION 1**

20. Navigate to `/var/www/mysite.dev/public_html` directory and use `ls` to double check the files have been moved.

## Setting a Permission Scheme for WordPress

21. During our Raspbian install, we used `ls –l` to view the details about some of the files and folders on our Pi, including the permission scheme. To allow WordPress to access the files in the directory we just created, we need to make a few changes.

22. Use `cd` to navigate to `/var/www/mysite.dev/`.

23. Use `ls –l` to view the folder contents as well as ownership and permissions information. 

24. We need to change the ownership so that the server can make changes to the directories.
 
25. Use `sudo chown –R www-data:www-data public_html` to change the permissions. Use `ls –l` to see the changes we just made.

<blockquote>Use http://explainshell.com for further explanation of these commands.</blockquote>

26. Now we need to change the permissions for the files and directories within the `public_html` directory. 
  a. use `chmod 775 public_html` to change the permissions on the `public_html` folder
  b. use `cd` to move into the `public_html` folder
  c. use `sudo find .–type d | xargs sudo chmod 777`
  e. use `sudo find . –type f –exec chmod 664 {} \;`
  f. use `ls -l` to view the changes

# Step 5: Run the WordPress installation script by accessing the URL in a web browser.

27. If you used method (1) above, navigate to http://mysite.dev/wp-admin in your web browser (Chromium). If you used method (2), then navigate to http://mysite.dev/wordpress. 

28. This will open the installation interface for WordPress. This page explains that we need the information from the database that we created in Step 2 to create the `wp-config.php file` (remember we skipped this optional step earlier in Step 3). 

29. Scroll to the bottom of the page and click <Let’s Go>.

30. Replace the database name, username, and password with the name of the MySQL database, username and password you created in Step 2. 

31. The database host can stay set to `localhost` and the table prefix can remain unchanged. Click <Submit>.

32. You should now see a `Welcome` page. This form asks for the information for your WordPress site. 

33. Fill in the form and press <install>. You have now successfully installed WordPress!

## Using WordPress

34. Use the user name and password generated during this step to access the admin functions on your site. 

## FTP for WordPress

35. WordPress uses FTP or File Transfer Protocol to upload themes and plugins. We need to take a few steps to configure FTP on our server.

<blockquote>FTP or File Transfer Protocol: FTP is a protocol just like HTTP that defines how files and directories are transferred between computers. FTP connects on Port 21.</blockquote>

36. Use `sudo apt-get install ftp` to install FTP

37. Use `sudo apt-get install vsftpd` to install the vsFTPd FTP client.

38. Configure the firewall to allow communication on port 20. Remember that we only set a limited number of rules when we installed the firewall a few weeks ago. 

39. Use `sudo ufw` to open the firewall. Use `sudo ufw allow ftp` to allow communication via ftp.

40. Use `sudo passwd pi` to set a password for the pi user. 

41. This is now the login password for your user. You will also be prompted to enter this password when you use sudo.

42. Use `sudo service vsftpd start` to start the ftp client

43. Use `ftp localhost` to test the ftp connection. 

44. You will be prompted for your user name (`pi`) and password (`the password you just created`). After you have successfully logged in, use `quit` to close the ftp connection.

45. You should now be able to make ftp connections between WordPress and your server to install plugins and themes. 

46. When prompted by WordPress use the host `ftp.mysite.dev` along with `pi` as your user name and your password for the Pi to connect.

# Create a WordPress Site

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_8.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_8.jpg?raw=true" /></a></p>

47. Now, take some time to add some content to your new WordPress site. Unlike our HTML example, we have a GUI interface for generating a website. Pick a theme, create a blog post, and add a page. You can use your HTML project site as an example

<blockquote>Q1: Compare your experience working in WordPress to building your own HTML site at the beginning of the term. Which do you prefer, why?</blockquote>

# Putting It All Together

WordPress is a useful platform for generating websites. In fact, as of 2016 it is estimated that over 25% of the web runs on WordPress. As a content management system it simplifies the process of building a website by storing all of the elements of your site in one place and providing a GUI interface for modifying, customizing, and adding content to your site. The self-hosted version of WordPress also allows you to generate a website without also having to maintain a server. Hopefully, you do see some benefit in the DIY method. Or at least in trying it out just this once.

So, what makes WordPress tick? Behind the GUI interface, your website can be reduced to a few main elements:
- `.php` files that drive the dynamic content
- `.css` files that provide some style
- `.html` files that create content

All of this is stored in a database behind the scenes. This is what makes WordPress a content management system.

<blockquote>Learn more about content management systems (CMS) in Wikipedia's <a href="https://en.wikipedia.org/wiki/Content_management_system">"Content management system" article</a></blockquote>

Let’s take a look under the hood.

48. Open the “Edit Themes” page by navigating to Appearance > Editor in the Dashboard.

49. While the Customize menu allows you to make changes to the theme that you selected in the GUI interface, this is where you can access all of the `.css` and `.php` files that drive the look and feel of your site.

<blockquote>Q2: Navigate through the .css and some of the .php files. Do you feel comfortable editing these files? What looks familiar? What do you have questions about?</blockquote>

<p align="center"><a href="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_9.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/installing-wordpress/blob/master/images/Image_9.jpg?raw=true" /></a></p>

50. Now let’s look at the WordPress database. 

51. In your browser open phpMyAdmin by navigating to http://www.mysite.dev/phpmyadmin. Login with your username and password. On the left side of the screen you should now see a number of tables with the prefix wp_. These are the tables containing the data from your WordPress site.

<blockquote>Q3: Explore a few tables. What information can you find? Record your observations in your notebook. Try wp_users, wp_options, and wp_posts.</blockquote>

# Lab Notebook Questions

All of the required questions are listed here for you to reference. Be sure to answer each question completely, including an explanation of how you arrived at your answer.

Q1: Compare your experience working in WordPress to building your own HTML site at the beginning of the term. Which do you prefer, why?

Q2: Navigate through the .css and some of the .php files. Do you feel comfortable editing these files? What looks familiar? What do you have questions about?

Q3: Explore a few tables. What information can you find? Record your observations in your notebook. Try wp_users, wp_options, and wp_posts.
