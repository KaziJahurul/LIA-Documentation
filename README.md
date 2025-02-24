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