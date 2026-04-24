# Attack Scenario

## Description
A suspicious PowerShell execution was detected on a Windows system. The command used ExecutionPolicy Bypass to run a script, which is commonly associated with malicious activity.

## Simulated Attack
powershell -ExecutionPolicy Bypass -File simulate-powershell-activity.ps1

## Objective
To simulate a real-world attack scenario involving PowerShell misuse and perform detection, analysis, and response.