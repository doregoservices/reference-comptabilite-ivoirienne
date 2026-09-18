# Sources officielles et état des vérifications

## Ce qui est vérifié (contre fichiers officiels DGI 2026)

| Élément | Source de vérification |
|---|---|
| Barème ITS 2024 (0/16/21/24/28/32 %, bornes 75 000/240 000/800 000/2 400 000/8 000 000) | formules du classeur DGI État 301 v1.2 (feuille SAISIE) |
| Cumuls aux bornes (26 400 · 144 000 · 528 000 · 2 096 000) | formules du classeur DGI |
| RICF 5 500 F par ½ part (1 part = 0, 2 parts = 11 000, max 5 parts = 44 000) | formules du classeur DGI |
| 23 champs État 301 + référentiels + règles d'ignore de ligne | feuille TradXML + macro MiseEnFormeEDI du classeur DGI |
| 15 champs annexe TVA + spécifications | classeur DGI annexe TVA v2.1 |
| Règles XML (CRLF, remplacements de caractères, UPPER, nommage `NCC-EDI-<type>-aaaammjj-hhmm.xml`) | code VBA de la macro officielle |
| Procédure e-Impôts étape 3 « Transmission des annexes détaillées par EDI » ; pièces ≤ 2 Mo ; citation « libre de générer un fichier XML par un autre moyen » | feuille « A SAVOIR » du classeur DGI |
| CNPS (6,3 / 7,7 / 5 / 0,75 / 3 % ; plafonds 3 375 000 et 70 000), CMU 1 000 (500+500), ITS employeur 1,2 %, FDFP 1,6 %, exonération transport 30 000/24 000/20 000 | pratique consolidée + paramètres de l'app, cohérents avec les barèmes DGI ci-dessus |

## Sites officiels à consulter pour tout complément

- **DGI / e-Impôts** : `impots.finances.ci` (déclarations en ligne, notices) · `dgi.ci`
- **CNPS** : `cnps.ci` (barèmes cotisations, plafonds)
- **OHADA** : `ohada.org` (SYSCOHADA révisé, actes uniformes)
- **Direction Générale du Trésor** (taux et régimes) : `tresor.economie.gouv.fr` (fiches pays Côte d'Ivoire)

## ⚠️ À vérifier avant usage (non confirmé contre source officielle)

1. **Échéances de dépôt** de l'État 301 et de la TVA (usuellement le mois suivant la période — confirmer la date limite exacte en vigueur).
2. **Seuils TVA réforme 2024** (assujettissement / remboursement de crédit) selon le chiffre d'affaires.
3. **BIC/IS et autres impôts** : taux selon la taille de l'entreprise (paramètres d'app : BIC 25 %, TEE 5 % — à confirmer par les textes).
4. Taux AT (2 à 5 % selon le risque métier) — le 3 % « industrie » est une convention.
5. Le barème ITS **journalier** (÷ 30) pour les paies courtes : vérifier les bornes jour dans le classeur DGI avant de l'appliquer.
6. RICF : vérifier si le plafond de 44 000 F (5 parts) a évolué.

## Historique

- Constitution : septembre 2026, à partir des classeurs officiels DGI 2026 (générateurs État 301 / annexe TVA) et de la construction de l'app CaféPro (v35.x).
- Convention : NCC fictif `1900000X` pour tous les exemples ; aucune donnée réelle dans ce dépôt.
