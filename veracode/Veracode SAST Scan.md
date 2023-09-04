# Veracode SAST Scan

## Install Veracode Java API Wrapper

```bash
# setup credential
export VERACODE_API_KEY_ID=<your api id>
export VERACODE_API_KEY_SECRET=<your api key>

# donwload latest wrapper
VERSION_URL=https://repo1.maven.org/maven2/com/veracode/vosp/api/wrappers/vosp-api-wrappers-java/maven-metadata.xml
VERACODE_VERSION=$(curl $VERSION_URL| grep latest |  cut -d '>' -f 2 | cut -d '<' -f 1)
WRAPPER_URL="https://repo1.maven.org/maven2/com/veracode/vosp/api/wrappers/vosp-api-wrappers-java/$VERACODE_VERSION/vosp-api-wrappers-java-$VERACODE_VERSION.jar"
curl -sS -o VeracodeJavaAPI.jar $WRAPPER_URL

# create an alias to call VeracodeJavaAPI.jar easily
mv VeracodeJavaAPI.jar ~/
alias v-scan='java -jar ~/VeracodeJavaAPI.jar'
```

## Upload Scan

### File Upload Preparation

```bash
# build and copy 'Just My Code' to a folder that being upload
mkdir -p veracode/dll
dotnet build
find src -name 'DNV*.dll' -exec cp {} veracode/dll \;
find src -name 'DNV*.pdb' -exec cp {} veracode/dll \;

# copy JS/TS source code to a folder that being upload
mkdir -p veracode/js
find src -name "*.js" -exec cp --parents {} veracode/js \;
find src -name "*.jsx" -exec cp --parents {} veracode/js \;
find src -name "*.ts" -exec cp --parents {} veracode/js \;
find src -name "*.tsx" -exec cp --parents {} veracode/js \;

# pack all the file that being upload
tar czvf veracode.tar.gz veracode/
```

### Policy Scan

```bash
# upload to Veracode (remove -scantimeout if you want to wait for result)
v-scan -action UploadAndScan \
  -appname <app_name> \
  -version <version> \
  -filepath veracode.tar.gz \
  -createprofile false
  -scantimeout 30
```

### Sanbox Scan

```bash
# create sanbox
v-scan -action CreateSandbox -appid <app_id> -sandboxname <box_name>

# run sanbox scan
v-scan -action UploadAndScan \
  -appname <app_name> \
  -version <version> \
  -filepath veracode.tar.gz \
  -createprofile false \
  -sandboxname <box_name>
```

## Pipeline Scan

```bash
### donwload latest scanner
SCANNER_URL=https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip
curl -sSO $SCANNER_URL
unzip pipeline-scan-LATEST.zip && rm pipeline-scan-LATEST.zip

# create an alias to call pipeline-scan.jar easily
mv pipeline-scan.jar ~/
alias p-scan='java -jar ~/pipeline-scan.jar'

# run pipeline scan
p-scan -f veracode.tar.gz
```

## Veracode JavaAPI Wrapper Parameters

