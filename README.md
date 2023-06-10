## Installation 

Follow the steps mentioned below to setup the project:

* git clone repository-link
 
* composer install 
**Note**: For database settings, please make sure the version of the PHP installed is same as mentioned in composer.json file.
 If still you face any issues, check the webiste https://packagist.org/ for necessary installation of packages compatible with the php version.

* Make an .env file to the root folder and copy the contents of .env.example file to it.

* Create a new database for the project in PHP MyAdmin.
**Note**: The name of the database should match with the name in the .env file.

* php artisan clear:all

* php artisan plan:sync

* php artisan passport:install

* npm install 

* npm run dev

* php artisan serve
