# Python release

GitHub Action for python project release in pypi and github

## Inputs

### `pre-release-tag`
    **Required:** Tag for pre-release. e.g. v1.0.1-*alpha*. If release is not a pre-release leave blank.
    Example: `pre-release-tag: alpha`
### `major-release`
    **Required:** Trigger a major release. Any non-empty value here indicates it's a major release. If not a major release leave blank.
    Example: `major-release: true`
### `pypi-username`
    **Required:** Pypi username for deployment
### `pypi-password`
    **Required:** Pypi password for deployment

## Known limitations

1. Despite `pre-release-tag` and `major-release` being required parameters, they're optional in real world. This action is supposed to be manually submitted when the release time comes. Since action inputs are always defined, in nothing's passed they will hold an empty string.

## Example

**Please note** the need to set permissions to write in order to be able to create GitHub releases.

```yml

on:
  workflow_dispatch:
    inputs:
      pre-release-tag:
        required: false
        description: 'Tag for pre-release (optional)'
      major-release:
        required: false
        description: 'Trigger a major release (optional). Leave empty for regular release.'
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
    - name: Check out code
      uses: actions/checkout@v2
      with:
        fetch-depth: 0
        token: ${{ secrets.RELEASE_PAT }}

    - name: 'Publish action'
      uses: firebolt-db/action-python-release@main
      with:
        pre-release-tag: ${{ inputs.pre-release-tag }}
        major-release: ${{ inputs.major-release }}
        pypi-username: ${{ secrets.PYPI_USERNAME }}
        pypi-password: ${{ secrets.PYPI_PASSWORD }}
```