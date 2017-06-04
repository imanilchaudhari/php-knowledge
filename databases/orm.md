# What is ORM?

[ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) (Object-relational mapping), also known as O/RM, and O/R mapping is a programming approach for converting data between incompatible type systems.

Many full stack frameworks provide their own database abstraction approaches or ORMs.

Standalone database abstraction layers and ORMs to check out:

* [Aura SQL](https://github.com/auraphp/Aura.Sql) - Extension to the native PDO along with a profiler and connection locator.
* [DataMonkey](https://github.com/devsdmf/datamonkey) - Database ORM for PHP build on top of Doctrine.
* [Doctrine](http://www.doctrine-project.org/) - Home to several PHP libraries primarily focused on database storage and object mapping. The core projects are a [Object Relational Mapper (ORM)](http://www.doctrine-project.org/projects/orm.html) and the [Database Abstraction Layer (DBAL)](http://www.doctrine-project.org/projects/dbal.html) it is built upon.
* [Eloquent](https://github.com/illuminate/database) - Illuminate Database component, used in Laravel framework but also a standalone component.
* [Medoo](http://medoo.in/) - Light PHP database framework to accelerate development.
* [Propel](http://propelorm.org/) - A highly customizable and blazing fast ORM library.
* [ProxyManager](https://github.com/Ocramius/ProxyManager) - Library that aims at providing abstraction for generating various kinds of proxy classes.
* [RedBeanPHP](http://redbeanphp.com/) - Easy to use ORM for PHP.
* [safemysql](https://github.com/colshrapnel/safemysql) - A real safe and convenient way to handle MySQL queries.
* [Spot ORM](http://phpdatamapper.com/) - simple and efficient DataMapper built on Doctrine Database Abstraction Layer.
* [Zend\\Db](http://packages.zendframework.com/docs/latest/manual/en/index.html#zend-db) - Zend Database Component.

## Design patterns

There are mainly two main design patterns used in ORMs - [Active Record](https://en.wikipedia.org/wiki/Active_record_pattern) and [Data Mapper](https://en.wikipedia.org/wiki/Data_mapper_pattern).

## When not to use ORM?

As some articles have pointed out ([1](http://www.yegor256.com/2014/12/01/orm-offensive-anti-pattern.html), [2](http://seldo.com/weblog/2011/08/11/orm_is_an_antipattern), [3](http://en.wikipedia.org/wiki/Object-relational_impedance_mismatch)), ORM is anti-pattern that violates principles of object-oriented programming.

As an alternative to ORM, there is [Pomm](https://github.com/pomm-project/ModelManager). It does not propose an abstraction layer and is dedicated to Postgres making developpers able to code directly in SQL to fetch entities. It matches object/relational impedance by using the projection relational operation in order to hydrate flexible instances. It is particulariry suited to build in-house transactional or reporting applications.

## See also

Here are some other useful and updated resources to read or check as well:

* [ActiveRecord and the Beauty Lost in Translation](http://matthewmachuga.com/blog/2015/activerecord-and-the-beauty-lost-in-translation.html)
* [Lessql](http://lessql.net/) - Lightweight and efficient alternative to ORM for PHP.
