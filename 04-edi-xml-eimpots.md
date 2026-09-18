# EDI XML e-Impôts — spécification complète

Extraite des générateurs officiels DGI (État 301 v1.2, annexe TVA v2.1). **Citation officielle (feuille « A SAVOIR »)** : « vous êtes également libre de générer un fichier XML par un autre moyen, si celui-ci respecte scrupuleusement la syntaxe EDI XML e-Impôts et les référentiels présents » — c'est la base légale d'un générateur tiers.

## Processus officiel de déclaration

1. Remplir la feuille SAISIE du classeur DGI (NCC, exercice, mois, code impôt, tableau des lignes).
2. Bouton orange **« Génération du XML »** (macro officielle) → fichier XML dans /Documents.
3. Sur **e-Impôts**, étape 3 de la déclaration → **« Transmission des annexes détaillées par EDI »** → importer le XML.
4. La notice précise que le remplissage peut se faire « par copier/coller depuis un autre fichier Excel (éventuellement exporté de votre système d'information) ».

Pièces jointes e-Impôts : **≤ 2 Mo** chacune.

## Squelette XML (identique ITS et TVA)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<EDI>
 <informations>
  <type>etat_301_mensuel</type>          <!-- ou TVA -->
  <ncc>1900000X</ncc>
  <codeTaxe>ITS</codeTaxe>               <!-- ITS / ITSPUB / ITPARA / TVA -->
  <mois>8</mois>
  <exercice>2026</exercice>
 </informations>
 <tableaux>
  <tableau>
   <donnees>
    <ligne>
     <champ><code>numero_cnps</code><valeur>188021640501</valeur></champ>
     <champ><code>identite</code><valeur>KOUAME JEAN</valeur></champ>
     …
    </ligne>
   </donnees>
  </tableau>
 </tableaux>
</EDI>
```

- Montants : **virgule décimale** (ex. `20535,36`).
- Dates : `JJ/MM/AAAA`. Valeur vide autorisée : `<valeur></valeur>`.
- Champs dans **l'ordre officiel** (voir `02-its-etat301.md` / `03-tva-annexe.md`).

## Règles strictes de la macro officielle (MiseEnFormeEDI)

La conformité ne s'arrête pas aux champs. Le XML produit par la macro DGI impose :

1. **Un élément par ligne** : en-tête XML, chaque `<ligne>`, éléments et pied — chacun sur sa propre ligne → **CRLF entre tous les éléments et en fin de fichier**.
2. **Remplacements de caractères sur tout le fichier** : accents supprimés (é→e, è→e, à→a, ç→c, É→E…) · **& → ET** · **( et ) → espaces** · **% → pourcentage** · € → EUR · ° → O · œ → OE · æ → AE · … → ... · ´ et ’ → ' · $ → S · £ → Pounds · # ] ¤ ¨ → espaces · ² → carré.
3. **MAJUSCULES (UPPER)** sur `numero_cnps`, `identite`, `emploi_qualite`, `designation_indemnites` (État 301) — **PAS** sur les champs TVA.
4. **Ligne ignorée** si n° CNPS / identité / ITS brut / ITS net vides (État 301), ou type / date / montant vides (TVA).
5. **Nom du fichier** : `NCC-EDI-<type>-aaaammjj-hhmm.xml` (ex. `1900000X-EDI-etat_301_mensuel-20260914-0218.xml`).
6. Montants : virgule décimale (locale Excel fr). Encodage : texte plein (ASCII après remplacements, cohérent avec la déclaration UTF-8).

## Le classeur officiel (chirurgie, pas réécriture)

Pour produire le **fichier DGI officiel** (.xlsm) pré-rempli depuis une application :
- Injecter les **valeurs** dans la feuille SAISIE sans **jamais toucher une cellule formule** (`<f>` dans le XML interne : barème ITS/RICF, numérotation partagée, totaux).
- Dates en **série Excel** ; NCC/exercice/mois en tête (cellules dédiées).
- Conserver : logos DGI, formules officielles, feuille TradXML (valeurs cachées) et la macro « Génération du XML ».
- Technique éprouvée : réécriture ZIP du classeur en JavaScript natif (`CompressionStream`/`DecompressionStream` deflate-raw + CRC32), zéro bibliothèque.

## Validation d'un XML généré

- XML bien formé ; hiérarchie `EDI/informations/tableaux/tableau/donnees` ; ordre des champs = officiel ; montants à la virgule ; CRLF partout ; remplacements de caractères appliqués ; nommage conforme.
- Contrôle croisé : l'ITS net de chaque ligne doit correspondre au bulletin du salarié (mêmes arrondis).
