# MONGODB notes

### Find and item using regex and only output the _id_ and another column
`db.items.find({ "path" : /.*assets.*/ }, {path: 1});`

### Count how many in the array of objects found.
`db.items.find({ "path" : /.*assets.*/ }, {path: 1}).itcount();`

### Mongo CLI show batches when doing find() queries.
`DBQuery.shellBatchSize = 30000`
