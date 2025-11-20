
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
```
Summary Report
Scan ID                                        fb46b614-e19a-4856-bd23-1e3c459fc911
Scan Date & Time                               Sep 04 2023 11:56AM CST
Account type                                   ENTERPRISE
Scan engine                                    3.8.36 (latest 3.8.38)
Analysis time                                  9 seconds
User                                           eric
Project                                        /SampleProject
Package Manager(s)                             MSBuildDotNet

Open-Source Libraries
Total Libraries                                255
Direct Libraries                               17
Transitive Libraries                           241
Vulnerable Libraries                           4

Security
With Vulnerable Methods                        0
Critical Risk Vulnerabilities                  0
High Risk Vulnerabilities                      4
Medium Risk Vulnerabilities                    0
Low Risk Vulnerabilities                       1

Vulnerabilities - Public Data
CVE-2019-0815                                  High Risk         Denial Of Service (DoS)     Microsoft.AspNetCore 2.2.0
CVE-2019-0548                                  High Risk         Denial Of Service (DoS)     Microsoft.AspNetCore.Server.IISIntegration 2.2.0
CVE-2022-21986                                 High Risk         Denial Of Service (DoS)     Microsoft.AspNetCore.Http.Features 2.2.0
CVE-2023-29331                                 High Risk         Denial Of Service (DoS)     System.Security.Cryptography.Pkcs 6.0.1

Vulnerabilities - Premium Data
NO-CVE                                         Low Risk          Spoofable Cookies           Microsoft.AspNetCore 2.2.0

Licenses
Unique Library Licenses                        5
Libraries Using GPL                            0
Libraries With High Risk License               0
Libraries With Medium Risk License             0
Libraries With Low Risk License                255
Libraries With Multiple Licenses               93
Libraries With Unassessable License            153
Libraries With Unrecognizable License          0

Issues
Issue ID     Issue Type          Severity    Description                                        Library Name & Version In Use
207437513    Vulnerability       4.3         CVE-2022-21986: Denial Of Service (DoS)            Microsoft.AspNetCore.Http.Features 2.2.0
207437514    Vulnerability       5.0         CVE-2019-0548: Denial Of Service (DoS)             Microsoft.AspNetCore.Server.IISIntegration 2.2.0
207437515    Vulnerability       5.0         CVE-2019-0815: Denial Of Service (DoS)             Microsoft.AspNetCore 2.2.0
207437516    Vulnerability       2.6         NO-CVE: Spoofable Cookies                          Microsoft.AspNetCore 2.2.0
207437517    Vulnerability       7.1         CVE-2023-29331: Denial Of Service (DoS)            System.Security.Cryptography.Pkcs 6.0.1
207437518    Outdated Library    3.0         Latest version at scan: 7.0.0                      AspNetCore.HealthChecks.AzureKeyVault 6.0.3
207437519    Outdated Library    3.0         Latest version at scan: 12.0.1                     AutoMapper.Extensions.Microsoft.DependencyInjection 12.0.0
207437520    Outdated Library    3.0         Latest version at scan: 12.0.1                     AutoMapper 12.0.0
207437521    Outdated Library    3.0         Latest version at scan: 1.10.0                     Azure.Identity 1.8.0
207437522    Outdated Library    3.0         Latest version at scan: 4.5.0                      Azure.Security.KeyVault.Secrets 4.4.0
207437523    Outdated Library    3.0         Latest version at scan: 2.22.0-beta3               Microsoft.ApplicationInsights.AspNetCore 2.21.0
207437524    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.9    Microsoft.AspNetCore.Authentication.OpenIdConnect 6.0.11
207437525    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.9    Microsoft.AspNetCore.SpaServices.Extensions 6.0.11
207437526    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.6    Microsoft.Extensions.Http 6.0.0
207437527    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.6    System.Security.Cryptography.Xml 6.0.1
207437528    Outdated Library    3.0         Latest version at scan: 3.5.2                      Veracity.Common.Authentication.AspNetCore 3.0.1
207437529    Outdated Library    3.0         Latest version at scan: 3.5.2                      Veracity.Common.OAuth.AspNetCore 3.0.1


Full Report Details                            https://sca.analysiscenter.veracode.com/teams/X33hWoRb/scans/54543262
```

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

