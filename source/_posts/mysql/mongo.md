---
title: " Mongo Query and Write Operation Commands"
date: 2016-08-16 16:50
---

### find     Selects documents in a collection.

> Definition

    {
        "find": <string>,
        "filter": <document>,
        "sort": <document>,
        "projection": <document>,
        "hint": <document or string>,
        "skip": <int>,
        "limit": <int>,
        "batchSize": <int>,
        "singleBatch": <bool>,
        "comment": <string>,
        "maxScan": <int>,
        "maxTimeMS": <int>,
        "readConcern": <document>,
        "max": <document>,
        "min": <document>,
        "returnKey": <bool>,
        "showRecordId": <bool>,
        "snapshot": <bool>,
        "tailable": <bool>,
        "oplogReplay": <bool>,
        "noCursorTimeout": <bool>,
        "awaitData": <bool>,
        "allowPartialResults": <bool>
    }

> Examples

##### Specify a Sort and Limit

    db.runCommand(
        {
                 find: "restaurants",
                 filter: { rating: { $gte: 9 }, cuisine: "italian" },
                 projection: { name: 1, rating: 1, address: 1 },
                 sort: { name: 1 },
                 limit: 5
        }
    )

##### Override Default Read Concern

    db.runCommand(
        {
             find: "restaurants",
             filter: { rating: { $lt: 5 } },
             readConcern: { level: "majority" }
        }
    )

### insert   Inserts one or more documents.

### update  Updates one or more documents.

## delete  Deletes one or more documents.

### findAndModify  modifies and returns a single document.

> Definition

    {
        findAndModify: <collection-name>,
        query: <document>,
        sort: <document>,
        remove: <boolean>,
        update: <document>,
        new: <boolean>,
        fields: <document>,
        upsert: <boolean>,
        bypassDocumentValidation: <boolean>,
        writeConcern: <document>
    }

> Output  reutrns a document with the folloings fields:

    value                 document
    lastErrorObject document
    ok                      number

> Behavior

###### Upsert and Unique Index

    db.runCommand(
        {
             findAndModify: "people",
             query: { name: "Andy" },
             sort: { rating: 1 },
             update: { $inc: { score: 1 } },
             upsert: true
        }
    )


> Examples

######  Update and Return

    db.runCommand(
        {
             findAndModify: "people",
             query: { name: "Tom", state: "active", rating: { $gt: 10 } },
             sort: { rating: 1 },
             update: { $inc: { score: 1 } }
        }
    )

###### upsert: true    the update operation to either update a matching document or, if no matching document exists, create a new document.

    db.runCommand(
           {
                 findAndModify: "people",
                 query: { name: "Gus", state: "active", rating: 100 },
                 sort: { rating: 1 },
                 update: { $inc: { score: 1 } },
                 upsert: true
           }
         )

###### Return New Document

    db.runCommand(
        {
             findAndModify: "people",
             query: { name: "Pascal", state: "active", rating: 25 },
             sort: { rating: 1 },
             update: { $inc: { score: 1 } },
             upsert: true,
             new: true
        }
    )

###### Sort and Remove

    db.runCommand(
        {
             findAndModify: "people",
             query: { state: "active" },
             sort: { rating: 1 },
             remove: true
        }
    )

### getMore    Returns batches of documents currently pointed to by the cursor.

### getLastError   Returns the success status of the last operation.

### getPrevError    Returns status document containing all errors since the last resetError command.

### resetError         Resets the last error status.

[官方网站](https://docs.mongodb.com/manual/reference/command/findAndModify/)
