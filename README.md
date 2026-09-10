# Releasing Moodle plugin versions in Moodle Marketplace from GitHub Actions

## Simple Usage

This is a [reusable workflow](https://docs.github.com/en/actions/using-workflows/reusing-workflows). Instead of copying the whole file, add a small caller workflow to your plugin repository.

1. Create `.github/workflows/moodle-release.yml` in your plugin repository with the following content:

   ```yaml
   name: Release Plugin version to Moodle Marketplace

   on:
     push:
       tags:
         - 'v*'
     workflow_dispatch:
       inputs:
         tag:
           description: 'Tag to be released (e.g. v1.4.0)'
           required: true

   jobs:
     release-to-marketplace:
       uses: moodlehq/moodle-plugin-release/.github/workflows/moodle-release.yml@main
       with:
         tag: ${{ inputs.tag }}
       secrets:
         MOODLE_MARKETPLACE_TOKEN: ${{ secrets.MOODLE_MARKETPLACE_TOKEN }}
   ```

2. Log in to the Moodle Marketplace. Navigate to "Account Settings" > "Security" (https://marketplace.moodle.com/account/security) and create a new API token. Copy the token immediately, as it is only displayed once.

3. Go to your plugin repository on GitHub. Navigate to "Settings" > "Secrets and Variables" > "Actions". Click "New repository secret", name it `MOODLE_MARKETPLACE_TOKEN`, and paste your API access token as the value.

4. That's it! Now when you tag the repository with a tag that matches the configured condition (starts with `v`, e.g. `v1.4.0`), the tagged version will be released in Moodle Marketplace.


## Advanced release notes handling

By default, the workflow takes the release notes from the description of the GitHub Release which belongs to the released tag. The simplest way to provide release notes is therefore to create a GitHub Release for your tag and to write the notes into its description.

If you do not create GitHub Releases, or if the changelog of your plugin lives somewhere else, you can tell the workflow where to take the release notes from by using the `release-notes-source` input:

| Value | Where the release notes are taken from |
|-------|----------------------------------------|
| `ghrelease` | The description of the GitHub Release which belongs to the tag. This is the default. |
| `input` | The additional optional `notes` input of this workflow. Use this if you want to extract or generate the notes yourself in your caller workflow. |
| `changelog` | The first of these files which exists in the root of your plugin, matched regardless of upper and lower case: `CHANGES.md`, `CHANGES.txt`, `CHANGES`, `CHANGELOG.md`, `CHANGELOG.txt`, `CHANGELOG`. If none of them exists, the version is submitted without release notes. |

For example, to publish the changelog file of your plugin as the release notes:

```yaml
jobs:
  release-to-marketplace:
    uses: moodlehq/moodle-plugin-release/.github/workflows/moodle-release.yml@main
    with:
      tag: ${{ inputs.tag }}
      release-notes-source: changelog
    secrets:
      MOODLE_MARKETPLACE_TOKEN: ${{ secrets.MOODLE_MARKETPLACE_TOKEN }}
```

Or, to compose the release notes in your caller workflow and hand them over:

```yaml
jobs:
  build-notes:
    runs-on: ubuntu-latest
    outputs:
      notes: ${{ steps.notes.outputs.notes }}
    steps:
      # Produce the release notes and set them as the notes output of this job.

  release-to-marketplace:
    needs: build-notes
    uses: moodlehq/moodle-plugin-release/.github/workflows/moodle-release.yml@main
    with:
      tag: ${{ inputs.tag }}
      release-notes-source: input
      notes: ${{ needs.build-notes.outputs.notes }}
    secrets:
      MOODLE_MARKETPLACE_TOKEN: ${{ secrets.MOODLE_MARKETPLACE_TOKEN }}
```

Regardless of the chosen source, a version whose release notes could not be determined at all is still submitted, but the workflow emits a warning about it.


## Additional notes

* If your release tags do not start with `v` character (such as `v9.0.1`) and you want to trigger the workflow for any tag, change the condition in your caller workflow as:

  ```
  on:
    push:
      tags:
        - '*'
  ```
* Marketplace API documentation is located at [moodledev.io](https://moodledev.io/general/community/plugincontribution/moodlemarketplaceapi).


## License

This program is free software: you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software Foundation,
either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this
program. If not, see <http://www.gnu.org/licenses/>.

