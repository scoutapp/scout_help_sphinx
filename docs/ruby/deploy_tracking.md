# Scout Ruby Deploy Tracking Config

Scout can [track deploys](#deploy-tracking), making it easier to correlate changes in your app to performance. To enable deploy tracking, first ensure you are on the latest version of `scout_apm`. See our [upgrade instructions](#updating-to-the-newest-version).

Scout identifies deploys via the following:

1. If you are using Capistrano, no extra configuration is required. Scout reads the contents of the `REVISION` and/or `revisions.log` file and parses out the SHA of the most recent release.
2. If you are using Heroku, enable [Dyno Metadata](https://devcenter.heroku.com/articles/dyno-metadata). This adds a `HEROKU_SLUG_COMMIT` environment variable to your dynos, which Scout then associates with deploys.
3. If you are deploying via a custom approach, set a `SCOUT_REVISION_SHA` environment variable equal to the SHA of your latest release.
4. If the app resides in a Git repo, Scout parses the output of `git rev-parse --short HEAD` to determine the revision SHA.
