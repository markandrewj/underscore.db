[![Build Status](https://travis-ci.org/typicode/underscore.db.svg)](https://travis-ci.org/typicode/underscore.db)
[![NPM version](https://badge.fury.io/js/underscore.db.svg)](http://badge.fury.io/js/underscore.db)
[![Bower version](https://badge.fury.io/bo/underscore.db.svg)](http://badge.fury.io/bo/underscore.db)

# Underscore.db

> Adds functions to Underscore/Lo-Dash for manipulating database-like objects.

It adds `get`, `insert`, `update`, `updateWhere`, `remove`, `removeWhere`, `save`, `load` and `createId` and can be used in Node and the browser.

Data can be persisted using the filesystem or localStorage.

You can try it online [here](http://typicode.github.io/underscore.db/).

__Note: If you need more, check [LowDB](https://github.com/typicode/lowdb).__

## Usage

```javascript
// let's create an empty database object
var db = {
  posts: []
}

// and create a post
var newPost = _.insert(db.posts, {title: 'foo'});

// db content is now
/*  
{ 
  posts: [
    {title: "foo", id: "5ca959c4-b5ab-4336-aa65-8a197b6dd9cb"}
  ]
}
*/

// now let's retrieve it using its id
var post = _.get(db.posts, newPost.id);

// or query it using an underscore method
var post = _.find(db.posts, {title: 'foo'});

// finally let's save it
_.save(db);
```


## Install

__Node__

```bash
$ npm install underscore underscore.db
```

```javascript
var _ = require('underscore');
require('underscore.db').mixWith(_);
```

__Browser__

```bash
$ bower install underscore underscore.db
```

```html
<script src="underscore.js" type="text/javascript"></script>
<script src="underscore.db.js" type="text/javascript"></script>
```

To use Underscore.db with Lo-Dash, just replace `underscore` with `lodash`

## API

The following database object is used in API examples. 

```javascript
var db = {
  posts: [
    {id: 1, body: 'one', published: false},
    {id: 2, body: 'two', published: true}
  ],
  comments: [
    {id: 1, body: 'foo', postId: 1},
    {id: 2, body: 'bar', postId: 2}
  ]
}
```

### get

__get(collection, id)__

Finds and returns document by id or undefined.

```javascript
var post = _.get(db.posts, 1);
```

### insert

__insert(collection, document)__

Adds document to collection, sets an id and returns created document.

```javascript
var post = _.insert(db.posts, {body: 'New post'});
```

### update

__update(collection, id, attrs)__

Finds document by id, copies properties to it and returns updated document or undefined.

```javascript
var post = _.update(db.posts, 1, {body: 'Updated body'});
```

### updateWhere

__updateWhere(collection, whereAttrs, attrs)__

Finds documents using `_.where`, updates documents and returns updated documents or an empty array.

```javascript
// Publish all unpublished posts
var posts = _.updateWhere(db.posts, {published: false}, {published: true});
```

### remove

__remove(collection, id)__

Removes document from collection and returns it or undefined.

```javascript
var comment = _.remove(db.comments, 1);
```

### removeWhere

__removeWhere(collection, whereAttrs)__

Removes documents from collection using `_.where` and returns removed documents or an empty array.

```javascript
var comments = _.removeWhere(db.comments, {postId: 1});
```

### save

save(db, [destination])

Persists database using localStorage or filesystem. If no destination is specified it will save to `db` or `./db.json`.

```javascript
_.save(db);
_.save(db, '/some/path/db.json');
```

### load

load([source])

Loads database from localStorage or filesystem. If no source is specified it will load from `db` or `./db.json`.

```javascript
var db = _.load();
var db = _.load('/some/path/db.json');
```

### id

Overwrite it if you want to use another id property.

```javascript
_.id = '_id';
```

### createId

__createId(collectionName, doc)__

Called by Underscore.db when a document is inserted. Overwrite it if you want to change id generation algorithm.

```javascript
_.createId = function(collectionName, doc) {
  return collectionName + '-' + doc.name + '-' + _.random(1, 9999);
}
```

## FAQ

### How to query?

Everything you need for querying is present in Underscore and Lo-Dash: `where`, ```find```, ```map```, ```reduce```, ```filter```, ```reject```, ```sortBy```, ```groupBy```, ```countBy```, ...

See http://underscorejs.org/ or http://lodash.com/docs.

Example:

```javascript
// Using Underscore
var topFivePosts = _(db.posts)
  .chain()
  .where({published: true})
  .sortBy(function(post) {
     return post.views;   
   })
  .first(5)
  .value();

// Using Lo-Dash
var topFivePosts = _(db.posts)
  .where({published: true})
  .sortBy('views')
  .first(5)
  .value();
```

### How to reduce file size?

With Lo-Dash, you can create optimal builds and include just what you need. 

Minimal build for Underscore.db to work (~2kb min gzipped):

```bash
$ npm install -g lodash-cli
$ lodash underscore include=find,where,clone,indexOf
```

For more build options, see http://lodash.com/custom-builds.

## License

Underscore.db is released under the MIT License.
