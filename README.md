# LdapTools [![Build Status](https://travis-ci.org/ldaptools/ldaptools.svg)](https://travis-ci.org/ldaptools/ldaptools) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/ldaptools/ldaptools/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/ldaptools/ldaptools/?branch=master)
-----------

LdapTools is a feature-rich LDAP library for PHP 5.6+. It was designed to be customizable for use with pretty much any 
directory service, but contains default attribute converters and schemas for Active Directory and OpenLDAP. 
 
 * A fluent and easy to understand syntax for generating LDAP queries.
 * A dynamic and customizable attribute converter system to translate data between LDAP and PHP. 
 * Active Directory specific features to help ease development of applications.
 * Includes a comprehensive set of specs for the code.

### Installation

The recommended way to install LdapTools is using [Composer](http://getcomposer.org/download/):

```bash
composer require ldaptools/ldaptools
```

### Getting Started

The easiest way to get started is by creating a YAML config file. See the [example config](resources/config/example.yml) file for basic usage.

Once you have a configuration file defined, you can get up and running by doing the following:

```php
use LdapTools\Configuration;
use LdapTools\LdapManager;

$config = (new Configuration())->load('/path/to/ldap/config.yml');
$ldap = new LdapManager($config);

$query = $ldap->buildLdapQuery()
    ->select()
    ->fromUsers()
    ->where(['firstName' => 'Foo'])
    ->orWhere(['firstName' => 'Bar']);
    
// Returns an array of all users whose first name is 'Foo' or 'Bar'
$users = $query->getLdapQuery()->execute();

// It also supports the concepts of repositories...
$userRepository = $ldap->getRepository('user');

// Find all users whose last name equals Smith.
$users = $userRepository->findByLastName('Smith');

// Get the first user whose username equals 'jsmith'
$user = $userRepository->findOneByUsername('jsmith');
```

The query syntax is very similar to [Doctrine ORM](http://www.doctrine-project.org).

### Documentation

Browse [the docs folder](/docs/en) for more information about LdapTools.

* [Main Configuration Reference](/docs/en/reference/main-configuration.md)
* [Schema Configuration](/docs/en/reference/schema-configuration.md)
* [Using the LdapManager](/docs/en/tutorials/using-the-ldap-manager.md)
* [Building LDAP Queries](/docs/en/tutorials/building-ldap-queries.md)
* [Default Schema Attributes](/docs/en/reference/default-schema-attributes.md)

### TODO

Things that still need to be implemented:

* Creating and modifying LDAP entries.
* An object hydration process in addition to the array hydrator.
* Automatic generation of the schema based off of information in LDAP.
* A logging mechanism.
* An event system.
* More work needed on the OpenLDAP schema.
