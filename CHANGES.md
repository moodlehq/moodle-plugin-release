### 2021-07-02 ###

* Improved authentication security. The workflow no longer needs moodle.org username
  and password. Instead, developers are expected to generate the token directly via
  the web interface in the plugins directory and use it.
* The workflow submits additional meta-data for the new version: changelogurl,
  altdownloadurl, vcssystem, vcsrepositoryurl and vcstag. This also fixes issue #6.
* Improved server-side validation in the plugins directory fixes issue #7.
