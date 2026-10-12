# Integrations

> **Note**: this document is a work in progress.

## GitHub Container Registry

[GitHub Container registry](https://github.com/features/packages) is used to store Application
images. By default, GitHub Actions don't have permission to write to the ghcr.io registry.

1. Go to: *Settings (of this repository)* -> *Actions* -> *General* -> *Workflow permissions*
2. Enable: *Read and write permissions*

![GitHub Actions Permissions](img/github-actions-permissions.png)
