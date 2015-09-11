Naming conventions are necessary if you want to have your [entity](entity) or [repository](repository) classes under the certain namespace.
Just define your mask with `*`.

**Example**

```php
use UniMapper\NamingConventions as UNC;

UNC::setMask("YourApp\Model\Entity\*", UNC::ENTITY_MASK); // Default is 'Model\Entity\*'
UNC::setMask("YourApp\Model\Repository\*Repository", UNC::REPOSITORY_MASK); // Default is 'Model\Repository\*Repository'
```

> All conventions must be set before you start using ORM.