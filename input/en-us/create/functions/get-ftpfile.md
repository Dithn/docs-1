﻿---
Order: 70
xref: get-ftpfile
Title: Get-FtpFile
Description: Information on Get-FtpFile function
RedirectFrom:
  - docs/helpers-get-ftp-file
  - docs/helpersgetftpfile
---

# Get-FtpFile

<!-- This documentation is automatically generated from https://github.com/chocolatey/choco/blob/master/src/chocolatey.resources/helpers/functions/Get-FtpFile.ps1 using https://github.com/chocolatey/choco/blob/master/GenerateDocs.ps1. Contributions are welcome at the original location(s). -->

Downloads a file from a File Transfter Protocol (FTP) location.

## Syntax

~~~powershell
Get-FtpFile `
  [-Url <String>] `
  -FileName <String> `
  [-Username <String>] `
  [-Password <String>] `
  [-Quiet] `
  [-IgnoredArguments <Object[]>] [<CommonParameters>]
~~~

## Description

This will download a file from an FTP location, saving the file to the
FileName location specified.

## Notes

This is a low-level function and not recommended for use in package
scripts. It is recommended you call `Get-ChocolateyWebFile` instead.

Starting in 0.9.10, will automatically call Set-PowerShellExitCode to
set the package exit code to 404 if the resource is not found.

## Aliases

None

## Inputs

None

## Outputs

None

## Parameters

###  -Url [&lt;String&gt;]
This is the url to download the file from.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | 1
Default Value          | 
Accept Pipeline Input? | false
 
###  -FileName &lt;String&gt;
This is the full path to the file to create. If FTPing to the
package folder next to the install script, the path will be like
`"$(Split-Path -Parent $MyInvocation.MyCommand.Definition)\\file.exe"`

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | true
Position?              | 2
Default Value          | 
Accept Pipeline Input? | false
 
###  -Username [&lt;String&gt;]
The user account to connect to FTP with.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | 3
Default Value          | 
Accept Pipeline Input? | false
 
###  -Password [&lt;String&gt;]
The password for the user account on the FTP server.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | 4
Default Value          | 
Accept Pipeline Input? | false
 
###  -Quiet
Silences the progress output.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | False
Accept Pipeline Input? | false
 
###  -IgnoredArguments [&lt;Object[]&gt;]
Allows splatting with arguments that do not apply. Do not use directly.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | 
Accept Pipeline Input? | false
 
### &lt;CommonParameters&gt;

This cmdlet supports the common parameters: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer, and -OutVariable. For more information, see `about_CommonParameters` http://go.microsoft.com/fwlink/p/?LinkID=113216 .


## Links

 * [Get-ChocolateyWebFile](xref:get-chocolateywebfile)
 * [Get-WebFile](xref:get-webfile)


[Function Reference](xref:powershell-reference)

> :memo: **NOTE** This documentation has been automatically generated from `Import-Module "$env:ChocolateyInstall\helpers\chocolateyInstaller.psm1" -Force; Get-Help Get-FtpFile -Full`.

View the source for [Get-FtpFile](https://github.com/chocolatey/choco/blob/master/src/chocolatey.resources/helpers/functions/Get-FtpFile.ps1)
