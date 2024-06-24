---
layout: page
title:  "03 - Rails snippets"
---

## ACTIVESTORAGE S3 WITH CUSTOM STORAGE CLASS

```yml
local:
  service: Disk
  root: <%= Rails.root.join("storage") %>

test:
  service: Disk
  root: <%= Rails.root.join("tmp/storage") %>

amazon_s3:
  service: S3
  bucket: bucket_name
  upload:
    storage_class: INTELLIGENT_TIERING # STANDARD REDUCED_REDUNDANCY INTELLIGENT_TIERING STANDARD_IA ONEZONE_IA GLACIER DEEP_ARCHIVE
```

