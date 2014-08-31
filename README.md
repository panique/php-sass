php-sass
========

Automatic SASS-to-CSS compiling (while being in development, you'll for sure not do this in production).
Every time you run your app (hitting index.php for example) php-sass will automatically compile all .scss files in your
scss folder to .css files in your css folder. Supports 3.2 version features of SASS (scss syntax) and mixins. Boom!
You can also use Compass, but you'll add an older version of Compass's files (as the latest Compass is for SASS 3.3,
not 3.2) to your scss folder manually.

## Installation & Usage

In your project's composer.json, add these dependencies. If you are not using Composer, then go into the corner and
be very very very ashamed of yourself. And then, after standing there for 2-3 hours, watch some tutorials on how to
use Composer, it's super-easy and a common minimum-standard in PHP development these days.

Please note that this is a **require-dev**, not a normal **require**. This divides real dependencies from ones you only
need for local development.

```json
"require-dev": {
    "panique/php-sass": "dev-master"
}
```

Install or update your Composer dependencies to add php-sass by doing `composer install` or `composer update`.
Composer automatically installs everything in require-dev by default.

**IMPORTANT:** When you later deploy your application and don't want to install the require-dev stuff, then do
`composer install --no-dev` (or `composer update --no-dev`).

In your application, add this line into your `index.php` (or wherever you want to compile SASS to CSS).
If you use this inside common frameworks, make sure you add this **before** the final application call, so in Laravel
add it before `$app->run();` (some frameworks do stuff like `exit();` or caching, so the SASS compiler would never be
called).

```php
SassCompiler::run("scss/", "css/");
```

The first parameter is the *relative* path to your scss folder (create one) and the second parameter is the *relative*
path to your css folder. Make sure PHP can write into the css folder. For local development, a hardcore
`sudo chmod -R 777 public/css` (when being in /var/www) is totally okay (remember, SASS compiling only happens locally,
for production you'll deploy compiled .css files for sure).

## [optional] Good way to use it only in development, not on production

To prevent using the SASS compiler in production, use an environment switch. Nearly all framework have something like
this (google for "YOURFRAMEWORK environment"). For native usage:
In your Apache vhost config, add a SetEnv variable, depending on what the server is (development, testing, production,
etc):

```
<VirtualHost *:80>
    ...
    SetEnv APPLICATION_ENV production
    ...
<VirtualHost/>
```

and in PHP ask for your environment like this:

```php
if (getenv('APPLICATION_ENV') == 'development') {
    SassCompiler::run("scss/", "css/");
}
```

## Optional features

There's an optional third parameter for `SassCompiler::run()` that expects one of the strings explained on
http://leafo.net/scssphp/docs/#output_formatting. This defines the desired output. `scss_formatter` is the standard
laravel-sass uses, choose `scss_formatter_compressed` if you need a minimized css file. `scss_formatter_nested` is
for nested output, optimized for readability.

## How @import works

The `@import` of sass rules from other files works perfectly. Make sure to import the files like it should be:
If the file is called _colors.scss and is in the basic scss folder:
```
@import 'colors';
```
If the file is called _colors.scss and is in the subfolder `modules` of the basic scss folder:
```
@import 'modules/colors';
```
Read the official docs for more.

## Used scripts

This tool uses the excellent [scssphp SASS compiler](http://leafo.net/scssphp/).
scssphp supports the latest SCSS syntax (3.2.12).

## Other projects

- https://github.com/panique/php-login
- https://github.com/panique/php-mvc
- https://github.com/panique/php-long-polling
- My blog DEV METAL: http://www.dev-metal.com

## License

Licensed under [MIT](http://www.opensource.org/licenses/mit-license.php).
Totally free for private or commercial projects.

## Support / Donate

If you think this script is useful, then think about supporting the project:

1. Rent your next server at [A2 Hosting](http://www.a2hosting.com/4471.html) or
[DigitalOcean](https://www.digitalocean.com/?refcode=40d978532a20).
2. Donate via [PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=P5YLUK4MW3LDG)
   or [GitTip](https://www.gittip.com/Panique/)
   or [Flattr](https://flattr.com/submit/auto?user_id=panique&url=https%3A%2F%2Fgithub.com%2Fpanique%2Flaravel-sass)
3. Contribute to this project.

## Hire me

I'm available for freelance work. Remote worldwide or locally around Central Europe. Mail me if you like, the address
can be found easily.
