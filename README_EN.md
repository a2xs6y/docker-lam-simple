# OpenLDAP + LDAP Account Manager (LAM)

A simple setup for deploying **OpenLDAP** with a web-based management interface using **LDAP Account Manager (LAM)**.

## Components
- **OpenLDAP** (`osixia/openldap:1.5.0`)
- **LAM** (`ldapaccountmanager/lam:8.3`)

## Preparation

1. Create a `.env` file next to `docker-compose.yml`:

   ```bash
   cp .env.example .env && chmod 0600 .env
   ```

2. Initial setup is required on the first run:

   ```bash
   docker compose --file docker-compose-init.yml --env-file .env up -d
   mkdir ./lam_config
   docker cp lam:/var/lib/ldap-account-manager/config ./lam_config
   docker cp lam:/etc/ldap-account-manager/config.cfg ./lam_config
   docker compose stop
   docker compose up -d
   docker compose exec lam bash
      # inside the container:
      chown -R www-data:root /var/lib/ldap-account-manager/config
      chown www-data:root /etc/ldap-account-manager/config.cfg
   ```

## Access

- **LDAP**:
  - `ldap://localhost:${LDAP_PORT_LDAP}`
  - Admin DN: `cn=admin,${LDAP_BASE_DN}`
  - Password: `${LDAP_ADMIN_PASSWORD}`

- **LAM (web interface)**:
  [Profile setup](http://localhost:${LAM_PORT_HTTP}/lam/templates/config/conflogin.php)
  Profile name: `lam`
  Password: `${LAM_PASSWORD}`

## Data

- Persistent LDAP data is stored in:
  - `./ldap_data` — LDAP database
  - `./ldap_config` — slapd configuration
  - `./lam_config` — LAM configuration

Start services:
```bash
docker compose up -d
```

Stop services:
```bash
docker compose stop
```

Restart services:
```bash
docker compose restart
```

## Links

- [LDAP Account Manager (official website)](https://www.ldap-account-manager.org)
- [osixia/openldap (Docker Hub)](https://hub.docker.com/r/osixia/openldap)
- [ldapaccountmanager (Docker Hub)](https://hub.docker.com/r/ldapaccountmanager/lam)
- [ldapaccountmanager (GitHub)](https://github.com/LDAPAccountManager/docker/pkgs/container/lam)
