ng-admin-for-laravel
====================

This help you can create a quick AngularJS backend with ng-admin for Laravel
#How to ?
- Require [Laravel API Handler](https://github.com/marcelgwerder/laravel-api-handler)
- Create a [RESTful Controller with Laravel](http://laravel.com/docs/4.2/controllers#restful-resource-controllers)

Ex: `php artisan controller:make UserController`
- Place this function in the class
- Copy `APIController.php` to `app/controllers.php`.
- extends `APIController` class instead of `BaseController` in your controller/

Here is an example of User API
````
<?php

class UserV2Controller extends \APIController {

	/**
	 * Display a listing of the resource.
	 *
	 * @return Response
	 */
    protected $user;
    public function __construct(User $user)
    {
        parent::__construct();
        $this->user         = $user;
    }
	public function index()
	{
        try{
            $statusCode = 200;
            $builder = ApiHandler::parseMultiple($this->user->with('history'), array('name,username'), $this->passParams('users'));
            $users = $builder->getResult();
            $response = [];
            foreach($users as $user){
                $response = [
                    'id'    => $user->id
                    'user'  => $user->username,
                    'name'  => $user->name,
                ];
            }

            return $this->makeResponse($response,$statusCode, $builder);
        }catch (Exception $e){
            $statusCode = 500;
            $message = $e->getMessage();
            return Response::json($message, $statusCode);
        }
	}
}
````
Finally, using [ng-admin](https://github.com/marmelab/ng-admin) to call index and you will see the magic
