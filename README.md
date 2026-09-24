# sites — project registry

`sites.yml` describes where each project's staging lives and where production is deployed.
The repo is private; the control server pulls it read-only with a deploy token (`/etc/wpd/sites-repo`).

## Keys

| Key | Level | Meaning |
| --- | --- | --- |
| `staging_domain` | root | Domain for `<slug>.<staging_domain>` |
| `staging_servers.<name>.backend` | server | `native` or `docker` |
| `staging_servers.<name>.host` | server | IP/host the control server uses for SSH |
| `staging_servers.<name>.public_ip` | server | Public IP for DNS records |
| `staging_servers.<name>.php` | server | Installed PHP versions |
| `defaults.*` | root | Values for sites not listed under `sites:` or that don't override a key |
| `sites.<repo>.slug` | site | Subdomain when the repo name doesn't fit (≤ 28 chars, `a-z0-9-`) |
| `sites.<repo>.theme_dir` | site | Theme folder, defaults to the repo name |
| `sites.<repo>.php` | site | PHP version (must be in the server's `php` list) |
| `sites.<repo>.wp_version` | site | `fresh` only; latest if omitted |
| `sites.<repo>.staging.server` | site | A name from `staging_servers` |
| `sites.<repo>.staging.mode` | site | `fresh` or `clone-prod` |
| `sites.<repo>.staging.ssl_cert` | site | `true` → DNS only + Let's Encrypt |
| `sites.<repo>.staging.uploads` | site | `proxy` → missing uploads are served from `prod.url` |
| `sites.<repo>.paths` | site | `repo_folder: wp-content/...`; without it the whole repo is the theme |
| `sites.<repo>.prod` | site | Without this key a push to `main` deploys nothing |
| `prod.driver` | prod | `ssh` (`ftp` — later) |
| `prod.wp_root` | prod | WordPress root on production |
| `prod.healthcheck` | prod | Paths that must return 200 without a fatal error |
| `prod.post_deploy` | prod | Commands run after the switch (in `wp_root`) |

## New project

1. For an existing site, a project with production, or a site not on the default server — add the entry here **first**.
2. Create the repo in `devphase` and protect `main`.
3. *Settings → CI/CD → General pipelines → CI/CD configuration file* = `wp-theme.yml@team-devphase/ci-templates:stable`.
4. Enable the Slack integration.
5. Push to `develop`.
