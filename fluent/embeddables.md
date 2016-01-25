# Embeddables

While sometimes scalar types such as strings, integers and booleans are enough to save our entity's state, we may find
ourselves in the need to model objects that hold some part of the state and also contain behavior associated to that 
data. This objects are known as **ValueObjects** and are very important when designing a rich domain model.

Since **Doctrine 2.5**, we can map ValueObjects to our database by declaring them **Embeddables**.

## What is an embeddable

An embeddable is an object that can be mapped to a set of columns in a database table. This object will **not** be 
mapped to a table by itself, but will be used **inside an entity**, so its columns will be mapped to the entity's table 
instead.

For example, consider a `Scientist` with a given email address `"a.einstein@example.com"`. 
If we were to model this, we could do it as follows:

```php
class Scientist
{
    /** @var string */
    private $email;
}
```

If we were to protect ourselves against malformed emails on our `Scientist` class, we'd have to do something as follows:

```php
class Scientist
{
    public function __construct(string $email)
    {
        if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new \InvlidArgumentException("Given email [$email] is not valid.");
        }
        
        $this->email = $email;
    }
}
```

As soon as any other object requires an email address, we'll have to duplicate this piece of code to validate it. Unless,
of course, we **model the email address as an object**:

```php
class Email
{
    /** @var string */
    private $address;
    
    public function __construct(string $address)
    {
        if (filter_var($address, FILTER_VALIDATE_EMAIL)) {
            throw new \InvlidArgumentException("Given email [$address] is not valid.");
        }

        $this->address = $address;
    }
    
    public function getAddress(): string
    {
        return $this->address; 
    }
}
```

## Mapping embeddables

While this helps in reducing duplicated code, we still need a way to map the given email address to the database. To do
so, we'll map the `Email` class as an `Embeddable`:

```php
class EmailMapping extends EmbeddableMapping
{
    public function mapFor()
    {
        return Email::class;
    }
    
    public function map(Fluent $builder)
    {
        $builder->string('address');
    }
}
```

As you can see, embeddable mappings look just as entity mappings, but they extend from a different base class. This will
tell the mapping driver that those classes are not meant to be mapped to a table, but are meant to be used _inside_ 
other classes:

```php
class ScientistMapping extends EntityMapping
{
    public function mapFor()
    {
        return Scientist::class;
    }
    
    public function map(Fluent $builder)
    {
        $builder->bigIncrements('id');
        $builder->string('name');
        
        $builder->embed(Email::class);
    }
}
```

The `Fluent` driver will deduce that you have an `$email` field in your `Scientist` class, and will take care of the 
object-columns conversion for you.

Embeddables are not limited to a single column. They can hold any number of columns, and even hold other embeddables!

Check [the Doctrine docs](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/tutorials/embeddables.html) and [our mapping reference](/docs/{{version}}/fluent/reference#embeddables) for more details on embeddables.