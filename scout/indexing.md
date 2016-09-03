# Indexing

### Configuration

Each Doctrine Entity is synced with a given search "index", which contains all of the searchable records for that model. 
In other words, you can think of each index like a MySQL table. 
By default, each entity will be persisted to an index matching the model's typical "table" name.
Typically, this is the plural form of the model name; however, you are free to customize the model's index by overriding the `searchableAs` method on the model:

```php
class PostRepository extends SearchableRepository
{
    /**
    * @return string
    */
    public function searchableAs()
    {
        return 'posts_index';
    }
}
```

By default, the entire entity will be serialized (based on the available getters) and will be persisted to its search index. 
If you would like to customize the data that is synchronized to the search index, 
you may override the `toSearchableArray` method on the entity:

```php
class Post implements Searchable 
{
    public $id;

    /**
    * @return array
    */
    public function toSearchableArray()
    {
        return [
            'title'   => $this->title,
            'content' => $this->content
        ];
    }
}
```

> Please note that currently it is required to have the primary key (`id`) of the entity marked as public to make it possible to index and search them.

### Batch indexing

If you are installing Scout into an existing project, 
you may already have database records you need to import into your search driver. 
Scout provides an `import` Artisan command that you may use to import all of your existing records into your search indexes:

```
php artisan doctrine:scout:import "App\Post"
```

### Adding entities

Once you have activated the `LaravelDoctrine\Scout\SearchableExtension` extension, 
all you need to do is `persist` and `flush` an entity instance and it will automatically be added to your search index. 

```php
$post = new App\Post;

// ...

$em->persist($post);
$em->flush();
```

### Updating entities

To update a searchable model, you only need to update the entity instance's properties and `flush` the entity to your database. 
Scout will automatically persist the changes to your search index:

```php
$post = $em->find(Post::class, 1);

// Update the post

$em->flush();
```

### Removing entities

To remove a record from your index, simply `remove` and `flush` the entity from the database.

```php
$post = $em->find(Post::class, 1);
$em->remove($post);
$em->flush();
```
