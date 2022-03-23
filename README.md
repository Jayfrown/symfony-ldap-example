# Symfony LDAP example

An example Symfony 6 application implementing LDAP authentication. A docker-compose configuration is provided that spins up an OpenLDAP container and exposes the required ports so the local application can connect to it.

## PHPLdapAdmin

A PHPLdapAdmin container is included which is available on http://localhost:8080

LDAP Admin credentials are as follows:

- username: cn=admin,dc=example,dc=org
- password: admin

## Start the application

First, clone this repository, install and build dependencies:

```
composer install
yarn install
yarn dev
```

Then, spin up the database and LDAP containers:

```
docker compose up --build -d
```

Then, run the local symfony application:

```
symfony serve --no-tls --port 8000
```

## Users

The OpenLDAP container is seeded with two users in different OUs:

```
uid=testuser,ou=Users,dc=example,dc=org
uid=otheruser,ou=OtherUsers,dc=example,dc=org
```

By default (in the master branch) the application is configured to look for users in `ou=Users,dc=example,dc=org`, which means a single user is available with the following credentials:

- username: testuser
- password: testuser

In the `query_string` branch, the application is configured to search for `(&(uid={username}))` in `dc=example,dc=org`, which means in addition to the user mentioned above, the following credentials are also valid:

- username: otheruser
- password: otheruser
