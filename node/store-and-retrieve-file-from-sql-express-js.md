## Store file in database using express.js

### Why?

Even though it is usually better to store files (and images) on file system and then save URL in database, sometimes there is no option to use file system and files have to be stored in database as a BLOB.

### How?

To store in [`sqlite`](https://sqlite.org/index.html) using [`node-sqlite3`](https://github.com/mapbox/node-sqlite3) (and async/await wrapper [`sqlite`](https://github.com/kriasoft/node-sqlite)) modules, all you have to do is pass file `Buffer` as a param to sqlite's [`run`](https://github.com/kriasoft/node-sqlite#inserting-rows) function.

This is an example of how to store multiple files:

```js
const express = require("express");
const fileUpload = require("express-fileupload");

const sqlite = require("sqlite");
const sqlite3 = require("sqlite3");

const router = express.Router();
router.use(fileUpload());

/**
 * @param {express.Request} req
 * @param {express.Response} res
 */
router.post("/", async (req, res) => {
  if (!req.files || Object.keys(req.files).length === 0) {
    return res.status(400).send("No files were uploaded.");
  }

  let uploadImage = req.files.uploadImage;

  // when only one file is uplaoded express-fileupload treat that as an
  // object. If more than one, then an array.
  // make sure we always work with an array, even if only one file is uploaded
  if (!Array.isArray(uploadImage)) {
    uploadImage = [uploadImage];
  }

  const db = await sqlite.open({
    filename: "./database.db",
    driver: sqlite3.cached.Database,
  });
  const imageIds = [];
  for (const rawImage of uploadImage) {
    const { lastID } = await db.run(
      "INSERT INTO image (image) VALUES(?);",
      rawImage.data // actual type is Buffer
    );
    imageIds.push(lastID);
  }

  res.status(200).json({ imageIds });
});
```

## Retrieve files from database using express.js

### How?

To retrieve files in express.js from a BLOB do following:

- Retrieve BLOB from database
- Set correct headers
- Send blob as a stream

Example:

```js
/**
 * @param {express.Request} req
 * @param {express.Response} res
 */
router.get("/:imageId", async (req, res) => {
  const db = await sqlite.open({
    filename: "./database.db",
    driver: sqlite3.cached.Database,
  });
  const result = await db.get(
    "SELECT image FROM image WHERE id = ?",
    req.params.imageId
  );

  if (!result) {
    return res.status(404).send("File not found");
  }
  res.header("Content-Type", "image/png");
  res.end(result.image);
});
```

## References

 * https://stackoverflow.com/questions/31468221/node-return-image-stored-in-sqlite-blob
 * https://github.com/mapbox/node-sqlite3/blob/master/test/blob.test.js
