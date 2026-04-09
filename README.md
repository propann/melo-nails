# melo-nails
site  autoentrepreneur
📄 MicroERP Pi — Cahier des charges V2
1. Vision et Objectif Produit
Créer une plateforme tout-en-un, auto-hébergée sur Raspberry Pi 5, dédiée au pilotage intégral d'une micro-entreprise.
Le système ne doit posséder qu'un seul point d'entrée (un seul back-office) pour gérer le site public, les rendez-vous, les clients, les stocks et la facturation.

Cible initiale : Prothésie ongulaire / Esthétique.

Mantra : Zéro friction. Si une action prend plus de 3 clics, le système est trop complexe.

2. Architecture Technique (La "Stack" ultra-simplifiée)
Pour éviter la dispersion et les API fragiles, nous abandonnons la séparation stricte Front/Back au profit d'un écosystème unifié, soutenu par une infrastructure robuste.

Matériel : Raspberry Pi 5 (8 Go) + SSD NVMe (Crucial) + Alimentation officielle 27W.

Système OS : Debian / Raspberry Pi OS 64-bit Lite (sans interface graphique).

Orchestration : Docker + Docker Compose.

Réseau & Sécurité : Traefik ou Nginx Proxy Manager (pour la gestion visuelle et automatisée des certificats HTTPS Let's Encrypt) + Fail2Ban.

Le Cœur Applicatif (Recommandation) : WordPress All-in-One.

Pourquoi ? C'est le seul outil capable de faire vitrine, e-commerce (WooCommerce), réservation pro (Amelia ou Bookly) et génération de factures légales/CRM simple sur une seule base de données, gérable depuis un smartphone.

Alternative "ERP pur" : Odoo Community (plus lourd, mais tout-en-un natif).

3. Périmètre Fonctionnel (Ce que l'outil fait)
A. Le Front Public (Ce que voit la cliente)

Vitrine : Présentation, galerie de réalisations avant/après, FAQ, formulaire de contact.

Réservation : Sélection de la prestation (pose complète, remplissage, nail art), choix du créneau, calcul automatique de la durée.

Boutique légère : Vente de produits d'entretien (huiles cuticules, etc.) en click-and-collect ou livraison.

Espace Client : Historique des rendez-vous, factures, annulation/report autonome.

B. Le Back-Office "Poste de Commandement" (Ce que voit la gérante)

CRM (Clients) : Fiches clientes automatisées à chaque réservation, historique d'achats, notes internes (allergies, préférences), base email/SMS.

Agenda : Vue calendrier (jour/semaine), blocage des indisponibilités, temps de "buffer" (nettoyage entre deux clientes).

Ventes & Facturation : Création de devis et factures (générés automatiquement lors d'une réservation ou commande), gestion des acomptes, suivi des paiements.

Stock : Inventaire simple des produits à la vente et alertes de seuil bas pour les consommables (vernis).

Tableau de bord : Chiffre d'affaires du mois, rendez-vous du jour, paiements en attente.

4. Sécurité, Sauvegarde et Conformité (Non-négociable)
Le Raspberry Pi hébergeant la vie de l'entreprise, ces éléments sont vitaux :

Chiffrement : HTTPS obligatoire partout.

Sauvegarde Règle des 3-2-1 : Script quotidien automatique (via Restic ou BorgBackup) sauvegardant la base de données et les fichiers. Envoi automatique de l'archive chiffrée sur un Cloud tiers (ex: Google Drive, AWS S3, ou NAS distant).

Conformité : Pages de Mentions Légales et CGV/CGU. Case à cocher RGPD lors de la réservation et conservation limitée des données.

Accès distant : Le back-office ne doit pas être indexé par les moteurs de recherche. Connexion via VPN (Tailscale/Wireguard) recommandée pour l'administration pure de l'OS.

5. Feuille de Route du Déploiement
Phase 1 — L'Infrastructure (Le socle de béton) : Installation OS, Docker, Reverse Proxy, configuration du nom de domaine et sécurisation du serveur. Mise en place des scripts de backup.

Phase 2 — Le MVP Métier (Le produit minimum) : Déploiement de l'applicatif unifié. Création des pages statiques (Accueil, Tarifs, Galerie). Paramétrage strict du module de réservation (agenda, prestations, durée).

Phase 3 — Administratif & CRM : Configuration de la facturation automatique, des modèles d'emails transactionnels (confirmations de rendez-vous) et des mentions légales.

Phase 4 — Extension (Boutique & Paiement) : Activation du paiement en ligne (Stripe/PayPal) pour la prise d'acompte à la réservation et lancement du catalogue de produits annexes.

Phase 5 — Exploitation : Tests de restauration à froid (simuler une panne du Pi et vérifier qu'on peut tout remonter en moins d'une heure).
