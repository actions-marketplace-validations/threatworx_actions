# ThreatWorx GitHub Actions

A set of [GitHub Action](https://github.com/features/actions) for using [ThreatWorx](https://threatworx.io) [twigs CLI](https://threatworx.io/twigs-user-guide/) to check for
vulnerabilities in your GitHub projects. Following actions that are currently available:

- [Install twigs](install-twigs)
- [Repository scan](repo-scan)

The [Install twigs](install-twigs) action is a pre-requisite for any workflow that needs to run the twigs CLI. Also these actions require 
Here's an example of a workflow that uses these actions to scan a Github repository:

```yaml
name: Example workflow using ThreatWorx Github Actions
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo content
        uses: actions/checkout@v2 # checkout the repository content to github runner
          
      - name: Install ThreatWorx discovery CLI twigs
        uses: threatworx/actions/install-twigs@master

      - name: Run ThreatWorx discovery CLI twigs
        uses: threatworx/actions/repo-scan@master
        env: 
          TW_HANDLE: ${{ secrets.TW_HANDLE }}
          TW_INSTANCE: ${{ secrets.TW_INSTANCE }}
          TW_TOKEN: ${{ secrets.TW_TOKEN }}
        with:
          args: -vv
          mode_args: --repo ${{ github.workspace }}
```

Details on options/arguments available for scanning repositories including, SAST checks, secrets scan, IaC scan etc. are available in [the twigs user guide](https://threatworx.io/twigs-user-guide/#source-discovery)


The example here uses `actions/setup-go` would you would need to select the right actions to install the relevant development requirements for your project. If you are already using the same pipeline to build and test your application you're likely already doing so.

### Getting your ThreatWorx API token

The Actions example above refer to a ThreatWorx API token:

```yaml
env:
  TW_TOKEN: ${{ secrets.TW_TOKEN }}
```

You can create an API token through your ThreatWorx account either from your account on [ThreatWorx SaaS](https://threatworx.io/i3) or by logging in to your dedicated ThreatWorx instance. Follow the Profile -> Key Management menu to generate a token to use in your workflow.

[twigs CLI on Github]: https://github.com/threatworx/twigs 'twigs CLI'
[twigs user guide]: (https://threatworx.io/twigs-user-guide/) 'twigs user guide'
