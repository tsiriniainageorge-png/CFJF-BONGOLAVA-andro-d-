YAML

name: CFJF BONGOLAVA APK

on:
  workflow_dispatch:
  push:
    paths:
      - 'CFJF_BONGOLAVA_APK_Project.zip'
      - '.github/workflows/build-apk.yml'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Get project
        uses: actions/checkout@v4

      - name: Install Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Install Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Extract Android project
        run: unzip -o CFJF_BONGOLAVA_APK_Project.zip

      - name: Build APK
        run: gradle -p CFJF_BONGOLAVA_APK_Project assembleDebug

      - name: Save APK
        uses: actions/upload-artifact@v4
        with:
          name: CFJF-BONGOLAVA-APK
          path: CFJF_BONGOLAVA_APK_Project/app/build/outputs/apk/debug/*.apk
