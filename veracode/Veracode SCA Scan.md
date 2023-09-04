
# Veracode SCA Scan

## Install SourceClear (Veracode AgentBased Scan)

```bash
# setup access token
export SRCCLR_API_TOKEN=<the token of your agent>

# download srcclr
SRCCLR_VERSION=$(curl https://download.srcclr.com/LATEST_VERSION)
curl -s -L https://download.sourceclear.com/srcclr-$SRCCLR_VERSION-linux.tgz | tar zxvf - -C ./
mv ./srcclr-$SRCCLR_VERSION ~/srcclr
export PATH=$PATH:~/srcclr/bin
```

## Trust no one with your source code

> SCA scan should only scan your BOM file, not your source or your build.

## Scan .NET projects

```bash
# run package restore to generate BOM file (obj/project.assets.json)
dotnet restore

# avoid package restore by srcclr if private feed in use
echo "skip_dotnet_restore: true" > srcclr.yml
srcclr scan ./ --recursive --scan-collectors "msbuilddotnet"
```

### Result

| Summary Report | | 
| :--- | ---: |
| Scan ID | 1b163f1a-b8cc-4811-adf3-a7f46d642401 | 
| Scan Date & Time | Sep 01 2023 12:30AM CST | 
| Account type | ENTERPRISE | 
| Scan engine | 3.8.36 (latest 3.8.38) | 
| Analysis time | 8 seconds | 
| User | eric | 
| Project | /mnt/c/git/dnv/SolutionPackage/src/Web | 
| Package Manager(s) | MSBuildDotNet | 
| | | 
| **Open-Source Libraries** | | 
| Total Libraries | 15 | 
| Direct Libraries | 3 | 
| Transitive Libraries | 12 | 
| Vulnerable Libraries | 0 | 
| | | 
| **Security** | | 
| With Vulnerable Methods | 0 | 
| Critical Risk Vulnerabilities | 0 | 
| High Risk Vulnerabilities | 0 | 
| Medium Risk Vulnerabilities | 0 | 
| Low Risk Vulnerabilities | 0 | 
| | | 
| **Licenses** | | 
| Unique Library Licenses | 4 | 
| Libraries Using GPL | 0 | 
| Libraries With High Risk License | 0 | 
| Libraries With Medium Risk License | 0 | 
| Libraries With Low Risk License | 15 | 
| Libraries With Multiple Licenses | 4 | 
| Libraries With Unassessable License | 7 | 
| Libraries With Unrecognizable License | 0 | 

| Issues |||||
| :--- | --- | --- | --- | --- |
| **Issue ID** | **Issue Type** | **Severity** | **Description** | **Library Name & Version In Use** |
| 207043360 | Outdated Library | 3.0 | Latest version at scan: 7.1.0-preview | Microsoft.IdentityModel.Tokens 6.30.0 |

## Scan NPM/Yarn project

```bash
# run package restore to generate BOM file (package-lock.json or yarn.ock)
yarn install

# avoid package restore by srcclr if private feed in use
echo 'skip_npm_install: true' > srcclr.yml

# collectors: npm, yarn
srcclr scan ./ --scan-collectors "yarn"
```

### Result

| Summary Report | | 
| :--- | ---: |
| Scan ID | 1b163f1a-b8cc-4811-adf3-a7f46d642401 | 
| Scan Date & Time | Sep 01 2023 12:30AM CST | 
| Account type | ENTERPRISE | 
| Scan engine | 3.8.36 (latest 3.8.38) | 
| Analysis time | 250 seconds | 
| User | eric | 
| Project | /mnt/c/git/dnv/SolutionPackage/samples/Web/ClientApp | 
| Package Manager(s) | Yarn| 
| | | 
| **Open-Source Libraries** | | 
| Total Libraries | 189 | 
| Direct Libraries | 26 | 
| Transitive Libraries | 165 | 
| Vulnerable Libraries | 2 | 
| Third Party Code | 99.9% | 
| | | 
| **Security** | | 
| With Vulnerable Methods | 0 | 
| Critical Risk Vulnerabilities | 1 | 
| High Risk Vulnerabilities | 0 | 
| Medium Risk Vulnerabilities | 1 | 
| Low Risk Vulnerabilities | 0 | 

| Vulnerabilities - Public Data ||||
| --- | --- | --- | --- |
| CVE-2021-44906 | Critical Risk | Prototype Pollution | minimist 1.2.5 |
| CVE-2022-0536 | Medium Risk | Information Disclosure | follow-redirects 1.14.7 |

| Licenses ||
| :--- | ---: |
| Unique Library Licenses | 8 |
| Libraries Using GPL | 0 |
| Libraries With High Risk License | 0 |
| Libraries With Medium Risk License | 0 |
| Libraries With Low Risk License | 190 |
| Libraries With Multiple Licenses | 2 |
| Libraries With Unassessable License | 2 |
| Libraries With Unrecognizable License | 1 |

