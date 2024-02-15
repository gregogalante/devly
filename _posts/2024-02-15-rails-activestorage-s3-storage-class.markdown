---
layout: post
title:  "How to set S3 Storage Class to Rails ActiveStorage configuration"
date:   2024-02-15 00:00:00 +0100
categories: webdev
---

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

