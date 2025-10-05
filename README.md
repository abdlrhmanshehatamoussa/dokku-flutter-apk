# Dokku Flutter APK GitHub Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Dokku%20Flutter%20APK-blue.svg?logo=github)](https://github.com/abdlrhmanshehatamoussa/dokku-flutter-apk)

Easily deploy your Flutter Android APK as a static file to a [Dokku](https://dokku.com/) app using this GitHub Action.

---

## Features

- **Build & Deploy**: Uploads a Flutter APK to a Dokku server and serves it as a static file.
- **Metadata Extraction**: Reads APK metadata (applicationId, version code, version name) for deployment.
- **Customizable Paths**: Supports custom APK and metadata file paths.
- **Secure SSH**: Uses an SSH key for secure communication with your Dokku server.

---

## Usage

Add this action as a step in your workflow after building your Flutter APK:

```yaml
- name: Deploy APK to Dokku
  uses: abdlrhmanshehatamoussa/dokku-flutter-apk@main
  with:
    app-name: "your-dokku-app"
    dokku-host: "your.dokku.host"
    dokku-ssh-key: ${{ secrets.DOKKU_SSH_KEY }}
    # Optional:
    # apk-path: "build/app/outputs/apk/release/app-release.apk"
    # metadata-path: "build/app/outputs/apk/release/output-metadata.json"
```

### Inputs

| Name            | Required | Default                                              | Description                                      |
|-----------------|----------|------------------------------------------------------|--------------------------------------------------|
| `app-name`      | Yes      | -                                                    | The name of the Dokku app                        |
| `apk-path`      | No       | `build/app/outputs/apk/release/app-release.apk`      | Full path to the APK file                        |
| `metadata-path` | No       | `build/app/outputs/apk/release/output-metadata.json` | Full path to the APK metadata JSON file          |
| `dokku-host`    | Yes      | -                                                    | Hostname or IP address of your Dokku server      |
| `dokku-ssh-key` | Yes      | -                                                    | SSH private key with access to the Dokku server  |

**Note:** Make sure your SSH key is stored in GitHub Secrets and has access to push to your Dokku server.

---

## Example Workflow

```yaml
name: Build and Deploy APK

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: "3.10.0"

      - name: Build APK
        run: flutter build apk --release

      - name: Deploy APK to Dokku
        uses: abdlrhmanshehatamoussa/dokku-flutter-apk@main
        with:
          app-name: "your-dokku-app"
          dokku-host: "your.dokku.host"
          dokku-ssh-key: ${{ secrets.DOKKU_SSH_KEY }}
```

---

## Output

- The APK will be deployed to your Dokku app and served as a static file.
- The deployment directory will be named using your app package, version name, and version code.

---

## Requirements

- [jq](https://stedolan.github.io/jq/) must be available on the runner (already present on Ubuntu runners).
- Your Dokku app must be configured to serve static files.

---

## License

[MIT](LICENSE)

---

**Author**: [Abdelrahman Shehata](https://github.com/abdlrhmanshehatamoussa)
