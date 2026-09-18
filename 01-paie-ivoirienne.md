# Paie Côte d'Ivoire — barèmes et algorithme complet du bulletin

Tous les taux ci-dessous sont ceux **en vigueur dans les fichiers officiels DGI 2026** (réforme ITS 2024) et identiques aux paramètres de l'app CaféPro. Montants en FCFA.

## 1 · Barème ITS mensuel (impôt sur traitements et salaires) — réforme 2024

ITS retenu **au mois le mois**, sur le **brut imposable** (avantages en nature inclus, transport exonéré dans la limite du plafond, rubriques non imposables exclues). Barème **progressif par tranches** :

| Tranche (brut imposable mensuel) | Taux |
|---|---|
| 0 – 75 000 | 0 % |
| 75 001 – 240 000 | 16 % |
| 240 001 – 800 000 | 21 % |
| 800 001 – 2 400 000 | 24 % |
| 2 400 001 – 8 000 000 | 28 % |
| > 8 000 000 | 32 % |

**Cumuls aux bornes** (pour vérifier toute implémentation) :
- brut 240 000 → ITS brut = 26 400
- brut 800 000 → 144 000
- brut 2 400 000 → 528 000
- brut 8 000 000 → 2 096 000

Formule officielle du classeur DGI (État 301, feuille SAISIE) : taux = `SI(brut ≤ 75000; 0; SI(≤240000; 0,16; SI(≤800000; 0,21; SI(≤2400000; 0,24; SI(≤8000000; 0,28; 0,32)))))` puis ITS = taux × brut − cumul de la tranche précédente.

> ⚠️ Le barème est exprimé **en mois** ; le fichier DGI le décline aussi **en jour** (÷ 30) pour les paies courtes.

## 2 · RICF — réduction d'impôt pour charges de famille

Déduite de l'ITS **uniquement si l'ITS brut > 0** :

| Parts fiscales | RICF |
|---|---|
| 1 part | 0 |
| 1,5 part | 5 500 |
| 2 parts | 11 000 |
| 2,5 parts | 16 500 |
| 3 parts | 22 000 |
| … (5 500 par ½ part) | … |
| 5 parts (max) | 44 000 |

**ITS net retenu = max(0 ; ITS barème − RICF)**.

## 3 · Cotisations CNPS et autres retenues salariales

| Retenue | Taux / montant | Base | Plafond de base |
|---|---|---|---|
| CNPS retraite — **part salarié** | **6,3 %** | brut imposable (avantages en nature inclus) | 3 375 000 |
| CMU — part salarié | **500 F** forfait | — | — |

## 4 · Charges patronales (à la charge de l'employeur)

| Charge | Taux | Base | Plafond de base |
|---|---|---|---|
| CNPS retraite patronale | **7,7 %** | brut imposable | 3 375 000 |
| Prestations familiales (PF) | **5 %** | brut imposable | **70 000** |
| Maternité | **0,75 %** | brut imposable | **70 000** |
| Accidents du travail (AT) | **3 %** (2 à 5 % selon le risque ; industrie ≈ 3 %) | brut imposable | **70 000** |
| CMU employeur | **500 F** forfait | — | — |
| ITS employeur (appel « impôt sur salaires ») | **1,2 %** | brut imposable | — |
| FDFP — taxe d'apprentissage (TA) | **0,4 %** | brut imposable | — |
| FDFP — formation prof. continue (TFPC) | **1,2 %** | brut imposable | — |

⚠️ PF, maternité et AT sont **plafonnés à 70 000 F de base** → à partir d'un brut de 70 000, ces trois charges deviennent fixes : PF 3 500 · maternité 525 · AT 2 100.

## 5 · Exonération de la prime de transport (ITS)

La prime de transport est **non imposable dans la limite** de :

| Zone | Plafond exonéré / mois |
|---|---|
| Abidjan | 30 000 |
| Bouaké | 24 000 |
| Autres localités | 20 000 |

Le surplus éventuel devient imposable (à intégrer au brut imposable).

## 6 · Règles de base

