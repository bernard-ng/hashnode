## DDD With Symfony : How to configure Doctrine XML Mapping

This article was originally published in French [here](https://devscast.tech/posts/ddd-avec-symfony-comment-configurer-mapping-xml-doctrine-26).

[Doctrine](https://www.doctrine-project.org/) is an ORM for [PHP](https://php.net) that provides transparent persistence for PHP objects. It uses the [Data Mapper](https://martinfowler.com/eaaCatalog/dataMapper.html) pattern, which aims to completely separate your domain/business logic from persistence in a relational database management system.

In a classic Symfony application the configuration of Doctrine Entities is done through annotations (or attributes with PHP +8.0).

in this article we will see

- Why we should not use annotations (or attributres) when doing DDD
- How to configure Doctrine Mapping with XML

## Why we should not use annotations (or attributres) when doing DDD

Let's start with a simple example, in a classical symfony application here is how an entity would look like 

```php
<?php

declare(strict_types=1);

namespace App\Entity;

use App\Repository\UserRepository;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Table(name: '`user`')]
#[ORM\Entity(repositoryClass: UserRepository::class)]
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private ?int $id = null;

    #[ORM\Column(type: 'string', length: 180, unique: true)]
    private ?string $email = null;

    #[ORM\Column(type: 'json')]
    private array $roles = ['ROLE_USER'];

    #[ORM\Column(type: 'string')]
    private ?string $password = null;

    // ...
}
```

of course let's not forget the accessors and mutators for each property since they are private, for the sake of concision I won't put it in my examples.

One thing we can notice very quickly by looking at our user class which obviously represents a user in our domain is somehow coupled to doctrine, this is problematic because the domain code should not depend in any way on another outside the domain, outside the domain layer, this is one of the principles of the hexagonal layer architecture also known as onion architecture.

In other words our entity user should not depend on any of our code outside the domain yet doctrine is a library i.e. a vendor, which should be in the infrastructure layer.

The designers of the doctrine have thought about this very particular use case and have developed other ways of mapping outside the annotations (or attributes), we can use [PHP](https://www.doctrine-project.org/projects/doctrine-orm/en/2.11/reference/php-mapping.html), [YAML](https://www.doctrine-project.org/projects/doctrine-orm/en/2.11/reference/xml-mapping.html) or XML, unfortunately the support of YAML and PHP will be removed from version 3.0 and it is recommended to use [XML](https://www.doctrine-project.org/projects/doctrine-orm/en/2.11/reference/xml-mapping.html) which we will do right now.

## How to configure Doctrine Mapping with XML
The example I gave before is valid in a classical symfony application but here we are doing DDD and our architecture should look more like this 

```
src/
├── Application/
├── Domain/
│   └── Authentication/
│       ├── Entity/
│       │   ├── User.php
│       │   └── ...
│       └── Repository/
│           └── UserRepository.php (interface)
└── Infrastructure/
    └── Authentication/
        └── Doctrine/
            ├── Mapping/
            │   └── User.orm.xml
            ├── Repository/
            │   └── UserRepository (implementation)
            └── ...
```

Okay, there is a lot of information here, first notice that we have two times the `UserRepository`, the one that is in the domain is a simple interface and its implementation (concrete class) is in the infrastructure this to avoid depending again on doctrine in our domain.

In order for your XML mapping to be taken into account by doctrine before configuring anything, the filename must match this pattern `[EntityName].orm.xml`, the file extension `.orm.xml` is important

With this way of doing our entity user should now look like this

```php
<?php

declare(strict_types=1);

namespace App\Entity;

use App\Repository\UserRepository;

class User
{
    private ?int $id = null;
    private ?string $email = null;
    private array $roles = ['ROLE_USER'];
    private ?string $password = null;

    // ...
}
```

There is no more annotations (or attributes) coming from doctrine in our domain, the next step is to transcribe in XML the mapping previously written with annotations (or attributes) into our Infrastructure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<doctrine-mapping xmlns="https://doctrine-project.org/schemas/orm/doctrine-mapping"
                  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
                  xsi:schemaLocation="https://doctrine-project.org/schemas/orm/doctrine-mapping
                          https://www.doctrine-project.org/schemas/orm/doctrine-mapping.xsd">

    <entity name="Domain\Authentication\Entity\User" repository-class="Infrastructure\Authentication\Doctrine\Repository\UserRepository" table="user">
        <id name="id" type="integer" column="id">
            <generator strategy="IDENTITY"/>
        </id>

        <field name="name" type="string"/>
        <field name="email" type="string" length="180" unique="true"/>
        <field name="roles" type="json"/>
        <field name="password" type="string"/>
</doctrine-mapping>
```

Notice that we specify the [FQCN](https://www.acronymfinder.com/Fully_Qualified-Class-Name-(Java)-(FQCN).html) of our entity and the associated repository (implementation) directly in the mapping, for more information : [the documentation](https://www.doctrine-project.org/projects/doctrine-orm/en/2.11/reference/xml-mapping.html)

we are almost there, there is one last step, we should now "tell" doctrine how to find our mapping and where to find it and this is done through the doctrine configuration located in `/config/packages/doctrine.yaml`

```yaml
    orm:
        mappings:
            Domain\Authentication\Entity:
                type: xml
                dir: '%kernel.project_dir%/src/Infrastructure/Authentication/Doctrine/Mapping'
                prefix: 'Domain\Authentication\Entity'
                alias: Authentication
                is_bundle: false
```

To test that everything works well, just type the following command : ```php bin/console doctrine:mapping:info```.

## Conclusion
To finish, let's make a small recap, when doing DDD we will avoid by all possible means to couple our code with a code outside the domain layer, in the case of a symfony application for entities we use the XML configuration which has the advantage to stay in the infrastructure layer which makes our entity independent and unaware of the ORM used.