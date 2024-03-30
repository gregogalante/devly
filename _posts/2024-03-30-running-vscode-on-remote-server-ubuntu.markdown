---
layout: post
title:  "How to run VSCode on a remote server with Ubuntu"
date:   2024-03-30 00:00:00 +0100
categories: dev
---

## Introduction

In this post, I will show you how to run Visual Studio Code on a remote server with Ubuntu. This is useful when you want to develop on a remote server with more resources than your local machine.

## Prerequisites

- A remote server with Ubuntu installed
- Visual Studio Code installed on your local machine

## Steps

- Install the `code-server` package on your remote server:

```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

- Edit the `~/.config/code-server/config.yaml` file to set bind address to `0.0.0.0`:

```yaml
bind-addr: 0.0.0.0:8080 # instead of 127.0.0.1:8080
# NOTE: Optionally update the password
```

- Start the `code-server` service:

```bash
sudo systemctl enable --now code-server@$USER
```

- Open a browser and navigate to `http://<your-server-ip>:8080` to access Visual Studio Code running on your remote server.

