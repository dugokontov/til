## Coalesce
The `coalesce()` function returns a copy of its first non-NULL argument, or NULL if all arguments are NULL. Coalesce() must have at least 2 arguments.

## Why?
Useful when you update an object. Using it can produce simpler code.

## How?

```sql
UPDATE user set 
 name = COALESCE(?,name), 
 email = COALESCE(?,email), 
 password = COALESCE(?,password) 
WHERE id = ?
```

With this there is no need to write update with if cases, even though only one property has changed.
