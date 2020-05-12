---
layout: post
title:  "Cloudify laravel php REST application"
date:   2020-05-04 15:40:14 +0100
categories: helm, php
---
It is a guide from A to Z to create a laravel and deploy to cloud under docker, kubernetes technology.

# Prerequisite 
In order to make the environment portable, I use vagrant to have my dev environment. So vagrant need to be pre-installed, otherwise below stack should be installed or provided:
* php -> 7.2.5
* composer -> 1.10.5
* laravel -> 7

For more detail:
https://laravel.com/docs/7.x/installation  

# Create php app skeleton 
Create a new laravel project:

    laravel new <app-name>

Once all dependencies are installed, run below command to check :

    php artisan serve
    
Then log will output:
    
    PHP 7.4.5 Development Server (http://127.0.0.1:8000) started
    
Since we use vagrant as dev environment, the previous command will run the application in the host of vagrant box, where localhost is not accessiable from outside, in order to get access to the application,
we need to do a port forward in Vagrantfile:

    config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"

After doing that, relaunch the application by binding the host to 0.0.0.0 which will forward your host 8000 flux to vagrant box 8000
    
    php artisan serve --host=0.0.0.0
    
Check it again in the browser, it will run correctly.

# Architecture
# Configuration & preparation database
I want to use postgresql as the relational database which is provisioned in ibmcloud, and then configure the laravel application to connect.
By default, under root directory, if we dive into "database" directory, a UserFactory.php is created which aims to help dev to create a user ans persist to database. Under "migration", 2 php files created aims to create two table in the database by using php syntax which is not dependent of the database technology.
So once we provide correct database configuration, we just need to run these 2 scripts to create tables.
## Creation postgresql in ibm cloud
## Configuration of laravel to connect postgresql 
Under root of the project, a file .env which contains all the necessary configurable environmental variable to connect to external system who also include the part of database connection. For example, by default, it use mysql configuration as default database:
    
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=
   
Then we need to replace them with our postgresl configuration, just be careful that DB_CONNECTION=pgsql, the name of postgresql connection is pgsql:

    DB_CONNECTION=pgsql
    DB_HOST=dd451baf-6258-4307-b21d-b9bf8f0ab834.bn2a0fgd0tu045vmv2i0.databases.appdomain.cloud
    DB_PORT=30600
    DB_DATABASE=ibmclouddb
    DB_USERNAME=ibm_cloud_c10b1c48_aaf1_41e6_b09c_548cfb86a5ea
    DB_PASSWORD=<should-be-password>  
    
In order to check whether all the variables are correct and the connection is establed, we can check with tinker:

    php artisan tinker
Once connected, run below command to check:

    DB::connection()->getPdo();
If success, a message is shown:

    >>>   DB::connection()->getPdo();
    => PDO {#3013
     inTransaction: false,
     attributes: {
       CASE: NATURAL,
       ERRMODE: EXCEPTION,
       PERSISTENT: false,
       DRIVER_NAME: "pgsql",
       SERVER_INFO: "PID: 103287; Client Encoding: UTF8; Is Superuser: off; Session Authorization: ibm_cloud_c10b1c48_aaf1_41e6_b09c_548cfb86a5ea; Date Style: ISO, MDY",
       ORACLE_NULLS: NATURAL,
       CLIENT_VERSION: "10.12 (Ubuntu 10.12-0ubuntu0.18.04.1)",
       SERVER_VERSION: "12.2",
       STATEMENT_CLASS: [
         "PDOStatement",
       ],
       EMULATE_PREPARES: false,
       CONNECTION_STATUS: "Connection OK; waiting to send.",
       DEFAULT_FETCH_MODE: BOTH,
     },
    }
    >>> 

Run migration script to create tables:
    
    php artisan migrate
    
In case of any problem or error, we can run below command to rollback:

    php artisan migrate:rollback

## Adding fake users by seed
For testing purpose, sometimes we need to generate fake users, in order to do that, we should modify the file DatabaseSeeder.php under database/seeds/
For example, we use the factory defined under database/factories/UserFactory.php to create 100 random users:
   
    public function run()
    {
        //$this->call(UserSeeder::class);
        $users = factory(App\User::class, 100)->create();
    }
    
And regenerating the users, in order to simplify the things, we can use below command, which combine 3 following functionalities:
* rollback to zero
* migrate pre-defined creation script
* generate seeds


    php artisan migrate:refresh --seed
    
Here is an example of successful output:

    vagrant@vagrant:/vagrant/complete$ php artisan migrate:refresh --seed
    Rolling back: 2019_08_19_000000_create_failed_jobs_table
    Rolled back:  2019_08_19_000000_create_failed_jobs_table (2.3 seconds)
    Rolling back: 2014_10_12_000000_create_users_table
    Rolled back:  2014_10_12_000000_create_users_table (1.53 seconds)
    Migrating: 2014_10_12_000000_create_users_table
    Migrated:  2014_10_12_000000_create_users_table (2.46 seconds)
    Migrating: 2019_08_19_000000_create_failed_jobs_table
    Migrated:  2019_08_19_000000_create_failed_jobs_table (2.23 seconds)
    Database seeding completed successfully.
# Add two RESTful API
Here we want to add two endpoints:

* /api/users
* /api/users/${id}

## Add Controller
Create a UserController:

    class UserController extends Controller {
        //use AuthorizesRequests, DispatchesJobs, ValidatesRequests;
        public function index() {
            //return "hello";
            return User::all();
        }
    
        public function show($id) {
            //Log::emergency( $id);
            return json_encode(User::find($id));
        }
    } 

Add the function in User.php to query database based on userId:
    
    public static function find($id) {
        //return DB::table('users')->find($id);
        return DB::select('select id, name, email from users where id = :id', ['id' => $id]);
    }
    
Add route in api

    Route::get('/users',      'UserController@index');
    Route::get('/users/{id}', 'UserController@show');
    
Check the below urls to verify the 2 apis:
* http://localhost:8000/api/users
* http://localhost:8000/api/users/1