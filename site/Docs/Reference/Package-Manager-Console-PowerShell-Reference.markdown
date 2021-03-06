﻿# Package Manager Console Powershell Reference
The Package Manager Console is a PowerShell console within Visual Studio used to interact with NuGet and 
automate Visual Studio. Note that for more detailed help, you can run the <code>Get-Help <em>CommandName</em></code> 
from within the NuGet Package Manager Console in Visual Studio.

## Add-BindingRedirect
Adds binding redirects to the config file.

    Add-BindingRedirect [-ProjectName] <string>

Examines all assemblies within the output path for a project and adds binding redirects to the 
application (or web) configuration file where necessary. NOTE: As of NuGet 1.2, NuGet automatically 
runs this command when installing a package.
    
<pre>
PARAMETERS
    <strong>-ProjectName</strong> <string>
        Specifies the project to analyze and add binding redirects to.
        
        Required: true
</pre>
### Examples
    
    PM> Add-BindingRedirect MyProjectName
    
    
Adds binding redirects to the specified project, MyProjectName.

## Get-Package
Gets the set of packages available from the local repository folder. Use the -ListAvailable flag to list packages available from the online package source.

    Get-Package -Source <string> [-ListAvailable] [-Updates] [-ProjectName] [-Filter <string>] [-First <int>] [-Skip <int>] [-AllVersions]

Gets the set of packages available from the package source. Defaults to only showing the list of installed packages. 
Use the -ListAvailable flag to list packages available from the package source.

<pre>
PARAMETERS
    <strong>-Source</strong> <string>
        Specifies the URL or directory path for the package source containing the package to install. 
        If omitted, looks in the currently selected package source to find the corresponding package URL.
        
        Required: false
        
    <strong>-ListAvailable</strong>
        Gets packages available from the online package source.
        
        Required: false
        
    <strong>-Updates</strong>
        Gets packages that have an update available from the package source.
        
        Required: false
        
    <strong>-ProjectName</strong>
        Specifies the project to get installed packages from. If omitted, the command will return installed projects for the entire solution.

        Required: false

    <strong>-Filter</strong>
        Specifies a filter string used to narrow down the list of packages returned. The filter is searched 
        for in the package Id, the description and tags.
        
        Required: false
        
    <strong>-First</strong>
        Specifies the number of packages to return from the beginning of the list.
        
        Required: false
        
    <strong>-Skip</strong>
        Skips (does not return) the specified number of packages, counting from the beginning of the list.
        
        Required: false
        
    <strong>-AllVersions</strong>
        Displays all available versions of a package. The latest version of each package is listed by default.
        
        Required: false

    <strong>-IncludePrerelease</strong>
        Indicates whether to include prerelease packages in the returned results.

        Required: false
</pre>
### Examples

    
    PM> Get-Package
    
    
Returns the packages installed in the default project.
    
    PM> Get-Package -ListAvailable
    
    
Returns a list of packages available online in the current package source.
    
    
    PM> Get-Package -ListAvailable -Filter Ninject
    
    
Returns a list of packages available online using "Ninject" as a search term.
    
    
    PS> Get-Package -Updates
    
    
Returns a list of packages installed in the default project that have updates available in the current package source.
    
    PM> Get-Package -Recent
    
Returns a list of recently installed packages.
    

## Get-Project
Gets the specified project. If none is specified, returns the default project.

    Get-Project [[-Name] <string>] [-All]

Returns a reference to the DTE (Development Tools Environment) for the specified project. If none is specified, 
returns the default project selected in the Package Manager Console.
<pre>  

PARAMETERS
    <strong>-Name</strong> <string>
        Specifies the project to return. If omitted, the default project selected in the Package Manager Console is returned.
        
        Required: false
        
    <strong>-All</strong>
        Returns every project in the solution.
        
        Required: false
</pre>
### Examples

    
   PM> Get-Project
    
    
Returns a reference to the default project.
    
 
   PM> Get-Project MyProjectName
    
    
Returns a reference to the specified project, MyProjectName.
 
   PM> Get-Project -All
    
    
Returns a reference to every project in the solution.

## Install-Package
Installs a package.

    Install-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [-Version <string>] [-Source <string>] [-FileConflictAction] [-DependencyVersion <dependencyVersion>] [-WhatIf]

Installs a package and its dependencies into the project.

