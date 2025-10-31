---
layout: default
title: Linode
has_children: false
parent: Cloud Infrastructure
---

## Create small vps with curl

```bash
curl -H "Content-Type: application/json" \
-H "Authorization: Bearer $TOKEN" \
-X POST -d '{
    "authorized_users": [
        "aike_djc"
    ],
    "backups_enabled": false,
    "booted": true,
    "firewall_id": 306729,
    "image": "linode/debian11",
    "label": "scanx",
    "private_ip": false,
    "region": "nl-ams",
    "root_pass": "..........",
    "tags": [],
    "type": "g6-nanode-1",
    "metadata": {
        "user_data": "I2Nsb3VkLWNvbmZpZwoKaG9zdG5hbWU6IHNlcnZlcnRqZQoKZ3JvdXBzOgogIC0gZG9ja2VyCgp1c2VyczoKICAtIG5hbWU6IHlvdQogICAgZ2Vjb3M6IFlvdXIgTmFtZQogICAgc3VkbzogQUxMPShBTEwpIE5PUEFTU1dEOkFMTAogICAgZ3JvdXBzOiB1c2Vycywgc3VkbywgZG9ja2VyCiAgICBzc2hfaW1wb3J0X2lkOiBudWxsCiAgICBzaGVsbDogL2Jpbi9iYXNoCiAgICBsb2NrX3Bhc3N3ZDogdHJ1ZQogICAgc3NoX2F1dGhvcml6ZWRfa2V5czoKICAgICAgLSBzc2gtZWQyNTUxOSBBQUFBQzNOemFDMQoKcGFja2FnZV91cGRhdGU6IHRydWUKcGFja2FnZV91cGdyYWRlOiB0cnVlCgpwYWNrYWdlczoKICAtIGh0b3AKICAtIHZpbQogIC0gY3VybAogIC0gcnN5bmMKICAtIHZuc3RhdAogIC0gZ2l0CiAgLSB1ZncKICAtIGRvY2tlci5pbwogIC0gZG9ja2VyLWNvbXBvc2UKICAtIHByb21ldGhldXMtbm9kZS1leHBvcnRlcgogIC0gbm1hcAoKYm9vdGNtZDoKICAtIHN3YXBvZmYgLWEKICAtIGNwIC9ldGMvZnN0YWIgL2V0Yy9mc3RhYi5iYWsKICAtIHNlZCAtaSAnL1NXQVAvZCcgL2V0Yy9mc3RhYgogIC0gcm0gLWYgL2V0Yy9tYWNoaW5lLWlkIC92YXIvbGliL2RidXMvbWFjaGluZS1pZCAmJiBkYnVzLXV1aWRnZW4gLS1lbnN1cmU9L2V0Yy9tYWNoaW5lLWlkICYmIGRidXMtdXVpZGdlbiAtLWVuc3VyZQogIC0gZ2l0IGNvbmZpZyAtLWdsb2JhbCB1c2VyLmVtYWlsICJlbWFpbCIKICAtIGdpdCBjb25maWcgLS1nbG9iYWwgdXNlci5uYW1lICJuYW1lIgoK"
    }
}' https://api.linode.com/v4/linode/instances
```
