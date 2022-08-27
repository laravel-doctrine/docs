# Testing

- [Entity Factories](#entity-factories)
- [Old Entity Factories](#old-entity-factories)
- [Available Assertions](#available-assertions)

<a name="entity-factories"></a>

Laravel Doctrine provides a variety of helpful tools and assertions to make it easier to test your database driven
applications.

### New Factories (post 8.x)

To help with this Laravel Doctrine provides Entity Factories, which are similar to Laravel's Model Factories. This
allows you to define values for each property of your Entities and quickly generate many of them.

You can check [Laravel documentation](https://laravel.com/docs/8.x/database-testing) on factories. The doctrine
implementation mostly replicates it but some features are missing.

#### Factory Definition

Factories should be placed in the `Database\Factories` namespace. The class name must be your entity name with added
`Factory` in the end. The class must extend the `LaravelDoctrine\ORM\Testing\Factories\Factory` class.

```php
namespace Database\Factories;
 
use LaravelDoctrine\ORM\Testing\Factories\Factory;
use Illuminate\Support\Str;
 
class UserFactory extends Factory
{
    /**
     * Define the entity's default state.
     *
     * @return array
     */
    public function definition()
    {
        return [
            'name' => $this->faker->name(),
            'email' => $this->faker->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
            'remember_token' => Str::random(10),
        ];
    }
}
```

The default namespace and factory name can be changed via `Factory::useNamespace()`
and `Factory::guessModelNamesUsing()` static functions.

#### Factory States

State manipulation methods allow you to define discrete modifications that can be applied to your entity factories in
any
combination. For example, your `Database\Factories\UserFactory` factory might contain a `suspended` state method that
modifies one of its default attribute values.

State transformation methods typically call the state method provided by Laravel's base factory class. The state method
accepts a closure which will receive the array of raw attributes defined for the factory and should return an array of
attributes to modify:

```php
/**
 * Indicate that the user is suspended.
 *
 * @return $this
 */
public function suspended()
{
    return $this->state(function (array $attributes) {
        return [
            'accountStatus' => 'suspended',
        ];
    });
}
```

#### Factory Callbacks

Factory callbacks are registered using the `afterMaking` and `afterCreating` methods and allow you to perform additional
tasks after making or creating a entity. You should register these callbacks by defining a `configure` method on your
factory class. This method will be automatically called by Laravel when the factory is instantiated:

```php
namespace Database\Factories;
 
use App\Entities\User;
use LaravelDoctrine\ORM\Testing\Factories\Factory;
 
class UserFactory extends Factory
{
    /**
     * Configure the entity factory.
     *
     * @return $this
     */
    public function configure()
    {
        return $this->afterMaking(function (User $user) {
            //
        })->afterCreating(function (User $user) {
            //
        });
    }
 
    // ...
}
```

#### Creating Entities Using Factories

Once you have defined your factories, you may use the static `factory` method provided to your entities by the
`LaravelDoctrine\ORM\Testing\Factories\HasEntityFactory` trait in order to instantiate a factory instance for that
entity.

```php
namespace App\Entities;

use Doctrine\ORM\Mapping as ORM;
use LaravelDoctrine\ORM\Testing\Factories\HasEntityFactory;

/**
 * @ORM\Table(name="user")
 * @ORM\Entity
 */
class User 
{
    use HasEntityFactory;
    
    // ...
}
```

Let's take a look at a few examples of creating entity. First, we'll use the `make` method to create entities without
persisting them to the database:

```php
use App\Entities\User;
 
public function test_entities_can_be_instantiated()
{
    $user = User::factory()->make();
 
    // Use entity in tests...
}
```

Or you can create factory directly

```php
$users = UserFactory::new()->make();
```

You may create a collection of many entities using the count method:

```php
$users = User::factory()->count(3)->make();
```

#### Applying States

You may also apply any of your states to the entities. If you would like to apply multiple state transformations to the
entities, you may simply call the state transformation methods directly:

```php
$users = User::factory()->count(5)->suspended()->make();
```

#### Overriding Attributes

If you would like to override some of the default values of your entities, you may pass an array of values to the `make`
method. Only the specified attributes will be replaced while the rest of the attributes remain set to their default
values as specified by the factory:

```php
$user = User::factory()->make([
    'name' => 'Abigail Otwell',
]);
```

Alternatively, the `state` method may be called directly on the factory instance to perform an inline state
transformation:

```php
$user = User::factory()->state([
    'name' => 'Abigail Otwell',
])->make();
```

#### Persisting Entities

The `create` method instantiates entity instances and persists them to the database using Doctrine's
EntityManager `persist` and `flush`:

```php
use App\Entities\User;
use Database\Factories\UserFactory;
 
public function test_entities_can_be_persisted()
{
    // Create a single App\Entities\User instance...
    $user = User::factory()->create();
    $anotherUser = UserFactory::new()->create();
 
    // Create three App\Entities\User instances...
    $users = User::factory()->count(3)->create();
    $anotherUsers = UserFactory::times(3)->create();
 
    // Use entities in tests...
}
```

You may override the factory's default entity attributes by passing an array of attributes to the create method:

```php
$user = User::factory()->create([
    'name' => 'Abigail',
]);
```

#### Sequences

Sometimes you may wish to alternate the value of a given entity attribute for each created entity. You may accomplish
this by defining a state transformation as a sequence. For example, you may wish to alternate the value of an `admin`
column between `Y` and `N` for each created user:

```php
use App\Entities\User;
use Illuminate\Database\Eloquent\Factories\Sequence;
 
$users = User::factory()
                ->count(10)
                ->state(new Sequence(
                    ['admin' => 'Y'],
                    ['admin' => 'N'],
                ))
                ->create();
```

In this example, five users will be created with an `admin` value of `Y` and five users will be created with an `admin`
value of `N`.

If necessary, you may include a closure as a sequence value. The closure will be invoked each time the sequence needs a
new value:

```php
$users = User::factory()
                ->count(10)
                ->state(new Sequence(
                    fn ($sequence) => ['admin' => rand(0,1) ? 'Y' : 'N'],
                ))
                ->create();
```

Within a sequence closure, you may access the `$index` or `$count` properties on the sequence instance that is injected
into the closure. The `$index` property contains the number of iterations through the sequence that have occurred thus
far, while the `$count` property contains the total number of times the sequence will be invoked:

```php
$users = User::factory()
                ->count(10)
                ->sequence(fn ($sequence) => ['name' => 'Name '.$sequence->index])
                ->create();
```

#### Factory Relationships

We don't support Laravel's `has` and `for`. But you may create required methods in the factory as states:

```php
namespace Database\Factories;
 
use App\Entities\User;
use App\Entities\Avatar;
use LaravelDoctrine\ORM\Testing\Factories\Factory;
 
class UserFactory extends Factory
{
    /**
     * Adds comments to the user
     * 
     * @param array $comments
     */
    public function hasComment(...$comments): self
    {
        return $this->afterCreating(function (User $user) use ($comments){
            foreach ($comments as $comment){
                CommentFactory::new()->create(array_merge($comment, ['user' => $user]));
            }
        });
    }

    /**
     * Adds comments to the user
     * 
     * @param array|\App\Entities\Avatar $avatar
     */
    public function forAvatar($avatar = []): self
    {
        return $this->state([
            'avatar' => ($avatar instanceof Avatar)
                ? $avatar
                : AvatarFactory::new()->create($avatar),
        ]);
    }

    // ...
}
```

Comments and avatar may be added to the user as follows:

```php
$user = User::factory()
    ->hasComments(
        ['text' => 'first comment'],
        ['text' => 'second comment']
    )
    ->forAvatar(['image' => 'avatar.jpg'])
    ->create();
```

<a name="old-entity-factories"></a>

### Old Factories (pre 8.x)

These factories are kept for backward compatibility. They replicate Laravel's Model Factories pre 8.x.

Place your Factory files in `database/factories`. Each file in this directory will be run with the variable `$factory`
being an instance of `LaravelDoctrine\ORM\Testing\Factory`.

#### Entity Definitions

To define an Entity simple pass the factory its classname and a callback which details what its properties should be set
to

```php
$factory->define(App\Entities\User::class, function(Faker\Generator $faker) {
    return [
        'name' => $faker->name,
        'emailAddress' => $faker->email
    ];
});
```

Faker allows you to get Entities with different values each time you generate one. Note that as usual with Doctrine,
we reference class property names, and not database columns!

The factory allows you to define multiple types of the same Entity using `defineAs`

```php
$factory->defineAs(App\Entities\User::class, 'admin', function(Faker\Generator $faker) {
    return [
        'name' => $faker->name,
        'emailAddress' => $faker->email,
        'isAdmin' => true
    ];
});
```

#### Using Entity Factories in Seeds and Tests

After you have defined your Entities you can then use the factory to generate them or insert them directly into the
database.

A helper named `entity()` is defined to aid you in this or you can use the factory directly.

To make an instance of an Entity (but not persist it to the database), call

```php
entity(App\Entities\User::class)->make();

// OR

$factory->of(App\Entities\User::class)->make();
```

If you need an Entity of a specific type (see the 'admin' example above)

```php
entity(App\Entities\User::class, 'admin')->make();

// OR

$factory->of(App\Entities\User::class, 'admin')->make();
```

If you need multiple Entities

```php
entity(App\Entities\User::class, 2)->make();
entity(App\Entities\User::class, 'admin', 2)->make();

// OR

$factory->of(App\Entities\User::class)->times(2)->make();
```

These methods will return an instance of `Illuminate\Support\Collection` containing your Entities.

If you want to instead persist the Entity before being given an instance of it, replace calls to `->make()`
with `->create()`,
e.g:

```php
entity(App\Entities\User::class)->create(); // The User is now in the database
```

#### Passing Extra Attributes to Factories

Factory definition callbacks may receive an optional second argument of attributes.

```php
$factory->define(App\Entities\User::class, function(Faker\Generator $faker, array $attributes) {
    return [
        'name' => isset($attributes['name']) ? $attributes['name'] : $faker->name,
        'emailAddress' => $faker->email
    ];
});

$user = entity(App\Entities\User::class)->make(['name' => 'Taylor']);

// $user->getName() = 'Taylor'
```

<a name="available-assertions"></a>

### Available Assertions

Laravel Doctrine provides several database assertions for your PHPUnit feature tests. We'll discuss each of these
assertions below. You should add trait `LaravelDoctrine\ORM\Testing\Concerns\InteractsWithEntities` to use them.

#### entityExists / entityDoesNotExist

Assert that database has (or hasn't) given entity by id:

```php
use App\Entities\User;
 
$this->entityExists(User::class, 42);
$this->entityDoesNotExist(User::class, 43);

```

#### entitiesMatch

Assert that a table in the database contains records matching the given key / value query constraints. The third
optional argument constraints the number of such records:

```php
use App\Entities\User;

$this->noEntitiesMatch(User::class, ['email' => 'sally2@example.com']);

// must be exactly 2 admins in the database
$this->entitiesMatch(User::class, ['admin' => 'Y'], 2);
```

#### noEntitiesMatch

Assert that a table in the database does not contain records matching the given key / value query constraints:

```php
use App\Entities\User;

$this->noEntitiesMatch(User::class, ['email' => 'sally2@example.com']);
```