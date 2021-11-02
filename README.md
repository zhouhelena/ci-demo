# CI Demo

Time to publish. Fork this repo to get started.

## Installation

Make sure you have [Node and NPM installed](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#using-a-node-installer-to-install-nodejs-and-npm).

## Setup

Make yourself a [Github Personal Access token](https://github.com/settings/tokens/new) and save it. This token needs the `write:packages` permission. We'll use this to pull down our NPM package after we publish it.

Then, go to the `Actions` tab on your repo and enable Github Actions.

## Demo Time

First, look around you and find a partner (or two)! Y'all will publish separate repos, but work together to try and figure out why things are or aren't working.

For this demo, you're going to be publishing your own Node.js application! This app will be a simple CLI command that you can customize to say whatever you want. Before going any further, check out `index.js`! Add whatever functionality/message you want, and move on.

Next, open up `package.json`. We've got some 188-specific configuration in there - specifically the repository and name. Change these to reflect that this is now under _your_ user, not the `cis188` organization.

Finally, run `npm install` to install your dependencies and `node .` to make sure your app will actually print something out.

### Testing in CI

As we mentioned in lecture, Github lets you define some jobs to run when a push to a repo happens. We've given you a very basic config in `.github/workflows/publish.yaml` that just echoes `"todo"` whenever you push. Change this step so that it will install your packages with `npm install` and check that your package passes linting with `npm run lint`.

Once you've added this config, commit and push up your code. Go to the `Actions` tab and see if your job succeeds! If it does, feel free to move on. If it doesn't, check the CI output and try to debug.

### Publishing your package

Now that you've checked that your package lints automatically, it's time to automatically publish it! In this case, we're going to publish to the [Github NPM registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry).

You need to add a job named `publish` that:

1. Needs `lint` to complete before starting
1. Checks out the code
1. Publishes your npm package

We'll give you what the step looks like for 3, but the first two you've gotta figure out! 3:

```yaml
- uses: JS-DevTools/npm-publish@v1
  with:
    registry: https://npm.pkg.github.com
    token: ${{ secrets.GITHUB_TOKEN }}
```

Once you've gotten the config figured out, push up and see if CI works.

### Using your package

We're in the home stretch now. Edit the file `~/.npmrc` if it exists, and create it if it doesn't already exist. In the file, add the following config, replacing OWNER with your Github username and PAT with your Github personal access token.

```
//npm.pkg.github.com/:_authToken=PAT
@OWNER:registry=https://npm.pkg.github.com
```

After editing this file, cross your fingers and try to install your package.

```
$ npm install -g @OWNER/ci-demo
$ ci-demo
Hello World: 6aa43f18-4d3c-4f05-a29a-4c25907c8f7b
```

If this worked, then you've successfully automated publishing for your own npm app! Once you're done, call over Armaan and ask him how many people pied him today.