<pre>
PARAMETERS
    <strong>-Id</strong> <string>
        Specifies the package ID of the package to install.
        
        Required: true
        
    <strong>-IgnoreDependencies</strong>
        Installs only this package and not its dependencies.
        
        Required: false
        
    <strong>-ProjectName</strong> <string>
        Specifies the project to install the package into. If omitted, the default project is chosen.
        
        Required: false
        
    <strong>-Source</strong> <string>
        Specifies the URL or directory path for the package source containing the package to install. If omitted, 
        looks in the currently selected package source 
        to find the corresponding package URL.
        
        Required: false
        
    <strong>-Version</strong> <string>
        Specifies the version of the package to install. If omitted, defaults to the latest version.
        
        Required: false

    <strong>-IncludePrerelease</strong>
        Indicates whether this command will consider prerelease packages. If omitted, only 
        stable packages are considered.

        Required: false
        
    <strong>-FileConflictAction</strong>
        Specify the action to take, when asked to overwrite or ignore existing files referenced by the project.             
        Possible values are Overwrite, Ignore and None.
        
        Required: false
		
	<strong>-DependencyVersion</strong>
		Specifies the version of the dependency package to be selected from the list of valid
		dependency packages. The defult value is Lowest. You can override this default value
		by specifying a new default value in nuget.config file:
          &lt;configuration>
		    &lt;config>
              &lt;add key="DependencyVersion" value="HighestPatch" />
            &lt;/config>
          &lt;/configuration>
		  
        Note that for NuGet 2.7.2 or ealier, the default value is HighestPatch, and it cannot be
		changed.
		
		Possible values are:
		- Lowest: the lowest version;
		- HighestPatch: the version with the lowest major, lowest minor, highest patch;
		- HighestMinor: the version with the lowest major, highest minor, highest patch;
		- Highest: the highest version;
		
		For example, package A has a dependency on package B >= 2.0. Available versions of package
		B are: 1.0, 2.1.1, 2.1.2, 2.2.1, 2.2.2, 3.0.1, 3.0.2.
		The version of package b selected will be:
		- 2.1.1, when DependencyVersion is Lowest;
		- 2.1.2, when DependencyVersion is HighestPatch;
		- 2.2.2, when DependencyVersion is HighestMinor;
		- 3.0.2, when DependencyVersion is Highest;
		
        Required: false

    <strong>-WhatIf</strong>
        Shows what would happen if the cmdlet runs. The cmdlet is not run.

        Required: false
</pre>
### Examples

    PM> Install-Package Elmah
    
    
Installs the latest version of the Elmah package from the current package source into the default project.

    PM> Install-Package Glimpse -Version 1.0.0 -Project MvcApplication1
    
Installs version 1.0.0 of Glimpse into the project named MvcApplication1

    PM> Install-Package Ninject.Mvc3 -IgnoreDependencies -Source c:\temp\packages

Installs the package, Ninject.Mvc3, but not its dependencies. It looks in the directory, 
c:\temp\packages to find the package.

With NuGet 2.8 client or higher, Install-Package can be used to downgrade the existing packages in your project, if necessary. For example, if you had installed a pre-release version of a package to try out new features but would like to go back to a previous stable version you can do so using Install-Package (or Update-Package).

Let's say you had 5.1.0-rc1 version of Microsoft.AspNet.MVC in your project but would like to go back to 5.0.0 version you could type the following command in PMC.

     PM> Install-Package Microsoft.AspNet.MVC -Version 5.0.0. 

In NuGet 2.7 or lower clients, if you try to downgrade a package, you would get an error message saying that a newer version is already installed.

## Open-PackagePage
Open the browser pointing to ProjectUrl, LicenseUrl or ReportAbuseUrl of the specified package.

    Open-PackagePage -Id <string> [-Version] [-Source] [-License] [-ReportAbuse] [-PassThru]

Open the browser pointing to ProjectUrl, LicenseUrl or ReportAbuseUrl of the specified package.
    
<pre>
PARAMETERS
    <strong>-Id</strong> <string>
        Specifies the Id of the package to search for.
        
        Required: false
        
    <strong>-Version</strong>
        Specifies the version of the package to search for. If omitted, defaults to the latest version.
        
        Required: false
        
    <strong>-Source</strong>
        Specifies the source of the repository to search for package. If omitted, defaults to the selected source in 
        the package source dropdown control.
        
        Required: false
        
    <strong>-License</strong>
        Indicates the cmdlet should open the LicenseUrl of the specified package. If neither LicenseUrl nor ReportAbuseUrl 
        is set, the cmdlet will open the Proje
        ctUrl by default.
        
        Required: false
        
    <strong>-ReportAbuse</strong>
        Indicates the cmdlet should open the ReportAbuseUrl of the specified package. If neither LicenseUrl nor ReportAbuseUrl 
        is set, the cmdlet will open the P
        rojectUrl by default.
        
        Required: false
        
    <strong>-PassThru</strong>
        If specified, the cmdlet will return the value of the requested URL.
        
        Required: false
</pre>
### Examples

    
    PM> Open-PackagePage Ninject
    
Opens a browser to the project URL specified in the Ninject package.
    
    PM> Open-PackagePage Ninject -License
    
    
Opens a browser to the license URL specified in the Ninject package.
    
    PM> Open-PackagePage Ninject -ReportAbuse
    
