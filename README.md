# gridpane-wordpress-template-all-plugins

This repo is an example of a repo configured for full deployment (all wp core files, plugins and themes managed by git repo), with all of the requisite plugins required for the following GridPane panel functionality:
- Nginx Page Caching
- Redis Object Caching
- WPFail2Ban Integration
- SendGrid SMTP Integration
- Additional Security Features
  - Disable Emoji
  - Disable RSS
  - Disable Username Enumeration
  - Block WP Scan Agent
  - Block WP Version
- SSO WP Login

## .gpconfig directory

### Deployment Type

As no `.gpconfig/hybrid` token file exists within this repo, it will be deployed as `GridPane Full Git` type deployment.

This means that the repository will be expected to contain all:
- Core
- Plugins
- Themes
  For your WordPress site.

WordPress Core, Plugins and Themes will be owned by root and the system php user will not be able to write to these files.
In addition we will be enabling the `define('DISALLOW_FILE_MODS',true);` constant to disallow file edits.

All updates will need to be done via Git.

### Deployment Scripts

You will find the following empty scripts:
```shell
.gpconfig/predeploy-server.sh
.gpconfig/predeploy.sh
.gpconfig/postdeploy.sh
.gpconfig/postdeploy-server.sh
```

These scripts will execute in the above order.

The `-server.sh` scripts are run as root, with the others running as the site system user.

The scripts are run from the `/.gpconfig` directory.

The root scripts can be useful for running scripts across the server and microservices which need elevated privileges, be careful.

The system user scripts can be useful for running system user functionality, php, wp-cli etc.

## Plugins

Based on the sites GridPane configuration
- Nginx Page Caching
- Redis Object Caching
- WP Fail2Ban integration active  

This repo contains:

```shell
wp-content/plugins/gridpane-redis-object-cache
wp-content/plugins/nginx-helper
wp-content/plugins/wp-fail2ban
wp-content/object-cache.php     <-- drop in from gridpane-redis-object-cache
```

#### Plugin links

If you are using the Nginx Stack and Nginx Page Caching and Redis Object Caching you will need to use these plugins:

[Nginx-Helper (WordPress.org download link)](https://downloads.wordpress.org/plugin/litespeed-cache.4.6.zip)

[GridPane Redis Object Cache (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-redis-object-cache)

If you are using Nginx FastCGI caching, you will also need the GridPane Nginx-Helper Helper. This is not needed for Redis based Page Caching.

[GridPane Nginx Cache Purger (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-nginx-cache-purger)

If, on the other hand, you are using the OLS stack, you will need to configure with the LSCache plugin (for both Page and Redis Object Caching)

[LiteSpeed Cache (WordPress.org download link)](https://downloads.wordpress.org/plugin/litespeed-cache.4.6.zip)

If your site is using the WPFail2Ban integration in the Panel, then you will need this plugin too. On Deploy we detect if this plugin exists and if your site is convigured for it, and look after the symlinking to MU.

[WP Fail2Ban (WordPress.org download link)](https://downloads.wordpress.org/plugin/wp-fail2ban.4.4.0.4.zip)

## MU-Plugins

Based on this site being configured with
- Additional Security Settings
  - Disable Emoji
  - Disable RSS
  - Disable Username Enumeration
  - Block WP Scan Agent
  - Block WP Version
- Sendgrid SMTP integration active
- SSO login active

This repo contains:

```shell
wp-content/mu-plugins/gridpane-block-username-enumeration
wp-content/mu-plugins/gridpane-block-wpscan-agent
wp-content/mu-plugins/gridpane-disable-rss
wp-content/mu-plugins/gridpane-kill-all-emoji
wp-content/mu-plugins/gridpane-mailer
wp-content/mu-plugins/gridpane-nginx-cache-purger
wp-content/mu-plugins/gridpane-remove-wp-version
wp-content/mu-plugins/mu-gridpane-block-username-enumeration.php
wp-content/mu-plugins/mu-gridpane-block-wpscan-agent.php
wp-content/mu-plugins/mu-gridpane-disable-rss.php
wp-content/mu-plugins/mu-gridpane-kill-all-emoji.php
wp-content/mu-plugins/mu-gridpane-mailer.php
wp-content/mu-plugins/mu-gridpane-nginx-cache-purger.php
wp-content/mu-plugins/mu-gridpane-remove-wp-version.php
wp-content/mu-plugins/wp-cli-login-server.php
```

#### MU Plugin links

If your site is using the GridPane Sendgrid Mailer integration, then you will need to include that plugin as an MU-Plugin. The repo linked here has instructions, but you can check the mu-plugins directory of this repo to confirm correct configuration. 

[GridPane Mailer (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-mailer)

If your site is using any of the GridPane Additional Security Beta settings, then you will need the appropriate MU-Plugin for that feature. The repos linked here have instructions, but you can check the mu-plugins directory of this repo to conform correct configuration.

[GridPane Block Username Enumeration (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-block-username-enumeration)  
[GridPane Block-WPScan-Agent (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-block-wpscan-agent)  
[GridPane Disable RSS (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-disable-rss)  
[GridPane Disable Emoji / Kill All Emoji (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-kill-all-emoji)  
[GridPane Remove WP Version (GridPane-Dev GitHub Page)](https://github.com/GridPane-Dev/gridpane-remove-wp-version)

All sites require the wp-cli-login-server.php mu plugin for SSO to function. If this is not found we will install it on SSO if possible, but it might make sense to just include in your repo and control it's update too.

[WP-CLI login server (aaemnnosttv Github)](https://github.com/aaemnnosttv/wp-cli-login-server/blob/master/wp-cli-login-server.php)

After deploy, a function runs and checks for all of the above and will sync the state to the app with the site set as the source of truth. If anything is missing (apart from the login server)
we will just adjust the app data to match.









