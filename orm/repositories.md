# Repositories

The Repository Design Pattern, defined by Eric Evans in his Domain Driven Design book, is one of the most useful and most widely applicable design patterns ever invented. It's a place to write collecting logic, build queries, ... Methods can be called `$repository->find(1)` or `$repository->findByName('Patrick')`.

The easiest way to get a repository is to let the EntityManager generate one for the Entity you want:

```php
$repository = EntityManager::getRepository(Article::class);
```

If you want to have more control over these repositories and inject them inside controllers, instead of always calling it on the EntityManager, you can create your own repository class.
When we bind this concrete repository to an interface, it also makes that we can easily swap the data storage behind them. It also makes testing easier, because we can easily swap the concrete implementation for a mock.

Given we have a ArticleRepository:

```php
<?php

interface ArticleRepository
{
    public function find($id);
    public function findByTitle($title);
}
```

Now we can make a concrete Doctrine implementation:

```php
<?php

use Doctrine\ORM\EntityRepository;

class DoctrineArticleRepository extends EntityRepository implements ArticleRepository
{
    // EntityRepository already offers methods like find and findBy{Field}
}
```

Now we have to bind the concrete implementation to the interface. We can use Laravel's container for that.

In one of your ServiceProviders:

```php
<?php

use Doctrine\ORM\Mapping\ClassMetadata;

public function register()
{
    $this->app->bind(ArticleRepository::class, function($app) {
        return new DoctrineArticleRepository(
            $app['em'],
            new ClassMetaData(Article::class)
        );
    });
}
```

Inside your controller, you can now inject your repository interface:

```php

class ExampleController extends Controller
{
    protected $repository;

    public function __construct(ArticleRepository $repository)
    {
        $this->repository = $repository;
    }

    public function index()
    {
        $articles = $this->repository->findAll();
    }

}
```

More about the EntityRepository: http://www.doctrine-project.org/api/orm/2.5/class-Doctrine.ORM.EntityRepository.html

Learning more about the Repository Pattern: http://shawnmc.cool/the-repository-pattern

### Pagination

If you want to add easy pagination, you can add the `Brouwers\LaravelDoctrine\Pagination\Paginatable` trait to your repository. It offers two methods `paginateAll($perPage = 15, $pageName = 'page')` and `paginate($query, $perPage, $pageName = 'page', $fetchJoinCollection = true)`.

`paginateAll` will work out of the box, and will return all (if not softdelete enabled) entities inside Laravel's `LengthAwarePaginator`.

If you want to add pagination to your custom queries, you will have to pass the query object through `paginate()`

```php
public function paginateAllPublishedArticles($perPage = 15, $pageName = 'page')
{
    $builder = $this->createQueryBuilder('o');
    $builder->where('o.status = 1');

    return $this->paginate($builder->getQuery(), $perPage, $pageName);
}
```
