# Update Bit Components with GitHub's Dependabot

Solve the problem of updating consuming projects of your components by utilizing Dependabot and Bit together. This way when new versions of components are available you get automated PRs to update the components you consume.

[YouTube video toturial on how this works and how to set up](https://www.youtube.com/watch?v=PZ2MhC5N6uI)

This repository contains example configuration and workflow for how a project that depends on Bit Components as packages in `package.json` can utilize GitHub's Dependabot feature to get automated PRs for updating dependencies.

## What you need?

- Ensure you have a `.npmrc` file in the root of your workspace with your [scoped registry configured to your bit.cloud account](https://bit.dev/reference/packages/npmrc).
- Generate an authentication token in bit.cloud and set it as a Dependabot secret for your repository.
- Configure your `.github/dependabot.yml` file with the right policy to update your Bit Components.
- Configure Dependabot in your repository settings.

## References

See the below links from GitHub to learn more:

- [Setting up `npmrc` for installing Bit components](https://bit.dev/reference/packages/npmrc)
- [Using private registries with Dependabot](https://github.blog/2021-03-15-dependabot-private-dependencies/)
- [Setting version updates](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#scheduleinterval)
- [Configuring Dependabot for NPM registries](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/guidance-for-the-configuration-of-private-registries-for-dependabot#npm)
- [Dependabot quick start](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide)

## Examples

From this repository:

### `.npmrc`

Below you see an example `.npmrc` with a scoped registry, and `replace-registry-host=never` as recommended by GitHub.

```
@itaysso:registry=https://node-registry.bit.cloud
replace-registry-host=never
```

### `.github/dependabot.yml`

```
version: 2
registries:
  # Define the Bit NPM registry
  npm-bit:
    type: npm-registry
    url: https://node-registry.bit.cloud
    # Use your Dependabot-Bit token
    token: ${{secrets.DEPENDABOT_TOKEN}}
    replaces-base: true
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    allow:
      # Only check for components coming from Bit
      - dependency-name: "@itaysso/*"
    registries:
      # Use the Bit Registry for this update policy
      - npm-bit
```