```
Veracode SCA agent scanning engine ready
Running the Yarn scanner
Scanning completed
Found 38 lines of code
Processing results...
Processing results complete

Summary Report
Scan ID                                        fe053d13-030d-4643-8601-f929ed4e8171
Scan Date & Time                               Sep 04 2023 11:30AM CST
Account type                                   ENTERPRISE
Scan engine                                    3.8.36 (latest 3.8.38)
Analysis time                                  137 seconds
User                                           eric
Project                                        /SampleProject/ClientApp
Package Manager(s)                             Yarn

Open-Source Libraries
Total Libraries                                189
Direct Libraries                               26
Transitive Libraries                           165
Vulnerable Libraries                           2
Third Party Code                               99.9%

Security
With Vulnerable Methods                        0
Critical Risk Vulnerabilities                  1
High Risk Vulnerabilities                      0
Medium Risk Vulnerabilities                    1
Low Risk Vulnerabilities                       0

Vulnerabilities - Public Data
CVE-2021-44906                                 Critical Risk     Prototype Pollution        minimist 1.2.5
CVE-2022-0536                                  Medium Risk       Information Disclosure     follow-redirects 1.14.7

Licenses
Unique Library Licenses                        8
Libraries Using GPL                            0
Libraries With High Risk License               0
Libraries With Medium Risk License             0
Libraries With Low Risk License                190
Libraries With Multiple Licenses               2
Libraries With Unassessable License            2
Libraries With Unrecognizable License          1

Issues
Issue ID     Issue Type          Severity    Description                                               Library Name & Version In Use
207435786    Vulnerability       4.3         CVE-2022-0536: Information Disclosure                     follow-redirects 1.14.7
207435787    Vulnerability       7.5         CVE-2021-44906: Prototype Pollution                       minimist 1.2.5
207435788    Outdated Library    3.0         Latest version at scan: 0.2.0                             @fortawesome/react-fontawesome 0.1.16
207435789    Outdated Library    3.0         Latest version at scan: 17.0.1-nightly.2307-10            @microsoft/applicationinsights-react-js 3.2.2
207435790    Outdated Library    3.0         Latest version at scan: 3.0.3-nightly3.2308-06            @microsoft/applicationinsights-web 2.7.2
207435791    Outdated Library    3.0         Latest version at scan: 3.42.0                            apexcharts 3.36.0
207435792    Outdated Library    3.0         Latest version at scan: 1.5.0                             axios 0.24.0
207435793    Outdated Library    3.0         Latest version at scan: 2.3.2                             classnames 2.3.1
207435794    Outdated Library    3.0         Latest version at scan: 7.0.0-alpha.0                     connected-react-router 6.9.2
207435795    Outdated Library    3.0         Latest version at scan: 3.1.0                             d3-format 2.0.0
207435796    Outdated Library    3.0         Latest version at scan: 5.3.0                             history 4.10.1
207435797    Outdated Library    3.0         Latest version at scan: 3.0.0-beta.1                      mapbox-gl 2.11.1
207435798    Outdated Library    3.0         Latest version at scan: 8.1.0                             query-string 7.1.3
207435799    Outdated Library    3.0         Latest version at scan: 1.4.1                             react-apexcharts 1.4.0
207435800    Outdated Library    3.0         Latest version at scan: 18.3.0-next-992911981-20220718    react-dom 18.2.0
207435801    Outdated Library    3.0         Latest version at scan: 7.1.5                             react-map-gl 6.1.21
207435802    Outdated Library    3.0         Latest version at scan: 9.0.0-alpha.0                     react-redux 7.2.6
207435803    Outdated Library    3.0         Latest version at scan: 6.15.0                            react-router-dom 5.3.0
207435804    Outdated Library    3.0         Latest version at scan: 7.0.1                             react-swipeable 6.2.0
207435805    Outdated Library    3.0         Latest version at scan: 1.0.20                            react-virtualized-auto-sizer 1.0.6
207435806    Outdated Library    3.0         Latest version at scan: 10.0.0-alpha.17                   react-window 1.8.6
207435807    Outdated Library    3.0         Latest version at scan: 18.3.0-next-992911981-20220718    react 18.2.0
207435808    Outdated Library    3.0         Latest version at scan: 1.2.3                             redux-saga 1.1.3
207435809    Outdated Library    3.0         Latest version at scan: 5.0.0-beta.0                      redux 4.1.2
207435810    Outdated Library    3.0         Latest version at scan: 5.0.0-alpha.2                     reselect 4.1.5


Full Report Details                            https://sca.analysiscenter.veracode.com/teams/X33hWoRb/scans/54542954
```