| Issues |||||
| :--- | --- | --- | --- | --- |
| Issue ID | Issue Type | Severity | Description | Library Name & Version In Use |
| 207435786 | Vulnerability | 4.3 | CVE-2022-0536: Information Disclosure | follow-redirects 1.14.7 |
| 207435787 | Vulnerability | 7.5 | CVE-2021-44906: Prototype Pollution | minimist 1.2.5 |
| 207435788 | Outdated Library | 3.0 | Latest version at scan: 0.2.0 | @fortawesome/react-fontawesome 0.1.16 |
| 207435789 | Outdated Library | 3.0 | Latest version at scan: 17.0.1-nightly.2307-10 | @microsoft/applicationinsights-react-js 3.2.2 |
| 207435790 | Outdated Library | 3.0 | Latest version at scan: 3.0.3-nightly3.2308-06 | @microsoft/applicationinsights-web 2.7.2 |
| 207435791 | Outdated Library | 3.0 | Latest version at scan: 3.42.0 | apexcharts 3.36.0 |
| 207435792 | Outdated Library | 3.0 | Latest version at scan: 1.5.0 | axios 0.24.0 |
| 207435793 | Outdated Library | 3.0 | Latest version at scan: 2.3.2 | classnames 2.3.1 |
| 207435794 | Outdated Library | 3.0 | Latest version at scan: 7.0.0-alpha.0 | connected-react-router 6.9.2 |
| 207435795 | Outdated Library | 3.0 | Latest version at scan: 3.1.0 | d3-format 2.0.0 |
| 207435796 | Outdated Library | 3.0 | Latest version at scan: 5.3.0 | history 4.10.1 |
| 207435797 | Outdated Library | 3.0 | Latest version at scan: 3.0.0-beta.1 | mapbox-gl 2.11.1 |
| 207435798 | Outdated Library | 3.0 | Latest version at scan: 8.1.0 | query-string 7.1.3 |
| 207435799 | Outdated Library | 3.0 | Latest version at scan: 1.4.1 | react-apexcharts 1.4.0 |
| 207435800 | Outdated Library | 3.0 | Latest version at scan: 18.3.0-next-992911981-20220718 | react-dom 18.2.0 |
| 207435801 | Outdated Library | 3.0 | Latest version at scan: 7.1.5 | react-map-gl 6.1.21 |
| 207435802 | Outdated Library | 3.0 | Latest version at scan: 9.0.0-alpha.0 | react-redux 7.2.6 |
| 207435803 | Outdated Library | 3.0 | Latest version at scan: 6.15.0 | react-router-dom 5.3.0 |
| 207435804 | Outdated Library | 3.0 | Latest version at scan: 7.0.1 | react-swipeable 6.2.0 |
| 207435805 | Outdated Library | 3.0 | Latest version at scan: 1.0.20 | react-virtualized-auto-sizer 1.0.6 |
| 207435806 | Outdated Library | 3.0 | Latest version at scan: 10.0.0-alpha.17 | react-window 1.8.6 |
| 207435807 | Outdated Library | 3.0 | Latest version at scan: 18.3.0-next-992911981-20220718 | react 18.2.0 |
| 207435808 | Outdated Library | 3.0 | Latest version at scan: 1.2.3 | redux-saga 1.1.3 |
| 207435809 | Outdated Library | 3.0 | Latest version at scan: 5.0.0-beta.0 | redux 4.1.2 |
| 207435810 | Outdated Library | 3.0 | Latest version at scan: 5.0.0-alpha.2 | reselect 4.1.5 |



## Scan Dockerfile

```bash
srcclr scan ./ --scan-dockerfile
```

### Result

| Issue | Category | Section | Recommendation | Line Numbers |
| --- | --- | --- | --- | --- |
| Missing HEALTHCHECK instruction | CisDockerBenchmark | 4.6 | Ensure that HEALTHCHECK instruction has been added to container images | n/a |
| Found secrets in ENV | CisDockerBenchmark | 4.10 | Ensure secrets are not stored in Dockerfile | 8 |
| Missing USER instruction | CisDockerBenchmark | 4.1 | Ensure that a user for the container has been created | n/a |
| Missing --no-install-recommends in apt-get commands | Functional | 1.1 | Avoid installing additional packages by specifying --no-install-recommends in apt-get command | 36, 37 |
| Detect curl bashing | FilesAndResources | 1.1 | Avoid wget or curl commands with external URLs/repos and then piping into shell in RUN instruction | 7, 33, 34 |

## Scan Docker Image

```bash
srcclr scan --image mcr.microsoft.com/azure-functions/dotnet:4-slim
```

### Result

| **Summary Report** ||
|:--- | ---:|
| Scan ID | 2131b433-d9e3-4f9f-9c1b-a0d61fbbd9b8 |
| Scan Date & Time | Aug 31 2023 06:02PM CST |
| Account type | ENTERPRISE |
| Scan engine | 3.8.38 (latest 3.8.38) |
| Analysis time | 19 seconds |
| User | eric |
| Project | mcr.microsoft.com/azure-functions/dotnet:4-slim |
| Package Manager(s) | Container |
| | |
| **Open-Source Libraries** ||
| Total Libraries | 55 |
| Direct Libraries | 55 |
| Transitive Libraries | 0 |
| Vulnerable Libraries | 0 |
|||
| **Security** ||
| With Vulnerable Methods | 0 |
| Critical Risk Vulnerabilities | 0 |
| High Risk Vulnerabilities | 0 |
| Medium Risk Vulnerabilities | 0 |
| Low Risk Vulnerabilities | 0 |
| | |
| **Licenses** ||
| Unique Library Licenses | 13 |
| Libraries Using GPL | 27 |
| Libraries With High Risk License | 35 |
| Libraries With Medium Risk License | 1 |
| Libraries With Low Risk License | 34 |
| Libraries With Multiple Licenses | 19 |
| Libraries With Unassessable License | 0 |
| Libraries With Unrecognizable License | 24 |















