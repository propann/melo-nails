# Melo Nails — MicroERP auto‑hébergé

Projet de plateforme **tout‑en‑un** pour piloter une micro‑entreprise de prothésie ongulaire / esthétique, hébergée sur un **Raspberry Pi 5**.

## 🎯 Objectif

Construire un système unique avec un seul point d’entrée (back‑office) pour gérer :

- le site vitrine,
- les réservations,
- la relation client (CRM léger),
- la facturation,
- le suivi de stock,
- la sécurité et les sauvegardes.

> Mantra : **zéro friction**. Si une action prend plus de 3 clics, le système doit être simplifié.

---

## 📌 État actuel du dépôt (audit du 2026-04-10)

Le dépôt contient aujourd’hui **deux pistes techniques en parallèle** :

1. **Une vitrine Next.js (App Router)** dans `app/` avec une landing page moderne animée.
2. **Une vitrine statique HTML/CSS** dans `site-vitrine/`.
3. **Une stack Docker WordPress + MariaDB + Nginx Proxy Manager** dans `docker-compose.yml`.

### Constats importants

- Le code applicatif actif pour `npm run build` est la version **Next.js**.
- La stack Docker vise une architecture **WordPress**, non branchée sur l’app Next.js.
- Le périmètre “MicroERP” (CRM, réservations, facturation, stock) n’est pas encore implémenté dans le code.
- Les variables d’environnement listées dans `.env.example` couvrent uniquement MariaDB (pas encore de variables Next.js documentées).

👉 Décision à prendre rapidement : **conserver WordPress comme cible principale** ou **basculer full Next.js** pour éviter la dérive d’architecture.

---

## 🧱 Périmètre fonctionnel cible

### Front public (cliente)
- Présentation des services
- Galerie avant/après
- FAQ et contact
- Réservation en ligne (prestation + créneau)
- Espace client (historique, factures, annulation/report)

### Back‑office (gérante)
- CRM client (profil, notes, historique)
- Agenda (jour/semaine, indisponibilités, buffers)
- Facturation / acomptes / suivi paiements
- Stock simple + alertes de seuil
- Tableau de bord d’activité (CA, RDV, paiements en attente)

---

## ⚙️ Socle technique présent dans le repo

### Application web
- **Framework** : Next.js 15 (App Router)
- **UI** : React 19, Tailwind CSS v4, Framer Motion, Lucide icons
- **Build** : `output: 'standalone'` (compatible conteneur Node)

### Option CMS/ERP (prototype infra)
- **Reverse proxy** : Nginx Proxy Manager
- **CMS** : WordPress latest
- **Base de données** : MariaDB 10.11
- **Orchestration** : Docker Compose

---

## 🔐 Sécurité, sauvegarde et conformité (cible)

- HTTPS obligatoire
- Sauvegarde **3‑2‑1** (base + fichiers, sauvegarde chiffrée hors site)
- Pages légales (mentions, CGV/CGU) + consentement RGPD
- Back‑office non indexé + accès admin via VPN (Tailscale/WireGuard recommandé)

---

## 🚀 Lancer le projet en local

### Prérequis
- Node.js 22+
- npm 10+

### Commandes

```bash
npm install
npm run dev
```

Build de production :

```bash
npm run build
npm run start
```

> Note : `npm run lint` nécessite une configuration ESLint initiale (assistant interactif Next.js non finalisé dans ce dépôt).

---

## 🌐 Ressources du projet

- Vitrine statique initiale :
  - [`site-vitrine/index.html`](site-vitrine/index.html)
  - [`site-vitrine/styles.css`](site-vitrine/styles.css)
- Feuille de route : [`docs/ROADMAP.md`](docs/ROADMAP.md)
- Suivi opérationnel : [`docs/SUIVI_TACHES.md`](docs/SUIVI_TACHES.md)

---

## 🧭 Priorités recommandées (prochaine session)

1. Trancher l’architecture cible (**WordPress** vs **Next.js full custom**).
2. Supprimer la piste non retenue (ou la passer explicitement en archive) pour réduire la dette de décision.
3. Ajouter un document d’architecture exécutable (schéma + flux + responsabilités des services).
4. Démarrer le MVP réservation sur la pile validée.
