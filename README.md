# esphome-action
Workflow action to run ESPHome builds

Sample usage:

```yaml
name: ci
on:
  push:
    branches: [main]
env:
  ESPHOME_VERSION: "2024.12.4"
  PYTHON_VERSION: "3.12"
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v4
      - name: Generate fake secrets.yaml
        run: |
          touch secrets.yaml
          echo "wifi_ssid: 'fake_ssid'" >> secrets.yaml
          echo "wifi_password: '12345678'" >> secrets.yaml
          echo "wifi_ap_password: '12345678'" >> secrets.yaml
      - name: Build project
        uses: dentra/esphome-action@main
        id: esphome
        with:
          version: ${{ env.ESPHOME_VERSION }}
          python-version: ${{ env.PYTHON_VERSION }}
          config: device-configuration.yaml
          substitutions: '{"var1": "1", "var2": "2"}'
      - name: Show firmware artifacts
        run: |
          ls -l ${{ steps.esphome.outputs.firmware-path }}/firmware.*
```