# Authentication

## Configuration

### Implementing Authenticatable
 
First you must implement the `LaravelDoctrine\ORM\Contracts\Auth\Authenticatable` contract on the entity you wish to use with authentication.

**Note:** `Authenticatable` extends Laravel's authentication contract, `Illuminate\Contracts\Auth\Authenticatable`, so if you have already done this your entity does not require any changes.

```
class User implements \LaravelDoctrine\ORM\Contracts\Auth\Authenticatable
{

    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    protected $id;
    
    public function getAuthIdentifierName()
    {
        return 'id';
    }

    public function getAuthIdentifier()
    {
        return $this->id;
    }
    
    public function getPassword()
    {
        return $this->password;
    }
}
```

You may also use the provided trait `LaravelDoctrine\ORM\Auth\Authenticatable` in your entity and override where necessary.


```
class User implements \LaravelDoctrine\ORM\Contracts\Auth\Authenticatable
{
    use \LaravelDoctrine\ORM\Auth\Authenticatable
    
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    protected $userId;

    public function getAuthIdentifierName()
    {
        return 'userId';
    }
}
```

### Configuring Laravel

Edit Laravel's Auth configuration (`/config/auth.php`) to set up use with Doctrine.

```
return [

	/*
	|--------------------------------------------------------------------------
	| Default Authentication Driver
	|--------------------------------------------------------------------------
	|
	| This option controls the authentication driver that will be utilized.
	| This driver manages the retrieval and authentication of the users
	| attempting to get access to protected areas of your application.
	|
	|
	*/

	'driver' => 'doctrine',

	/*
	|--------------------------------------------------------------------------
	| Authentication Model
	|--------------------------------------------------------------------------
	|
	| This is the entity that has implemented Authenticatable
	|
	*/

	'model' => 'App\User',


	/*
	|--------------------------------------------------------------------------
	| Password Reset Settings
	|--------------------------------------------------------------------------
	|
	| Here you may set the options for resetting passwords including the view
	| that is your password reset e-mail. You can also set the name of the
	| table that maintains all of the reset tokens for your application.
	|
	| The expire time is the number of minutes that the reset token should be
	| considered valid. This security feature keeps tokens short-lived so
	| they have less time to be guessed. You may change this as needed.
	|
	*/

	'password' => [
		'email' => 'emails.password',
		'table' => 'password_resets',
		'expire' => 60,
	],

];
```

## Using Authentication

Authentication usage is covered by [Laravel's Documentation.](http://laravel.com/docs/5.1/authentication)