- **Base jour = 30** (déduction absence = base × jours d'absence / 30 ; prorata du transport pareil).
- **Arrondi** : à chaque étape, arrondi « commercial » JS/Excel (`Math.round` = `floor(x + 0,5)`). Un recalcul doit reproduire ces arrondis exacts, sinon le net ne tombe pas juste.
- Avantages en nature (logement…) : **imposables CNPS et ITS** (intégrés au brut), versés avec le net en nature.
- Rubriques non imposables (transport dans la limite, représentation, salissure…) : exonérées d'ITS, **hors base CNPS**, versées en espèces avec le net.

## 7 · Algorithme du bulletin (brut → net)

```
entrées : base, transport, primes[], parts, zone, absences
brut_ap          = arrondi(base + logement + primes_taxables − déduction_absences)
parts_f          = min(5, max(1, parts))
ricf             = min(5500 × (parts_f − 1) × 2, 44000)        # par ½ part au-delà de la 1re
cnps             = arrondi(min(brut_ap, 3 375 000) × 6,3 %)
cmu              = 500
transport_payé   = arrondi(transport × (30 − absences) / 30)
transport_exo    = min(transport_payé, plafond_zone)
its_brut         = barème_progressif(brut_ap)                    # §1
its              = max(0, its_brut − (ricf si its_brut > 0 sinon 0))
retenues         = cnps + cmu + its
NET À PAYER      = brut_ap + transport_payé + primes_non_imposables − retenues − autres − avances
coût_employeur   = brut_ap + transport_payé + primes_ni
                   + retraite_pat(7,7 %) + PF + maternité + AT + CMU_er
                   + FDFP(1,6 %) + ITS_er(1,2 %)
```

## 8 · Primes « au net » — retrouver le brut pour un net demandé

Cas fréquent : *« elle perçoit 150 000 F net, quel salaire de base saisir ? »*
Méthode : recherche de la base `b` telle que `net(b) = cible`, **en respectant tous les arrondis** (le barème est discontinu aux bornes, la solution est un intervalle dont on prend le centre). Ne JAMAIS diviser le net par un « taux de charges » approximatif.

**Exemple vérifié** : net cible 150 000 F, zone Abidjan, 1 part, transport 30 000 F
→ **base = 139 639 F** : ITS 10 342 · CNPS 8 797 · CMU 500 → net exactement 150 000 F.
Coût employeur : 190 926 F.

**Exemple vérifié (bulletin type)** : base 75 290 F + transport 30 000 F (Abidjan, 1 part)
→ ITS 46 · CNPS 4 743 · CMU 500 → **net 100 001 F**. Coût employeur 119 820 F.

## 9 · Structure du bulletin de paie (recommandée)

1. **GAINS ET SALAIRE BRUT** : salaire de base, primes taxables → SALAIRE BRUT imposable CNPS/ITS
2. **RETENUES** : ITS net retenu (barème 2024) · CNPS retraite 6,3 % (base affichée) · CMU 500 → total retenues
3. **RUBRIQUES NON IMPOSABLES** : transport (dont montant exonéré), représentation, salissure…
4. **NET À PAYER** + ligne de contrôle : `brut − retenues + non imposables = net`
5. **POUR INFORMATION — charges patronales** : détail par nature + COÛT TOTAL EMPLOYEUR
6. Zones de signature employé / employeur

Mentions utiles : matricule, zone, parts fiscales, période, mode (mensuel/journalier), imposable fiscal et base CNPS rappelés en pied de bulletin.

## 10 · Pièges rencontrés en pratique

- Saisir le **net** en base → le net réellement payé est inférieur (les retenues tombent). Toujours résoudre §8.
- Oublier que **PF/maternité/AT sont plafonnés à 70 000** (surcoût patronal surestimé sinon).
- RICF déduite alors que l'ITS brut est nul → ITS « négatif » absurde. Garder le `max(0, …)`.
- Un salarié peut avoir un **ITS nul** (brut ≤ 75 000, ou RICF ≥ ITS) tout en payant CNPS + CMU.
- Primes « au net » : itérer sur le brut en répercutant CNPS **et** ITS marginaux, pas seulement CNPS.
- Journaliers : la déduction d'absences ne s'applique pas comme aux mensuels (la base est × jours travaillés).
