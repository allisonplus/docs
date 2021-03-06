---
title: acf/input/admin_enqueue_scripts
description: Called during the enqueue_scripts action when editing a post.
category: actions
status: draft
---

## Description
Used to enqueue scripts and styles on pages where ACF fields are rendered. For example, the page / post edit screen, front end form, Options page, etc.

It is similar to the WordPress action [admin_enqueue_scripts](https://codex.wordpress.org/Plugin_API/Action_Reference/admin_enqueue_scripts).

## Example
This example demonstrates how to use the action to register a style and/or script.
```
function my_acf_admin_enqueue_scripts() {

	// Register style.
	wp_register_style( 'my-acf-input-css', get_stylesheet_directory_uri() . '/css/my-acf-input.css', false, '1.0.0' );
	wp_enqueue_style( 'my-acf-input-css' );

	// Register script.
	wp_register_script( 'my-acf-input-js', get_stylesheet_directory_uri() . '/js/my-acf-input.js', false, '1.0.0');
	wp_enqueue_script( 'my-acf-input-js' );
}

add_action( 'acf/input/admin_enqueue_scripts', 'my_acf_admin_enqueue_scripts' );
```
