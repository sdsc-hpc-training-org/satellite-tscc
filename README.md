# Satellite Deployment for TSCC

This repo contains the TSCC-specific config for satellite
(but not the vpn/wireguard configs which are also necessary).

There's a sub-repo to pull in the upstream satellite repo. To use this either
  ```
  git clone ...
  git submodule update --init --remote
  ```

  or
  `git clone --recurse-submodules ...`


# Deploy

The mkdeployment script will produce a production deployment in a directory called 'deployment', suitable for `/var/www/satellite`.  This means:
1. Init and update submodule (with latest changes), in case someone forgot to do that.
2. Create `deployment` directory.
3. Merge upstream and customizations in `deployment`.
4. Set ownership if root.
5. Create empty state database in `deployment/var`.
6. Set permissions.
7. Create empty deployment/dynconf/proxyconf.conf file.
   (so bin/cron doesn't blow up)

# Other Deployment Steps
1. If not exists, create sqlite state database in /var/secrets/satellite-state/state.sqlite, or copy it from deployment.
2. Create cron job to call bin/cron. It can be done as a separate user (not the sameone apache runs as) or root.  Make sure the command to reload apache is correct at the bottom of bin/cron.  Run it every minute.
3. Create symlinks in /var/secrets/ for http-server.pem and http-derver.privkey.
4. Copy httpd.conf to /etc/httpd/conf/httpd.conf.
