title: The fastest way to update all records in a collection with mongodb
tags:
  - mongodb
id: 59
categories:
  - code snippets
date: 2012-06-24 13:35:29
---

```
db.collection.update( { &quot;_id&quot; : { $exists : true } }, objNew, upsert, true);
```