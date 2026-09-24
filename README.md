# sites — реєстр проєктів

`sites.yml` описує, де живе staging кожного проєкту і куди деплоїться прод.
Репо приватне; control-сервер тягне його read-only deploy-токеном (`/etc/wpd/sites-repo`).

## Ключі

| Ключ | Де | Значення |
| --- | --- | --- |
| `staging_domain` | корінь | Домен для `<slug>.<staging_domain>` |
| `staging_servers.<name>.backend` | сервер | `native` або `docker` |
| `staging_servers.<name>.host` | сервер | IP/хост для SSH від control |
| `staging_servers.<name>.public_ip` | сервер | Публічний IP для DNS-записів |
| `staging_servers.<name>.php` | сервер | Встановлені версії PHP |
| `defaults.*` | корінь | Значення для сайтів, яких немає в `sites:` або які не перевизначили ключ |
| `sites.<repo>.slug` | сайт | Піддомен, якщо назва репо не підходить (≤ 28 символів, `a-z0-9-`) |
| `sites.<repo>.theme_dir` | сайт | Тека теми, за замовчуванням = назва репо |
| `sites.<repo>.php` | сайт | Версія PHP (має бути в `php` сервера) |
| `sites.<repo>.wp_version` | сайт | Лише для `fresh`; без неї latest |
| `sites.<repo>.staging.server` | сайт | Ім'я з `staging_servers` |
| `sites.<repo>.staging.mode` | сайт | `fresh` або `clone-prod` |
| `sites.<repo>.staging.ssl_cert` | сайт | `true` → DNS only + Let's Encrypt |
| `sites.<repo>.staging.uploads` | сайт | `proxy` → відсутні uploads з `prod.url` |
| `sites.<repo>.paths` | сайт | `тека_в_репо: wp-content/...`; без ключа все репо = тема |
| `sites.<repo>.prod` | сайт | Без цього ключа push у `main` нічого не деплоїть |
| `prod.driver` | прод | `ssh` (`ftp` — пізніше) |
| `prod.wp_root` | прод | Корінь WordPress на проді |
| `prod.healthcheck` | прод | Шляхи, які мають віддати 200 без fatal |
| `prod.post_deploy` | прод | Команди після перемикання (виконуються в `wp_root`) |

## Новий проєкт

1. Для існуючого сайту, проєкту з продом або сайту не на сервері за замовчуванням — **спершу** запис тут.
2. Створити репо в `devphase`, захистити `main`.
3. *Settings → CI/CD → General pipelines → CI/CD configuration file* = `wp-theme.yml@team-devphase/ci-templates:stable`.
4. Увімкнути Slack-інтеграцію.
5. Push у `develop`.
