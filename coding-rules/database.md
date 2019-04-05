# Database Design Rules

## Basic concepts

Goal of database design is to make the following things clear:

  * Purpose of tables & columns
  * Data types of each columns
  * Relation between tables

## Table name

Table name must be:

  * Plural form
  * Snake case ( use only small letters )

And you must not use any kind of abbreviation forms. It's because abbreviation form sometimes has multiple meanings and someone may not understand what it means properly.

And relation table must be [singular form of parent table] _ [plural form of children table]. For example, you want to make many to many relationship between `users` and `organizations`, relation table's name should be `user_organizations`.

And you should not add any kind of postfix such as `_table`. And you should avoid using prefix also ( but sometimes you may need to use prefix )

You can refer some example:

|Good|Bad|
|:---|:---|
|users|User|
|post_categories|postCategories|
|user_roles|userroles|
|user_addresses|user_addrs|
|user_organizations|user_organization_relation|

## Column name

### Basic rules

We defined rules for column name to include "purpose" and "data type" cleary in the name. For example, The column for storing integer user id must be named `user_id`. 

Column name must be:

  * Plural form
  * Snake case ( use only small letters )
  * Must not use any kind of abbreviation forms

You can refer some example:

|Good|Bad|
|:---|:---|
|user_id|users_id|
|post_id|postId|
|device_serial_number|device_sn|
|facebook_token|fb_token|

### Primary Key

All table must have a column named `id` as a primary key. It must be unsigned bigint(20) and be auto incremented. Name and type are fixed and must not be changed.

You must not use composite primary key. On some cases, such as relation tables, composite key might be a better way theoretically. But some wireframes assumed that all tables have single primary key, so we fixed to use `id` as a primary key to make the situation simpler.

### created_at, updated_at and deleted_at

All tables must have `created_at` and `updated_at` for storing created datetime and updated datetime. Types for both are `timestamp`.

The tables which uses soft delete must have `deleted_at`. It must also be `timestamp` type.

### Datetime
If you want to store datetime to the database, must use `_at` postfix and use int and store unixtimestamp. Never use datetime type because MySQL datetime type cannot store timezone and it may cause of bugs and confusion. And use timestamp only for `created_at`, `updated_at` and `deteled_at` because it is used in the framework.

### Gender

Gender ( male, female...) must use varchar as datatype and store string like type ( `male` & `female`). Must not use integer value like 1 means male, 2 means female. It is because number is not human readable and also it may increase the kind of genders ( Now facebook has 50 genders). And Model must define constants of all options

### Status

Many table can have a column which represents some kind of the "status" of the record.

  * Blog post can have a status of the post ( "published" / "draft")
  * Approval procedure can have a status of approval ( "submitted", "approved", "rejected")

This kind of status must be stored as "text" such as "published" / "submitted". Must not use number ( 1 .. published. 0 .. draft). Because it is quite difficult to understand what it indicated in the future. And number is a bit difficult to extend sometimes. if there are 3 status and want to add 2 more status between statys 1 and 2. on that case, work flow becomes 1->4->5->2->3 and looks difficult to understand. So text might be more flexible from the human psychological point of vuiews.

And Model must have constants of all options. Laravel Rocket will generate all constants if you define them on `app.json`.

### Types

Sometimes table must store "type" of the entity.

  * uploaded file can have a type that "text", "image", "video"...

Type also be a text from the same reason of Status. And Model must have constants of all options. Laravel Rocket will generate all constants if you define them on `app.json`.

### Code

Sometimes we need to store code. Such as country code, currency code, and so on. on that case, you should use `_code` prefix such as `country_code` or `currency_code`.

### Prefix & Postfix

Some data types must follow this rules of prefix/postfix. It is used fof

|column type|prefix|postfix|data type|
|:--|:--|:--|:--|
|boolean flags|`is_` or `has_`|-|tinyint|
|datetime|-|`_at`|unsigned int(10)|
|date|-|`_date`|date|
|reference to other table|-|`_id`|unsigned bigint(20)|
|status|-|_status|varchar|
|type|-|_type|varchar|
