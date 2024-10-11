Bills of Material (BOM)
===
Following the common `BOM` that any projects can include/inherit from

# 🛠️ Pre-requisites
Following are the pre-requisites to run the following module :

* [Java](https://openjdk.org) (version used `22.0.2`) — *this is only required if you plan to run the [zookeeper-kafka-test-container](zookeeper-kafka-test-container)*
* [Maven](https://maven.apache.org/download.cgi) (version used `3.9.6`) — *this is only required if you plan to run the [zookeeper-kafka-test-container](zookeeper-kafka-test-container)*


# ⚛ Developer Notes
The `BOM` should contain all if-not most of the dependencies that other modules can inherit from. This `BOM` is a child of the [org.springframework.boot::spring-boot-starter-parent](https://github.com/spring-projects/spring-boot/tree/3.3.x/spring-boot-project/spring-boot-starters/spring-boot-starter-parent) ; so this means it is fully capable to be used if you need a Spring Boot application typed

## Pre-requisites (Deploy)
Before you can perform the command `mvn clean install deploy -Dsnapshot` —OR— `mvn clean install deploy -Drelease`, the following settings are required in your Maven Settings (i.e. `~/.m2/settings.xml`)

* Ensure you had generated and obtained your [GitHub Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
* Add a new `<server>` section with the following content : 

```xml
<servers>
  <server>
    <id>github-krakenninja</id>
    <username>$USERNAME</username>
    <password>$TOKEN</password>
    <passphrase>$TOKEN</passphrase>
  </server>
</servers>
```

### Install `locally`
Following is used to install locally without deploying to GitHub. This is primarily useful if you would like to add/modify the existing `pom.xml` and test whether it works for you in your own project/module

| ℹ️ INFORMATION |
|---|
| **DEFAULT** installs as `snapshot` for the version, you can change this in command as well by using the property `-Dchangelist=-RELEASE` <br/> <p>Example : `mvn clean install -Dchangelist=-RELEASE`</p> |

Below is the command to use : 

```sh
mvn clean install
```

### Deploy `snapshot`
Following is used to deploy a `$VERSION-SNAPSHOT` into the [GitHub Packages](https://github.com/users/krakenninja/packages?repo_name=kraken-bom)

| ℹ️ INFORMATION |
|---|
| **MULTIPLE** deploy `snapshot` for the same version is allowed (but **NOT RECOMMENDED**). Only the latest will be used when it is included in your project `pom.xml` |

Below is the command to use : 

```sh
mvn clean install deploy -Drelease
```

### Deploy `release`
Following is used to deploy a `$VERSION-RELEASE` into the [GitHub Packages](https://github.com/users/krakenninja/packages?repo_name=kraken-bom) and [GitHub Releases](https://github.com/krakenninja/kraken-bom/releases)

| ⚠️ IMPORTANT |
|---|
| ‼️ **DO NOT** deploy `release` for the same version ever. Please MAKE SURE you change the `pom.xml` revision accordingly before performing the deploy `release` <br/><p>‼️ **ALWAYS PREPARE** the release notes that is located in source folder [release-notes.md](src/main/resources/release-notes.md)</p> |

Below is the command to use : 

```sh
mvn clean install deploy -Drelease
```

## ⚠️ Notes
Before you actually perform the `deploy` operation, ensure that the `<project><version>` is proper ; otherwise GitHub will throw HTTP error `Conflict (409)`. You can check what is [released](https://github.com/krakenninja/kraken-bom/releases) and/or [packaged](https://github.com/users/krakenninja/packages?repo_name=kraken-bom) before you run the `deploy` command against the `pom.xml` under the `properties` section ; change/manage only the `revision` element

```xml
<project>
  <properties>
    <revision>$VERSION</revision>
  </properties>
</project>
```

Generally the practices before `deploying` are : 

1. Check deployment pre-requisites
2. Check `pom.xml` source version (*change if you need*)
3. Check GitHub [kraken-bom](https://github.com/krakenninja/kraken-bom) repository to ensure no duplicated version(s) in either the [released](https://github.com/krakenninja/kraken-bom/releases) and/or [packages](https://github.com/users/krakenninja/packages?repo_name=kraken-bom) 
4. Commit and push latest changes if any (see *Step #2*)
5. Run the deploy command mentioned in the `Deploy xxxx` section above

## Using the BOM
There are **TWO(2)** ways that you can make use of this `BOM`

### Pre-requisites (Usage As ...)
* Your project `pom.xml` will need the following repository defined : 

```xml
<project>
  <repositories>
    <repository>
      <id>github-krakenninja</id>
      <name>GitHub KrakenNinja Packages</name>
        <url>https://maven.pkg.github.com/krakenninja/kraken-bom</url>
    </repository>
  </repositories>
</project>
```

### 1️⃣ As `dependency`
* You get the dependency versions decided globally by the `BOM` itself
* You get to maintain your POM with your own parent structure
* Sample snippet to declare in your own `pom.xml` : 

```xml
<dependencies>
  <dependency>
    <groupId>com.github.krakenninja</groupId>
    <artifactId>kraken-bom</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <type>pom</type>
    <scope>import</scope>
  </dependency>
</dependencies>
```

### 2️⃣ As `parent`
* You get the dependency versions managed for you by the `BOM` itself
* You get `Maven` plugin configurations and executions prepared
* You get `Maven` common properties inherited (and you can override it by redefining in your `POM` `<properties>` section
* Sample snippet to declare in your own `pom.xml` : 

```xml
<parent>
  <groupId> com.github.krakenninja </groupId>
  <artifactId>kraken-bom</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</parent>
```

# 📚 References
* [Bill of Materials (BOM) – A Complete Guide with Examples](https://www.mrpeasy.com/blog/bill-of-materials/)
* [Using Maven’s Bill of Materials (BOM)](https://reflectoring.io/maven-bom/)