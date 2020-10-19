# Install PHIVE in GitHub Actions

The Phar Installation and Verification Environment :)

### Usage

```yaml
jobs:
  phive:
    runs-on: ubuntu-latest
    steps:
      - name: "Install PHIVE"
        uses: "szepeviktor/phive@v1"
```

Full example.

```yaml
jobs:
  phive:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        php-version:
          - "7.4"
          - "7.3"
    steps:
      - name: "Set default PHP version"
        run: "sudo update-alternatives --set php /usr/bin/php${{ matrix.php-version }}"
      - name: "Checkout code"
        uses: "actions/checkout@v2"
      - name: "Cache tools installed with PHIVE"
        uses: "actions/cache@v2.1.2"
        with:
          path: "${{ runner.temp }}/.phive"
          key: "php-${{ matrix.php-version }}-phive-${{ hashFiles('.phive/phars.xml') }}"
          restore-keys: "php-${{ matrix.php-version }}-phive-"

      - name: "Install PHIVE"
        uses: "szepeviktor/phive@v1"
        with:
          home: "${{ runner.temp }}/.phive"
          binPath: "${{ github.workspace }}/tools/phive"

      - name: "Install PHP tools with PHIVE"
        uses: "szepeviktor/phive-install@v1"
        with:
          home: "${{ runner.temp }}/.phive"
          binPath: "${{ github.workspace }}/tools/phive"
          trustGpgKeys: "4AA394086372C20A,CF1A108D0E7AE720,E82B2FB314E9906E"
```
