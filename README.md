sails-hook-mongoat
-------------------


Provides advanced mongo indexing options for sails.js models that use the sails-mongo adapter.

## Usage ##

    npm i sails-hook-mongoat

Then simply add an 'indexes' array property to your sails model(s) that you want to add custom indexers on.  This contains all your indexes.

Index properties:

 - **attributes** - an object with the attributes to index (can also be text indexes)
 - **options** (optional) - index options (see [Mongo Index Options](http://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#options-for-all-index-types))

In the config.model set createIndexes attribute as true
```javascript
  config.model.createIndexes = true
```
To force the indexes creations on each lift: https://github.com/Josebaseba/sails-hook-mongoat/blob/master/index.js#L45

Just go to the models file and set:

```javascript
module.exports.models = {

  createIndexes: true, // Generate the indexes in each lift

  //etc...

  //connection: 'mongodb-development'

  migrate:"safe",

  schema: true

};
```

## Examples ##

**Creating a 'expires after' index**
```javascript
// MY MODEL WITH A DATE FIELD
module.exports = {
  attributes: {
    myDate: {
      type: 'date',
      required: true
    }
  },
  indexes: [
    {
      attributes: {
        myDate: 1
      },
      options: {
        expireAfterSeconds: 60  // expire 60s after myDate
      }
    }
  ]
};
```


**Creating a composite unique index**
```javascript
// MY EVENTS MODEL
module.exports = {
  attributes: {
    event_id: {
      type: 'integer',
      required: true
    },
    match_id: {
      type: 'integer',
      required: true
    }
  },
  indexes: [
    //event & match composite index
    {
      attributes: {
        event_id: -1,    // desc
        match: 1         // asc
      },
      options: {
        unique: true
      }
    }
  ]
};
```


## Contributing ##

Fork the repo, make your changes and submit a pull request.


## Todo ##

 - Options & Attribute validations (?)
