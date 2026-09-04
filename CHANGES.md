### 2026090401 ###

* Pass the resolved tag to `actions/checkout` so that manual runs (`workflow_dispatch`)
  correctly checkout and package the specified tag commit instead of default branch `HEAD`.
  Issue #16

### 2026082501 ###

* Converted the template into a reusable workflow (`workflow_call`), located at
  `.github/workflows/moodle-release.yml`. Plugin repositories now add a small caller
  workflow instead of copying the whole file — see README.md for the updated usage.

### 2026081901 ###

* Enable publishing new plugin versions to Moodle Marketplace.

### 2021070201 ###

* Use `--data-urlencode` for curl arguments to fix troubles with special characters in
  tag names and potentially elsewhere.
* Started to use revision number in the template for easier reference. Format similar
  to what we use in plugins.

### 2021070200 ###

* Improved authentication security. The workflow no longer needs moodle.org username
  and password. Instead, developers are expected to generate the token directly via
  the web interface in the plugins directory and use it.
* The workflow submits additional meta-data for the new version: changelogurl,
  altdownloadurl, vcssystem, vcsrepositoryurl and vcstag. This also fixes issue #6.
* Improved server-side validation in the plugins directory fixes issue #7.
