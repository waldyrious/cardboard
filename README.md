# cardboard

[![build status](https://secure.travis-ci.org/mapbox/cardboard.png)](http://travis-ci.org/mapbox/cardboard)

Demo platform for [s2](https://github.com/mapbox/node-s2) on DynamoDB.
This provides query, indexing, and storage logic. Look to `node-s2` for
bindings and higher level code for interfaces.

## install

    npm install cardboard
    # or globally
    npm install -g cardboard

## api

```js
var c = Cardboard({
    awsKey: config.awsKey,
    awsSecret: config.awsSecret,
    table: config.DynamoDBTable,
    endpoint: 'http://localhost:4567'
});
```

Initialize a new cardboard database connector given a config object that is
sent to dyno.

```js
c.createTable(tableName, callback);
```

Create a cardboard table with the specified name.

```js
c.insert(primarykey: string, feature: object, layer: string, callback: fn);
```

Insert a single feature, indexing it with a primary key in a given layer.

```js
// query a bbox, callback-return array of geojson
c.bboxQuery(bbox: array, layer: string, callback: fn);
// dump all features as geojson
c.dumpGeoJSON(callback: fn);
// delete a feature
c.del(primarykey: string, layer: string, callback: fn);
c.dump(); // -> stream
c.export(); // -> stream
```

## Approach

This project aims to create a simple, fast geospatial index as a layer on top
of [DynamoDB](https://aws.amazon.com/dynamodb/). This
means that the index will not be built into the database or
contained in a single R-Tree - it will be baked into the indexes by which data is stored.

Support target:

* [All GeoJSON geometry types](http://geojson.org/geojson-spec.html#geometry-objects) should be storable
* BBOX queries should be supported
