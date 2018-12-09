# Quick Start

## Install 

Laravel Rocket has its own installer and this installer generate initial project for your project instead of Laravel's original installer. So you can just install Laravel Rocket installer with the following command.

```
composer global require "laravel-rocket/installer"
```


## Create new project
And generate your project with the following command.

```
rocket new [awesome project name]
```


## Generate migrations, model and repositories

Laravel Rocket provide generators to prepare migrations, model, repositories, presenters, Admin CRUDs and related test files. To use it, you need to prepare your database schema with [MySQL Workbench](https://www.mysql.com/jp/products/workbench/).

A base template MySQL Workbench file ( .mwb ) is already placed on `/documents/db.mwb`. You should update it instead of creating new one from scratch.

After you update mwb file, run the following command to generate Laravel related files from your database table definition.

```
php artisan rocket:generate:from-mwb --rebuild
```
