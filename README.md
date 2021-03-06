# Drupal Tangler

This library provides tools for an opinionated composer workflow with Drupal.

When invoked, it creates a Drupal root that can respond to requests routed to
it from a web server. This allows you to develop at the module and/or project
level and treat Drupal itself as a dependency.

# Installation

Use composer.

# Usage

The algorithm is something like this:

1. copy `drupal/drupal` out of vendor and into the given drupal path (default:
  `./www`)
2. link modules and themes installed with composer from vendor into the drupal
   root
3. link directories from the `./modules` directory into `sites/all/modules`
4. link directories from the `./themes` directory into `sites/all/themes`
5. link files that look like module files into a directory in
   `sites/all/modules` according to the basename of the `*.info` file
6. link `cnf/settings.php` into `sites/default`
7. link `vendor` into `sites/default`
8. link `cnf/files` into `sites/default`

You have the choice of using a small commandline application or a script
handler.

## Commandline

```
vendor/bin/drupal_tangle -h
Usage:
 drupal:tangle [project] [drupal]

Arguments:
 project               path to project to tangle
 drupal                path to drupal in which to tangle (default: "www")
```

## Composer Script Configuration

You can automate the use of the tangler in response to composer events like so:

```
{
...
    "scripts": {
        "post-install-cmd": [
          "Drupal\\Tangler\\ScriptHandler::postUpdate",
        ],
        "post-update-cmd": [
          "Drupal\\Tangler\\ScriptHandler::postUpdate"
        ]
    },
...
}
```

Note that you can just trigger the executable with these events, in which case
the values for the different `*-cmd` events above would be like this:

```
[
  "vendor/bin/drupal_tangle"
]
```

## Roadmap

* Allow appropriate configuration, such as name of drupal subdir, and origin of
  `settings.php`
* Support development of a theme or profile
* Support for placing things in the `sites/all/libraries` directory (not
  because this is ever a good idea, but because some projects require it)

## Not Ever Going To Be On The Roadmap

* Support for multi-site
