# NMG Identity Automation With Powershell

PowerShell tooling for identity lifecycle management, built for a fictional company called Northstar Medical Group.

## The Problem

Northstar had no way to know when someone left. Offboarding ran through one employee, manually, by email. When
she retired, the emails stopped, and nobody noticed for 102 days. A manual sweep finally caught 23 stale accounts.
It took 11 hours over 4 days. But it only found what it knew to look for. 

It missed:
• Departures that were never recorded
• Contractors who never touched payroll
• Service accounts that were never people 

Zero automation. Zero alerts. One point of failure holding up the entire offboarding process, 
and no idea how muchit had already missed. 


## The Approach

Instead of cross-checking the directory records against payroll manually. A process that 
only works if the paperwork gets filed and the names actually match. These tools go straight to the source. 

They query the domain controller directly  for eacxh account's last authentication date. 
No paperwork. No name mathching. No trusting that HR and IT are in sync. 

If an account hasn't logged in, the domain controller knows, regardless of what
any spreadsheet says. 

## Before you start

- Windows Server with the ActiveDirectory PowerShell module

      Import-Module ActiveDirectory

- Rights to modify user objects in the domain
- An authorising ticket number, in the form NMG-0000
- A Disabled Users OU at the root of the domain
- A writable reports folder. Create it if it does not exist:

      New-Item -Path "C:\Reports\Offboarding" -ItemType Directory -Force


## Tools

### Find-StaleAccounts.ps1

Identifies enabled accounts that have not authenticated within a given
number of days, including accounts that have never authenticated.
Exports a timestamped CSV plus a summary recording the exact query used.

    .\Find-StaleAccounts.ps1
    .\Find-StaleAccounts.ps1 -Days 30
    .\Find-StaleAccounts.ps1 -Days 180 -IncludeDisabled

| Parameter | Type | Default | Description |
|---|---|---|---|
| `-Days` | int | 90 | Days without authentication before an account is considered stale |
| `-ReportPath` | string | C:\Reports | Where reports are written |
| `-IncludeDisabled` | switch | off | Include disabled accounts in results |

**Output:** a timestamped CSV of findings, and a summary file recording
the question that produced them so the report can be reproduced.

## Repository Structure

    Scripts/        PowerShell tools
    Documentation/  Runbooks and process documentation
    Evidence/       Sample output and verification screenshots
    Logs/           Execution logs

## Environment

Windows Server with Active Directory Domain Services.
Requires the ActiveDirectory PowerShell module.

## About

Built during the TotalThreat 30-Day Challenge in a simulated
healthcare environment. Northstar Medical Group is fictional.

Author: Zevan Simon 
