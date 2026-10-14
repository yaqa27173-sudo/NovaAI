name: Build Nova APK

on:
  push:
    branches: [ main, master ]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: "17"

      - uses: android-actions/setup-android@v3

      - name: Build APK
        run: |
          chmod +x ./gradlew
          ./gradlew assembleRelease --no-daemon --stacktrace
          cp app/build/outputs/apk/release/app-release.apk Nova.apk

      - name: Upload APK (Actions artifact)
        uses: actions/upload-artifact@v4
        with:
          name: Nova-apk
          path: Nova.apk

      - name: Publish APK as a Release (easy phone download)
        uses: softprops/action-gh-release@v2
        with:
          tag_name: build-${{ github.run_number }}
          name: Nova build ${{ github.run_number }}
          body: Download **Nova.apk** below and open it on your phone to install.
          files: Nova.apk
          make_latest: true
