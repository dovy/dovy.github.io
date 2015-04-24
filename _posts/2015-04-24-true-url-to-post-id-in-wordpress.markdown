---
layout: post
title:  "True URL to Post ID Within WordPress"
date:   2015-04-24 11:18:49
categories: WordPress
tags: PHP
banner_image: "/media/banners/lost.jpg"
featured: true
comments: true
---

Sometimes, within WordPress you need to locate a page, a post, and get the ID before the post object is done with it's intial query. If you are like many developers out there, and found that the <a href="https://codex.wordpress.org/Function_Reference/url_to_postid">`url_to_postid`</a> function is not sufficient, you are NOT alone. This function I created will make your day a happy one.

<!--more-->

## What does this function do?

First, it checks for things like an existing `post_id` query variable. Then it checks if there's a custom post type by a prefix name. It works on secure and insecure URLs. It works with permalinks on or off. It works with virtually **any** URL that may exist within a WordPress install. If nothing can be found, 0 is returned.

## How did you make this?
Lots of trial and suffering! For Redux I created the <a href="http://reduxframework.com/extensions/metaboxes">metaboxes extension</a>. This extension has to discover the custom post data at any time, on any page, even outside of loops. It was tough and it's essentially been a year in the making. It finally came to a point where every use case works. It's tried, tested, by thousands of users world-wide.   :)