## Scan Dockerfile

```bash
srcclr scan ./ --scan-dockerfile
```

### Result

```
Summary Report
Scan ID                                        d6af61b7-2211-45e7-85d0-c091ccacf492
Scan Date & Time                               Sep 04 2023 12:07PM CST
Account type                                   ENTERPRISE
Scan engine                                    3.8.36 (latest 3.8.38)
Analysis time                                  646 seconds
User                                           eric
Project                                        /SampleProject
Package Manager(s)                             MSBuildDotNet

Open-Source Libraries
Total Libraries                                182
Direct Libraries                               21
Transitive Libraries                           164
Vulnerable Libraries                           0
Third Party Code                               99.8%

Security
With Vulnerable Methods                        0
Critical Risk Vulnerabilities                  0
High Risk Vulnerabilities                      0
Medium Risk Vulnerabilities                    0
Low Risk Vulnerabilities                       0

Licenses
Unique Library Licenses                        7
Libraries Using GPL                            0
Libraries With High Risk License               0
Libraries With Medium Risk License             1
Libraries With Low Risk License                169
Libraries With Multiple Licenses               81
Libraries With Unassessable License            135
Libraries With Unrecognizable License          12

Issues
Issue ID     Issue Type          Severity    Description                                        Library Name & Version In Use
207438996    Outdated Library    3.0         Latest version at scan: 1.10.0                     Azure.Identity 1.9.0
207438997    Outdated Library    3.0         Latest version at scan: 1.7.0                      DNV.OAuth.Web.Extensions 1.4.1
207438998    Outdated Library    3.0         Latest version at scan: 2.22.0-beta3               Microsoft.ApplicationInsights.AspNetCore 2.21.0
207438999    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.9    Microsoft.AspNetCore.DataProtection.StackExchangeRedis 6.0.8
207439000    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.9    Microsoft.AspNetCore.SpaProxy 6.0.3
207439001    Outdated Library    3.0         Latest version at scan: 7.0.0-preview              Microsoft.Azure.AppConfiguration.AspNetCore 6.0.1
207439002    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.9    Microsoft.Extensions.Caching.StackExchangeRedis 6.0.4
207439003    Outdated Library    3.0         Latest version at scan: 7.0.0-preview              Microsoft.Extensions.Configuration.AzureAppConfiguration 6.0.1
207439004    Outdated Library    3.0         Latest version at scan: 1.19.5                     Microsoft.VisualStudio.Azure.Containers.Tools.Targets 1.15.1
207439005    Outdated Library    3.0         Latest version at scan: 13.0.3                     Newtonsoft.Json 13.0.1
207439006    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.6.23329.7    System.Diagnostics.PerformanceCounter 6.0.1
207439007    Outdated Library    3.0         Latest version at scan: 8.0.0-preview.7.23375.6    System.Security.Cryptography.Pkcs 6.0.4


Container Images and Build File
Issue                                                        Category                 Section   Recommendation
                               Line Numbers
Missing HEALTHCHECK instruction                              CisDockerBenchmark       4.6       Ensure that HEALTHCHECK instruction has been added to container images                                 n/a
Found secrets in ENV                                         CisDockerBenchmark       4.10      Ensure secrets are not stored in Dockerfile
                               8
Missing USER instruction                                     CisDockerBenchmark       4.1       Ensure that a user for the container has been created
                               n/a
Missing --no-install-recommends in apt-get commands          Functional               1.1       Avoid installing additional packages by specifying --no-install-recommends in apt-get command          36, 37
Detect curl bashing                                          FilesAndResources        1.1       Avoid wget or curl commands with external URLs/repos and then piping into shell in RUN instruction     7, 33, 34

Configuration scan results are based on the CIS Benchmarks. Learn more: https://www.cisecurity.org/cis-benchmarks/


Full Report Details                            https://sca.analysiscenter.veracode.com/teams/X33hWoRb/scans/54544009
```

