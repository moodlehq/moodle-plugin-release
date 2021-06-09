# Releasing Moodle plugins in the Plugins directory automatically from GitHub Actions

## Usage

1. Copy the template file [moodle-release.yml](https://github.com/moodlehq/moodle-plugin-release/blob/main/moodle-release.yml)
   into the `.github/workflows/moodle-release.yml` location within your plugin
   repository.
2. Edit the file and set the `PLUGIN` environment variable to the full component name
   (aka frankenstyle) of your plugin. Example:

   ```
    env:
      PLUGIN: mod_subcourse
   ```

   There should be no need to edit any other line as long as you are happy with the
   default behaviour of the workflow. If your release tags do not start with `v`
   character (such as `v9.0.1`) then you may need to amend the trigger conditions.
   Please refer to the GitHub Actions documentation for details.
3. Go to your plugin's Settings > Secrets section and define two environmental
   variables there that will be used for authentication at moodle.org. Use the names
   `MOODLE_ORG_USERNAME` and `MOODLE_ORG_PASSWORD` and set their values to the
   username and password (respectively) of your account at <https://moodle.org>. This
   user account must be set as a maintainer of the plugin in the Plugins directory.

4. That's it! Now when you tag the repository with a tag that matches the configured
   condition, the tagged version will be released in the plugins directory.


## Tips

* Use settings `$plugin->requires`, `$plugin->supported` and
  `$plugin->incompatible` in your plugin's
  [version.php](https://docs.moodle.org/dev/version.php). They are used to
  automatically populate the list of supported Moodle versions. By default, all Moodle
  versions starting from the one described by `$plugin->requires` will be marked as
  supported.
* Keep the `CHANGES.md` file in the root of your repository. It will be automatically
  imported as release notes for the new version. Please see [Moodle dev
  docs](https://docs.moodle.org/dev/Plugin_files#CHANGES) for all supported names and
  formats.


## License

This program is free software: you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software Foundation,
either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this
program. If not, see <http://www.gnu.org/licenses/>.

