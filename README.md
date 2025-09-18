# Install Ansible via uv

A GitHub Action that installs Ansible. Provides `ansible` and `ansible-playbook` on PATH in seconds.

- Supports installing either the full `ansible` bundle or `ansible-core`.
- Uses `uv tool install <package>` (pipx-like) to install a standalone CLI.

## Usage

Quick start:

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
        uses: rishabhc32/install-ansible
        with:
          python-version: "3.12"
          ansible-package: "ansible==12.0.0"

      - name: Verify
        run: |
          ansible --version
          ansible-playbook --version
```

## Inputs

- `python-version` (string, default `"3.12"`)
  - Python version to provision alongside uv.
- `ansible-package` (string, default `"ansible"`)
  - Package spec to install, e.g. `ansible`, `ansible==9.3.0`.
- `uv-version` (string, default empty)
  - Version of uv to install. Examples: `latest`, `0.7.13`, `>=0.4.0`. If empty, setup-uv uses its default resolution.
- `skip-setup-uv` (string, default `"false"`)
  - Set to `"true"` to skip running `setup-uv` in this action when you already ran it earlier in the job.

 

## OS Support

This action relies on `astral-sh/setup-uv@v6` and works on GitHub-hosted runners for Ubuntu, macOS, and Windows. The example workflow uses `ubuntu-latest`.

## Examples
```yaml
- name: Install ansible-core as tool
  uses: rishabhc32/install-ansible
  with:
    ansible-package: "ansible==2.16.6"
```

If you prefer to control uv installation yourself, run `astral-sh/setup-uv@v6` first and skip it in this action:

```yaml
- name: Install Ansible via uv tool only
  uses: rishabhc32/install-ansible
  with:
    skip-setup-uv: "true"
    ansible-package: "ansible"
```

## License
MIT
