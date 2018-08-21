# Component-based Theming with Twig
This repository provides a detailed guide to setting up a local development environment that utilizes a Composer based workflow with Drupal 8.

Please ensure that you follow the directions outlined below to install and configure the necessary requirements for this training. We will not be able to cover these steps in class nor will we have time to stop class to assist with setting up laptops.

Below is a list of requirements that will ensure you get the most out of the training.

## Requirements
- Administrative rights to install and configure various applications
- Acquia Dev Desktop or Lando (See Alternative setup)
- Terminal
- Composer
- Node & NPM
- Grunt
- Git

### Administrative rights
You will need to ensure that you have administrative rights to install, configure or manage file permissions required by the list of tools outlined above.  If you do not have administrative rights, in the case of using a work laptop, then please have your company install the following items for you.


### Acquia Dev Desktop
To eliminate the need for various setups that may involve different AMP (Apache/MySQL/PHP) stacks we recommend using Acquia Dev Desktop to work with PHP, MySQL and Drupal. You can download and install Acquia Dev Desktop for both Windows and MAC by navigating to the [download](https://dev.acquia.com/downloads) page and following the install prompts for your operating system.

### Terminal
The terminal is an interface in which we can execute text based commands.  It can be much faster to complete some tasks using a Terminal than with graphical applications and menus. The remaining requirements will be mostly ran from a Terminal using a series of command line prompts.  Take a moment to ensure that you have a Terminal (MAC) or Command Prompt (Windows) available to use.

We will be using the terminal window to work with tools such as `Composer`, `NPM`, and `Grunt` throughout the training.  It is important to be comfortable using the command line as it should be part of any daily Front End development workflow.

### Composer
Composer (https://getcomposer.org/) is a dependency manager for PHP that allows us to perform a multitude of tasks; everything from creating a Drupal project to declaring libraries and even installing contributed modules. The advantage of using Composer is that it allows us to quickly install and update dependencies by simply running a few commands from a terminal window.

Both `Acquia Dev Desktop` and `Lando` will allow us to run these commands without the need to physically install `Composer` on our computer or laptop.  We will revisit the various `Composer` commands that will be used during the training.

### Node & NPM
[Node](https://nodejs.org/en/) is a cross platform runtime environment for creating server side and networking applications. JavaScript running outside the browser. [NPM](https://www.npmjs.com/) is the package manager for JavaScript used to install, share, and distribute code and is used to manage dependencies in projects.

> We will be using NPM to manage dependencies when working with themes in Drupal 8.

We can install `Node JS` and `NPM` globally by following the directions on the [download](https://nodejs.org/en/download/) page and selecting `8.11.4 LTS` installer for your current operating system.  Installing `Node JS` will automatically install `NPM` as well.

We can validate that both are installed by running the following commands in the terminal window:

```
  node -v
  npm -v
```

> Your terminal should be displaying **v8.11.4** for Node and **5.6.0** for NPM

### Grunt
[Grunt](https://gruntjs.com/) is a JavaScript task runner that allows us to perform repetitive tasks like minification, compilation, unit testing, linting and more. We use `Grunt` to compile Sass, Pattern Lab and watch for file changes during development.

We can use `npm` to globally install `grunt` by using the following command in the terminal window:

```
  sudo npm install -g grunt-cli
```

> Windows users should omit "sudo ", and may need to run the command-line with elevated privileges


## Using the training files and configuring Drupal

### Downloading the training files
Now that we have all the necessary requirements out of the way we can proceed to downloading a copy of the training files located within the Master branch.

Begin by locating the green `Clone or download` drop-down button and choosing **Download ZIP**.  Locate the zipped file named `components-master.zip` and extract it's contents. Make sure to expand the `components-master.zip` folder and rename the `components-master` folder to `components`.  For sake of demonstration, I will be copying this folder to a directory called **Sandbox**.

### Importing the project into Acquia Dev Desktop
Follow the steps outlined below to configure an existing Drupal site with Acquia Dev Desktop:

**Step One:**
Open Acquia Dev Desktop

- If you are presented with the Startup window then select **Start with an existing Drupal site located on my computer**
- Otherwise select the + sign located in the bottom left of the UI
- Select **Import local Drupal site.**

**Step Two:**
Complete the following fields to import your Drupal instance:

- Local codebase folder: Select the Change button and navigate to the `/web` root of the `components` folder located at `/Sandbox/components/web`
- Local site name: **components**
- Use PHP: Select **7.1.12**
- Database: Select Start with MySQL database dump file
- Dump file: Select the Browse button and navigate to the `/db` folder located one level up from the `web` folder and select `components.sql`
- New database name: **components**
- Select the OK button

> You may be prompted to enter your admin credentials to complete the process.

### Using Composer to install Drupal.
Currently we have the skeleton of a Drupal 8 project.  The main reason for using a Composer based workflow is to ensure that our code base or repository contains minimal artifacts or files.  In fact if we take a quick look at the folder structure we will see the following:

- **config/sync** : Configuration files that we can use to manage Drupal instances
- **db** : Database snapshots that we will use throughout the training
- **drush**: A command line tool we will use to clear cache and other tasks with Drupal
- **scripts/composer**: Composer scripts that run to automate various tasks
- **web**: Drupal’s web root where we will find all it’s files including the Theme directory

Also if we look we will see a file called `composer.json` which is often referred to as a package.  The `composer.json` file allows us to manage Drupal core, Modules and dependencies, patches that a module may need and various other settings.  It is the `composer.json` file that allows us to distribute a Drupal project to team members that will ensure every Drupal instance is identical.

To complete the scaffolding of our Drupal 8 project we will need to open Acquia Dev Desktop terminal window.
- Select the `More button` located in the bottom left of the UI.
- Choose `Open Console`

We should now be within the web root of our Drupal 8 site.  The `composer.json` file is always located outside the `web` root so we will need to navigate up a level by entering the following command:

```
cd ..
```

We can now run composer install by entering the following command:

```
composer install
```

If we now review the folder structure we will see the following:

- **vendor** : All vendor files and dependencies needed by Drupal
- **web/core** : Drupal core folder and files


### Using NPM and Grunt to install our theme.
We will be using the [Gesso](https://www.drupal.org/project/gesso) theme which is a Sass-based starter theme the outputs accessible HTML5 markup. It uses a mobile-first responsive approach and leverages [SMACSS](https://smacss.com/) to organize styles as outlined in the [Drupal 8 CSS architecture guidelines](https://www.drupal.org/node/1887918). This encourages a component-based approach to theming through the creation of discrete, reusable UI elements.

Gesso also includes [Pattern Lab](https://patternlab.io/).

At its core, Pattern Lab is a static site generator (powered by either PHP or Node) that stitches together UI components. But there's a whole lot more to it than that, as we will find out once we begin looking at components.

To ensure Gesso and Pattern Lab are properly configured we will need to execute a few commands from the terminal window to ensure that all the necessary files and depenedencies are downloaded and configured.

Make sure to open your terminal window and navigate to the gesso directory by entering the following command from the web directory:

```
cd themes/gesso
```

We can now run npm install by entering the following command:

```
npm install
```
Once NPM has finished downloading its dependencies we will need to only run a single Grunt command to compile Pattern Lab and Sass files used by Gesso to display our theme.  With the terminal window still open, execute this final command:

```
grunt gessoBuild
```







## Alternative Setup
The following setup directions are only for those users that did not setup there Drupal instance using Acquia Dev Desktop.

> This is for Advanced users only!


### Lando

To eliminate the need for various setups that may involve different **AMP** (Apache/MySQL/PHP) stacks and consider yousrself an advanced user then you can choose to use `Lando` a Docker based development environment to work with PHP, MySQL and Drupal.  You can download and install Lando for Windows, MACOS and Linux by navigating to the [download](https://docs.devwithlando.io/installation/installing.html) page and following the install prompts for your operating system.

Once completed we will revisit how to use Lando to import a Drupal 8 website as well as how to start a server, run composer, drush and import the initial database snapshot that will be used throughout the training.


## Using Lando (Alternative setup)
If you chose to use Lando instead of Acquia Dev Desktop then you can follow the steps outlined below to configure our Drupal 8 instance.

**Step One**
Starting Lando
- Open a terminal widow and navigate to the `components` directory.

```
cd

```
  lando start
```


Using `Composer` to install Drupal.

Currently we have the skeleton of a Drupal 8 project.  The main reason for using a Composer based workflow recommended by Drupal is to ensure that our codebase or repository contains minimal artifacts or files.  In fact if we take a quick look at the folder structure we will see the following:

- **config/sync** : Configuration files that we can use to manage Drupal instances
- **db** : Database snapshots that we will use throughout the training
- **drush**: A command line tool we will use to clear cache and other tasks with Drupal
- **scripts/composer**: Composer scripts that run to automate various tasks
- **web**: Drupal’s web root where we will find all it’s files including the Theme directory

Also if we look we will see a file called `composer.json` which is often referred to as a package.  The `composer.json` file allows us to manage Drupal core, Modules and dependencies, patches that a module may need and various other settings.  It is the `composer.json` file that allows us to distribute a Drupal project to team members that will ensure every Drupal instance is identical.

To complete the scaffolding of our Drupal 8 project we will need to open a terminal window and run the following command:

```
  lando composer install
```

**Step Three**
Importing the database

```
  lando db-import database.sql.gz
```

**Step Four**
We can now preview our Drupal 8 website by either selecting the URL within the terminal window or by opening a browser and navigating to http://components.lndo.site/user and logging in with the following credentials:

- username: **admin**
- password: **admin**

## Congratulations
We now have a Drupal 8 project titled Pacific Whale Conservancy that we will be using throughout the remaining training.  This Drupal 8 instance is configured with the latest best practices in mind for site building.  This includes use of the Media module, Paragraphs, various Twig modules and the Component and UI Libraries modules.

This training does not cover site building but we will briefly discuss various decision made when implementing a component-based theme using Twig and Pattern Lab.
