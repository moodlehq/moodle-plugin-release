# Releasing Moodle plugin versions in Moodle Marketplace from GitHub Actions

## Usage

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


## Tips

* Provide release notes when creating a GitHub Release. The workflow will automatically use your GitHub Release description.
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

