Repository makes data accessing easy in an OOP friendly way. It takes care about [entities](entity), their persistance and loading. An additional advantage of repositories is that your model layer can be easily mocked in your unit tests.

# Default methods

- **count** ( `array` [$filter](#filters) ) : `integer` - Returns data count.
- **create** ( `UniMapper\Entity` $entity ) : `mixed` - creates data from entity in your storage as a new record. Primary value is ignored but returned.
- **destroy** ( `UniMapper\Entity` $entity ) : `bool` - deletes data related to an entity that you passed in as an argument.
- **destroyBy** ( `array` [$filter](#filters) ) : `int` - delete data defined by given filter and returns number of affected records.
- **find** ( `array` [$filter](#filters), `array` $orderBy, `int` $limit, `int` $offset, `array` $associate ) : `UniMapper\EntityCollection` - finds all data and returns it as the entity [collection](entity#collection).
- **findOne** ( `mixed` $primary, `array` $associate ) : `UniMapper\Entity` - returns a single entity by its unique primary value.
- **findPrimaries**( `array` $primaryValues, `array` $associate ): `UniMapper\EntityCollection` - finds data by set of primary values.
- **save** ( `UniMapper\Entity` $entity ) : `UniMapper\Entity` - persists given entity. If primary value set on entity it calls insert, if not then update.
- **update** ( `UniMapper\Entity` $entity ) - updates data from entity in your storage.
- **updateBy** ( `UniMapper\Entity` $entity, `array` [$filter](#filters) ) : `int` - update data defined by given filter and returns number of affected records.

# Filters
You can optionally pass an array with filter specifying the criteria under which the data are filtered in some repository methods. The syntax is similar to MongoDB criteria, but it's slightly different and much easier.

All criteria are stored in associative array, while the key name specify property we want to filter and value should be an array with [modifiers](#modifiers) as the key value.

```php
["id" => ["=" => 1]] // WHERE id = 1
```

You can also combine multiple modifiers with one property.

```php
[
	"time" => [
		">" => new DateTime,
		"<" => new DateTime('+ 1 day')
	]
]
// WHERE time > '...' AND time < '...'
```

## Modifiers

- `=` equal
- `!` not equal
- `>` greater than
- `<` less than
- `>=` greater than or equal 
- `<=` less than or equal
- `like` string comparison, similar to SQL with `%` pattern usage in value

> `IS NOT/IS NULL` can be defined as `!` or `=` modifier and null value combination.

> `NOT IN/IN` can be defined as `!` or `=` modifier and null value combination.

## Groups

Groups are similar to parentheses in SQL and they are defined by a numeric array.

```php
[
	["time" => [">" => new DateTime]],
	["time" => ["<" => new DateTime('+ 1 day')]
]
// WHERE (time > '...') AND (time < '...')
```

> There is no depth limit, groups can be nested as needed.

## OR

In the most cases you need to use OR modifier with these groups.

```php
[
	"or" => [
		["time" => [">" => new DateTime]],
		["time" => [">" => new DateTime('+ 1 day')]
	]
]
// WHERE (time > '...') OR (time < '...')
```

Your filter can be even more complicated with AND,OR combinations.

```php
[
	["id" => [">" = 1]],
	[
		"or" => [
			["time" => [">" => new DateTime]],
			["time" => [">" => new DateTime('+ 1 day')]
		]
	]
]
// WHERE (id > 1) AND ((time > '...') OR (time < '...'))
```