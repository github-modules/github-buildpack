# Heroku Buildpack for GitHub Authentication

This is a Heroku buildpack that enables GitHub-authenticated operations
within a Heroku dyno.

It detects a `GITHUB_AUTH_TOKEN` environment variable and creates a `.netrc`
file with a GitHub entry.

## Setup

First, create a new GitHub access token at
[github.com/settings/tokens/new](https://github.com/settings/tokens/new).

Then save this token in your Heroku app's config:

```sh
heroku config:set GITHUB_AUTH_TOKEN=YOUR_TOKEN_HERE
```

Then configure your app to use this buildpack:

```sh
heroku buildpacks:add --index 1 https://github.com/zeke/github-buildpack
```

The next time you push your app to Heroku, this buildpack will create a
`.netrc` file containing your GitHub token in the base directory of the app:

```sh
heroku run bash
cat .netrc
machine github.com login YOUR_TOKEN_HERE password x-oauth-basic
```

Now you can perform authenticated git operations on the dyno. Note that if
you want to `git commit`, you'll need to specify the git user and email:

```sh
git clone https://github.com/you/some-private-repo
cd some-private-repo
git config user.email "zeke@sikelianos.com"
git config user.name "Zeke Sikelianos"
touch some/file
git commit -am "git commit from a dyno!"
git push origin master
```
