# Mongo Dump Restore

Dump to archive
```bash
mongodump --gzip --archive=/path/to/archive.tgz --db=DbName --collection=CollectionName
```

Restore from archive
```bash
mongorestore --gzip --archive=/path/to/archive.tgz --db=DbName --collection=CollectionName
```
