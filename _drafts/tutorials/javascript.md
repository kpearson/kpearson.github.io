
## Async

A few patterns for working with promises.

```javascript
const fucName = arg => {
    let resolveFn
    const promise = new Promise((resolve, reject) => {
      resolveFn = resolve
    })

    // do some stuf

    return promis

}
```

Imidiatly invoke an anonomus async funtion

```javascript
(async () => {
  // ... do some await-ing

})();
```