Opens a browser to the URL at the current package source used to report abuse for the specified package.
    
    PM> $url = Open-PackagePage Ninject -License -WhatIf -PassThru
    
    
Assigns the license URL to the variable, $url, without opening the URL in a browser.

## Uninstall-Package
Uninstalls a package.

    Uninstall-Package [-Id] <string> [-RemoveDependencies] [-Force] [-Version <string>] [-WhatIf]

Uninstalls a package. If other packages depend on this package, the command will fail unless the –Force option is specified.
<pre>    

PARAMETERS
    <strong>-Id</strong> <string>
        Specifies the package ID of the package to uninstall.
        
        Required: true
        
    <strong>-RemoveDependencies</strong>
        Uninstalls the package and its unused dependencies.
        
        Required: false
        
    <strong>-ProjectName</strong> <string>
        Specifies the project to uninstall the package from. If omitted, the default project is chosen.
        
        Required: false
        
    <strong>-Force</strong>
        Forces a package to be uninstalled, even if there are dependencies on it.
        
        Required: false
        
    <strong>-Version</strong> <string>
        The version of the package to uninstall. If omitted, defaults to the latest version.
        
        Required: false

    <strong>-WhatIf</strong>
        Shows what would happen if the cmdlet runs. The cmdlet is not run.

        Required: false
</pre>
### Examples

    
    PM> Uninstall-Package Elmah
    
    
Uninstalls the Elmah package from the default project.
    
    
    PM> Uninstall-Package Elmah -RemoveDependencies
    
    
Uninstalls the Elmah package as well as all its dependencies. If any dependency has another package that depends on it, 
it is skipped.
    
    
    PM> Uninstall-Package Elmah -Force
    
    
Uninstalls the Elmah package even if another package depends on it.

## Update-Package
Updates a package.

    Update-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [-Version <string>] [-Source <string>] [-Safe] [-IncludePrerelease] [-Reinstall] [-FileConflictAction] [-WhatIf]

Updates a package and its dependencies to a newer version.
    
<pre>
PARAMETERS
    <strong>-Id</strong> <string>
        Specifies the package ID of the package to update.
        
        Required: false
        
    <strong>-IgnoreDependencies</strong>
        Updates all of the package’s dependencies to the latest version. False by default.
        
        Required: false
        
    <strong>-ProjectName</strong> <string>
        Specifies the project containing the project to update. If omitted, all projects are updated.
        
        Required: false

    <strong>-Safe</strong> <string>
        The `-Safe` flag constrains upgrades to only versions with the same Major and Minor version component. 
        
       Required: false
        
    <strong>-Source</strong> <string>
        Specifies the URL or directory path for the package source containing the package to install. If omitted, 
        looks in the currently selected package source to find the corresponding package URL.
        
        Required: false
        
    <strong>-Version</strong> <string>
        Specifies the version that the package will be upgraded to. If omitted, defaults to the latest version.
        
       Required: false

    <strong>-IncludePrerelease</strong>
        Indicates whether to include prereleases when searching for updates. If omitted, only 
        stable packages are considered.

        Required: false
        
    <strong>-Reinstall</strong>
        Reinstall packages with the existing versions.

        Required: false
        
    <strong>-FileConflictAction</strong>
        Specify the action to take, when asked to overwrite or ignore existing files referenced by the project. 
        Possible values are Overwrite, Ignore and None.
        
        Required: false

    <strong>-WhatIf</strong>
        Shows what would happen if the cmdlet runs. The cmdlet is not run.

        Required: false
</pre>
### Examples
    
    PM> Update-Package Elmah
    
Updates the Elmah package in every project to the latest version.
    
    PM> Update-Package Elmah -Version 1.1.0
    
Updates the Elmah package to a specific version in every project.

    PM> Update-Package Elmah -Project MvcApplication1
    
Updates the Elmah package within the MvcApplication1 project.

    PM> Update-Package

Updates every package in every project.

    PM> Update-Package -Project MvcApplication1

Updates every package in the MvcApplication1 project.

    PM> Update-Package Elmah -Safe

Updates Elmah to the highest "safe" version. For example, if Elmah version 1.0.0 of a package 
is installed, and versions 1.0.1, 1.0.2, and 1.1 are available in the feed, the `-Safe` 
flag updates the package to 1.0.2 instead of 1.1 as it would without the flag.

With NuGet 2.8 client or higher, Update-Package can be used to downgrade the existing packages in your project, if necessary. For example, if you had installed a pre-release version of a package to try out new features but would like to go back to a previous stable version you can do so using Install-Package or Update-Package

Let's say you had 5.1.0-rc1 version of Microsoft.AspNet.MVC in your project but would like to go back to 5.0.0 version you could type the following command in PMC.

     PM> Update-Package Microsoft.AspNet.MVC -Version 5.0.0. 

In NuGet 2.7 or lower clients, if you try to downgrade a package, you would get an error message saying that a newer version is already installed.
