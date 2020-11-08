# QueryBuilderParser

**QueryBuilderParser** is designed mainly to be used inside Laravel projects, however it can be used outside Laravel
projects by using Illuminate/Database.

A simple to use query builder for the [jQuery QueryBuilder plugin](http://querybuilder.js.org/demo.html#plugins).

# Using QueryBuilderParser

## Building a new query from QueryBuilder rules.

```php
    use App\Models\Product;
    use RachediAbdenacer\QueryBuilderParser\QueryBuilderParser;
    use Illuminate\Http\Resources\Json\JsonResource;

    $filterRules = $request->get('rules');
  
    $query = new Product();
    
    $queryParser = new QueryBuilderParser(
        // provide here a list of allowable rows from the query builder.
        // NOTE: if a row is listed here, you will be able to create limits on that row from QueryBuilderParser.
        ['name', 'description', 'price']
    );

    $query = $query->where(function ($query) use ($queryParser, $filterRules) {
        $queryParser->parse($filterRules, $query);
    });
    
    ...
    ...

    $products = $query->get();

    return new JsonResource($products);
```

![jQuery QueryBuilder](/querybuilder.png?raw=true "jQuery QueryBuilder")

This query when posted will create the following SQL query:

```sql
SELECT * FROM products WHERE `name` LIKE '%tim%' AND `description` LIKE '%my description'
```
