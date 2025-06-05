# AbstractModel
```
- Metodos:
 - find(int)
 - findBy(array)
 - findOneBy(array)
 - findAll()
 - update(int, array)
 - delete(int, array)
 - deleteBy(array)
 - updateBy(array)
 - queryBuilder(array)
```
### La variable "criteria" es un array con la siguiente estructura:
```
$criteria = ['key' => 'value'];
// La propiedad table, operator, clause, subQuery son opcionales
$criteria = [
  'key' => [
    'table' => 'value',
    'value' => '',
    'operator' => 'value',
    'clause' => 'value',
    'subQuery' => 'value'
  ]
]; // Para evitar ambiguedad
$entity->findBy($criteria);
$entity->findOneBy($criteria);
```

### Metodo "queryBuilder" recibe como parametro un array asociativo que puede contener las siguientes propiedades:
```
$entity->queryBuilder(compact('criteria', 'joins', 'groupBy'...));
```
- table: ?string
- tableAlias: ?string
- schema: ?string
- limit: ?int
- fillables: ?array
- onlyFillables: ?array
```
fillables: toma los valores de la entidad y agrega los que definan en el array
onlyFillables: Omite los valores de la entidad y solo toma los definidos en el array
[
  ['table' => 'value', 'value' => 'value'], // Para evitar ambiguedad
  "column"
]
```
- joins: ?array
```
[
  'table' => [
    'key' => 'value', // 'PK'
    'foreign_key' => 'value',
    'schema' => 'value',
    'join_type' => 'value', // opcional por default toma el inner
    'criteria' => [],
    'join' => 'value', // Opcional en caso de que el join no se haga la con la tabla principal, se define la tabla con la que hara el join
    'alias' => 'value'
  ]
]
```
- groupBy: ?array
```
Similar a los fillables
[
  ['table' => 'value', 'value' => 'value'], // Para evitar ambiguedad
  "column"
]
```
- orderBy: ?array
```
key: columna; value: ACS, DESC
['key' => 'value']
```
- criteria: ?array
- onlyQuery: bool
