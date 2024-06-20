---
layout: default
title: Digital Ocean - Registry
parent: Cloud Infrastructure
---

# Digital Ocean - Registry cleanup

##

The DO Registry requires you to manually delete untagged manifests and start the garbage collection.

The easiest way to do this is use the doctl utility and cron on a server. But a more reliable way is
to create a serverless function that does this using the API. Below is an example script. You should
probably add some monitoring to it.

```python
import requests
import os
import time

ACCESS_TOKEN = os.getenv('DIGITALOCEAN_ACCESS_TOKEN')
API_URL = "https://api.digitalocean.com/v2"

headers = {
    "Authorization": f"Bearer {ACCESS_TOKEN}",
}

def get_registry_name():
    """Get the name of the container registry."""
    url = f"{API_URL}/registry"
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        return response.json().get('registry', {}).get('name')
    else:
        print(f"Failed to retrieve registry name, status code: {response.status_code}")
        return None

def list_repositories(registry_name):
    """List all repositories in the specified registry."""
    url = f"{API_URL}/registry/{registry_name}/repositories"
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        repositories = response.json().get('repositories', [])
        return [repo['name'] for repo in repositories]
    else:
        print(f"Failed to list repositories, status code: {response.status_code}")
        return []

def list_all_manifests(registry_name, repo_name):
    """List all manifests for a given repository."""
    url = f"{API_URL}/registry/{registry_name}/repositories/{repo_name}/digests"
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        manifests = response.json().get('manifests', [])
        return [(manifest['digest'], manifest.get('tags', [])) for manifest in manifests]
    else:
        print(f"Failed to list manifests, status code: {response.status_code}")
        return []

def get_latest_digest(manifests):
    """Get the latest digest from the list of manifests."""
    latest_manifest = max(manifests, key=lambda x: x[1] if x[1] else [''])
    return latest_manifest[0] if latest_manifest else None

def delete_manifest(registry_name, repo_name, digest):
    """Delete a specific manifest from a repository."""
    time.sleep(1)
    url = f"{API_URL}/registry/{registry_name}/repositories/{repo_name}/digests/{digest}"
    response = requests.delete(url, headers=headers)
    if response.status_code == 204:
        print(f"Deleted manifest {digest} from repository {repo_name}")
    else:
        print(f"Failed to delete manifest {digest} from repository {repo_name}, status code: {response.status_code}")

def start_garbage_collection(registry_name):
    """Start garbage collection for the registry."""
    url = f"{API_URL}/registry/{registry_name}/garbage-collection"
    response = requests.post(url, headers=headers)
    if response.status_code in [201, 202]:
        print("Garbage collection started successfully.")
    else:
        print(f"Failed to start garbage collection, status code: {response.status_code}")

def main():
    registry_name = get_registry_name()
    if registry_name:
        print(f"Registry Name: {registry_name}")
        repositories = list_repositories(registry_name)
        for repo_name in repositories:
            all_manifests = list_all_manifests(registry_name, repo_name)
            if all_manifests:
                latest_digest = get_latest_digest(all_manifests)
                for digest, tags in all_manifests:
                    if digest != latest_digest:
                        delete_manifest(registry_name, repo_name, digest)
            else:
                print(f"No manifests found for repository {repo_name}")
        start_garbage_collection(registry_name)
    else:
        print("Could not retrieve the registry name or no repositories found.")
```
