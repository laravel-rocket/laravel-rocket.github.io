# Code Structure

Laravel Rocket uses common structure and design patterns of Laravel and adding some components ( Service, Repository, Helper, Presenter .. ) to make their responsibility more smaller.

You need to understand responsibilities of each components and follow the rules

## Component details

### Controller

Responsibilities of controllers are the following:

  * Input data validation ( Basic validation should be done by Request class)
  * Call multiple services' business logic methods
  * Build Response Data and return it

You MUST NOT do the following things on controllers

#### Must not generate Model instance directory

All controllers must use repositories to access ( get / create / update / delete ) models. Must not create model instance and/or use model's static method directly. Accessing constant of models are only exception to use Model in controllers.

#### Must not write business logics directory

Use service to write business logics. Repositoroes are not a place to write business logics. You need to use services to write business logics and 

#### Only action handler methods allowed in the Controller.

Never add protected/private methods on the controllers even if the purpose is that to make the code DRY. You need to move these kind of code to service ( business logic ) layer.

### Services

Responsibilities of services are the following:

* Write business logics.

You can create new Service with the command `php artisan rocket:make:service [ServiceName]`. This command generate service, service interface, and unit test.

Call service from other service is allowed, but be careful with circler dependency.

You MUST NOT do the following things on services

#### Must not generate Model directory

All services must use repositories to access ( get / create / update / delete ) models same as controllers. Must not create model instance and/or use model's static method directly. Accessing constant of models are only exception to use Model in services.

#### Should not create service manually

Need to use `rocket:make:service` command to create new service.


### Repositories

Responsibilities of repositories are the following:

  * Interface to the Model and Service/Controller.
  * Provide the way to create/get/update/delete Model
  * One repository must only access to one target model. It means app has same number of repositories as models.

You can generate repositories with `rocket:generate:from-mwb` command along with models, presenters, admin CRUDs and all related tests. 


* Must NOT do:
  * Must not access multiple models from one repository.
  * Must not access Service / Controller methods.
  * Must not access other repository's methods/properties.
* Only Repositories can access to the models directory.
  * All other classes must use repository to create/get/update/delete Models.
* 
  * This command generate repository, repository interface, and unit test.
  * Should not create repository manually, should use this command to create.

### Models
* Responsibilities of models are the following:
  * Represents entities.
* Must NOT do:
  * Convert data to show on the view ( it is presenter's responsibility )
  * Must not access Repository / Service / Controller methods.
* Changed name space from default `App` to `App\Models`
* You can create new Model with the command `php artisan rocket:make:model [ModelName]`.
  * This command generate model, presenter, and unit test.
  * Should not create model manually, should use this command to create.
* And if model has some kind of "type" or "status" ( and variations are defined in the system and not stored in the table), need to have set of constants which include all variations of status/role.
  * `status` of `Reports` model can have the following constants:
    * STATUS_CREATED = 'created'
    * STATUS_SUBMITTED = 'submitted'
    * STATUS_APPROVED = 'approved'
    * STATUS_REJECTED = 'rejected'

### Presenters
* Responsibilities of Presenter are the following:
  * Presentation for Models
  * Provide methods which is used for views.
* You can create new Presenter with the command `php artisan rocket:make:model [ModelName]` accompany with Models
  * This command generate model, presenter, and unit test.
  * Should not create presenter manually, should use this command to create.
* Must NOT do:
  * Must not change any data in the Model.
  * Must not access Repository / Service / Controller methods.
* All methods in the presenters must be stateless. Should not store any state in the presenters.

### Helpers
* Responsibilities of Helper are the following:
  * Provide utility functions which can be called from every other classes ( Include Models/Responsibilities/Services/Controllers/Presenters/Views).
* All helpers have facade to access. So you can access like `\DatetimeHelper::now()`.
* You can create new Helper with the command `php artisan rocket:make:helper [HelperName]`.
  * This command generate helper, helper interface, unit test, and facade test.
  * Should not create helper manually, should use this command to create.


There are several helpers defined in the [laravel-rocket/foundation](https://github.com/laravel-rocket/foundation/tree/master/src/Helpers).

|Name|Purpose|
|:--|:--|
|ArrayHelper||
|CollectionHelper||
|DataHelper|Provide some functions to handle data stored in the system. Such as list of countries , currencies, languages|
|DateTimeHelper|Provide some functions to handle date & time|
|FileHelper||
|LocaleHelper||
|PaginationHelper||
|RedirectHelper||
|StringHelper||
|TypeHelper||
|URLHelper|Provide some functions to handle assets and urls|


## Interface and DI

All repositories/services/helpers must be provided with interface. And to use them ( except for helpers ), you should use DI( Dependency Injection) on constructor of each classes.

Be careful with circler dependency.

## DateTime and Timezone

All datetime handling, you must not directly create `DateTime` or `Carbon` classes, but use `DateTimeHelper`. Because `DateTimeHelper` is designed to handle Timezone properly.

In the app, there are 2 timezone. One is For Database, and another one is for presentation ( shown on the web pages ). If you use `DateTimeHelper`, all datetime are stored with proper timezone( UTC ) to the database, and shown in the user's timezone.

Timezone settings are stored in the app.php config.

