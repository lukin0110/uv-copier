# Troubleshooting

## Existing projects and the eMudhra CA compromise (GHSA-6q48-6c7w-9gw2)

Eight intermediate CA keys of eMudhra (emSign) were compromised in October 2026. uv before 0.13.4, pip before 26.2.2
and certifi before 2026.10.19 still trust them. Projects generated with older template versions pin uv 0.12.6 and
Python 3.14.3, so their Docker images are affected. To fix an existing project:

1. Update the project: `copier update --vcs-ref=HEAD --defaults`, or set `UV_VERSION` and `PYTHON_VERSION` in the
   `Dockerfile` (and the Python version in CI) to at least uv `0.13.5` and Python `3.14.9` yourself.
2. Bump the `uv_build` range in `pyproject.toml` to `uv_build>=0.13.5,<0.14.0` and run `uv lock`.
3. Rebuild every image without cache. Old images and build caches keep the vulnerable certificate stores:
   `docker compose build --no-cache --pull`.

## `Permission denied (publickey)` in the DevContainer on macOS

1. Add all private keys to the ssh-agent: `ssh-add -A` (on macos: `ssh-add --apple-use-keychain  --apple-load-keychain`)
2. Reboot Docker Desktop
3. Reopen VSCode

`SSH Keys` should be shared now with the `DevContainer` 😎

### Other useful commands

1. Restart the ssh-agent: `eval "$(ssh-agent -s)"`
2. View ssh keys: `ssh-add -l`

### Problem description
On macOS, `SSH Keys` are not always automatically shared between the host and the `DevContainer`. They are lazily added to 
the *SSH Agent*. When they're not added to the *SSH Agent*, you'll get the following error when trying to execute 
`git pull`, `git push` or `git clone` commands in the `DevContainer`:
```bash
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Related issue on `vscode-remote-release`: [Automatically add SSH keys to ssh-agent](https://github.com/microsoft/vscode-remote-release/issues/4024)
