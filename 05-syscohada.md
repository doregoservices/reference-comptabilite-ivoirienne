# Plan comptable SYSCOHADA — usage pratique (approuvé dans une PME industrielle)

Cadre : **SYSCOHADA révisé** (OHADA). Ci-dessous le plan réellement utilisé, journal par journal. (Montants FCFA.)

## Journaux

| Code | Journal | Usage |
|---|---|---|
| VE | Ventes | factures clients |
| AC | Achats | factures fournisseurs |
| CA | Caisse | espèces encaissées/décaissées |
| BQ | Banque | mouvements bancaires |
| PA | Paie | bulletins + charges sociales |
| OD | Opérations diverses | régularisations, virements internes, OD de clôture |

## Comptes utilisés

**Classes 1–5 (bilan)**
| Compte | Intitulé |
|---|---|
| 101000 | Capital / apports |
| 401100 | Fournisseurs locaux — dettes |
| 411100 | Clients locaux — créances |
| 421100 | Personnel — avances et acomptes |
| 422000 | Personnel — rémunérations dues (net à payer) |
| 428000 | Personnel — autres retenues |
| 431000 | CNPS — sécurité sociale (dette) |
| 447200 | État — impôts sur salaires ITS (dette) |
| 521000 | Banques |
| 552100 | Mobile Money — monnaie électronique |
| 571000 | Caisse espèces |
| 585000 | Virements de fonds — compte de passage |

**Classes 6–7 (gestion)**
| Compte | Intitulé |
|---|---|
| 602100 | Achats de matières premières (café vert) |
| 604200 | Matières combustibles (carburant) |
| 605200 | Électricité (eau : 6051) |
| 605800 | Achats divers — travaux, matériels |
| 608100 | Emballages perdus |
| 612000 | Transports sur ventes (sur achats : 611) |
| 622000 | Locations et charges locatives |
| 624000 | Entretien, réparations et maintenance |
| 641200 | Impôts et taxes — patente et licences (vignettes : 6463) |
| 641300 | ITS employeur — 1,2 % du brut |
| 641400/641500 | FDFP — taxe d'apprentissage / formation continue |
| 661000 | Salaires et rémunérations |
| 664100 | Charges sociales CNPS employeur |
| 702000 | Ventes de produits finis |

## Écritures de paie (journal PA)

Au paiement d'un bulletin :
```
661000 Salaires (brut imposable)                      D
641300 ITS employeur 1,2 %                            D
641400 FDFP (0,4 % + 1,2 %)                           D
664100 CNPS employeur (retraite 7,7 % + PF + mat. + AT + CMU 500)   D
    422000 Personnel — net à payer                                    C
    431000 CNPS (retenue 6,3 % + part patronale)                      C
    447200 État — ITS net retenu + ITS employeur                      C
```
Puis règlement : `422000 D / 571000 ou 521000 C` · CNPS : `431000 D / 521000 C` · ITS : `447200 D / 521000 C`.

**Contrôle** : débit 661+6413+6414+6641 = crédit 422+431+4472 = coût employeur du bulletin.

## Autres écritures types

- Virement caisse → Mobile Money : `552100 D / 571000 C` (ou via 585000 en deux OD si passage par compte de passage).
- Achat matière : `602100 D (+445 État TVA récupérable si assujetti) / 401100 ou 571000 C`.
- Vente : `411100 ou 571000 D / 702000 C (+4431 État TVA facturée)`.

## Règles de prudence

- Ne jamais créer de compte « hors plan » sans intitulé SYSCOHADA clair.
- Mobile Money = trésorerie (5521) — à rapprocher comme la caisse et la banque.
- Les dépôts/versements au propriétaire ne sont PAS des charges : compte de l'exploitant (103/104) ou association (455), jamais la classe 6.
