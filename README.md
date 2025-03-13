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
            'before_widget' => '<aside id="%1$s" class="widget %2$s">',
            'after_widget' => '</aside>',
            'before_title' => '<h1 class="widget-title">',
            'after_title' => '</h1>'

        )
    )
}

add_function('widgets_init', 'widget_setup');
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

## Custom taxonomy and term

```php
function custom_taxonomies() {

    // taxonomy hierarchical
    $label = array(
        'name' => 'Fields',
        'singular_name' => 'Field',
        'search_items' => 'Search Fields',
        'all_items' => 'All Fields',
        'parent_item' => 'Parent Field',
        'parent_item_colon' => 'Parent Field:',
        'edit_item' => 'Edit Field',
        'update_item' => 'Update Field',
        'add_new_item' => 'Add New Field',
        'new_item_name' => 'New Field Name',
        'menu_name' => 'Field'
    );

    $args = array(
        'hierarchical' => true,
        'labels' => $label,
        'show_ui' => true,
        'show_admin_column' => true,
        'query_var' => true,
        'rewrite' => array('slug' => 'field')
    );

    register_taxonomy('field', array('portfolio'), $args);

    // taxonomy NOT hierarchical
    register_taxonomy('software', 'portfolio', array(
        'label' => 'Software',
        'rewrite' => array('slug' => 'software'),
        'hierarchical' => false
    ));
}

add_action('init', 'awesome_custom_taxonomies');
```

*to display all the of the taxonomies with a link on them, you can use this function*

```php
function get_terms($postID, $term) {
    $terms_list = wp_get_post_terms($postID, $term);
    $output = '';

    $i = 0;
    foreach($terms_list as $term) { 
        $i++;
        if($i > 1) { $output .= ', '; }
        $output .= '<a href="' . get_term_link($term) . '">' . $term->name . '</a>';
    }

    return $output;
}
```

*and then add this to html to display all of them*

```php
<?php echo get_terms($post->ID, 'field')?> ||
<?php echo get_terms($post->ID, 'software')?>
<?php
    if (current_user_can('manage_options')) {
        echo '|| '; edit_post_link();
    }
?>
```

---

## Add Footer and Header

*add this to footer.php*

```html
<footer>
    <div>Some footer content</div>
</footer>

<?php wp_footer() ?>

</body>
</html>
```

*add this where you want the footer to be displayed*

```php
<?php get_footer() ?>
```

*add this to header.php*

```html
<!DOCTYPE html>
<html lang="en" <?php language_attributes(); ?>>
<head>
    <meta charset="UTF-8" <?php bloginfo('charset'); ?>>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?php bloginfo('name'); ?> <?php wp_title('|'); ?></title>
    <meta name="description" content="<?php bloginfo('description'); ?>">
    <?php wp_head() ?>
</head>
<body <?php body_class(); ?>>
    
<header>
    <div>some header content</div>
</header>
```

*use this code where you want the header to be*

```php
<?php get_header() ?>
```

---

## Add Full Site Editing (FSE)

*add this code to functions.php for unlocking full site editing*

```php 

function mytheme_setup() {
    add_theme_support('block-based-theme');
}

add_action('after_setup_theme', 'mytheme_setup');
```

---


## Adding a code based Template

Create a folder called "templates" and add the names of the custom templates in there. Then you will be able to find your custom template in your FSE page at Appereance -> Editor -> Templates

## Adding a code based Pattern

Create a folder called "patterns" and add the name of your custom pattern. (Can cause "desynced" problems in wordpress). Then you will be able to find your custom pattern in your FSE page at Appereance -> Editor -> Patterns

--- 


## Differences between Post, Page and Template

### Post

*content components*

is where you for example add a person with their name, photo, job title and description. If you want to add a different person, you create a new post. Or you can add a recipe with all the ingredients as well as photos. Posts are for content that regularly updates.

### Page 

*display of content components*

is where you display the posts that you have created. It can be a page about displaying a bunch of recipes or portfolio of art that you have created. You can also create a Privacy Policy page and other pages that are necessary.

### Template

*overall layout of a page*

you create templates as a basic layout for each type of page that you have. In the template you don't really display the content itself, instead you include other content such as the Header and Footer. You always create an Index template, Index will be the fallback for if your other template is broken or doesn't exist. Other templates can for example be Home, Front Page, Portfolio, 404 page and Portfolio.