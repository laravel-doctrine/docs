# From metadata to migration

```php
$ php artisan doctrine:migrations:diff
```

The command generates a migration by comparing project current database to mapping information. 
Doctrine provides this command to generate migration classes by changing entity mappings instead of manually adding modifications to migration class.

Lets add id and name columns to a User entity.

```php
<?php

/** 
 * @ORM\Entity
 */
class User
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;
    
     /**
     * @ORM\Column(type="string")
     */
     protected $name;
}
```

When we run the command

```
Generated new migration class to "Version20150914223731" from schema differences.
```

Our new migration file will look like:

```php
class Version20150914223731 extends AbstractMigration
{
    /**
     * @param Schema $schema
     */
    public function up(Schema $schema): void
    {
        $this->addSql('CREATE TABLE users (id INT AUTO_INCREMENT NOT NULL, name VARCHAR(255) NOT NULL, PRIMARY KEY(id)) DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci ENGINE = InnoDB');
    }

    /**
     * @param Schema $schema
     */
    public function down(Schema $schema): void
    {
        $this->addSql('DROP TABLE users');
    }
}
```

> Note: If you are using Postgres, there is a known issue with doctrine/dbal which causes the diff tool to always detect a change, and add an extra line to the down function.  https://github.com/doctrine/dbal/issues/1110
