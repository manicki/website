# Configuration


You can use JSON, YAML or INI files to configure PhpMetrics, with the `--config=<file> option.

Full example a configuration file:

```yaml
---
composer: false  # Analyse the composer.* files
includes:  #  directory and files to analyze, relative to config file directory
    - "src"
excludes: # regex of files (or directory) to exclude from analyze
    - tests
extensions: # default: ["php", "inc"]
    - php
    - php8
report: # list of reports to generate
    html: "/tmp/report/"
    csv: "/tmp/report.csv"
    json: "/tmp/report.json"
    violations: "/tmp/violations.xml"

# "layers" of code. You can group your classes and packages by regex,
# to visualise specific HTML report for each of them
groups:                 
    - name: Component   
      match: "!component!i" # regex to match the group
    - name: Hexagon
      match: "!hexagon!i"
plugins:
    git:
        binary: git # if defined, runs git analyze
    junit:
        file: "/tmp/junit.xml" # if defined, analyze junit report
        
# You can define patterns of code to search
# That's useful for Continuous Integration
searches:
    Repository which uses Service: # You can name your search as you want
        type: class
        instanceOf:
            - App\MyRepository
        nameMatches: ".*Repository.*"
        usesClasses:
            - ".*Service"
        failIfFound: true # stop execution if pattern of code is found
    Class with too complex code:
        type: class
        ccn: ">=10"
        failIfFound: true
    Class with too many responsabilitites:
        type: class
        lcom: ">=3"
        failIfFound: true
    Controller which use doctrine:
        type: class
        nameMatches: ".*Controller.*"
        usesClasses:
            - ".*Connection.*"
        failIfFound: true
```