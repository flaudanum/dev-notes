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
  - [Typing several cmdlets](#typing-several-cmdlets)
- [Solutions to common use cases](#solutions-to-common-use-cases)
  - [Measure the execution time of an application](#measure-the-execution-time-of-an-application)
  - [Running processes](#running-processes)
    - [List of processes](#list-of-processes)
    - [Detailed information](#detailed-information)
  - [Hash value](#hash-value)
- [Glossary](#glossary)

Microsoft [official reference](https://docs.microsoft.com/en-us/powershell/).

# Basics

## Typing several cmdlets

When typing several cmdlets on the same line, use a semi-colon `;` to separate them.


# Solutions to common use cases

## Measure the execution time of an application

Use the command `Measure-Command` for that purpose:
```powershell
Measure-Command { Get-EventLog "windows powershell" }
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

# Glossary

<b style="color:#179755;font-size:1.2em">cmdlet</b>

A **cmdlet** is a lightweight command that is used in the Windows PowerShell environment. The Windows PowerShell runtime invokes these cmdlets within the context of automation scripts that are provided at the command line. The Windows PowerShell runtime also invokes them programmatically through Windows PowerShell APIs. See [Microsoft reference](https://docs.microsoft.com/en-us/powershell/developer/cmdlet/cmdlet-overview).

