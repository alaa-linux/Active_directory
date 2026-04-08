# 🏦 Active Directory - Gestion des accès et des privilèges

## 📌 Description du projet

Ce projet consiste à concevoir et déployer un environnement **Active Directory** sous Windows Server dans un contexte métier bancaire.  
L’objectif est de mettre en place un annuaire, des groupes de sécurité, des utilisateurs, des droits d’accès sélectifs et des postes clients intégrés au domaine.  
Le projet inclut également la mise en place de scripts PowerShell pour la supervision et la sécurité.

---

## 🎯 Objectifs du projet

- Installer et configurer Active Directory Domain Services  
- Créer des utilisateurs et groupes  
- Mettre en place des droits d’accès selon les rôles métiers  
- Joindre des postes clients au domaine  
- Vérifier les permissions  
- Automatiser avec PowerShell  
- Mettre en place des mécanismes de sécurité  

---

## 🧠 Présentation d’Active Directory

Active Directory permet :

- l’authentification centralisée  
- la gestion des utilisateurs et ordinateurs  
- la gestion des groupes de sécurité  
- l’application de stratégies (GPO)  
- la gestion des droits d’accès  

---

## 🏗️ Architecture

- 1 Windows Server → contrôleur de domaine  
- 1 ou plusieurs postes clients  
- organisation par rôles métiers  

---

## 👥 Modèle de droits métier

### Catégorie A
- Lecture uniquement  

### Catégorie B
- Opérations jusqu’à 10 000€  

### Catégorie C
- Jusqu’à 100 000€ avec validation  

### Catégorie D
- Accès total  

### Autres
- Clients → lecture  
- IT → lecture + exécution  
- Chef de projet → modification  

---

## ⚙️ Installation Active Directory

### Installation du rôle

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

### Création du domaine

```powershell
Install-ADDSForest `
  -DomainName "creditindustriel.local" `
  -DomainNetbiosName "CREDITINDUSTRIEL" `
  -InstallDNS
```

### Vérification

```powershell
Get-Service ADWS
Get-ADDomain
Get-ADForest
```

---

## 🗂️ Organisation (OU)

```powershell
New-ADOrganizationalUnit -Name "OU_Utilisateurs" -Path "DC=creditindustriel,DC=local"
New-ADOrganizationalUnit -Name "OU_Groupes" -Path "DC=creditindustriel,DC=local"
New-ADOrganizationalUnit -Name "OU_Postes" -Path "DC=creditindustriel,DC=local"
```

---

## 🔐 Création des groupes

```powershell
New-ADGroup -Name "Grp_Categorie_A" -GroupScope Global -GroupCategory Security
New-ADGroup -Name "Grp_Categorie_B" -GroupScope Global -GroupCategory Security
New-ADGroup -Name "Grp_Categorie_C" -GroupScope Global -GroupCategory Security
New-ADGroup -Name "Grp_Categorie_D" -GroupScope Global -GroupCategory Security

New-ADGroup -Name "Grp_Clients_Lecture" -GroupScope Global -GroupCategory Security
New-ADGroup -Name "Grp_IT_Execute" -GroupScope Global -GroupCategory Security
New-ADGroup -Name "Grp_ChefsProjet_Modif" -GroupScope Global -GroupCategory Security
```

---

## 👤 Création des utilisateurs

```powershell
New-ADUser `
  -Name "Amelie Lotte" `
  -SamAccountName "am.lotte" `
  -UserPrincipalName "am.lotte@creditindustriel.local" `
  -AccountPassword (ConvertTo-SecureString "Laplateforme.io" -AsPlainText -Force) `
  -Enabled $true
```

Ajout au groupe :

```powershell
Add-ADGroupMember -Identity "Grp_Categorie_A" -Members "am.lotte"
```

---

## 🖥️ Joindre un poste au domaine

```powershell
Add-Computer -DomainName "creditindustriel.local" -Credential "CREDITINDUSTRIEL\Administrator" -Restart
```

Vérification :

```powershell
systeminfo | findstr /B /C:"Domaine"
```

---

## 📁 Gestion des droits NTFS

```powershell
icacls "C:\Dossiers\Operations" /grant "CREDITINDUSTRIEL\Grp_Categorie_A:(R)"
icacls "C:\Dossiers\Operations" /grant "CREDITINDUSTRIEL\Grp_Categorie_B:(M)"
icacls "C:\Dossiers\Operations" /grant "CREDITINDUSTRIEL\Grp_Categorie_C:(M)"
icacls "C:\Dossiers\Operations" /grant "CREDITINDUSTRIEL\Grp_Categorie_D:(F)"
```

- R = Lecture  
- M = Modification  
- F = Full control  

---

## 🛡️ Script PowerShell (surveillance)

```powershell
$fileToMonitor = "C:\systeme-banque\operations\file.txt"
$maxModificationsPerDay = 3
$currentDate = Get-Date -Format "yyyy-MM-dd"

if (Test-Path $fileToMonitor) {
    $fileDetails = Get-Item $fileToMonitor
    Write-Host "Fichier surveillé"
}
```

Fonctions utilisées :
- Test-Path  
- Get-Item  
- Get-Date  
- Where-Object  
- Measure-Object  
- New-Item  

---

## 📊 Commandes utiles

```powershell
Get-ADUser -Filter *
Get-ADGroup -Filter *
Get-ADComputer -Filter *
Get-ADGroupMember "Grp_Categorie_B"
Get-ADOrganizationalUnit -Filter *
Get-SmbShare
icacls "C:\Dossiers\Operations"
```

---

## 🧪 Tests réalisés

- connexion utilisateurs  
- vérification permissions  
- test scripts PowerShell  
- vérification domaine  

---

## 🔒 Sécurité

- principe du moindre privilège  
- restriction des accès  
- audit des connexions  
- désactivation comptes inactifs  
- GPO de sécurité  
- surveillance activité  

---

## 🧰 Technologies utilisées

- Windows Server  
- Active Directory  
- PowerShell  
- NTFS  
- GPO  

---

## 📈 Compétences développées

- Administration système  
- Gestion des accès  
- Active Directory  
- PowerShell  
- Sécurité  

---

## 📌 Conclusion

Projet complet permettant de :

- comprendre Active Directory  
- gérer des accès métiers  
- sécuriser un système  
- automatiser avec PowerShell  

---

## 👨‍💻 Auteur

Alaa Kaid Ahmed
