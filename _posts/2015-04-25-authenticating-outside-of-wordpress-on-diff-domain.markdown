---
layout: post
title:  "Authenticate to WordPress Outside of WP"
date:   2015-04-25 11:59:49
categories: WordPress
tags: PHP
banner_image: "/media/banners/escape.jpg"
featured: false
comments: true
---

Sometimes you don't want to run ALL of WordPress, but you want to get the benefits of working *within* WordPress. In my case I needed to run a small PHP file, on a different domain (same server), and still use the same WordPress authentication of a primary site. I searched everywhere for a valid solution. I couldn't find anything that works, so I made my own.

<!--more-->

## Why Authenticate Outside of WordPress?

I've built a service for developers that you can see here: <a href="http://support.rdx.io">http://support.rdx.io</a>. This service allows developers to get a view into their user's profiles via a very secure URL. The results of this produced a screen like this:


So obviously I wanted to keep this information safe. I also planned to use EDD to verify if they had an active subscription to this service. I wanted to make sure I could set the WordPress auth cookie as well to remove the need to re-authenticate at every visit.

## Wait, Isn't This Insecure?
Well, yes-ish. If you're in a shared hosting environment, and your host has not locked down PHP execution outside of the directory, this may be able to be used maliciously. Yet, at the same time, you have to be authenticated to get anything of merit. Either way, it's my hope that your host is smart!



## One Step at a Time
We're going to build a relatively easy bit of code, but it needs to be understood. So let's take it one step at a time. I want you to be very confident in what you are doing. Please, don't misuse this code.

## Before the Core, Set Your Defines
```php
<?php
	define( 'WP_USE_THEMES', false );
    define( 'COOKIE_DOMAIN', false );
    define( 'DISABLE_WP_CRON', true );

    //$_SERVER['HTTP_HOST'] = ""; // For multi-site ONLY. Provide the 
    							  // URL/blog you want to auth to.
```
We have a few defines. We're setting these standard WordPress defines because we are going to still load the WordPress core, and we want to ensure these don't run. Let's look at each one.

* `WP_USE_THEMES` - We do NOT want to load the theme at all or any of that content.
* `COOKIE_DOMAIN` - We don't want to verify the cookie domain. We just want to take the cookie as it is.
* `DISABLE_WP_CRON` - I'm not sure if this is necessary, but I didn't want to have any cronjobs running remotely. So I disabled it.

### Multi-Site Declaration
If you're using multi-site, you're going to want to set `$_SERVER['HTTP_HOST']` to the domain you wish to authenticate to. This will "help" WordPress Multi-Site believe that you are authenticating with that domain. I tried doing the standard multi-site functions, but nothing was succesful until I did this. It just works.

## Load the WordPress Core
In order to use the WordPress functions, we have to load the core. It will run similar to WP-CLI and run without a UI. We're trying to instantiate the objects and get things running so we can authenticate. Run it!

```php
<?php
require("/var/www/yourdomain.com/htdocs/wp-load.php");
```

## Verify, Authenticate, Set Cookie
Now we get to the meat.

```php
<?php
    if ( is_user_logged_in() ) {
        $user = wp_get_current_user();
    } else {
        $creds                  = array();
        // If you're not logged in, you should display a form or something
        // Use the submited information to populate the user_login & user_password
        $creds['user_login']    = "";
        $creds['user_password'] = "";
        $creds['remember']      = true;
        $user                   = wp_signon( $creds, false );
        if ( is_wp_error( $user ) ) {
            echo $user->get_error_message();
        } else {
            wp_set_auth_cookie( $user->ID, true );
        }
    }
```

Because for all intensive purposes WordPress is running, we can now use basic functions! We can check if the user is logged in. The beaty of this, is even though we're on a different domain, because we set the `COOKIE_DOMAIN` define to false, it will authenticate to this domain without verifying the actual blog cookie!

If the user's logged in, then grab the user object.

If not, let's authenticate. Now here is where you're going to have to do some of your own modifications. I would probably suggest doing an form of some kind, unless you want to manually fill the `$creds` array. Either way, you need to figure that part out for yourself.

Now if the credentials work, we run `wp_set_auth_cookie( $user->ID, true )` which sets the auth cookie for this domain for this user! We're golden, we're in, and we can have the joys of an elongated cookie session for better user experience.

## Last Check & Using the Session
Now that we have a `$user` object, let's make sure it's a real user.

```php
<?php
	if ( !is_wp_error( $user ) ) {
    	// Success! We're logged in! Now let's test against EDD's purchase of my "service."
        if ( edd_has_user_purchased( $user->ID, '294', NULL ) ) {
            echo "Purchased the Services and is active.";
        } else {
            echo "Not Purchased";
        }
    }
```

We're testing that the returned object is actually a user. If not, we don't run our other code. Now for this example I'm using Easy Digital Downloads to verify that this user account has a valid subscription to one of my downloads/services.

## Final Thoughts and Full Code
So now we see how relatively easy it is to authenticate on the same server using WordPress authentication from a totally different domain. 

I urge you to be careful. Don't go making it so easy to hack that you destroy all of WordPress' security. And if you're a hacker, don't be evil! I posted this not to cause problems, but to help actual people like myself who needed to bypass the whole WordPress UI.

Just for closure though, here's the full bit of code we went over, with comments. Happy coding!

```php
<?php
    define( 'WP_USE_THEMES', false ); // Do not use the theme files
    define( 'COOKIE_DOMAIN', false ); // Do not append verify the domain to the cookie
    define( 'DISABLE_WP_CRON', true ); // We don't want extra things running...

    //$_SERVER['HTTP_HOST'] = ""; // For multi-site ONLY. Provide the 
    							  // URL/blog you want to auth to.

    // Path (absolute or relative) to where your WP core is running
    require("/var/www/yourdomain.com/htdocs/wp-load.php");

    if ( is_user_logged_in() ) {
        $user = wp_get_current_user();
    } else {
        $creds                  = array();
        // If you're not logged in, you should display a form or something
        // Use the submited information to populate the user_login & user_password
        $creds['user_login']    = "";
        $creds['user_password'] = "";
        $creds['remember']      = true;
        $user                   = wp_signon( $creds, false );
        if ( is_wp_error( $user ) ) {
            echo $user->get_error_message();
        } else {
            wp_set_auth_cookie( $user->ID, true );
        }
    }

    if ( !is_wp_error( $user ) ) {
    	// Success! We're logged in! Now let's test against EDD's purchase of my "service."

        if ( edd_has_user_purchased( $user->ID, '294', NULL ) ) {
            echo "Purchased the Services and is active.";
        } else {
            echo "Not Purchased";
        }
    }
```
