# GitLab Releases

GitLab 11.7 introduces [Releases](https://docs.gitlab.com/ce/user/project/releases.html) to create release entries (much
like GitHub), including release assets.

[GitLab releases](https://docs.gitlab.com/ee/workflow/releases.html) work just like GitHub releases:

- Configure `gitlab.release: true`.
- Obtain a [personal access token](https://gitlab.com/profile/personal_access_tokens) (release-it only needs the "api"
  scope).
- Make sure the token is available as an environment variable. Example:

```bash
export GITLAB_TOKEN="f941e0..."
```

GitLab Releases do not support pre-releases or drafts.

## Release notes

By default, the output of `git.changelog` is used for the GitLab release notes. This is the printed `Changelog: ...`
when release-it boots. Override this with the `gitlab.releaseNotes` option. This script will run just before the actual
GitLab release itself. Make sure it outputs to `stdout`. An example:

```
{
  "gitlab": {
    "release": true,
    "releaseNotes": "generate-release-notes.sh ${latestVersion} ${version}"
  }
}
```

## GitLab 11.6 (and lower)

For GitLab 11.6 and lower, a [GitLab Release](https://docs.gitlab.com/ce/workflow/releases.html) means release-it will
automatically fall back to [attach releases notes to a tag](https://docs.gitlab.com/ce/workflow/releases.html). In this
case, assets will not get included.

## Attach binary assets

To upload binary release assets with a GitLab release (such as compiled executables, minified scripts, documentation),
provide one or more glob patterns for the `gitlab.assets` option. After the release, the assets are available to
download from the project's releases page. Example:

```json
{
  "gitlab": {
    "release": true,
    "assets": ["dist/*.dmg"]
  }
}
```

## Origin

The `origin` can be set to a string such as `"http://example.org:3000"` to use a different origin from what would be
derived from the Git url (e.g. to use `http` over the default `https://${repo.host}`).