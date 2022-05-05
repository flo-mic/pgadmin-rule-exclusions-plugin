# OWASP ModSecurity Core Rule Set - pgAdmin Rule Exclusions Plugin

## Description

This plugin contains rule exclusions for [pgAdmin](https://www.pgadmin.org/),
a tool intended to handle the administration of PostgreSQL over the Web, so it can be
run flawlessly togather with OWASP ModSecurity Core Rule Set (CRS).

## Prerequisities

 * CRS version 4.0 or newer (or see "Preparation for older installations" below)

## Installation

Copy all files from `plugins` directory into the `plugins` directory of your CRS
installation.

### Preparation for older installations

 * Create a folder named `plugins` in your existing CRS installation. That
   folder is meant to be on the same level as the `rules` folder. So there is
   your `crs-setup.conf` file and next to it the two folders `rules` and
   `plugins`.
 * Update your CRS rules include to follow this pattern:

```
<IfModule security2_module>

 Include modsecurity.d/owasp-modsecurity-crs/crs-setup.conf

 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-config.conf
 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-before.conf

 Include modsecurity.d/owasp-modsecurity-crs/rules/*.conf

 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-after.conf

</IfModule>
```

_Your exact config may look a bit different, namely the paths. The important
part is to accompany the rules-include with two plugins-includes before and
after like above. Adjust the paths accordingly._

## Configuration

This plugin can be enabled globally or in single locations with the variable in rule id `9516010` inside the file `plugins/pgadmin-rule-exclusions-config.conf`.


### tx.pgadmin-rule-exclusions-plugin_enabled

Allows enabling/disabling the plugin. You can for example disable the plugin globaly by setting the value to `0` and enable the excludion rules only in the related location. Therefore add another rule to the `modsecurity_rules` directive of the location and set the value to `1`. This will enable the plugin only in the specific location.

### tx.pgadmin-rule-exclusions-plugin_session_cookie_name

The plugin identifies the requests from pgAdmin based on the session-cookie name and URL path. If you have configured a custom session cookie name you will need to replace the cookie name `pga4_session` in the variable `tx.pgadmin-rule-exclusions-plugin_session_cookie_name`.

## Testing

After the plugin is enabled, your pgAdmin instance should work without any
problems possibly caused by CRS (for example, false positives while blocking
requests). If you are still having any problems, please file a new issue.

