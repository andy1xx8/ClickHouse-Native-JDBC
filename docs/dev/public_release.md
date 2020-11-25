Public Release Manuel
=====================

::: warning
Public Release means deploy to Maven Central. Only core team members are granted to 
deploy into Public Repository.
:::

## Setup GPG

Install on macOS
```shell script
brew install gnupg
```

Generate key if you don't have it.
```shell script
gpg --full-generate-key
```

Publish public key
```shell script
gpg --send-keys [key-id] --keyserver hkp://subkeys.pgp.net
```

Show keys
```shell script
gpg --list-keys
```

## Setup Sonatype Authentication

Edit and put below configurations into `~/.m2/settings.xml`

```xml
<servers>
  <server>
    <id>sonatype-nexus-snapshots</id>
    <username>{username}</username>
    <password>{password}</password>
  </server>
  <server>
    <id>sonatype-nexus-staging</id>
    <username>{username}</username>
    <password>{password}</password>
  </server>
</servers>
```

## Cut Branch

It's only need for **feature release**, you should cut a branch from master.

```shell script
git checkout -b 2.5
```

## Bump Release Version

```shell script
mvn versions:set -DnewVersion=2.5.0
mvn versions:commit
git commit -am '(release) prepare release v2.5.0-rc0'
```

## Package and Deploy to Sonatype

```shell script
mvn clean deploy -DskipTests -Prelease
mvn clean deploy -DskipTests -Prelease -Pscala-2.12
```

## Publish Release

Go to [Sonatype Repository](https://oss.sonatype.org/#stagingRepositories)

1. verify artifacts
2. close repository
3. release to Maven Central

## Announce on GitHub

Edit at [GitHub Release Page](https://github.com/housepower/ClickHouse-Native-JDBC/releases)

## Bump Development Version

```shell script
mvn versions:set -DnewVersion=2.6.0-SNAPSHOT
mvn versions:commit
git commit -am '(release) prepare for next development iteration'
```