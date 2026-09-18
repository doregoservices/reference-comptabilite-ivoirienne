# TVA — annexe détaillée (e-Impôts)

## Principes

- Taux de TVA en Côte d'Ivoire : **18 %** (taux normal).
- Régime de déclaration : **mensuel** en pratique pour une PME industrielle ; l'annexe détaillée des achats/ventes est transmise par EDI avec la déclaration.
- TVA déductible sur les achats (locaux, importations) ; collectée sur les ventes ; la différence = TVA à payer (ou crédit de TVA reportable).

## Les 15 champs par ligne — ORDRE OFFICIEL

```
1  type_operation_odr         Achats_locaux / Achats_étrangers / Importations
2  specification              libellé officiel de la nature (voir ci-dessous)
3  date_facture               JJ/MM/AAAA
4  raison_sociale_fournisseur
5  ncc_fournisseur            NCC du fournisseur
6  pays_fournisseur
7  ref_facture                référence de la facture
8  nature_bien_odr            nature du bien ou de l'opération
9  date_reglement             JJ/MM/AAAA
10 montant_ht                 montant hors taxes
11 montant_val_douane         valeur en douane (importations)
12 montant_tva                TVA
13 type_redevable             REDEVABLE TOTAL / REDEVABLE PARTIEL
14 prorata_deduction          1 par défaut (redevable partiel : < 1)
15 montant_taxe_deductible    TVA déductible
```

## Spécifications officielles (référentiel DGI)

Exemples de valeurs exactes attendues dans `specification` (orthographe officielle conservée, y compris les particularités — ex. « Achats de marchandises et matières prémières locales ») : reprendre **mot pour mot** la liste du référentiel du classeur officiel avant de générer.

## Règles

- Ligne ignorée si type / date / montant vides.
- La TVA déductible = montant_tva × prorata (redevable partiel).
- Fournisseurs : ajouter le **NCC fournisseur** à la fiche pour l'EDI.
- Montants au format **virgule décimale** (ex. `20535,36`).
- ⚠️ Seuils d'assujettissement et régime réel/simplifié (réforme 2024) : à vérifier contre les textes en vigueur — voir `07-sources-et-verifications.md`.