```bash
Usage:
  -action                                <Required> Must be one of the
                                         following: AllDetailedReports, Archer,
                                         BeginPreScan, BeginScan,
                                         CreateAndSubmitDynamicRescan,
                                         CreateApp, CreateBuild, CreateSandbox,
                                         CreateTeam, CreateUser, DeleteApp,
                                         DeleteBuild, DeleteSandbox,
                                         DeleteTeam, DeleteUser,
                                         DetailedReport, DownloadArcherReport,
                                         DownloadFlawReport,
                                         GenerateArcherReport,
                                         GenerateFlawReport, GetAppBuilds,
                                         GetAppInfo, GetAppList,
                                         GetApplications, GetBuildInfo,
                                         GetBuildList, GetCallStacks,
                                         GetCurriculumList, GetFileList,
                                         GetMitigationInfo, GetPolicyList,
                                         GetPreScanResults, GetRegion,
                                         GetSandboxList, GetSharedReportInfo,
                                         GetSharedReportList, GetTeamInfo,
                                         GetTeamList, GetTrackList,
                                         GetUserInfo, GetUserList,
                                         GetVendorList, IsExpiring,
                                         IsFeatureEnabled, PassFail,
                                         PromoteSandbox, RemoveFile,
                                         RescanDynamicScan, SharedReport,
                                         SubmitDynamicScan, SummaryReport,
                                         SwitchToSaml, ThirdPartyReport,
                                         UpdateApp, UpdateBuild,
                                         UpdateMitigationInfo, UpdateSandbox,
                                         UpdateTeam, UpdateUser, UploadAndScan,
                                         UploadAndScanByAppId, UploadFile.
  -active                                Flag to indicate if the user is
                                         active.
  -api1                                  API one.
  -api2                                  API two.
  -api3                                  API three.
  -api4                                  API four.
  -appid                                 Application ID.
  -appidlist                             Application ID list (csv).
  -appname                               Application Name.
  -apptype                               Must be one of the following:
                                         ApplicationDesignConstructionIDEAnalysi
                                         s, ApplicationLifeCycleManagement,
                                         ApplicationServerIntegrationServer,
                                         BackOfficeEnterprise, CRM,
                                         CollaborationGroupwareMessaging,
                                         Consumer, ContentManagementAuthoring,
                                         Engineering,
                                         EnterpriseResourcePlanning,
                                         InformationAccessDeliveryMiningPortal,
                                         InformationDataManagementDatabase,
                                         MiddlewareMessageOrientedTransaction,
                                         NetworkManagement, Networking,
                                         NotSpecified, Other,
                                         OtherDevelopmentTools, Security,
                                         ServerWareClusteringWebVM, Storage,
                                         SystemLevelSoftware,
                                         SystemsManagement, TestingTools.
  -archerappname                         Archer Application Name.
  -autorecreate                          If true, the sandbox will be
                                         automatically re-created when the
                                         7-day expiration period starts.
  -autoscan                              True if you want to automatically
                                         submit a full scan of the application
                                         following a successful prescan.
  -buildid                               Build ID.
  -businessowner                         Business Owner.
  -businessowneremail                    Business Owner's e-mail address.
  -businessunit                          Business Unit.
  -comment                               Comment.
  -createprofile                         True to create a new application
                                         profile.
  -createsandbox                         True to create a new sandbox.
  -credprofile                           Credentials Profile.
  -criticality                           Must be one of the following: High,
                                         Low, Medium, VeryHigh, VeryLow.
  -custom1                               Custom one.
  -custom2                               Custom two.
  -custom3                               Custom three.
  -custom4                               Custom four.
  -custom5                               Custom five.
  -customfieldname                       Custom field name.
  -customfieldvalue                      Custom field value.
  -customid                              Custom ID.
  -debug                                 Turn on debug messages.
  -deleted                               Flag to indicate if deleted records
                                         are included in the results.
  -deleteincompletescan                  Delete scan if incomplete when
                                         handling UploadAndScan action.
  -deploymenttype                        Must be one of the following:
                                         ClientServer,
                                         EnterpriseApplicationEnhancement,
                                         Mobile, NotSpecified, StandAlone,
                                         WebBased.
  -description                           Description.
  -elearningcurriculum                   eLearning Curriculum.
  -elearningmanager                      eLearning Manager.
  -elearningtrack                        eLearning Track.
  -emailaddress                          E-mail address.
  -endtime                               Dynamic scan end time.
  -exclude                               Case-sensitive comma-separated list of
                                         module name patterns that represent
                                         the names of modules that should not
                                         be scanned as top level modules. The
                                         '*' wildcard matches 0 or more
                                         characters. The '?' wildcard matches
                                         exactly 1 character.
  -feature                               Feature Name.
  -fileid                                File ID.
  -filepath                              Filepath or folderpath of the file or
                                         directory to upload. (If the last
                                         character is a backslash it needs to
                                         be escaped: \\).
  -firstname                             First Name.
  -flawid                                Flaw ID.
  -flawidlist                            Comma-separated list of module flaw
                                         IDs.
  -flawonly                              Turn on DVR (Dynamic Vulnerability
                                         Rescan) functionality.
  -format                                Must be one of the following: csv,
                                         pdf, text, xml.
  -fromdate                              From Date.
  -help                                  Display this message.
  -inactive                              Flag to indicate if inactive users are
                                         included in the results.
  -include                               Case-sensitive comma-separated list of
                                         module name patterns that represent
                                         the names of modules that should be
                                         scanned as top level modules. The '*'
                                         wildcard matches 0 or more characters.
                                         The '?' wildcard matches exactly 1
                                         character.
  -includeapplications                   Enter Yes to view the members of the
                                         team. The default is No.
  -includeinprogress                     True to include build data for builds
                                         with unpublished scan reports.
  -includenewmodules                     True if you want to automatically
                                         select all new top-level modules for
                                         inclusion in the scan of the
                                         application following a successful
                                         prescan.
  -includeuserinfo                       True to include permission information
                                         for the current user.
  -includeusers                          Enter Yes to view applications
                                         assigned to the team. The default is
                                         No.
  -industry                              Must be one of the following: agmine,
                                         busiserv, compelec, conserv, edu,
                                         enerutil, fed, finserv, gvmt, hpb,
                                         local, manu, mediaent, nonprofit,
                                         notspec, prtnr, realconst, retail,
                                         softint, telcom, transerv, travel,
                                         wholedist.
  -inputfilepath                         The filepath of the csv file from
                                         which to read additional command-line
                                         arguments (The first row should
                                         contain the parameter names,
                                         subsequent rows should contain the
                                         corresponding values of those
                                         parameters).
  -iselearningmanager                    True if eLearning manager.
  -issamluser                            True if SAML user.
  -keepelearningactive                   True to keep eLearning Active.
  -lastname                              Last Name.
  -launchdate                            Launch date.
  -legacyscanengine                      True to use legacy scan engine.
  -lifecyclestage                        Must be one of the following:
                                         CannotDisclose,
                                         DeployedInProductionAndActivelyDevelope
                                         d, ExternalOrBetaTesting,
                                         InDevelopmentPreAlpha,
                                         InternalOrAlphaTesting,
                                         MaintenanceOnlyBugFixes, NotSpecified.
  -lifecyclestageid                      Lifecycle stage id.
  -logfilepath                           The filepath of the file where
                                         informational and error messages will
                                         be logged. If the file already exists
                                         new data will be appended.
  -loginaccounttype                      Login Account Type.
  -loginenabled                          True if Login is enabled.
  -maxretrycount                         Maximum retry count in the case of
                                         connectivity failure.
  -members                               Members.
  -mitigationaction                      Must be one of the following:
                                         accepted, acceptrisk, appdesign,
                                         comment, fp, netenv, osenv, rejected,
                                         remediated.
  -modules                               Comma-separated list of module IDs.
  -newcustomid                           New Custom ID.
  -nextdayschedulingenabled              Next-day scheduling enabled. Boolean,
                                         defaults to false. Specifies if a user
                                         can schedule next-day consultations.
                                         Only available to Veracode human user
                                         accounts with the Security Lead or
                                         Administrator roles and to non-human
                                         API accounts with the Upload API role.
  -onlylatest                            False to include build data for
                                         previous builds with published scan
                                         reports.
  -orgid                                 Organization ID.
  -orgname                               Organization Name.
  -origin                                Must be one of the following:
                                         Contractor, InternallyDeveloped,
                                         NotSpecified, OpenSource,
                                         OutsourcedTeam, PurchasedApplication,
                                         ThirdPartyLibrary.
  -outputfilepath                        Output filepath.
  -outputfolderpath                      Output folderpath. (If the last
                                         character is a backslash it needs to
                                         be escaped: \\).
  -pattern                               Case-sensitive filename pattern that
                                         represents the names of uploaded files
                                         that should be saved with a different
                                         name. The '*' wildcard matches 0 or
                                         more characters. The '?' wildcard
                                         matches exactly 1 character. Each
                                         wildcard corresponds to a numbered
                                         group that can be referenced in the
                                         replacement pattern.
  -period                                Period.
  -phone                                 Phone.
  -phost                                 Proxy host.
  -platform                              Must be one of the following: Android,
                                         ColdFusion, J2ME, Java, Linux,
                                         NotSpecified, PHP, Ruby, Solaris,
                                         Windows, WindowsMobile, iOS.
  -platformid                            Platform ID.
  -policy                                Policy.
  -position                              Job title of the user.
  -ppassword                             Proxy password.
  -pport                                 Proxy port.
  -puser                                 Proxy user.
  -replacement                           A replacement pattern that references
                                         groups captured by the filename
                                         pattern. For example, if the filename
                                         pattern is '*-*-SNAPSHOT.war' and the
                                         replacement pattern
                                         '$1-master-SNAPSHOT.war', an uploaded
                                         file named 'app-branch-SNAPSHOT.war'
                                         would be saved as
                                         'app-master-SNAPSHOT.war'.
  -reportchangedsince                    Build data is included only for builds
                                         with scan reports that have been
                                         published or scan reports that have
                                         changed since the specified date.
  -requirestoken                         True if the user requires token.
  -rest                                  True to use REST API call.
  -roles                                 Comma-delimited list. Case-sensitive.
                                         You can filter on theses human user
                                         roles: Administrator, Creator,
                                         Executive, Mitigation Approver, Policy
                                         Administrator, Reviewer, Security
                                         Lead, Submitter, Security Insights,
                                         eLearning.
  -samlsubject                           SAML Subject.
  -sandboxid                             Sandbox ID.
  -sandboxname                           The name of the sandbox.
  -saveas                                New filename for the uploaded file.
  -scanallnonfataltoplevelmodules        True if you want to automatically scan
                                         all top level modules of the
                                         application following a successful
                                         prescan.
  -scanpollinginterval                   Scan polling interval (in seconds).
  -scantimeout                           Scan timeout in minutes.
  -scantype                              Scan Type.
  -selected                              True if you want to scan all modules
                                         which are currently selected.
  -selectedpreviously                    True if you want to scan all modules
                                         which are previously selected.
  -sharedreportid                        Shared report id.
  -starttime                             Dynamic scan start time.
  -tags                                  Tags.
  -teamid                                Team ID.
  -teamname                              Team Name.
  -teams                                 Comma-separated list of team names.
                                         Case-sensitive.
  -title                                 User title.
  -todate                                To date.
  -token                                 Token associated with the archer
                                         report.
  -toplevel                              True if you want to ensure that the
                                         scan completes even though there are
                                         non-fatal errors, such as unsupported
                                         frameworks.
  -userid                                User ID.
  -username                              User name.
  -usertype                              User type.
  -vendorid                              Vendor ID.
  -version                               The name or version number of the new
                                         or updated build.
  -vid                                   Veracode API ID.
  -vkey                                  Veracode API key.
  -vpassword                             <Not supported> Veracode password.
  -vuser                                 <Not supported> Veracode username.
  -webapplication                        True if the application is a web
                                         application.
  -wrapperversion                        Display the version of this wrapper.

```









