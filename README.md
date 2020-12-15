# linker-router
PHP advanced routing
## Linker Framework Project
This project is made for [Linker Framework Project](https://github.com/eru123/linker) and can be use as independent component that can be use in other projects.
# Documentation
### Install with composer
```bash
composer require eru123/linker-router
```
### File Structure
With Linker Auto Routing, rendering dynamic URI made easier for you.
```
/index.php
/autorouting_dir
    /index.php
    /home.php
    /sample_folder
        /index.php
        /file.php
        /style.css
        /main.js
        /image.png
    /folder_with_param
        /_param1
            /index.php
            /_param2
                /index.php
                /somefile.php
    /user
        /_id
            /index.php
            /timeline.php
            /about.php
            /settings.php
            /posts
    /_param_in_route_dir
        /index.php
        /dir
        /_another_param
            /index.php
```
Here are some notes about the file structure for Linker Auto Routing
 - In every directory inside the auto route directory, only 1 params is allowed. If the dev create 2 or more param folder, only the first param will read.
 - Avoid using multiple param in the same directory.
## Usage
### Configuration
```php
<?php

require_once __DIR__."/vendor/autoload.php";

$config = [
    // 'dir' holds all files and folder that auto router can be accessed.
    // Using '.' and '..' in the URI is ignored to avoid unwanted access outside the routing dir.
    "dir" => "autorouting_dir", // default dir is 'public'
    
    // 'exts' holds the file extension records that will be use for the file search.
    // 'exts' value separated by single spaces and is case-sensitive.
    // If the uri request only provides a filename without an extention 'exts' magic occur.
    // Example URI: https://example.com/home
    // The exts will search for: home.ext1, home.ext2
    // However if a file home without an extension exists, exts will not execute
    "exts"=> "php html htm xml md", // default exts is 'php html' 

    // 'index' holds the index file for every directory
    // Example URI: https://example.com
    // Example index: 'index README'https://example.com
    // Example exts: 'php md'
    // index will search for: index.php, index.md, README.php, README.md
    "index" => "index readme README", // Default index is 'index'

    // 'level' holds the value of starting position for reading URI
    // This is effective if you are not using Linker Auto Route in the document root directory
    // Example URI https://example.com/dir/home
    
    // Example level: 0
    // Results https://example.com/dir/home

    // Example level: 1
    // Results https://example.com/home
    "level" => 0 // Default level is 0
];

$router = new Linker\Router\Auto($config);
```
### Getting the results
This example is based on the file structure above
```php

// Example URI https://example.com/user/3423/about

$router->path() // autorouting_dir/user/_id/about.php

$router->params() // ["id" => 3423]

$router->result() 
// Result: 
// [
//     "uri"=>"/user/3423/about",
//     "path" => "autorouting_dir/user/_id/about.php",
//     "params" => ["id" => 3423]
// ]

```