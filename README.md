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