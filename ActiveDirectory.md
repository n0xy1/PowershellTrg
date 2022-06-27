# Active Directory and Powershell

In order to use te following commandlets, you need to have the active directory powershell module installed.

It can be installed in the following ways:
- Install windows RSAT `Get-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools\~\~\~\~0.0.1.0 | Add-WindowsCapability –Online`
- Be on a domain controller.
- Try the command `Install-WindowsFeature RSAT-AD-PowerShell`

## Basic Usage

The following commandlets are usefull to query the basics:
- `Get-ADUser -Filter *`
- `Get-ADGroup -Filter *`
- `Get-ADComputer -Filter *`

The Filter parameter can be used to narrow down the search and only return relevant results, for example:

- `Get-ADUser -Filter 'Name -like "Administr*"`
- `Get-ADUser -Filter 'Enabled -eq "False"'`

This is much faster than retrieving all objects and piping into the Where-Object commandlet.

## Take it up a notch

In order to find additional properties, you can specify the `-Property ` parameter. This enables you to drill down into specific parts of the AD Objects.
For Example:

- `Get-ADUser -Filter 'Name -like "Admin*"' -Property EmailAddress`

To get an understanding of what properties each AD Object has, query one and pipe it into `Get-Member`

- `Get-ADUser -Filter 'Name -like "Administrator"' -Property * | Get-Member`

This will display every property that powershell knows about.

## Speed is key

Instead of querying every object and filtering it with Where-Object, aim to filter as early as possible. In large domains this is absolutely necessary.

DO:
`Get-ADUser -Filter 'Name -like "Username" -Property DistinguishedName`

DO NOT:
`Get-Aduser -Filter * | Where-Object Name -like "Username"`

## Search Bases

Beginning the query with a specific searchbase will also aid in speed, search bases are specified with a distinguished name.. For example:

- `Get-ADObject -Filter * -SearchBase "CN=USERS,DC=home,DC=lab"`

## More to come.

