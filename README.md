# OpenWrt CI native testing for GitHub Actions

GitHub Actions doesn't allow direct inclusion of remote YAML files so we need following custom plugin which wraps the
[OpenWrt CI](https://gitlab.com/ynezz/openwrt-ci) for better reusability and sharing in GitHub Actions CI environment.

## Example usage

```yaml
name: OpenWrt CI testing

on: [ push, pull_request ]
env:
  CI_ENABLE_UNIT_TESTING: 1
  CI_TARGET_BUILD_DEPENDS: ubus
  CI_CMAKE_EXTRA_BUILD_ARGS: -DLUAPATH=/usr/lib/lua

jobs:
  native_testing:
    name: Various native checks
    runs-on: ubuntu-20.04

    steps:
      - uses: actions/checkout@v2

      - uses: ynezz/gh-actions-openwrt-ci-native@v0.0.1

      - name: Upload build artifacts
        uses: actions/upload-artifact@v2
        if: failure()
        with:
          name: native-build-artifacts
          if-no-files-found: ignore
          path: |
            build/scan
            tests/cram/**/*.t.err
```
