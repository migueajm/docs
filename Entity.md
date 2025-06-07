# Entity

## Son clases que definen la estructura de una tabla.
```php
class User extends AbstractModel
{
  const _schema = 'public';
  const _table = 'user';
  const _primaryKey = 'id';
  const _fillables = [
    ['table' => self::_table, 'value' => self::_primaryKey],
    'username',...
  ];
  public function __construct()
	{
		parent::__construct(
			self::_schema,
			self::_table,
			self::_primaryKey,
			self::_fillables
		);
	}
}
```
