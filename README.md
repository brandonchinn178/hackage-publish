# hackage-publish

A GitHub action for publishing packages and documentation to Hackage

## Inputs

* `hackageToken` (required): An auth token from Hackage, which can be generated at `https://hackage.haskell.org/user/$USERNAME/manage`

* `publish` (optional): When false, uploads as a candidate.
    * Defaults to `true`
    * WARNING: Because Hackage uploads are permanent, it's usually not a good idea to do irreversible actions in an automatic pipeline. You likely want to set this to `false`

* `packagesPath` (optional): The path that contains package tarballs (defaults to `dist-newstyle/sdist/`)

* `docsPath` (optional): The path that contains packages' documentation tarballs.
    * If not set, does not upload documentation separately

* `hackageServer` (optional): The URL of the Hackage server to upload to (e.g. for self-hosted Hackage instances)

## Outputs

N/A

## Example

```yaml
- uses: haskell-actions/hackage-publish@v1
  with:
    hackageToken: ${{ secrets.HACKAGE_AUTH_TOKEN }}
```

One particularly useful workflow is to be able to use the hackage token associated with the GitHub user running the workflow. This allows multiple maintainers to use their own tokens and not have to share one user's token.

```yaml
- name: Load Hackage token secret name
  id: hackage_token_secret
  run: |
    USERNAME="$(echo "${GITHUB_ACTOR}" | tr '[:lower:]' '[:upper:]' | tr '-' '_')"
    echo "name=HACKAGE_TOKEN_${USERNAME}" >> "${GITHUB_OUTPUT}"

- uses: haskell-actions/hackage-publish@v1
  with:
    hackageToken: ${{ secrets[steps.hackage_token_secret.outputs.name] }}
```