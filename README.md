# Projecte Transversal ASIXc — InnovateTech  
### CPD · Serveis Multimèdia · AWS · Base de Dades · Seguretat · Ansible

---

## 📌 Índex
1. [Descripció General del Projecte](#descripció-general-del-projecte)  
2. [Objectius](#objectius)  
3. [1. Proposta de CPD](#1-proposta-de-cpd)  
   - [Ubicació Física](#ubicació-física)  
   - [Mesures de Camuflatge](#mesures-de-camuflatge)  
   - [Climatització i Condicions Ambientals](#climatització-i-condicions-ambientals)  
   - [Terra Tècnic, Sostre Tècnic i Cablejat](#terra-tècnic-sostre-tècnic-i-cablejat)  
   - [Estructura dels Racks](#estructura-dels-racks)  
   - [Infraestructura IT](#infraestructura-it)  
   - [Infraestructura Elèctrica](#infraestructura-elèctrica)  
   - [Seguretat Física i Lògica](#seguretat-física-i-lògica)  
4. [2. Implantació dels Serveis d’Àudio i Vídeo](#2-implantació-dels-serveis-dàudio-i-vídeo)  
   - [Servei d’Àudio](#servei-dàudio)  
   - [Servei de Vídeo + Videoconferència](#servei-de-vídeo--videoconferència)  
   - [Proves d’Amplada de Banda](#proves-damplada-de-banda)  
5. [3. Base de Dades MariaDB](#3-base-de-dades-mariadb)  
   - [Context](#context)  
   - [Disseny i Implementació](#disseny-i-implementació)  
   - [Rols i Permisos](#rols-i-permisos)  
   - [Triggers](#triggers)  
   - [Events i Backups](#events-i-backups)  
   - [Script de Creació d’Usuaris](#script-de-creació-dusuaris)  
   - [Diagrama ER](#diagrama-er)  
6. [4. AWS i Automatització](#4-aws-i-automatització)  
7. [5. Conclusions](#5-conclusions)

---

# Descripció General del Projecte

InnovateTech requereix una infraestructura completa que integri:

- Un **CPD segur i sostenible**  
- Serveis multimèdia (àudio, vídeo i videoconferència)  
- Un sistema de **gestió interna** basat en MariaDB  
- Automatització amb **Ansible**  
- Infraestructura desplegada a **AWS**  
- Documentació en **Markdown** i publicació a GitHub  

Aquest projecte cobreix tots els blocs del mòdul transversal ASIXc.

---

# Objectius

- Dissenyar i implantar un **CPD realista i segur**  
- Implementar serveis d’**àudio, vídeo i videoconferència**  
- Realitzar proves d’**amplada de banda**  
- Crear una **base de dades completa** amb rols, triggers i auditoria  
- Automatitzar servidors amb **Ansible**  
- Documentar-ho tot en **Markdown**  

---

# 1. Proposta de CPD

## Ubicació Física

El CPD s’ubica a la **planta subterrània**, amb:

- Accés restringit per **lector RFID**  
- Passadís perimetral de seguretat  
- Sala amagada darrere d’un magatzem  
- Esclusa *mantrap* amb:
  - Petjada dactilar  
  - Reconixement facial  
  - Targeta RFID  

Aquesta ubicació garanteix:

- Protecció contra intrusions  
- Aïllament tèrmic  
- Invisibilitat des de l’exterior  

---

## Mesures de Camuflatge

- Cap rètol ni indicació visible  
- Porta simple que dona accés a un magatzem fals  
- CPD ocult darrere d’una segona porta  
- Esclusa biomètrica integrada al mobiliari  

---

## Climatització i Condicions Ambientals

Compliment de la normativa **ASHRAE**:

| Paràmetre | Valor |
|----------|-------|
| Temperatura passadís fred | **18 °C** |
| Temperatura passadís calent | **35 °C** |
| Humitat relativa | **40–55%** |
| Filtratge | HEPA / MERV 13 |
| Sistemes | CRAC + CRAH + Free Cooling |

---

## Terra Tècnic, Sostre Tècnic i Cablejat

### Terra tècnic (40 cm)
- Impulsió d’aire fred  
- Cablejat elèctric de potència  

### Sostre tècnic (50 cm)
- Retorn d’aire calent  
- Canonades FM‑200  
- Tubs VESDA  

### Cablejat
- **Dades per dalt** (Cat6A + fibra)  
- **Potència per baix**  
- Passarel·la metàl·lica tipus Cablofil  

---

## Estructura dels Racks

### Rack 1 — Core + Web + LDAP + Logs
- Patch panel Cat6A  
- Cisco 2960 (L2)  
- Cisco 3650 (L3 Core)  
- SRV‑01 Web + SFTP  
- SRV‑02 LDAP  
- SRV‑03 Logs  
- PDU A/B  
- SAI + Battery Pack  

### Rack 2 — Àudio + Vídeo + BD
- Patch panel Cat6A  
- Cisco 2960 (L2)  
- SRV‑04 Àudio (Icecast2)  
- SRV‑05 Vídeo + Jitsi  
- SRV‑06 Base de Dades  
- PDU A/B  
- SAI + Battery Pack  

---

## Infraestructura IT

| Servidor | Funció | CPU | RAM | Disc | VLAN |
|---------|--------|-----|-----|------|------|
| SRV‑01 | Web + SFTP | Xeon 4314 | 16GB | 2×480GB RAID1 | 20 |
| SRV‑02 | LDAP | Xeon 4310 | 16GB | 2×480GB RAID1 | 20 |
| SRV‑03 | Logs | Xeon 4314 | 32GB | 4×960GB RAID10 | 40 |
| SRV‑04 | Àudio | Xeon 4310 | 16GB | 2×480GB RAID1 | 30 |
| SRV‑05 | Vídeo + Jitsi | Xeon 4314 | 32GB | 4×960GB RAID10 | 30 |
| SRV‑06 | Base de Dades | Xeon 4314 | 32GB | 4×960GB RAID10 | 40 |

---

## Infraestructura Elèctrica

- Doble circuit A/B  
- PDU redundants  
- SAI APC SRT5000RMXLI  
- Battery Pack SRT192BP  
- Autonomia total: **80 minuts**  

---

## Seguretat Física i Lògica

### Física
- CCTV 360°  
- VESDA  
- FM‑200  
- Extintors CO₂  
- Control d’accés biomètric  
- Porta tallafocs RF60 / RF90  

### Lògica
- Firewall  
- Monitorització  
- Backups  
- RAID  
- Control d’accés per rols  

---

# 2. Implantació dels Serveis d’Àudio i Vídeo

## Servei d’Àudio

### Software
- **Icecast2**  
- **Liquidsoap**  

### Configuració
- Hostname: `35.174.72.4`  
- Contrasenyes configurades  
- Música a `/opt/music`  
- Liquidsoap generant stream MP3 128kbps  

### Resultat
- Canal d’àudio funcional  
- Accés via navegador  
- Reproducció estable  

---

## Servei de Vídeo + Videoconferència

### Software
- **NGINX + RTMP**  
- **HLS**  
- **Jitsi Meet**  

### Configuració
- RTMP → HLS automàtic  
- Segments `.ts` de 3s  
- Reproductor HTML5 personalitzat  
- Jitsi operatiu a HTTPS  

### Resultat
- Vídeo en streaming funcional  
- VOD funcional  
- Videotrucada operativa  

---

## Proves d’Amplada de Banda

### Resultats
- Download: **1748 Mbps**  
- Upload: **1828 Mbps**  
- Latència: **1 ms**  

### Conclusió
**Sistema totalment acceptable** per a streaming i videoconferència simultània.

---

# 3. Base de Dades MariaDB

## Context

La BD gestiona:

- Personal  
- Departaments  
- Comunicacions internes  
- Trucades  
- Vídeos  
- Configuració de servidors  
- Mesures d’amplada de banda  
- Auditoria  
- Backups  

---

## Disseny i Implementació

Taules principals:

- `departaments`  
- `empleats`  
- `nomines`  
- `usuaris_comunicacio`  
- `grups_qualitat`  
- `config_servidor`  
- `registre_trucades`  
- `videos`  
- `mesures_ample_banda`  
- `avisos_auditoria`  
- `control_backups`  

---

## Rols i Permisos

Rols implementats:

- **admin** — Accés total  
- **vendes** — Clients, trucades, vídeos  
- **administracio** — Personal, nòmines, departaments  
- **treballador** — Lectura general + inserció de trucades  

Tots els rols comprovats amb `SHOW GRANTS`.

---

## Triggers

Triggers implementats:

- `tg_antifrau_nomines` — Protegeix nòmines  
- `audit_modificacio_restringida` — Control d’empleats  
- `check_quota_minuts` — Límit de minuts mensuals  
- `check_quota_diaria` — Límit de trucades diàries  
- `check_usuari_bloquejat` — Bloqueig d’usuaris  
- `tg_control_trucades_diaries`  
- `tg_control_minuts_i_estat`  

Tots testejats amb errors **1644**.

---

## Events i Backups

Event diari:

- Exporta taules crítiques amb `SELECT ... INTO OUTFILE`  
- Registra execució a `control_backups`  
- `event_scheduler = ON`  

---

## Script de Creació d’Usuaris

Script Bash interactiu:

- Demana nom, host, contrasenya i rol  
- Genera SQL automàtic  
- Executa `CREATE USER` i `GRANT`  
- Guarda resultats a `/home/adminbd/usuaris_generats.sql`  

---

## Diagrama ER

Inclou:

- Personal (1:N) Departaments  
- Empleats (1:1) Nòmines  
- Empleats (1:1) Usuaris Comunicació  
- Usuaris (1:N) Registre Trucades  
- Vídeos independents  
- Configuració de servidors  
- Auditoria i Backups desacoblats  

---

# 4. AWS i Automatització

- 6 instàncies EC2  
- Ubuntu 22.04 / 24.04  
- Clau SSH  
- Usuari no root  
- Accés per clau pública  
- 2 servidors configurats amb **Ansible**  

---

# 5. Conclusions

La infraestructura desplegada compleix tots els requisits del projecte:

- CPD segur, sostenible i realista  
- Serveis multimèdia completament funcionals  
- Base de dades robusta amb control d’accés i auditoria  
- Automatització amb Ansible  
- Documentació completa en Markdown  

El sistema és **estable, escalable i preparat per a producció**.

---
