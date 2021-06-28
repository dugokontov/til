## Submitting file asynchronously

### Why?

When creating SPA we don't want full page reload to happen.

### How?

Sending files from fronted without page reload can be done using [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects) object and [`input[type=file]`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file) HTML element.

```js
const formData = new FormData();

// fileInputElement is HTML element input[type=file]
formData.append("uploadFile", fileInputElement.files[0]);
// append as many files as you like to the same same key
formData.append("uploadFile", fileInputElement.files[1]);

const response = await fetch(url, {
  method: "post",
  body: formData,
});
```

`Content-Type` will be automatically set to `multipart/form-data`. As per [mdn documentation](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects#sending_files_using_a_formdata_object), this header should not be set manually.

### References

 * https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects#sending_files_using_a_formdata_object
 * https://stackoverflow.com/questions/35192841/how-do-i-post-with-multipart-form-data-using-fetch
