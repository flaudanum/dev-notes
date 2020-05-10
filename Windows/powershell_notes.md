<table style="border-style: none;">
    <tr >
        <td>
            <img src="../resources/powershell-logo.png" style="height: 60px">
        </td>
        <td>
            <span style="font-weight: 500; font-size: 2em;">Microsoft PowerShell</span>
        </td>
    </tr>
</table>

- [Basics](#basics)
  - [Getting Help](#getting-help)
  - [Variables](#variables)
    - [Create a variable](#create-a-variable)
    - [Getting information about a variable](#getting-information-about-a-variable)
  - [Typing several cmdlets](#typing-several-cmdlets)
  - [Accessing environment variables](#accessing-environment-variables)
  - [Common cmdlets](#common-cmdlets)
    - [Display a file content](#display-a-file-content)
    - [Remove a file or a directory](#remove-a-file-or-a-directory)
- [Solutions to common use cases](#solutions-to-common-use-cases)
  - [Change the location](#change-the-location)
  - [Display the content of a text file](#display-the-content-of-a-text-file)
  - [Colon `:` character and strings](#colon--character-and-strings)
  - [Get the content of a directory](#get-the-content-of-a-directory)
  - [Filtering outputs](#filtering-outputs)
  - [Getting the attributes of returned objects](#getting-the-attributes-of-returned-objects)
  - [Measure the execution time of an application](#measure-the-execution-time-of-an-application)
  - [Running processes](#running-processes)
    - [List of processes](#list-of-processes)
    - [Detailed information](#detailed-information)
  - [Hash value](#hash-value)
  - [Command history](#command-history)
  - [Creating an empty file](#creating-an-empty-file)
  - [Redirecting output to null](#redirecting-output-to-null)
  - [Limited script execution policy](#limited-script-execution-policy)
  - [Blocking of downloaded scripts](#blocking-of-downloaded-scripts)
- [Scripting](#scripting)
  - [Multi-line commands](#multi-line-commands)
  - [Strings with line breaks](#strings-with-line-breaks)
  - [Read input from keyboard](#read-input-from-keyboard)
  - [Testing the existence of a directory or a file](#testing-the-existence-of-a-directory-or-a-file)
- [Glossary](#glossary)

Microsoft [official reference](https://docs.microsoft.com/en-us/powershell/).

# Basics

## Getting Help

For getting help with a cmdlet, type it followed by `-?` or use `Get-Help`:
```PowerShell
> Get-Help Get-Content # get help for cmdlet Get-Content
```

## Variables

### Create a variable
Create a variable `$loc` with the current location path:
```PowerShell
PS C:\> $loc = Get-Location
PS C:\> echo $loc

Path
----
C:\
```

### Getting information about a variable

```PowerShell
PS C:\> $loc | Get-Member

   TypeName: System.Management.Automation.PathInfo

Name         MemberType Definition
----         ---------- ----------
Equals       Method     bool Equals(System.Object obj)
GetHashCode  Method     int GetHashCode()
GetType      Method     type GetType()
ToString     Method     string ToString()
Drive        Property   System.Management.Automation.PSDriveInfo Drive {get;}
Path         Property   string Path {get;}
Provider     Property   System.Management.Automation.ProviderInfo Provider {get;}
ProviderPath Property   string ProviderPath {get;}
```
Or by filtering `MemberType` to `Property`:
```PowerShell
PS C:\> $loc | Get-Member -MemberType Property

   TypeName: System.Management.Automation.PathInfo

Name         MemberType Definition
----         ---------- ----------
Drive        Property   System.Management.Automation.PSDriveInfo Drive {get;}
Path         Property   string Path {get;}
Provider     Property   System.Management.Automation.ProviderInfo Provider {get;}
ProviderPath Property   string ProviderPath {get;}
```

PowerShell also creates a variable drive `variable:`.

More information in the [official documentation](https://docs.microsoft.com/en-us/powershell/scripting/learn/using-variables-to-store-objects?view=powershell-6).

## Typing several cmdlets

When typing several cmdlets on the same line, use a semi-colon `;` to separate them.

## Accessing environment variables
The PowerShell environment provider lets you access Windows environment variables in PowerShell in the PowerShell drive `Env:`. This drive looks much like a file system drive, similarly to Linux directory `/proc/` which is not a conventional file folder.
```PowerShell
> Set-Location Env: # or cd Env:
> Get-ChildItem # or ls

Name                           Value
----                           -----
ALLUSERSPROFILE                C:\ProgramData
APPDATA                        C:\Users\f.laudarin\AppData\Roaming
CommonProgramFiles             C:\Program Files\Common Files
...
```
For instance listing the variable `APDDATA` from anywhere:
```PowerShell
> Get-ChildItem Env:\USERNAME # or ls Env:\USERNAME
```
or w/o the backslash:
```PowerShell
> Get-ChildItem Env:USERNAME
```
More information in the [official documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-6).

## Common cmdlets

### Display a file content

Use cmdlet `Get-Content`:
```powershell
> Get-Content myfile.txt
```
Similar to Linux bash command `cat` which by the way is an *alias* of `Get-Content`.

### Remove a file or a directory

The command `Remove-Item` can be used to remove a path element. If it is a non-empty directory (an element w/ children) then use option `-Recurse`
```powershell
> Remove-Item my-file.txt
> Remove-Item -Recurse .\my-dir\
```


# Solutions to common use cases

## Change the location
Change the current directory path with the cmdlet `Set-Location`.

## Display the content of a text file

The `Get-Content` cmdlet retrieves all text from a text file specified by the Path parameter.
```powershell
> Get-Content -Path c:\path\to\the\file
```
Getting first lines of a text file with option `-TotalCount`, e.g.:
```powershell
> Get-Content -Path .\LineNumbers.txt -TotalCount 5
```

## Colon `:` character and strings

The colon character is inserted in `string` expressions as ``"`n"`` that is preceded by a *backtick*.

## Get the content of a directory

Use the cmdlet `Get-ChildItem`. Aliases for that command are `GCI` and `ls`.

## Filtering outputs

As the results returned by cmdlets are not plain text like commands' standard output on Linux OS, they cannot be filter with such a command like `grep`. The output must be piped to the cmdlet `Where-Object` (aliases being `where` and `?`). The cmdlet `Where-Object` takes a *script block* as argument. A *script block* is composed of one or more PowerShell commands surrounded by braces ({}) that evaluates to *true* or *false*. `Where-Object` script blocks use the special variable `$_` to refer to the current object in the pipeline.

<span style="text-decoration: underlined;">Example</span>

```powerShell
> Get-ChildItem | where {$_.Name -like "MyGreatSoft*"}

    Directory: C:\Users\f.laudanum\Documents\DEV

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       11/22/2019   9:08 AM                MyGreatSoft-Core
d-----         9/4/2019   1:54 PM                MyGreatSoft-Grid-client
d-----        1/13/2020   1:35 PM                MyGreatSoft-Wind
d-----       12/18/2019  10:41 AM                MyGreatSoft-Wind-env
d-----       12/18/2019   5:23 PM                MyGreatSoft-Wind-install
```

## Getting the attributes of returned objects

Cmdlets return objects from the same class. Use the command `Get-Member` for getting a list of properties (properties and methods) of the returned objects.

## Measure the execution time of an application

Use the command `Measure-Command` for that purpose:
```powershell
> Measure-Command { Get-EventLog "windows powershell" }
```
The `Measure-Command` cmdlet runs a *script block* or *cmdlet* internally, times the execution of the operation, and returns the execution time. See [Microsoft reference](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/measure-command).

## Running processes
### List of processes
Get the complete list of running processes on the local machine with `Get-Process`. The following information is provided: Handles, NPM(K), PM(K), WS(K), CPU(s), Id, SI, ProcessName.
### Detailed information
Get all available data about one or more processes. Specify the process with their `ProcessName`.

```powershell
> Get-Process winword, explorer | Format-List *
```

## Hash value
`Get-FileHash` cmdlet computes the hash value for a file by using a specified hash algorithm. Default is *SHA256*:
```powershell
> Get-FileHash $pshome\powershell.exe | Format-List
Algorithm : SHA256
Hash      : 6A785ADC0263238DAB3EB37F4C185C8FBA7FEB5D425D034CA9864F1BE1C1B473
Path      : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```
Use the option `-Algorithm` for using another algorithm:
```powershell
> Get-FileHash C:\Users\Andris\Downloads\Contoso8_1_ENT.iso -Algorithm SHA384 | Format-List

Algorithm : SHA384
Hash      : 20AB1C2EE19FC96A7C66E33917D191A24E3CE9DAC99DB7C786ACCE31E559144FEAFC695C58E508E2EBBC9D3C96F21FA3
Path      : C:\Users\Andris\Downloads\Contoso8_1_ENT.iso
```

## Command history

List command history with the cmdlet `Get-History`.

## Creating an empty file

Using the append redirector `>>` resolves the issue where an existing file is deleted:
```powershell
> echo $null >> filename
```

## Redirecting output to null

```powershell
($foo = someFunction) | out-null
($foo = someFunction) > $null
```
Redirection to standard error
```
($foo = someFunction) 2>&1 | out-null
($foo = someFunction) 2> $null
```
See answer on [Stackoverflow](https://stackoverflow.com/a/6461021/7565402).


## Limited script execution policy

For instance, when connected to a remote machine, the **script execution policy** may prevent from executing scripts.
```
> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
```
The policy for the *scope* `CurrentUser` is `Undefined` therefore no script can be run. This can be changed with
**privileges**:
```
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -scope CurrentUser

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
PS C:\Windows\system32> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser    RemoteSigned
 LocalMachine    RemoteSigned
```

More details on [Microsoft docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies).

## Blocking of downloaded scripts

When a script is download, the user is automatically prompted at each run wether they want to allow the script execution. This checking can be removed with the cmdlet `Unblock-File`:
```powershell
Unblock-File -Path c:\path\to\the\downloaded\blocked\script.ps1
```


# Scripting

## Multi-line commands
For breaking a command on multiple lines one can use a space followed by the grave accent (backtick):
```powershell
Get-ChildItem -Recurse `
  -Filter *.jpg `
  | Select LastWriteTime
```
However, this is only ever necessary in such cases as shown above. Usually one get automatic line continuation when a command cannot syntactically be complete at that point. This includes starting a new pipeline element:
```powershell
Get-ChildItem |
  Select Name,Length
```
will work without problems since after the | the command cannot be complete since it's missing another pipeline element. Also opening curly braces or any other kind of parentheses will allow line continuation directly:
```powershell
$x=1..5
$x[
  0,3
] | % {
  "Number: $_"
}
```
(from [stackoverflow](https://stackoverflow.com/a/3235993))

## Strings with line breaks

Use ``"`n"``.
```powershell
echo "This text is followed by a linebreak`n"
```

## Read input from keyboard

Use the cmdlet `Read-Host`.
```powershell
$name = Read-Host 'What is your username?'
```

## Testing the existence of a directory or a file

Use the cmdlet `Test-Path`.
```powershell
if (!(Test-Path -Path $testedPath)) {
    Write-Error "Path '$testedPath' does not exist."
}
```


# Glossary

<b style="color:#179755;font-size:1.2em">cmdlet</b>

A **cmdlet** is a lightweight command that is used in the Windows PowerShell environment. The Windows PowerShell runtime invokes these cmdlets within the context of automation scripts that are provided at the command line. The Windows PowerShell runtime also invokes them programmatically through Windows PowerShell APIs. See [Microsoft reference](https://docs.microsoft.com/en-us/powershell/developer/cmdlet/cmdlet-overview).

*Note*: Cmdlets are case insensitive.

<b style="color:#179755;font-size:1.2em">module</b>

A **module** is a set of related Windows PowerShell functionalities, grouped together as a convenient unit (usually saved in a single directory). By defining a set of related script files, assemblies, and related resources as a module, you can reference, load, persist, and share your code much easier than you would otherwise.

The main purpose of a module is to allow the modularization (ie, reuse and abstraction) of Windows PowerShell code. For example, the most basic way of creating a module is to simply save a Windows PowerShell script as a .psm1 file. Doing so allows you to control (ie, make public or private) the functions and variables contained in the script. Saving the script as a .psm1 file also allows you to control the scope of certain variables. Finally, you can also use cmdlets such as Install-Module to organize, install, and use your script as building blocks for larger solutions.

The different flavors of PowerShell modules are:

- Script modules
- Binary modules
- Manifest modules
- Dynamic modules


The installation paths for sharing modules are listed in the variable `$ENV:PSModulePath`.

More in the [official documentation](https://docs.microsoft.com/en-us/powershell/scripting/developer/module/understanding-a-windows-powershell-module?view=powershell-6).