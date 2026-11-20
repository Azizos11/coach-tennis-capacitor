# Coach Tennis — build & sideload (SideStore)

App web autonome (hors-ligne) emballée avec **Capacitor** pour en faire une app iOS
installable via **SideStore**. Contenu de l'app déjà prêt dans `www/`.

---

## 0. Prérequis
- **Node.js 18+** (`node -v`)
- **CocoaPods** (`sudo gem install cocoapods`) — pour le projet iOS
- Pour fabriquer le `.ipa` :
  - **Le plus simple : un Mac avec Xcode** (gratuit sur l'App Store)
  - **Sans Mac :** un build macOS en ligne (Codemagic — offre gratuite — ou GitHub Actions runner `macos`)
- Sur l'iPhone : **SideStore** installé + ton **Apple ID gratuit**

---

## 1. Installer les dépendances
```bash
cd coach-tennis
npm install
```

## 2. Ajouter la plateforme iOS
```bash
npx cap add ios
npx cap copy ios
```
> `appId` = `de.azizelkateb.coachtennis`, `appName` = `Coach Tennis`
> (modifiables dans `capacitor.config.json`). Après toute modif de `www/`, relance `npx cap copy ios`.

## 3. Fabriquer le `.ipa`

### Option A — avec un Mac (recommandé)
```bash
npx cap open ios   # ouvre le projet dans Xcode
```
Dans Xcode :
1. Sélectionne la cible **App** → onglet **Signing & Capabilities** →
   coche **Automatically manage signing** et choisis ton **Apple ID** (Personal Team).
2. En haut, choisis **Any iOS Device (arm64)**.
3. Menu **Product → Archive**.
4. Dans l'Organizer : **Distribute App → Debugging (ou Development) → Export** → tu obtiens le **`.ipa`**.

> Astuce SideStore : tu peux aussi exporter **sans signer** (SideStore resigne lui-même).

### Option B — sans Mac (build en ligne)
1. Pousse ce dossier sur un repo **GitHub**.
2. Branche **Codemagic** (ou GitHub Actions avec un runner `macos-14`) sur le repo.
3. Workflow : `npm install` → `npx cap add ios` → `npx cap copy ios` →
   build de l'archive → **export `.ipa` (unsigned)**.
4. Récupère l'artefact `.ipa`.

## 4. Installer via SideStore
1. Mets le `.ipa` sur l'iPhone (AirDrop, fichiers, ou serveur SideStore).
2. Ouvre-le avec **SideStore → Install**.
3. SideStore **signe avec ton Apple ID gratuit** → l'app apparaît sur l'écran d'accueil.

> **Compte gratuit** : pas besoin du compte développeur à 99 €. Limites : **3 apps** max,
> **re-signature tous les 7 jours** (automatique quand l'iPhone et SideStore communiquent).

---

## Notes
- **Hors-ligne** : séances, défis, chrono, exos Tara (schémas SVG), match & stats, questionnaire → marchent sans réseau. La **Bibliothèque** (tes schémas importés dans `www/lib/`) est **embarquée** → hors-ligne aussi.
- **Polices** : Google Fonts se chargent en ligne ; hors-ligne l'app bascule sur les polices système (prévu). Pour du 100 % hors-ligne, on pourra embarquer les polices.
- **Icône** : icône Capacitor par défaut pour l'instant — on pourra mettre une vraie icône/splash ensuite.
- **Données** : tout est stocké sur l'appareil (localStorage + IndexedDB pour les vidéos).
