# TP FortiGate – Sécurisation de deux sites

Compte rendu de travaux pratiques : déploiement de deux pare-feux **FortiGate-VM (FortiOS 7.0)** dans GNS3, avec des serveurs et postes Windows sous VMware Workstation, sur un hôte Linux (Kali).

**Auteurs :** GANIOU Naofal Akanni · DARAMUS Ryann · BILAZA Tchaa Jeff

📄 **[Lire le rapport complet (PDF)](Rapport_TP_FortiGate.pdf)**

## Contenu du TP

| Étape | Sujet | Statut |
|---|---|---|
| 1 | Préparation de la maquette (VMware, GNS3, plan d'adressage) | ✅ |
| 2 | Configuration de base de FTG1-Site1 (interfaces, routage, administration) | ✅ |
| 3 | Objets, règles de pare-feu et NAT, diagnostic `debug flow` | ✅ |
| 4 | Filtrage Web selon les groupes Active Directory (FSSO sans agent) | ✅ |
| 5 | Sauvegarde automatique planifiée (automation stitch + FTP) | ✅ |
| 6 | Site 2 et VPN IPsec site à site (IKEv2, VPN par route) | ✅ |
| 7 | Détection et prévention d'intrusion (IPS) | ✅ |
| 8 | VPN SSL pour les télétravailleurs | ⏭️ Non réalisée (consigne) |
| 9 | Haute disponibilité (HA) | ⏭️ Non réalisée (consigne) |
| 10 | SD-WAN : deux accès Internet avec SLA et bascule | ✅ |

## Architecture

```
                    Internet (NAT GNS3)
                           │
        ┌──────────────────┴──────────────────┐
   FTG1-Site1  ◄════ VPN IPsec IKEv2 ════►  FTG2-Site2
   port3 │ port2                            port3 │ port2
  LAN-PDT  LAN-SRV                         LAN-PDT  LAN-SRV
192.168.1.0/24  192.168.2.0/28       192.168.21.0/24  192.168.22.0/28
   PDT1     SRV-DC1 (AD, DNS, DHCP, FTP)     PDT2     SRV2 (IIS)
```

## Environnement

- FortiGate-VM 7.0.9 (licence d'évaluation 15 jours) dans GNS3
- Windows Server 2022 et Windows 10 dans VMware Workstation Pro
- Hôte : Kali Linux (serveur GNS3 local avec KVM)

## Arborescence

```
.
├── Rapport_TP_FortiGate.pdf   # compte rendu complet (41 pages)
└── captures/                  # captures d'écran brutes, numérotées selon le sujet
```

> Les mots de passe présents dans le rapport et les captures sont des valeurs de laboratoire.