### The Function
```php
<?php
/* Copyright (C) 2013-2015 Dovy Paukstys - All Rights Reserved
 * You may use, distribute and modify this code under the
 * terms of The Do What The F*** You Want To Public License (WTFPL).
 *
 * If it helps you, please GratiPay me: https://gratipay.com/dovy/
 */
function DOVYio_url_to_postid( $url ) {
    global $wp_rewrite;

    if ( ! empty( $this->post_id ) ) {
        return $this->post_id;
    }

    if ( isset( $_GET['post'] ) && ! empty( $_GET['post'] ) && is_numeric( $_GET['post'] ) ) {
        return $_GET['post'];
    }

    // First, check to see if there is a 'p=N' or 'page_id=N' to match against
    if ( preg_match( '#[?&](p|page_id|attachment_id)=(\d+)#', $url, $values ) ) {
        $id = absint( $values[2] );
        if ( $id ) {
            return $id;
        }
    }

    // Check to see if we are using rewrite rules
    if ( isset( $wp_rewrite ) ) {
        $rewrite = $wp_rewrite->wp_rewrite_rules();
    }


    // Not using rewrite rules, and 'p=N' and 'page_id=N' methods failed, so we're out of options
    if ( empty( $rewrite ) ) {
        if ( isset( $_GET ) && ! empty( $_GET ) ) {

            /************************************************************************
             * ADDED: Trys checks URL for ?posttype=postname
             *************************************************************************/

            // Assign $url to $tempURL just incase. :)
            $tempUrl = $url;

            // Get rid of the #anchor
            $url_split = explode( '#', $tempUrl );
            $tempUrl   = $url_split[0];

            // Get rid of URL ?query=string
            $url_query = explode( '&', $tempUrl );
            $tempUrl   = $url_query[0];

            // Get rid of ? mark
            $url_query = explode( '?', $tempUrl );


            if ( isset( $url_query[1] ) && ! empty( $url_query[1] ) && strpos( $url_query[1], '=' ) ) {
                $url_query = explode( '=', $url_query[1] );

                if ( isset( $url_query[0] ) && isset( $url_query[1] ) ) {
                    $args = array(
                        'name'      => $url_query[1],
                        'post_type' => $url_query[0],
                        'showposts' => 1,
                    );

                    if ( $post = get_posts( $args ) ) {
                        return $post[0]->ID;
                    }
                }
            }
            //END ADDITION

            //print_r($GLOBALS['wp_post_types']);
            //if (isset($GLOBALS['wp_post_types']['acme_product']))
            // Add custom rules for non-rewrite URLs
            foreach ( $GLOBALS['wp_post_types'] as $key => $value ) {
                if ( isset( $_GET[ $key ] ) && ! empty( $_GET[ $key ] ) ) {
                    $args = array(
                        'name'      => $_GET[ $key ],
                        'post_type' => $key,
                        'showposts' => 1,
                    );
                    if ( $post = get_posts( $args ) ) {
                        return $post[0]->ID;
                    }
                }
            }
        }
    }

    // Get rid of the #anchor
    $url_split = explode( '#', $url );
    $url       = $url_split[0];

    // Get rid of URL ?query=string
    $url_query = explode( '?', $url );
    $url       = $url_query[0];


    // Add 'www.' if it is absent and should be there
    if ( false !== strpos( home_url(), '://www.' ) && false === strpos( $url, '://www.' ) ) {
        $url = str_replace( '://', '://www.', $url );
    }

    // Strip 'www.' if it is present and shouldn't be
    if ( false === strpos( home_url(), '://www.' ) ) {
        $url = str_replace( '://www.', '://', $url );
    }

    // Strip 'index.php/' if we're not using path info permalinks
    if ( isset( $wp_rewrite ) && ! $wp_rewrite->using_index_permalinks() ) {
        $url = str_replace( 'index.php/', '', $url );
    }

    if ( false !== strpos( $url, home_url() ) ) {
        // Chop off http://domain.com
        $url = str_replace( home_url(), '', $url );
    } else {
        // Chop off /path/to/blog
        $home_path = parse_url( home_url() );
        $home_path = isset( $home_path['path'] ) ? $home_path['path'] : '';
        $url       = str_replace( $home_path, '', $url );
    }

    // Trim leading and lagging slashes
    $url = trim( $url, '/' );

    $request = $url;
    if ( empty( $request ) && ( ! isset( $_GET ) || empty( $_GET ) ) ) {
        return get_option( 'page_on_front' );
    }

    // Look for matches.
    $request_match = $request;


    foreach ( (array) $rewrite as $match => $query ) {

        // If the requesting file is the anchor of the match, prepend it
        // to the path info.
        if ( ! empty( $url ) && ( $url != $request ) && ( strpos( $match, $url ) === 0 ) ) {
            $request_match = $url . '/' . $request;
        }

        if ( preg_match( "!^$match!", $request_match, $matches ) ) {

            // Got a match.
            // Trim the query of everything up to the '?'.
            $query = preg_replace( "!^.+\?!", '', $query );

            // Substitute the substring matches into the query.
            $query = addslashes( WP_MatchesMapRegex::apply( $query, $matches ) );

            // Filter out non-public query vars
            global $wp;
            parse_str( $query, $query_vars );
            $query = array();
            foreach ( (array) $query_vars as $key => $value ) {
                if ( in_array( $key, $wp->public_query_vars ) ) {
                    $query[ $key ] = $value;
                }
            }

            /************************************************************************
             * ADDED: $GLOBALS['wp_post_types'] doesn't seem to have custom postypes
             * Trying below to find posttypes in $rewrite rules
             *************************************************************************/

            // PostType Array
            $custom_post_type = false;
            $post_types       = array();
            foreach ( $rewrite as $key => $value ) {
                if ( preg_match( '/post_type=([^&]+)/i', $value, $matched ) ) {
                    if ( isset( $matched[1] ) && ! in_array( $matched[1], $post_types ) ) {
                        $post_types[] = $matched[1];
                    }
                }
            }

            foreach ( (array) $query_vars as $key => $value ) {
                if ( in_array( $key, $post_types ) ) {
                    $custom_post_type = true;

                    $query['post_type'] = $key;
                    $query['postname']  = $value;
                }
            }

            // print_r($post_types);

            /************************************************************************
             * END ADD
             *************************************************************************/

            // Taken from class-wp.php
            foreach ( $GLOBALS['wp_post_types'] as $post_type => $t ) {
                if ( isset( $t->query_var ) ) {
                    $post_type_query_vars[ $t->query_var ] = $post_type;
                }
            }

            foreach ( $wp->public_query_vars as $wpvar ) {
                if ( isset( $wp->extra_query_vars[ $wpvar ] ) ) {
                    $query[ $wpvar ] = $wp->extra_query_vars[ $wpvar ];
                } elseif ( isset( $_POST[ $wpvar ] ) ) {
                    $query[ $wpvar ] = $_POST[ $wpvar ];
                } elseif ( isset( $_GET[ $wpvar ] ) ) {
                    $query[ $wpvar ] = $_GET[ $wpvar ];
                } elseif ( isset( $query_vars[ $wpvar ] ) ) {
                    $query[ $wpvar ] = $query_vars[ $wpvar ];
                }


                if ( ! empty( $query[ $wpvar ] ) ) {
                    if ( ! is_array( $query[ $wpvar ] ) ) {
                        $query[ $wpvar ] = (string) $query[ $wpvar ];
                    } else {
                        foreach ( $query[ $wpvar ] as $vkey => $v ) {
                            if ( ! is_object( $v ) ) {
                                $query[ $wpvar ][ $vkey ] = (string) $v;
                            }
                        }
                    }

                    if ( isset( $post_type_query_vars[ $wpvar ] ) ) {
                        $query['post_type'] = $post_type_query_vars[ $wpvar ];
                        $query['name']      = $query[ $wpvar ];
                    }
                }
            }
            // Do the query
            if ( isset( $query['pagename'] ) && ! empty( $query['pagename'] ) ) {
                $args = array(
                    'name'      => $query['pagename'],
                    'post_type' => 'page',
                    'showposts' => 1,
                );
                if ( $post = get_posts( $args ) ) {
                    return $post[0]->ID;
                }
            }
            $query = new WP_Query( $query );

            if ( ! empty( $query->posts ) && $query->is_singular ) {
                return $query->post->ID;
            } else {
                return 0;
            }

        }
    }

    return 0;
}
?>
```

-
--
### Credits for this Work
I couldn't do this alone. I stood on the back of giants and simply finished off their work. I'd like to credit the following:

* The WordPress Core: `url_to_postid()` inside ~/wp-includes/rewrite.php
* Adaptions taken from <a href="http://betterwp.net/wordpress-tips/url_to_postid-for-custom-post-types/">http://betterwp.net/wordpress-tips/url_to_postid-for-custom-post-types/</a>