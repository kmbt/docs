# Panels

- [Main Panels](#main-panels)
- [Subpanels](#subpanels)
- [WordPress Subpanels](#wp-subpanels)

<a name="main-panels"></a>
## Main Panels

Panels (or menus) for your plugin will be defined in the `app/panels.php` file. Herbert panels refer to an option in left sidebar of WordPress admin area. They consist of a Type, Name, Title, Slug, Order, and a Closure callback. The Slug will be appended to the site admin url, for example: `http://example.com/wp-admin/admin.php?page=myplugin-index`. The `order` parameter relates to the position of the menu item in the dashboard sidebar.


#### Main Panel using Closure

``` php
$panel->add([
	'type'   => 'panel',
	'as'     => 'mainPanel',
	'title'  => 'My Plugin',
	'slug'   => 'myplugin-index',
	'order'  => '35.555',
	'uses'   => function()
	{
		return 'Hello World';
	}
]);
```


#### Main Panel using a Controller

``` php
$panel->add([
	'type'   => 'panel',
	'as'     => 'mainPanel',
	'title'  => 'My Plugin',
	'slug'   => 'myplugin-index',
	'order'  => '35.555',
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@index'
]);
```



#### Supplying an Icon
Panels support four different icon methods, each detailed below:

```
'dashicons-media-audio' //Dashicons Class
'none' //Styleable Div
'/img/icon.png' //Relative Path
'//site.com/icon.png' //HTTP
```

Below is an example using `dashicons`

``` php
$panel->add([
	'type'   => 'panel',
	'as'     => 'mainPanel',
	'title'  => 'My Plugin',
	'slug'   => 'myplugin-index',
	'icon'   => 'dashicons-media-audio',
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@index'
]);
```
Below is example using an icon from your asset folder `resources/assets`

``` php
$panel->add([
	'type'   => 'panel',
	'as'     => 'mainPanel',
	'title'  => 'My Plugin',
	'slug'   => 'myplugin-index',
	'icon'   => Hepler::assetUrl('/img/icon.png'),
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@index'
]);
```

<a name="subpanels"></a>
## Subpanels

If you require more than one panel for your plugin you may decided to add them as a subpanel:

```
My Plugin
├── Configure
├── Update
```

To add a subpanel, you set its Type to `sub-panel` and supply a Parent Name.


``` php
$panel->add([
	'type'   => 'sub-panel',
	'parent' => 'mainPanel',
	'as'     => 'configure',
	'title'  => 'Configure',
	'slug'   => 'myplugin-configure',
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@configure'
]);
```

#### Renaming the Root Subpanel

You will notice in adding the first subpanel that WordPress will automatically insert a subpanel with the same name as the parent:

```
My Plugin
├── My Plugin
├── Configure
```

To rename this simply set a `rename` attribute when creating your main panel.


``` php
$panel->add([
	'type'   => 'panel',
	'as'     => 'mainPanel',
	'title'  => 'My Plugin',
	'rename' => 'General',
	'slug'   => 'myplugin-index',
	'icon'   => 'dashicons-media-audio',
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@index'
]);
```

<a name="wp-subpanels"></a>
## WordPress Subpanels

If you require a subpanel under one of the standard WordPress panels, for example:

```
Dashboard
├── Home
├── Updates
├── Your Subpanel
```

To add a WordPress subpanel, you set its Type to `wp-sub-panel` and supply a Parent Name.

``` php
$panel->add([
	'type'   => 'wp-sub-panel',
	'parent' => 'index.php',
	'as'     => 'dashboardSubpanel',
	'title'  => 'Your Subpanel',
	'slug'   => 'myplugin-dashboard',
	'uses'   => __NAMESPACE__ . '\Controllers\AdminController@dashboard'
]);
```

There are 12 different types of WordPress panels which you can supply as a Parent Name:

```
Dashboard: 'index.php'
Posts: 'edit.php'
Media: 'upload.php'
Links: 'link-manager.php'
Pages: 'edit.php?post_type=page'
Comments:'edit-comments.php'
Custom Post Types: 'edit.php?post_type=your_post_type'
Appearance: 'themes.php'
Plugins: 'plugins.php'
Users: 'users.php'
Tools: 'tools.php'
Settings: 'options-general.php'
```
