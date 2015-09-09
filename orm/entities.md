# Entities

Out of the box this package uses the default Laravel connection which is provided in `config/database.php`, which means that you are ready to start fetching and persisting.

```php
<?php

$article = new Article;
$article->setTitle('Laravel Doctrine Quick start');

EntityManager::persist($article);
EntityManager::flush();
```
Unlike Eloquent, Doctrine is not an Active Record pattern, but a Data Mapper pattern. Every Active Record model extends a base class (with all the database logic), which has a lot of overhead and dramatically slows down your application with thousands or millions of records.
Doctrine entities don't extend any class, they are just regular PHP classes with properties and getters and setters. 

The domain/business logic is completely separated from the persistence logic. 
This means we have to tell Doctrine how it should map the columns from the database to our Entity class. In this example we are using annotations. Other possiblities are yaml, xml or php array's.

Entities are objects with identity. Their identity has a conceptual meaning inside your domain. In a CMS application each article has a unique id. You can uniquely identify each article by that id.

The `Article` entity used in the example above looks like this.

```php
<?php

use Doctrine\ORM\Mapping AS ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="articles")
 */
class Article
{
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    protected $id;

    /**
     * @ORM\Column(type="string")
     */
    protected $title;

    public function getId()
    {
        return $this->id;
    }

    public function getTitle()
    {
        return $this->title;
    }

    public function setTitle($title)
    {
        $this->title = $title;
    }
}
```
