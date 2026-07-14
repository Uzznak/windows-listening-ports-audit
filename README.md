# Auditer des ports windows / Windows Listening Ports Audit (4 juillet 2026)


Windows Firewall Security Audit

**FR**: Suite à l'achat d'un nouveau PC j'ai refais un auto diagnostique de mes ports.  
J'ai fait ce mini projet qui reprend la procédure que j'ai utilisé pour faire l'audit de mes ports, et identifier quoi sécuriser. Les ports sont fictifs.

**EN** : Following the purchase of a new PC, I performed an auto-diagnostic of my ports.
I did this mini project that follows the procedure I used to audit my ports and identify what to secure. The ports are fictitious.

## Objectif

**FR**: Générer un fichier CSV listant les ports TCP en écoute et fournir une base d’analyse.  
**EN** : Generate a CSV file listing TCP listening ports and provide a basis for analysis.  

## Environment

- Windows 11
- PowerShell
- Git

# Instructions
## 1. Structure du projet / Project structure  

mkdir windows-listening-ports-audit  
cd windows-listening-ports-audit  
mkdir scripts results docs  

New-Item README.md  
New-Item scripts\audit_ports.ps1  
New-Item docs\recommendations.md  

## 2. Script d’audit / Audit script  

Créer le fichier / create the file  
notepad scripts\audit_ports.ps1  

Y insérer / copy paste this content :

Get-NetTCPConnection -State Listen |  
Select-Object LocalAddress, LocalPort |  
Sort-Object LocalPort |  
Export-Csv ".\results\ports.csv" -NoTypeInformation  

## 3. Exécution / Execution  

.\scripts\audit_ports.ps1  

Si l’exécution est bloquée :  
*Set-ExecutionPolicy -Scope Process Bypass  
  
Import-Csv .\results\ports.csv  ou  cat .\results\ports.csv  

## 4. Analyse des ports / Port analysis  

### 🇫🇷 Interpréter les résultats  
- **135, 139, 445** : Services Windows (RPC, NetBIOS, SMB)
  À bloquer sur interface publique  
- **80, 443** : Services web  
- **22, 3389** : Accès distant (SSH, RDP) À sécuriser   
- **> 49152** : Ports dynamiques Windows, peu risqué 
- Identifier un processus :

Get-NetTCPConnection -State Listen | Select-Object LocalPort, OwningProcess

## 🇬🇧 Interpreting results  
- **135, 139, 445** : Windows services (RPC, NetBIOS, SMB)
  Block on public interfaces  
- **80, 443** : Web services  
- **22, 3389** : Remote access (SSH, RDP).  Must be secured 
- **> 49152** : Windows dynamic ports.  Usually safe  
- Identify the process:
Get-NetTCPConnection -State Listen | Select-Object LocalPort, OwningProcess


# Sources

- ANSSI — Recommandations de sécurité relatives aux services réseau  
- ANSSI — Guide d’hygiène informatique (https://messervices.cyber.gouv.fr/documents-guides/guide_hygiene_informatique_anssi.pdf)
- ANSSI — Recommandations de sécurisation des systèmes Windows  
