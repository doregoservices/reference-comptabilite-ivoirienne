# Référence — Comptabilité & Fiscalité Côte d'Ivoire

Dossier de référence constitué pendant la construction de l'application de gestion **CaféPro** (torréfacteur de café à Abidjan) : paie, ITS, TVA, déclarations e-Impôts (EDI XML), plan comptable SYSCOHADA, compte d'exploitation.

> **Objectif** : donner à un agent (humain ou IA) une base fiable, vérifiée contre les fichiers officiels de la DGI, pour produire bulletins de paie, déclarations et tableaux de bord conformes.

## Sommaire

| Fichier | Contenu |
|---|---|
| `01-paie-ivoirienne.md` | **Barèmes complets** (CNPS, CMU, ITS réforme 2024, RICF, FDFP) + algorithme de calcul du bulletin, du brut au net, avec exemples chiffrés vérifiés |
| `02-its-etat301.md` | Déclaration ITS mensuelle — État 301 : les 23 champs officiels, référentiels, mapping |
| `03-tva-annexe.md` | TVA 18 % — annexe détaillée : les 15 champs officiels, spécifications, référentiels |
| `04-edi-xml-eimpots.md` | Format EDI XML e-Impôts : squelette, règles strictes de la macro DGI, nommage, processus de dépôt |
| `05-syscohada.md` | Plan comptable SYSCOHADA utilisé en pratique : comptes, journaux, écritures de paie |
| `06-compte-exploitation.md` | Compte d'exploitation analytique d'une unité industrielle (structure par fonction) |
| `07-sources-et-verifications.md` | Sources officielles, ce qui est vérifié, ce qui reste à vérifier |

## Règles d'usage pour un agent IA

1. **Toujours recalculer, jamais recopier** : appliquer les algorithmes de `01-paie-ivoirienne.md` (arrondis compris) plutôt que de réutiliser un montant existant.
2. **Ne jamais inventer un montant** : si une information manque (parts fiscales, plafond, base…), laisser la cellule **vide** et signaler, plutôt que deviner.
3. **Jamais de données personnelles** dans ce dépôt : NCC réels, noms, salaires réels → interdits. Utiliser le NCC fictif `1900000X` pour les exemples.
4. **Conformité DGI = répliquer la source** : la conformité d'un fichier EDI se vérifie contre le générateur officiel de la DGI (macro « Génération du XML »), pas contre une intuition.
5. Les échéances et seuils fiscaux évoluent (réformes 2024+) : croiser avec `07-sources-et-verifications.md` avant toute affirmation.

## Avertissement

Ce dossier est un outil de travail constitué à partir des fichiers officiels DGI 2026 et de la pratique d'une PME agro-industrielle. Il ne remplace pas l'avis d'un expert-comptable inscrit à l'ordre, ni les textes officiels (impots.finances.ci, dgi.ci, cnps.ci).
