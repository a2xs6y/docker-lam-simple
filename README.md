# OpenLDAP + LDAP Account Manager (LAM)

Простая конфигурация для развёртывания **OpenLDAP** с веб-интерфейсом управления через **LDAP Account Manager (LAM)**.

## Состав
- **OpenLDAP** (`osixia/openldap:1.5.0`)
- **LAM** (`ldapaccountmanager/lam:8.3`)

## Подготовка

1. Создай `.env` файл рядом с `docker-compose.yml`:

   ```bash
   cp .env.example .env && chmod 0600 .env
   ```

2. Первый раз нужна инициализация:

   ```bash
   docker compose --file docker-compose-init.yml --env-file .env up -d
   mkdir ./lam_config
   docker cp lam:/var/lib/ldap-account-manager/config ./lam_config
   docker cp lam:/etc/ldap-account-manager/config.cfg ./lam_config
   docker compose stop
   docker compose up -d
   docker compose exec lam bash
      # внутри контейнера:
      chown -R www-data:root /var/lib/ldap-account-manager/config
      chown www-data:root /etc/ldap-account-manager/config.cfg
   ```

## Доступ

- **LDAP**:
  - `ldap://localhost:${LDAP_PORT_LDAP}`
  - DN администратора: `cn=admin,${LDAP_BASE_DN}`
  - Пароль: `${LDAP_ADMIN_PASSWORD}`

- **LAM (веб-интерфейс)**:
  [Настройка профиля](http://localhost:${LAM_PORT_HTTP}/lam/templates/config/conflogin.php)
  Имя профиля: `lam`
  Пароль: `${LAM_PASSWORD}`

## Данные

- Персистентные данные LDAP сохраняются в директориях:
  - `./ldap_data` — база данных LDAP
  - `./ldap_config` — конфигурация slapd
  - `./lam_config` - конфигурация LAM

Запустить сервисы:
```bash
docker compose up -d
```

Остановить сервисы:
```bash
docker compose stop
```

Перезапустить:
```bash
docker compose restart
```

## Ссылки

- [LDAP Account Manager (официальный сайт)](https://www.ldap-account-manager.org)
- [osixia/openldap (Docker Hub)](https://hub.docker.com/r/osixia/openldap)
- [ldapaccountmanager (Docker Hub)](https://hub.docker.com/r/ldapaccountmanager/lam)
- [ldapaccountmanager (Git Hub)](https://github.com/LDAPAccountManager/docker/pkgs/container/lam)
