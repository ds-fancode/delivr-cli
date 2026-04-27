# Delivr CLI — Command Reference

Complete reference for all `delivr` commands.

---

## Table of Contents

- [Authentication](#authentication)
  - [login](#login)
  - [logout](#logout)
  - [whoami](#whoami)
  - [register](#register)
  - [link](#link)
- [Access Keys](#access-keys)
  - [access-key add](#access-key-add)
  - [access-key patch](#access-key-patch)
  - [access-key list](#access-key-list)
  - [access-key remove](#access-key-remove)
- [Organisations](#organisations)
  - [org list](#org-list)
- [Apps](#apps)
  - [app add](#app-add)
  - [app rename](#app-rename)
  - [app remove](#app-remove)
  - [app list](#app-list)
  - [app transfer](#app-transfer)
- [Collaborators](#collaborators)
  - [collaborator add](#collaborator-add)
  - [collaborator list](#collaborator-list)
  - [collaborator remove](#collaborator-remove)
- [Deployments](#deployments)
  - [deployment add](#deployment-add)
  - [deployment rename](#deployment-rename)
  - [deployment remove](#deployment-remove)
  - [deployment list](#deployment-list)
  - [deployment history](#deployment-history)
  - [deployment clear](#deployment-clear)
- [Releases](#releases)
  - [release](#release)
  - [release-react](#release-react)
  - [promote](#promote)
  - [patch](#patch)
  - [rollback](#rollback)
- [Binary Patching](#binary-patching)
  - [create-patch](#create-patch)
  - [apply-patch](#apply-patch)
- [CI/CD Build Uploads](#cicd-build-uploads)
  - [upload-aab-build](#upload-aab-build)
  - [upload-regression-artifact](#upload-regression-artifact)
  - [upload-testflight-build-number](#upload-testflight-build-number)
- [Debugging](#debugging)
  - [debug](#debug)
- [Sessions](#sessions)
  - [session list](#session-list)
  - [session remove](#session-remove)

---

## Authentication

### login

Authenticate with the Delivr server.

```bash
delivr login [<serverUrl>] [--accessKey <key>]
```

| Option        | Alias   | Required | Description                                                  |
| ------------- | ------- | -------- | ------------------------------------------------------------ |
| `--accessKey` | `--key` | No       | Access key to authenticate with instead of username/password |

**Examples:**

```bash
delivr login
delivr login --accessKey mykey
delivr login https://my-server.com --accessKey mykey
```

### logout

Log out of the current session.

```bash
delivr logout
```

### whoami

Display account info for the current login session.

```bash
delivr whoami
```

### register

Register a new account.

```bash
delivr register [<serverUrl>]
```

### link

Link an additional authentication provider (e.g. GitHub) to an existing account.

```bash
delivr link [<serverUrl>]
```

---

## Access Keys

### access-key add

Create a new access key.

```bash
delivr access-key add <accessKeyName> --scope <scope> [--ttl <duration>]
```

| Option    | Required | Default | Description                                |
| --------- | -------- | ------- | ------------------------------------------ |
| `--scope` | **Yes**  | —       | Access scope: `Read`, `Write`, or `All`    |
| `--ttl`   | No       | `60d`   | Validity duration (e.g. `5m`, `60d`, `1y`) |

**Examples:**

```bash
access-key add "VSTS Integration" --scope All
access-key add "One time key" --scope Read --ttl 5m
```

### access-key patch

Update the name, TTL, or scope of an existing access key.

```bash
delivr access-key patch <accessKeyName> --scope <scope> [--name <newName>] [--ttl <duration>]
```

| Option    | Required | Default | Description                             |
| --------- | -------- | ------- | --------------------------------------- |
| `--scope` | **Yes**  | —       | Access scope: `Read`, `Write`, or `All` |
| `--name`  | No       | —       | New display name for the key            |
| `--ttl`   | No       | —       | New validity duration                   |

**Examples:**

```bash
access-key patch "Key for build server" --scope Write --name "Key for CI machine"
access-key patch "Key for build server" --scope Read --ttl 7d
```

### access-key list

List access keys associated with your account.

```bash
delivr access-key list [--format <format>]
```

| Option     | Default | Description                      |
| ---------- | ------- | -------------------------------- |
| `--format` | `table` | Output format: `table` or `json` |

**Aliases:** `access-key ls`

### access-key remove

Remove an existing access key.

```bash
delivr access-key remove <accessKeyName>
```

**Aliases:** `access-key rm`

---

## Organisations

### org list

List organisations associated with your account.

```bash
delivr org list [--format <format>]
```

| Option     | Default | Description                      |
| ---------- | ------- | -------------------------------- |
| `--format` | `table` | Output format: `table` or `json` |

**Aliases:** `org ls`

---

## Apps

### app add

Add a new app to your account.

```bash
delivr app add <ownerName>/<appName>
```

**Example:**

```bash
app add OrgName/MyApp
```

### app rename

Rename an existing app.

```bash
delivr app rename <currentAppName> <newAppName>
```

**Example:**

```bash
app rename CurrentName NewName
```

### app remove

Remove an app from your account.

```bash
delivr app remove <ownerName>/<appName>
```

**Aliases:** `app rm`

### app list

List apps associated with your account or an organisation.

```bash
delivr app list [--org <orgName>] [--format <format>]
```

| Option     | Default | Description                      |
| ---------- | ------- | -------------------------------- |
| `--org`    | —       | Organisation name to filter by   |
| `--format` | `table` | Output format: `table` or `json` |

**Aliases:** `app ls`

**Examples:**

```bash
app list
app list --org MyOrg
app list --format json
```

### app transfer

Transfer ownership of an app to another account.

```bash
delivr app transfer <ownerName>/<appName> <email>
```

**Example:**

```bash
app transfer MyApp foo@bar.com
```

---

## Collaborators

### collaborator add

Add a new collaborator to an app.

```bash
delivr collaborator add <ownerName>/<appName> <email>
```

**Example:**

```bash
collaborator add MyApp foo@bar.com
```

### collaborator list

List the collaborators for an app.

```bash
delivr collaborator list <ownerName>/<appName> [--format <format>]
```

| Option     | Default | Description                      |
| ---------- | ------- | -------------------------------- |
| `--format` | `table` | Output format: `table` or `json` |

**Aliases:** `collaborator ls`

### collaborator remove

Remove a collaborator from an app.

```bash
delivr collaborator remove <ownerName>/<appName> <email>
```

**Aliases:** `collaborator rm`

---

## Deployments

### deployment add

Add a new deployment to an app.

```bash
delivr deployment add <ownerName>/<appName> <deploymentName> [--key <deploymentKey>]
```

| Option  | Alias | Required | Description              |
| ------- | ----- | -------- | ------------------------ |
| `--key` | `-k`  | No       | Specify a deployment key |

**Examples:**

```bash
deployment add OrgName/MyApp MyDeployment
deployment add MyApp MyDeployment -k abc123
```

### deployment rename

Rename an existing deployment.

```bash
delivr deployment rename <ownerName>/<appName> <currentName> <newName>
```

**Example:**

```bash
deployment rename OrgName/MyApp CurrentDeploymentName NewDeploymentName
```

### deployment remove

Remove a deployment from an app.

```bash
delivr deployment remove <ownerName>/<appName> <deploymentName>
```

**Aliases:** `deployment rm`

### deployment list

List deployments associated with an app.

```bash
delivr deployment list <ownerName>/<appName> [--format <format>] [--displayKeys]
```

| Option          | Alias | Default | Description                      |
| --------------- | ----- | ------- | -------------------------------- |
| `--format`      | —     | `table` | Output format: `table` or `json` |
| `--displayKeys` | `-k`  | `false` | Show deployment keys             |

**Aliases:** `deployment ls`

### deployment history

Display the release history for a deployment.

```bash
delivr deployment history <ownerName>/<appName> <deploymentName> [--format <format>] [--displayAuthor]
```

| Option            | Alias | Default | Description                      |
| ----------------- | ----- | ------- | -------------------------------- |
| `--format`        | —     | `table` | Output format: `table` or `json` |
| `--displayAuthor` | `-a`  | `false` | Show the release author          |

**Aliases:** `deployment h`

### deployment clear

Clear the release history associated with a deployment.

```bash
delivr deployment clear <ownerName>/<appName> <deploymentName>
```

---

## Releases

### release

Release an update to an app deployment.

```bash
delivr release <ownerName>/<appName> <updateContentsPath> <targetBinaryVersion> [options]
```

| Option                      | Alias  | Default   | Description                                     |
| --------------------------- | ------ | --------- | ----------------------------------------------- |
| `--deploymentName`          | `-d`   | `Staging` | Target deployment                               |
| `--description`             | `-des` | —         | Description of the changes                      |
| `--disabled`                | `-x`   | `false`   | Prevent immediate download                      |
| `--mandatory`               | `-m`   | `false`   | Force users to accept this update               |
| `--noDuplicateReleaseError` | —      | `false`   | Warn instead of error on duplicate release      |
| `--rollout`                 | `-r`   | `100%`    | Percentage of users to target (1–100)           |
| `--isPatch`                 | `-p`   | `false`   | Whether update is a patch (diff) or full bundle |
| `--compression`             | `-c`   | `deflate` | Compression algorithm: `deflate` or `brotli`    |

**Examples:**

```bash
release OrgName/MyApp app.js "*"
release OrgName/MyApp ./platforms/ios/www 1.0.3 -d Production
release OrgName/MyApp ./platforms/ios/www 1.0.3 -d Production -r 20
```

### release-react

Release a React Native update to an app deployment.

```bash
delivr release-react <ownerName>/<appName> <platform> [options]
```

| Option                      | Alias   | Default   | Description                                                    |
| --------------------------- | ------- | --------- | -------------------------------------------------------------- |
| `--bundleName`              | `-b`    | Auto      | JS bundle file name                                            |
| `--deploymentName`          | `-d`    | `Staging` | Target deployment                                              |
| `--description`             | `-des`  | —         | Description of the changes                                     |
| `--development`             | `--dev` | `false`   | Generate dev build instead of release                          |
| `--disabled`                | `-x`    | `false`   | Prevent immediate download                                     |
| `--entryFile`               | `-e`    | Auto      | Path to app's entry JS file                                    |
| `--gradleFile`              | `-g`    | Auto      | Path to gradle file (Android only)                             |
| `--mandatory`               | `-m`    | `false`   | Force users to accept this update                              |
| `--noDuplicateReleaseError` | —       | `false`   | Warn instead of error on duplicate                             |
| `--plistFile`               | `-p`    | Auto      | Path to plist file (iOS only)                                  |
| `--plistFilePrefix`         | `--pre` | —         | Prefix for Info.plist file name (iOS only)                     |
| `--rollout`                 | `-r`    | `100%`    | Percentage of users to target                                  |
| `--sourcemapOutput`         | `-s`    | —         | Path to write the sourcemap                                    |
| `--targetBinaryVersion`     | `-t`    | Auto      | Semver expression for target binary version                    |
| `--outputDir`               | `-o`    | —         | Path to write bundle and sourcemap                             |
| `--useHermes`               | `-h`    | `false`   | Enable Hermes bytecode compilation                             |
| `--podFile`                 | `--pod` | —         | Path to CocoaPods config file (iOS only)                       |
| `--extraHermesFlags`        | `--hf`  | `[]`      | Additional flags for Hermes compiler                           |
| `--privateKeyPath`          | `-k`    | —         | Path to private key for code signing                           |
| `--xcodeProjectFile`        | `--xp`  | —         | Path to Xcode project file                                     |
| `--xcodeTargetName`         | `--xt`  | —         | Xcode target name (iOS only)                                   |
| `--buildConfigurationName`  | `-c`    | —         | Build configuration name, e.g. `Debug` or `Release` (iOS only) |

**Examples:**

```bash
release-react OrgName/MyApp ios
release-react OrgName/MyApp android -d Production
release-react OrgName/MyApp windows --dev
```

### promote

Promote the latest release from one deployment to another.

```bash
delivr promote <ownerName>/<appName> <sourceDeployment> <destDeployment> [options]
```

| Option                      | Alias  | Default   | Description                                 |
| --------------------------- | ------ | --------- | ------------------------------------------- |
| `--description`             | `-des` | Inherited | Updated description                         |
| `--label`                   | `-l`   | Latest    | Label of the source release to promote      |
| `--disabled`                | `-x`   | Inherited | Prevent immediate download                  |
| `--mandatory`               | `-m`   | Inherited | Force users to accept this update           |
| `--noDuplicateReleaseError` | —      | `false`   | Warn instead of error on duplicate          |
| `--rollout`                 | `-r`   | `100%`    | Percentage of users to target               |
| `--targetBinaryVersion`     | `-t`   | Inherited | Semver expression for target binary version |

**Examples:**

```bash
promote OrgName/MyApp Staging Production
promote OrgName/MyApp Staging Production --des "Production rollout" -r 25
```

### patch

Update the metadata for an existing release.

```bash
delivr patch <ownerName>/<appName> <deploymentName> [options]
```

| Option                  | Alias  | Default | Description                                    |
| ----------------------- | ------ | ------- | ---------------------------------------------- |
| `--label`               | `-l`   | Latest  | Label of the release to update                 |
| `--description`         | `-des` | —       | Updated description                            |
| `--disabled`            | `-x`   | —       | Whether the release should be downloadable     |
| `--mandatory`           | `-m`   | —       | Whether the release is mandatory               |
| `--rollout`             | `-r`   | —       | Updated rollout percentage (can only increase) |
| `--targetBinaryVersion` | `-t`   | —       | Updated target binary version                  |

**Examples:**

```bash
patch OrgName/MyApp Production --des "Updated description" -r 50%
patch OrgName/MyApp Production -l v3 --des "Updated description for v3"
```

### rollback

Rollback the latest release for an app deployment.

```bash
delivr rollback <ownerName>/<appName> <deploymentName> [--targetRelease <label>]
```

| Option            | Alias | Default  | Description                                      |
| ----------------- | ----- | -------- | ------------------------------------------------ |
| `--targetRelease` | `-r`  | Previous | Label of the release to roll back to (e.g. `v4`) |

**Examples:**

```bash
rollback MyApp Production
rollback MyApp Production --targetRelease v4
```

---

## Binary Patching

### create-patch

Create a binary diff (patch) between two bundle files.

```bash
delivr create-patch <oldFile> <newFile> <outputDirectory>
```

**Example:**

```bash
create-patch .old/index.android.bundle .new/index.android.bundle ./patch-dir
```

### apply-patch

Apply a previously created patch to a bundle file.

```bash
delivr apply-patch <oldFile> <patchFile> <outputFile>
```

**Example:**

```bash
apply-patch .old/index.android.bundle ./patch-dir/bundle.patch .new/index.android.bundle
```

---

## CI/CD Build Uploads

### upload-aab-build

Upload an AAB build artifact to Delivr release management.

```bash
delivr upload-aab-build <ciRunId> <artifactPath> --artifactVersion <version> --org <orgName> [--buildNumber <versionCode>]
```

| Option              | Alias | Required | Description                                             |
| ------------------- | ----- | -------- | ------------------------------------------------------- |
| `--artifactVersion` | —     | **Yes**  | Release version (e.g. `3.0.4`)                          |
| `--org`             | `-o`  | **Yes**  | Organisation name                                       |
| `--buildNumber`     | `-b`  | No       | Play Store versionCode (if CI already uploaded the AAB) |

**Supported files:** `.aab`

**Examples:**

```bash
upload-aab-build $BUILD_URL ./app-release.aab --artifactVersion "3.0.4" --org "MyOrg"
upload-aab-build $BUILD_URL ./app-release.aab --artifactVersion "3.0.4" --org "MyOrg" --buildNumber "12345"
```

### upload-regression-artifact

Upload APK or IPA regression build artifacts to Delivr.

```bash
delivr upload-regression-artifact <ciRunId> <artifactPath> --artifactVersion <version> --org <orgName>
```

| Option              | Alias | Required | Description                    |
| ------------------- | ----- | -------- | ------------------------------ |
| `--artifactVersion` | —     | **Yes**  | Release version (e.g. `3.0.4`) |
| `--org`             | `-o`  | **Yes**  | Organisation name              |

**Supported files:** `.apk`, `.ipa`

**Examples:**

```bash
upload-regression-artifact $BUILD_URL ./app-release.apk --artifactVersion "3.0.4" --org "MyOrg"
upload-regression-artifact $BUILD_URL ./MyApp.ipa --artifactVersion "3.0.4" --org "MyOrg"
```

### upload-testflight-build-number

Upload a TestFlight build number for iOS builds.

```bash
delivr upload-testflight-build-number <ciRunId> <testflightNumber> --artifactVersion <version> --org <orgName>
```

| Option              | Alias | Required | Description                    |
| ------------------- | ----- | -------- | ------------------------------ |
| `--artifactVersion` | —     | **Yes**  | Release version (e.g. `3.0.4`) |
| `--org`             | `-o`  | **Yes**  | Organisation name              |

**Example:**

```bash
upload-testflight-build-number $BUILD_URL 17965 --artifactVersion "3.0.4" --org "MyOrg"
```

---

## Debugging

### debug

View CodePush debug logs for a running app.

```bash
delivr debug <platform>
```

**Examples:**

```bash
debug android
debug ios
```

---

## Sessions

### session list

List current login sessions.

```bash
delivr session list [--format <format>]
```

| Option     | Default | Description                      |
| ---------- | ------- | -------------------------------- |
| `--format` | `table` | Output format: `table` or `json` |

**Aliases:** `session ls`

### session remove

Remove an existing login session.

```bash
delivr session remove <machineName>
```

**Aliases:** `session rm`

**Example:**

```bash
session remove "John's PC"
```
