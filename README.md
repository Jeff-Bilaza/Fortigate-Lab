# Sécurisation d'une infrastructure multi-site avec FortiGate

Conception, déploiement et validation d'une architecture réseau sécurisée sur deux sites, protégée par des pare-feux **FortiGate (FortiOS 7.0)** et intégrée à un domaine **Active Directory**. L'ensemble est réalisé dans un laboratoire virtualisé (GNS3 + VMware Workstation sur Kali Linux), et chaque fonction est vérifiée par des tests et des outils de diagnostic.

**Auteur :** Jeff BILAZA

📄 **[Documentation technique complète (PDF, 41 pages)](Rapport_Lab_FortiGate.pdf)** : configurations CLI, analyses et captures de validation.

---

## Compétences mises en œuvre

`FortiGate / FortiOS` · `Pare-feu à états & NAT` · `VPN IPsec IKEv2` · `IPS` · `SD-WAN` · `Active Directory / LDAP` · `FSSO` · `Automatisation` · `GNS3` · `VMware` · `Windows Server` · `Diagnostic réseau`

## Réalisations

| Domaine | Ce qui a été mis en place | Validation |
|---|---|---|
| **Fondations réseau** | Plan d'adressage sur deux sites, interfaces, routage, administration sécurisée (SSH, accès restreints par interface) | Table de routage, tests de connectivité |
| **Politique de sécurité** | Objets, services personnalisés pour AD, règles par flux avec NAT sortant, journalisation y compris des refus implicites | `diagnose debug flow`, compteurs de règles, journaux de trafic |
| **Contrôle d'accès par identité** | Navigation Web autorisée selon l'appartenance à un groupe AD, avec lecture des journaux du contrôleur de domaine sans agent (FSSO polling + LDAP) | Utilisateurs autorisés et refusés identifiés nommément dans les journaux |
| **Continuité** | Sauvegarde automatique quotidienne de la configuration vers un serveur FTP (automation stitch) | Fichier de sauvegarde horodaté, exécution du déclencheur |
| **Interconnexion de sites** | VPN IPsec site à site en IKEv2, de type « par route », avec routes statiques et règles sans NAT | Tunnel monté, compteurs de chiffrement, accès Web et ping inter-sites |
| **Détection et prévention d'intrusion** | Profil IPS ciblant les serveurs, déployé d'abord en mode détection puis en mode blocage | Injection SQL détectée puis bloquée, trafic légitime préservé |
| **Haute disponibilité de l'accès Internet** | SD-WAN sur deux liens, sonde de qualité (latence, gigue, pertes), seuils SLA et règle de pilotage | Bascule automatique avec une seule requête perdue |

## Architecture

```
                    Internet
                       │
        ┌──────────────┴──────────────┐
   FTG1-Site1  ◄═══ VPN IPsec ═══►  FTG2-Site2
   (+ SD-WAN : 2 liens WAN)
   port3 │ port2                  port3 │ port2
  LAN-PDT  LAN-SRV               LAN-PDT  LAN-SRV
192.168.1.0/24  192.168.2.0/28   192.168.21.0/24  192.168.22.0/28
   Postes   SRV-DC1               Postes   SRV2
            (AD, DNS, DHCP, FTP)           (Web IIS)
```

## Environnement technique

- **Sécurité :** FortiGate-VM, FortiOS 7.0.9
- **Serveurs et postes :** Windows Server 2022 (AD DS, DNS, DHCP, IIS, FTP), Windows 10
- **Virtualisation :** GNS3 (KVM) et VMware Workstation Pro sur Kali Linux

## Points d'attention documentés

La documentation distingue ce que permet l'environnement de laboratoire de ce qu'exigerait une mise en production : chiffrement AES-256 au lieu de DES, administration en HTTPS uniquement, sauvegarde via SFTP/SCP, mises à jour FortiGuard des signatures IPS, liens SD-WAN chez deux opérateurs distincts.

## Arborescence

```
.
├── Rapport_Lab_FortiGate.pdf   # documentation technique complète
└── captures/                  # captures de validation de chaque étape
```

> Les identifiants visibles dans la documentation sont des valeurs de laboratoire.
