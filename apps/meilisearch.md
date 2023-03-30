---
layout: default
title: Meilisearch
has_children: false
parent: Self Hosted Apps
---

# Meilisearch

## Deployment

The Helm chart works well.

## Configuration:

Here is a list of things you can an API key permission on. I could not easily find a complete list so that's why I'm posting it here.

```
`*`, 
`search`, 
`documents.*`, 
`documents.add`, 
`documents.get`, 
`documents.delete`, 
`indexes.*`, 
`indexes.create`, 
`indexes.get`, 
`indexes.update`, 
`indexes.delete`, 
`indexes.swap`, 
`tasks.*`, 
`tasks.cancel`, 
`tasks.delete`, 
`tasks.get`, 
`settings.*`, 
`settings.get`, 
`settings.update`, 
`stats.*`, 
`stats.get`,
`metrics.*`, 
`metrics.get`, 
`dumps.*`, 
`dumps.create`, 
`version`, 
`keys.create`, 
`keys.get`, 
`keys.update`, 
`keys.delete`"
```

Here is an example of a command that you can use to create an API key with search permissions. Good for public use:

```bash
curl -s -X POST https://search.company.com/keys   -H 'Content-Type: application/json'  -H 'Authorization: Bearer <snip>' --data-binary '{ "description": "search only", "actions": ["search"], "indexes": ["staging"], "expiresAt": "2030-01-01T00:00:00Z" }'  | jq
```

Here is an example of a command that you can use to create an API key with full permissions. Good for use in a backend app that can add data to the index:

```bash
curl -s -X POST https://search.company.com/keys   -H 'Content-Type: application/json'  -H 'Authorization: Bearer <snip> ' --data-binary '{ "description": "crud", "actions": ["search", "indexes.*", "documents.*", "settings.get", "stats.get"], "indexes": ["staging"], "expiresAt": "2030-01-01T00:00:00Z" }'  | jq
```


---

You need to install `jq` for this.
