# LIA-Documentation
Beginner level documentation for web development specially WordPress FrontEnd

## Create Menu in WordPress

add code to functions.php

```php
function theme_setup() {
    add_theme_support('menus');

    register_nav_menu('primary', 'Primary Header Navigation');
}

add_action('init', 'theme_setup');
```

Go to "Appearance" and you will see a "menu" tab, there you create your custom menu.

---

To print out the menu

```php
<nav>
    <?php wp_nav_menu(array('theme_location' => 'primary')) ?>
</nav>
```

*'primary' is the name of the location where the menu is assigned in Wordpress. Change name depending on the assigned location*

## Create Sidebar

*add to functions.php*

```php
function widget_setup() {
    register_sidebar(
        array(
            'name' => 'Sidebar',
            'id' => 'sidebar-1',
            'class' => 'custom',
            'description' => 'Standard Sidebar',
            'before_widget' => '<aside id="%1s" class="widget %2s">',
            'after_widget' => '</aside>',
            'before_title' => '<h1 class="widget-title">',
            'after_title' => '</h1>'

        )
    )
}

add_function('widgets_init', 'awesome_widget_setup');
```

*add to sidebar.php*

```php
<div id="sidebar" class="widget-area">
    <?php dynamic_sidebar('sidebar-1') ?>
</div>
```

*then use this where you want the sidebar to be*

```php
<?php get_sidebar() ?>
```

## Custom Post Type

*add this to functions.php*

```php
function awesome_post_type() {
    $labels = array(
        'name' => 'Portfolio',
        'singular_name' => 'Portfolio',
        'add_new' => 'Add Portfolio Item',
        'all_items' => 'All Items',
        'add_new_item' => 'Add Item',
        'edit_item' => 'Edit Item',
        'new_item' => 'New Item',
        'view_item' => 'View Item',
        'search_item' => 'Search Portfolio',
        'not_found' => 'No items found',
        'not_found_in_trash' => 'No items found in trash',
        'parent_item_colon' => 'Parent Item'
    );
    $args = array(
        'labels' => $labels,
        'public' => true,
        'has_archive' => true,
        'publicly_queryable' => true,
        'query_var' => true,
        'rewrite' => array('slug' => 'portfolio'),
        'capability_type' => 'post',
        'hierarchical' => false,
        'supports' => array(
            'title',
            'editor',
            'excerpt',
            'thumbnail',
            'revisions'
        ),
        'taxonomies' => array('category', 'post_tag'),
        'menu_position' => 5,
        'exclude_from_search' => false
    );

    register_post_type('portfolio', $args)
}

add_action('init', 'awesome_post_type');
```

*then add this to where you want to print the custom posts*

```php
<?php 

    $args = array('post_type' => 'portfolio', 'posts_per_page' => 3);
    $loop = new WP_Query($args);

    while( $loop->have_posts()) {
        $loop->the_post(); ?>

        <?php get_template_part('content', 'archive') ?>
        
    <?php }
?>
```