## Scan Docker Image

```bash
srcclr scan --image mcr.microsoft.com/azure-functions/dotnet:4-slim
```

### Result

```
Summary Report
Scan ID                                        e5af3045-ab42-45f4-9bd1-043bc36e87a6
Scan Date & Time                               Sep 04 2023 12:27PM CST
Account type                                   ENTERPRISE
Scan engine                                    3.8.36 (latest 3.8.38)
Analysis time                                  19 seconds
User                                           eric
Project                                        someimage:tag
Package Manager(s)                             Container

Open-Source Libraries
Total Libraries                                192
Direct Libraries                               160
Transitive Libraries                           32
Vulnerable Libraries                           4

Security
With Vulnerable Methods                        0
Critical Risk Vulnerabilities                  1
High Risk Vulnerabilities                      1
Medium Risk Vulnerabilities                    3
Low Risk Vulnerabilities                       0

Vulnerabilities - Public Data
CVE-2023-37920                                 Critical Risk     Authorization Bypass                               certifi 2022.12.7
CVE-2023-38325                                 High Risk         Improper Certificate Validation                    cryptography 40.0.1
CVE-2022-40896                                 Medium Risk       Regular Expression Denial Of Service (ReDoS)       Pygments 2.14.0
CVE-2023-32681                                 Medium Risk       Unintended Leaks Of Proxy-Authorization Header     requests 2.28.2

Vulnerabilities - Premium Data
NO-CVE                                         Medium Risk       Dependency On Vulnerable Third-Party Component     cryptography 40.0.1

Licenses
Unique Library Licenses                        18
Libraries Using GPL                            14
Libraries With High Risk License               23
Libraries With Medium Risk License             3
Libraries With Low Risk License                170
Libraries With Multiple Licenses               11
Libraries With Unassessable License            5
Libraries With Unrecognizable License          5

Issues
Issue ID     Issue Type          Severity    Description                                                       Library Name & Version In Use
207438172    Vulnerability       9.3         CVE-2023-37920: Authorization Bypass                              certifi 2022.12.7
207438173    Vulnerability       6.4         CVE-2023-38325: Improper Certificate Validation                   cryptography 40.0.1
207438174    Vulnerability       5.0         NO-CVE: Dependency On Vulnerable Third-Party Component            cryptography 40.0.1
207438175    Vulnerability       5.0         CVE-2022-40896: Regular Expression Denial Of Service (ReDoS)      Pygments 2.14.0
207438176    Vulnerability       5.4         CVE-2023-32681: Unintended Leaks Of Proxy-Authorization Header    requests 2.28.2
207438177    Outdated Library    3.0         Latest version at scan: 1.46.6-r0                                 libcom_err:3.17 1.46.5-r4
207438178    Outdated Library    3.0         Latest version at scan: 4.13.0                                    antlr4-python3-runtime 4.9.3
207438179    Outdated Library    3.0         Latest version at scan: 3.1.1                                     argcomplete 2.1.2
207438180    Outdated Library    3.0         Latest version at scan: 1.5.0b1                                   azure-appconfiguration 1.1.1
207438181    Outdated Library    3.0         Latest version at scan: 14.0.0                                    azure-batch 13.0.0
207438182    Outdated Library    3.0         Latest version at scan: 2.51.0                                    azure-cli 2.47.0
207438183    Outdated Library    3.0         Latest version at scan: 2.51.0                                    azure-cli-core 2.47.0
207438184    Outdated Library    3.0         Latest version at scan: 1.1.0                                     azure-cli-telemetry 1.0.8
207439285    Outdated Library    3.0         Latest version at scan: 1.29.3                                    azure-core 1.26.3
207439286    Outdated Library    3.0         Latest version at scan: 4.5.0                                     azure-cosmos 3.2.0
207439287    Outdated Library    3.0         Latest version at scan: 12.4.3                                    azure-data-tables 12.4.0
207439288    Outdated Library    3.0         Latest version at scan: 0.0.53                                    azure-datalake-store 0.0.52
207439289    Outdated Library    3.0         Latest version at scan: 0.61.1                                    azure-graphrbac 0.60.0
207439290    Outdated Library    3.0         Latest version at scan: 4.2.0                                     azure-keyvault 1.1.0
207439291    Outdated Library    3.0         Latest version at scan: 4.4.0b1                                   azure-keyvault-administration 4.0.0b3
207439292    Outdated Library    3.0         Latest version at scan: 4.9.0b1                                   azure-keyvault-keys 4.8.0b2
207439293    Outdated Library    3.0         Latest version at scan: 10.0.0b1                                  azure-mgmt-advisor 9.0.0
207439294    Outdated Library    3.0         Latest version at scan: 4.0.0                                     azure-mgmt-apimanagement 3.0.0
207439295    Outdated Library    3.0         Latest version at scan: 3.0.0b1                                   azure-mgmt-appcontainers 2.0.0
207439296    Outdated Library    3.0         Latest version at scan: 4.0.0                                     azure-mgmt-applicationinsights 1.0.0
207439297    Outdated Library    3.0         Latest version at scan: 3.1.0b1                                   azure-mgmt-authorization 3.0.0
207439298    Outdated Library    3.0         Latest version at scan: 17.1.0                                    azure-mgmt-batch 17.0.0
207439299    Outdated Library    3.0         Latest version at scan: 6.1.0b1                                   azure-mgmt-billing 6.0.0
207439300    Outdated Library    3.0         Latest version at scan: 12.1.0b1                                  azure-mgmt-cdn 12.0.0
207439301    Outdated Library    3.0         Latest version at scan: 13.5.0                                    azure-mgmt-cognitiveservices 13.3.0
207439302    Outdated Library    3.0         Latest version at scan: 30.1.0                                    azure-mgmt-compute 29.1.0
207439303    Outdated Library    3.0         Latest version at scan: 11.0.0b1                                  azure-mgmt-consumption 2.0.0
207439304    Outdated Library    3.0         Latest version at scan: 25.0.0                                    azure-mgmt-containerservice 22.0.0
207439305    Outdated Library    3.0         Latest version at scan: 1.4.0                                     azure-mgmt-core 1.3.2
207439306    Outdated Library    3.0         Latest version at scan: 10.0.0b1                                  azure-mgmt-cosmosdb 9.0.0
207439307    Outdated Library    3.0         Latest version at scan: 2.0.0b1                                   azure-mgmt-databoxedge 1.0.0
207439308    Outdated Library    3.0         Latest version at scan: 1.0.0b2                                   azure-mgmt-datalake-analytics 0.2.1
207439309    Outdated Library    3.0         Latest version at scan: 1.1.0b1                                   azure-mgmt-datalake-store 0.5.0
207439310    Outdated Library    3.0         Latest version at scan: 10.1.0b1                                  azure-mgmt-datamigration 10.0.0
207439311    Outdated Library    3.0         Latest version at scan: 10.0.0b1                                  azure-mgmt-devtestlabs 4.0.0
207439312    Outdated Library    3.0         Latest version at scan: 8.1.0                                     azure-mgmt-dns 8.0.0
207439313    Outdated Library    3.0         Latest version at scan: 10.3.0b1                                  azure-mgmt-eventgrid 10.2.0b2
207439314    Outdated Library    3.0         Latest version at scan: 11.0.0                                    azure-mgmt-eventhub 10.1.0
207439315    Outdated Library    3.0         Latest version at scan: 1.2.0b1                                   azure-mgmt-extendedlocation 1.0.0b2
207439316    Outdated Library    3.0         Latest version at scan: 1.2.0                                     azure-mgmt-imagebuilder 1.1.0
207439317    Outdated Library    3.0         Latest version at scan: 2.4.0                                     azure-mgmt-iothub 2.3.0
207439318    Outdated Library    3.0         Latest version at scan: 1.2.0b2                                   azure-mgmt-iothubprovisioningservices 1.1.0
207439319    Outdated Library    3.0         Latest version at scan: 10.2.3                                    azure-mgmt-keyvault 10.2.0
207439320    Outdated Library    3.0         Latest version at scan: 3.2.0                                     azure-mgmt-kusto 0.3.0
207439321    Outdated Library    3.0         Latest version at scan: 13.0.0b6                                  azure-mgmt-loganalytics 13.0.0b4
207439322    Outdated Library    3.0         Latest version at scan: 7.0.0b1                                   azure-mgmt-managedservices 1.0.0
207439323    Outdated Library    3.0         Latest version at scan: 1.1.0b1                                   azure-mgmt-managementgroups 1.0.0
207439324    Outdated Library    3.0         Latest version at scan: 2.1.0b1                                   azure-mgmt-maps 2.0.0
207439325    Outdated Library    3.0         Latest version at scan: 1.2.0b1                                   azure-mgmt-marketplaceordering 1.1.0
207439326    Outdated Library    3.0         Latest version at scan: 10.2.0                                    azure-mgmt-media 9.0.0
207439327    Outdated Library    3.0         Latest version at scan: 6.0.2                                     azure-mgmt-monitor 5.0.1
207439328    Outdated Library    3.0         Latest version at scan: 7.1.0b1                                   azure-mgmt-msi 7.0.0
207439329    Outdated Library    3.0         Latest version at scan: 10.1.0                                    azure-mgmt-netapp 9.0.1
207439330    Outdated Library    3.0         Latest version at scan: 1.1.0                                     azure-mgmt-privatedns 1.0.0
207439331    Outdated Library    3.0         Latest version at scan: 2.4.0                                     azure-mgmt-recoveryservices 2.2.0
207439332    Outdated Library    3.0         Latest version at scan: 6.0.0                                     azure-mgmt-recoveryservicesbackup 5.1.0
207439333    Outdated Library    3.0         Latest version at scan: 1.3.0b1                                   azure-mgmt-redhatopenshift 1.2.0
207439334    Outdated Library    3.0         Latest version at scan: 14.2.0                                    azure-mgmt-redis 14.1.0
207439335    Outdated Library    3.0         Latest version at scan: 2.0.0b1                                   azure-mgmt-relay 0.1.0
207439336    Outdated Library    3.0         Latest version at scan: 23.1.0b2                                  azure-mgmt-resource 22.0.0
207439337    Outdated Library    3.0         Latest version at scan: 5.0.0                                     azure-mgmt-security 3.0.0
207439338    Outdated Library    3.0         Latest version at scan: 2.1.0b1                                   azure-mgmt-servicefabric 1.0.0
207439339    Outdated Library    3.0         Latest version at scan: 2.0.0b4                                   azure-mgmt-servicefabricmanagedclusters 1.0.0
207439340    Outdated Library    3.0         Latest version at scan: 1.2.0                                     azure-mgmt-signalr 1.1.0
207439341    Outdated Library    3.0         Latest version at scan: 4.0.0b11                                  azure-mgmt-sql 4.0.0b8
207439342    Outdated Library    3.0         Latest version at scan: 1.0.0b6                                   azure-mgmt-sqlvirtualmachine 1.0.0b5
207439343    Outdated Library    3.0         Latest version at scan: 21.1.0b1                                  azure-mgmt-storage 21.0.0
207439344    Outdated Library    3.0         Latest version at scan: 1.1.0                                     azure-mgmt-trafficmanager 1.0.0
207439345    Outdated Library    3.0         Latest version at scan: 7.1.0                                     azure-mgmt-web 7.0.0
207439346    Outdated Library    3.0         Latest version at scan: 1.2.0                                     azure-multiapi-storage 1.0.0
207439347    Outdated Library    3.0         Latest version at scan: 2.1.0                                     azure-storage-common 1.4.2
207439348    Outdated Library    3.0         Latest version at scan: 0.7.0                                     azure-synapse-accesscontrol 0.5.0
207439349    Outdated Library    3.0         Latest version at scan: 0.16.0                                    azure-synapse-artifacts 0.15.0
207439350    Outdated Library    3.0         Latest version at scan: 0.7.0                                     azure-synapse-spark 0.2.0
207439351    Outdated Library    3.0         Latest version at scan: 5.2.0                                     chardet 3.0.4
207439352    Outdated Library    3.0         Latest version at scan: 3.2.0                                     charset-normalizer 3.1.0
207439353    Outdated Library    3.0         Latest version at scan: 41.0.3                                    cryptography 40.0.1
207439354    Outdated Library    3.0         Latest version at scan: 1.2.14                                    Deprecated 1.2.13
207439355    Outdated Library    3.0         Latest version at scan: 3.2.2                                     fabric 2.7.1
207439356    Outdated Library    3.0         Latest version at scan: 2.2.0                                     invoke 1.7.3
207439357    Outdated Library    3.0         Latest version at scan: 0.8.1                                     javaproperties 0.5.2
207439358    Outdated Library    3.0         Latest version at scan: 0.11.0                                    knack 0.10.1
207439359    Outdated Library    3.0         Latest version at scan: 1.24.0b1                                  msal 1.20.0
207439360    Outdated Library    3.0         Latest version at scan: 23.1                                      packaging 21.3
207439361    Outdated Library    3.0         Latest version at scan: 3.3.1                                     paramiko 3.1.0
207439362    Outdated Library    3.0         Latest version at scan: 23.2.1                                    pip 22.3.1
207439363    Outdated Library    3.0         Latest version at scan: 5.9.5                                     psutil 5.9.4
207439364    Outdated Library    3.0         Latest version at scan: 2.0.1rc0                                  PyGithub 1.58.1
207439365    Outdated Library    3.0         Latest version at scan: 2.16.1                                    Pygments 2.14.0
207439366    Outdated Library    3.0         Latest version at scan: 2.8.0                                     PyJWT 2.6.0
207439367    Outdated Library    3.0         Latest version at scan: 23.2.0                                    pyOpenSSL 23.1.1
207439368    Outdated Library    3.0         Latest version at scan: 3.1.1                                     pyparsing 3.0.9
207439369    Outdated Library    3.0         Latest version at scan: 6.0.1                                     PyYAML 6.0
207439370    Outdated Library    3.0         Latest version at scan: 2.31.0                                    requests 2.28.2
207439371    Outdated Library    3.0         Latest version at scan: 1.3.4                                     retrying 1.3.3
207439372    Outdated Library    3.0         Latest version at scan: 0.14.5                                    scp 0.13.6
207439373    Outdated Library    3.0         Latest version at scan: 3.0.1                                     semver 2.13.0
207439374    Outdated Library    3.0         Latest version at scan: 68.1.2                                    setuptools 65.6.0
207439375    Outdated Library    3.0         Latest version at scan: 0.4.0                                     sshtunnel 0.1.5
207439376    Outdated Library    3.0         Latest version at scan: 4.7.1                                     typing-extensions 4.5.0
207439377    Outdated Library    3.0         Latest version at scan: 2.0.4                                     urllib3 1.26.15
207439378    Outdated Library    3.0         Latest version at scan: 1.6.2                                     websocket-client 1.3.3
207439379    License             9.0         Library has High-Risk License                                     alpine-baselayout:3.17 3.4.0-r0
207439380    License             9.0         Library has High-Risk License                                     apk-tools:3.17 2.12.10-r1
207439381    License             9.0         Library has High-Risk License                                     libcom_err:3.17 1.46.5-r4
207439382    License             9.0         Library has High-Risk License                                     libcom_err:3.17 1.46.5-r4
207439383    License             9.0         Library has High-Risk License                                     libintl:3.17 0.21.1-r1
207439384    License             9.0         Library has High-Risk License                                     ssl_client:3.17 1.35.0-r29
207439385    License             9.0         Library has High-Risk License                                     chardet 3.0.4
207439386    License             9.0         Library has High-Risk License                                     paramiko 3.1.0
207439387    License             9.0         Library has High-Risk License                                     scp 0.13.6


Full Report Details                            https://sca.analysiscenter.veracode.com/teams/X33hWoRb/scans/54544181
```















