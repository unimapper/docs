Adapter is the middleman (or middlelady) between the app and the database, REST API or whatever else. The idea is that each of your data storage should have created such an adapter and every new instance keeps a unique connection to it.

> Every adapter should have its own requirements obviously passed in constructor, but all adapters must be registered under a unique name, which  should corespond with `@adapter` definition in [entity](entity#adapter).

# Writing adapters

## Adapter
You must implement all methods listed in the interface and the contents of these methods should concern itself only retrieving data from a data source, nothing more.

```php
CustomAdapter extends \UniMapper\Adapter
{
    public function createDelete($resource)
    {}

    public function createSelectOne($resource, $primaryName, $primaryValue)
    {}

    public function createSelect($resource, array $selection = [], array $orderBy = [], $limit = 0, $offset = 0)
    {}

    public function createCount($resource)
    {}

    public function createInsert($resource, array $values, $primaryName = null)
    {}

    public function createUpdate($resource, array $values)
    {}

    public function createUpdateOne($resource, $primaryName, $primaryValue, array $values)
    {}

    public function createModifyManyToMany(\UniMapper\Association\ManyToMany $association, $primaryValue, array $keys, $action = self::ASSOC_ADD)
    {
        // Process association changes
    }

    public function execute(\UniMapper\Adapter\IQuery $query)
    {
        // Run query and return the result from adapter
    }
}
```

> Don't forget to escape values!

## Query

Query is an object that is created by every *create\*()* method from adapter and is processed by *execute()* method.

```php
class CustomAdapterQuery implements \UniMapper\Adapter\IQuery
{
    public function setConditions(array $conditions);
    {
        // store unmapped conditions from UniMapper\Query\Conditionable
    }

    public function setAssociations(array $associations);
    {
        // store array of UniMapper\Association objects
    }

    public function getRaw()
    {
        return ... // SQL query as string for example
    }
}
```

## Mapping
There may be situation when received data from queries or data source, still need a little intervention during mapping so each adapter can implement its own mapping logic. All you need is to override a constructor from \UniMapper\Adapter ascendant and pass your mapping object to it.

```php
class MyAdapter extends \UniMapper\Adapter
{
    public function __construct()
    {
        parent::__construct(new Mapping);
    }
}

class Mapping extends \UniMapper\Adapter\Mapping
{
    public function unmapValue(UniMapper\Reflection\Property $property, $value)
    {}

    public function mapValue(UniMapper\Reflection\Property $property, $value)
    {}
}
```