## Branch code based on typescript type

### Why?

Some build in functions return values that are one of the two (or more) types. For example, [readux toolkit's](https://redux-toolkit.js.org) [mutation trigger function created using hook](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#signature-1) returns either error object or data object.

```ts
{ data: T } | { error: BaseQueryError | SerializedError }
```

### How?

interfaces and types are gone in runtime, so the only way to test what type our variable is, is to check if some of the keys exist. But if we try to write code like this:

```ts
if (response.error) {
  // handle error
} else {
  // handle success
}
```

typescript will inform us that `Property 'error' does not exist on type '{ data: ResponseType; } | { error: FetchBaseQueryError | SerializedError; }'`.

To work around this we need to use `in` operator for a check, like this:

```ts
if ("error" in response) {
  // typescript knows that response is of type FetchBaseQueryError | SerializedError
} else {
  // typescript knows that response is of type ResponseType
}
```

Reference

- https://stackoverflow.com/questions/54543838/typescript-how-to-branch-based-on-type
