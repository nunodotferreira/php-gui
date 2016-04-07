<p align="center"><img src="https://cloud.githubusercontent.com/assets/2197005/14036936/f3457ba0-f21c-11e5-886a-f754e8109c28.png" alt="php-gui" /></p>

<p align="center">Extensionless PHP Graphic User Interface library.</p>

<p align="center">
[![Build Status](https://travis-ci.org/gabrielrcouto/php-gui.svg?branch=master)](https://travis-ci.org/gabrielrcouto/php-gui)
[![Latest Stable Version](https://poser.pugx.org/gabrielrcouto/php-gui/v/stable)](https://packagist.org/packages/gabrielrcouto/php-gui)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](http://gabrielrcouto.mit-license.org/)
[![Packagist](https://img.shields.io/badge/packagist-install-brightgreen.svg)](https://packagist.org/packages/gabrielrcouto/php-gui)
</p>

<p align="center"><img src="https://cloud.githubusercontent.com/assets/2197005/14338716/85ef00a2-fc4f-11e5-8ae8-7a0d5be74723.gif" alt="made with PHP S2" /></p>

## Table of Contents

+ [Why](#why)
+ [Installation](#installation)
+ [Requirements](#requirements)
+ [Documentation](#documentation)
+ [How it works](#how-it-works)
+ [Contributors Guide](#contributors-guide)
+ [TO-DO](#to-do)
+ [Credits](#credits)

## Why

PHP can be more than a "Web Language", it's a fast language, with a cross platform interpreter and a good CLI. GUI is a natural step for completing this ecosystem.

For many years, GUI projects are being developed for PHP, like [PHP-GTK](http://gtk.php.net/), [PHP-QT](https://sourceforge.net/projects/php-qt/), [wxPHP](http://wxphp.org/) and so many others, but none of them became popular.

This project aims to solve the most common problems of existing "GUI Projects":

- The need of installing an extension
- Cross platform
- No external dependencies
- Easy to install (composer require php-gui) / Easy to use ($button = new Button)

## Installation

Using [composer](https://packagist.org/packages/gabrielrcouto/php-gui):

```bash
$ composer require gabrielrcouto/php-gui
```

## Requirements

The following PHP versions are supported:

+ PHP 5.6
+ PHP 7
+ HHVM

And OS:

+ Linux x64
+ Windows x64
+ Mac OSX (tested on 10.10.x and 10.11.x)

## Documentation

We are developing the first stable version to write a good documentation. You can use the examples (see the examples folder of this repository) while we are finishing the first release.

Here is a very basic tutorial:

```bash
# clone repository
git clone https://github.com/gabrielrcouto/php-gui.git
cd php-gui

# install dependencies
composer install

# run example
php examples/01-basic/example.php

```

After that, the window should be already open.

Basic Application:

```php
require 'vendor/autoload.php';

use Gui\Application;
use Gui\Components\Button;

$application = new Application();

$application->on('start', function() use ($application) {
    $button = (new Button())
        ->setLeft(40)
        ->setTop(100)
        ->setWidth(200)
        ->setValue('Look, I\'m a button!');

    $button->on('click', function() use ($button) {
        $button->setValue('Look, I\'m a clicked button!');
    });
});

$application->run();
```

## How it works

To create a GUI without the need of an extension, PHP executes a binary with proc_open and communicate with it using Stdin/Stdout Pipes, it's a fast and cross plataform solution.

PHP <=> Stdin/Stdout Pipes <=> Lazarus Application <=> GUI

The binary is made using Lazarus (Free Pascal). After a big research, I found a good advantage of Lazarus over other desktop languages (like C#, Java...):

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/LCLArchitecture.png/440px-LCLArchitecture.png" alt="LCL graph, showing that LCL will use an interface for building the GUI according to the operation system" /></p>

It doesn't have any dependencies (ok, on Linux needs GTK), has a good component library, is compiled, open source and has a nice slogan (Write Once, Compile Anywhere).

The communication (IPC) between PHP and Lazarus is made using a protocol based on JSON RPC. You can see the specification [here](PROTOCOL.md).

## Contributors Guide

### Components names

To be an easy to use library, this project will use HTML friendly names for the components, as PHP developers are more familiar with it.

Examples:

- On Lazarus, the property "caption" is for the text of a button. On php-gui, the property name is "value" (like HTML).
- On Lazarus, "Edit" is the component for text input, on php-gui, it's "InputText".

### Compiling Lazarus App

First, you need to [install Lazarus](http://www.lazarus-ide.org/index.php?page=downloads). For compiling the lazarus binary:

```bash
lazbuild phpgui.lpr
```

For generating the Linux binary, you can use Docker:

```bash
lazarus/linux-docker.sh
cd lazarus/
lazbuild phpgui.lpr
```

### Test

First install the dependencies, and after you can run:

```bash
bin/phing
```

## TO-DO

The "Issues" page from this repository is being used for TO-DO management, just search for the "to-do" tag.

## Credits

[@gabrielrcouto](http://www.twitter.com/gabrielrcouto)

[@reisraff](http://www.twitter.com/reisraff)