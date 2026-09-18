# ITS — déclaration mensuelle État 301 (e-Impôts)

L'ITS (impôt sur les traitements et salaires) est **retenu à la source chaque mois** et déclaré sur e-Impôts (impots.finances.ci) via l'**État 301** — la liste nominative des salariés. La déclaration s'accompagne du **paiement** et, en pratique, de la **transmission EDI** de l'annexe détaillée (voir `04-edi-xml-eimpots.md`).

## Les 23 champs par ligne — ORDRE OFFICIEL

```
1  numero_cnps                        N° CNPS du salarié (ex. 188021640501)
2  identite                           Nom et prénoms (MAJUSCULES)
3  emploi_qualite                     Emploi/qualité (MAJUSCULES)
4  code_emploi                        DR/CS/AM/CM/EQ/EN/OQ/ON/A
5  regime_general                     G (général) / A
6  sexe                               M / F
7  nationalite                        I (Ivoirien) / AA (Autre Africain) / F (Français) / SL (Syrien ou Libanais) / A (Autre)
8  loc_exp                            L (local) / E (expatrié)
9  situation_famille                  C (célibataire) / M (marié) / D (divorcé) / V (veuf)
10 nbre_enfants_charge_nat_cas        nombre
11 nbre_parts_igr                     parts fiscales (ex. 1, 2, 3)
12 nbre_jours_app_paiements           jours (ex. 30)
13 mnt_sala_remune_acs                salaire/rémunération (brut imposable CNPS)
14 mnt_avtgs_nat_reglm                avantage en nature réglementé
15 mnt_avtgs_nat_reele                avantage en nature réel (ex. logement)
16 sal_ttl_brut                       salaire total brut (brut imposable + avantages)
17 rev_non_imposable                  revenus non imposables (ex. transport exonéré)
18 rev_brut_imposable                 revenu brut imposable (base ITS)
19 ricf                               réduction pour charges de famille
20 its_sal_brut                       ITS sur salaire brut (avant RICF)
21 its_sal_net                       ITS net retenu
22 mnt_indemnites                    montant indemnités (ex. transport)
23 designation_indemnites             désignation (ex. TRANSPORT, MAJUSCULES)
```

## Mapping paie → État 301

| Champ État 301 | Source bulletin |
|---|---|
| identite / emploi_qualite | nom / poste du salarié |
| mnt_sala_remune_acs | brut imposable CNPS (avantages inclus) |
| mnt_avtgs_nat_reele | valeur du logement (avantage réel) |
| sal_ttl_brut | brut imposable + avantages |
| rev_non_imposable | transport exonéré |
| rev_brut_imposable | base ITS |
| ricf / its_sal_brut / its_sal_net | RICF / ITS barème / ITS net retenu |
| mnt_indemnites + designation | transport exonéré + « TRANSPORT » |
| nbre_parts_igr | parts fiscales |

**Ligne ignorée par la macro officielle** si n° CNPS, identité, ITS brut ou ITS net est vide.

## Règles

- NCC de l'entreprise + exercice + mois + code impôt en tête de la déclaration.
- codeTaxe : `ITS` (secteur privé) · `ITSPUB` (public) · `ITSPARA` (parapublic) — même code que la déclaration e-Impôts.
- Fiche salarié à compléter pour l'EDI : n° CNPS, sexe, nationalité, local/expatrié, situation familiale, nb enfants, code emploi.
- ⚠️ Échéance de dépôt/paiement : à confirmer contre la notice en vigueur (usuellement dans le mois suivant la période) — voir `07-sources-et-verifications.md`.
