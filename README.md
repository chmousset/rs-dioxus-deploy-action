# deploy-action

> You can use this action to auto-deploy your Dioxus web app.


# Dioxus configuration

By default, Dioxus expects dist files to be served at the root of the server. In this example, this would be `https://user.github.io/` instead of `https://user.github.io/repo-name/`.

If you want to deploy your Dioxus app residing at `github.com/user/repo-name/` on Github Page at `https://user.github.io/repo-name/`, you will need to make some specific configuration.

To fix this, you need to edit your `Dioxus.toml` file and add the `base_path` property under the `[web.app]` section:

```toml
[web.app]

# required for GH pages
base_path = "repo-name"
```

If on the contrary you want to serve your Dioxus app from `https://user.github.io/`, your repo needs to be called `user.github.io`. No need to change your `Dioxus.toml` file then.

# Write permission

By default, Github Actions don't have permission to modify the repo it's working on. As a result, it cannot create the `gh-pages` branch this action will push to.
This can be fixed by adding the `contents: write` permission in the workflow configuration file. See in the example below.

# Workflow file

This Provided you configured your Dioxus app and the repo settings correctly, creating `.github/workflows/deploy.yml` workflow will publish to Github Pages when pushing on branch `main`.

```yml
name: github pages

permissions:
  contents: write

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: "Dioxus Deploy"
        uses: chmousset/rs-dioxus-deploy-action@0.1.3
```

# Github repo configuration
As documented in [JamesIves/github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action) this action depends on:

> You must configure your repository to deploy from the branch you push to. To do this, go to your repository settings, click on `Pages`, and choose `Deploy from a Branch` from the `Source` dropdown. From there select the branch you supplied to the action. In most cases this will be `gh-pages` as that's the default.

Since this setting requires the `gh-pages` branch to exist, it will only be available after a successfull build.
