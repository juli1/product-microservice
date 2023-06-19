1. Checkout the code
2. Make it work according to the instructions in `README.md`
3. Go to http://127.0.0.1:5000/product/list?limit=20&offset=10 and check it returns products
4. Now, we want to avoid doing the filtering in memory. To do so, in the file `database.py`, change the function with the following content

```python
def get_products(db_connection, limit, offset):
    productFromDatabase = []
    cursor = db_connection.cursor();
    res = cursor.execute(f"SELECT id, title from products LIMIT {limit} OFFSET {offset}");
    for v in res:
        productFromDatabase.append(Product(v[0], v[1]))

    return productFromDatabase
 ```

The objective is to not only filter the products in memory but ask
the database layer to do it.

This will create a SQL issue (without a fix) and a BEST_PRACTICES/CODE_STYLE issue (with a fix).


```json
{
  "violations": [
    {
      "start": {
        "line": 12,
        "col": 11
      },
      "end": {
        "line": 12,
        "col": 90
      },
      "message": "Do not use f-string in SQL queries, it leads to SQL injections",
      "severity": "ERROR",
      "category": "SAFETY",
      "filename": "database.py",
      "rule": "python-security/sql-injection",
      "fixes": []
    },
    {
      "start": {
        "line": 10,
        "col": 5
      },
      "end": {
        "line": 10,
        "col": 24
      },
      "message": "use snake_case and not camelCase",
      "severity": "UNKNOWN",
      "category": "BEST_PRACTICES",
      "filename": "database.py",
      "rule": "python-code-style/assignment-names",
      "fixes": [
        {
          "description": "product_from_database instead of productFromDatabase",
          "edits": [
            {
              "start": {
                "line": 10,
                "col": 5
              },
              "end": {
                "line": 10,
                "col": 24
              },
              "edit_type": "UPDATE",
              "content": "product_from_database"
            }
          ]
        }
      ]
    }
  ],
  "rule_results_with_error": []
}
```
