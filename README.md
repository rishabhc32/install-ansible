# Install Ansible via uv (Composite Action)

This repository provides a reusable composite GitHub Action that installs Ansible using the [uv](https://github.com/astral-sh/uv) package manager.

- Supports installing either the full `ansible` bundle or `ansible-core`.
- Uses `uv tool install <package>` (pipx-like) to install a standalone CLI into `$HOME/.local/bin`.
- Exposes the `ansible` path and version as outputs.

## Usage

Reference in your workflow:

```yaml
name: Use install-ansible via uv
on: [push]

jobs:
  demo:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Ansible via uv
        id: ansible
        uses: <your-org-or-user>/<repo-name>@v1
        with:
          python-version: "3.12"
          ansible-package: "ansible"        # or "ansible-core==2.16.6"

      - name: Verify
        run: |
          ansible --version
          echo "Ansible path: $(command -v ansible)"
```

To test locally within this repo before publishing, you can point to the local action path:

```yaml
      - name: Install Ansible via uv (local)
        id: ansible
        uses: ./
        with:
          python-version: "3.12"
          ansible-package: "ansible"
```

## Inputs

- `python-version` (string, default `"3.12"`)
  - Python version to provision alongside uv.
- `ansible-package` (string, default `"ansible"`)
  - Package spec to install, e.g. `ansible`, `ansible==9.3.0`, or `ansible-core==2.16.6`.

 

## OS Support

This action relies on `astral-sh/setup-uv@v6` and works on GitHub-hosted runners for Ubuntu, macOS, and Windows. The example workflow uses `ubuntu-latest`.

## Examples

- Install `ansible-core` as a standalone tool:

```yaml
      - name: Install ansible-core as tool
        uses: <your-org-or-user>/<repo-name>@v1
        with:
          ansible-package: "ansible-core==2.16.6"
```

 

## Publish to GitHub Marketplace

1. Create a release tag following semver (e.g. `v1.0.0`).
2. Create a major version tag (e.g. `v1`) that points to the latest compatible release. Consumers can then use `@v1`.
3. Add a comprehensive description, categories, and the provided `branding` in `action.yaml` will display in the marketplace.

Quick commands after pushing to `main`:

```bash
# Create a release tag
git tag v1.0.0
git push origin v1.0.0

# Create/Update major tag
git tag -f v1
git push -f origin v1
```

## License

MIT (or your preferred license)
