# Developer Documentation

## Local development

### @wordpress/env

It is recommended to use the [`wp-env`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) package to load a WordPress development. It requires docker to be installed.
`wp-env` lets you easily set up a local WordPress environment for building and testing plugins and themes. It’s simple to install and requires no configuration.
If you have a node package manager like `npm` installed you can install `wp-env` like:

```sh
npm i -g @wordpress/env
```

### Prepare development environment

This plugin needs both the [ActivityPub](https://github.com/Automattic/wordpress-activitypub) and the [GatherPress](github.com/GatherPress/gatherpress) WordPress plugins alongside with the WordPress core.
In order to improve debugging within all these plugins it is recommended to create the following directory structure:

```shell
.
├── .vscode
├── activitypub
├── gatherpress
├── gatherpress-activitypub
```

Where each directory contains either the plugins release code or repository. If you want to use the repository for GatherPress you must run `composer instal`, `npm install` and `npm run build` within `gatherpress` for the GatherPress plugin to work.

Then put the following `.wp-env.override.json` within the `gatherpress-activitypub` directory.

```
{
	"$schema": "https://schemas.wp.org/trunk/wp-env.json",
	"plugins": [
		"./../gatherpress",
		"./../activitypub",
		"."
	]
}
```

This will override the `.wp-env.json`'s plugins config, to use the local plugins and not download them from WordPress.org within the docker containers.

You may use the following `launch.json` file within the directory `.vscode` to be able to step debug in all three plugins:

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "/var/www/html/wp-content/plugins": "${workspaceFolder}/"
            }
        }
    ]
}
```


### Start the development environment

In your terminal window, run:

```sh
cd gatherpress-activitypub
wp-env start --xdebug
```

You should then see that a development site has been configured for you on localhost port 8888, see the terminal logs.

### Log in to Site / Log into Site

You can log in using the username `admin` and the password `password`.

### Installing Dependencies

1. Install the version of Node in `.nvmrc.`. [NVM](https://github.com/nvm-sh/nvm) can be used to achieve this.
2. Install [Composer](https://getcomposer.org/doc/00-intro.md)
3. In your terminal window, `cd` to the `gatherpress` directory
4. Run `npm install` to get node dependencies
5. Run `npm run build` to compile scripts and styles
6. Run `composer install` to get PHPUnit dependencies

### To shut down your development session

Simply run:

```sh
wp-env stop
```

Other useful commands may be: `wp-env clean`, or `wp-env destroy`.

For more info on `wp-env` package, consult the [Block Handbook's page](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/).

### Running tests locally

To run PHPUnit tests locally you can use the commands available via `package.json`:

```shell
npm run test:unit:php-debug
```
