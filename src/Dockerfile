FROM sixeyed/msbuild:netfx-4.5.2-webdeploy AS build-agent

WORKDIR C:\src\SignUp.Web
COPY src\SignUp.Web\packages.config .
RUN nuget restore packages.config -PackagesDirectory ..\packages

COPY src\ C:\src
RUN msbuild SignUp.Web.csproj /p:OutputPath=C:\out /p:DeployOnBuild=true `
        /p:VSToolsPath=C:\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\tools\VSToolsPath

FROM microsoft/aspnet:windowsservercore
SHELL ["powershell"]

WORKDIR C:\web-app
RUN Remove-Website -Name 'Home site'; `
    New-Website -Name 'web-app' -Port 80 -PhysicalPath 'C:\web-app'

COPY --from=build-agent C:\out\_PublishedWebsites\SignUp.Web C:\web-app    