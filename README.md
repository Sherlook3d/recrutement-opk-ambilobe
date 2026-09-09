# Recrutement OPK — Ambilobe

Outil de test de recrutement pour les candidats OPK (formation à l'enrôlement biométrique) : frappe & orthographe (note manuscrite générée dynamiquement), saisie de données, quiz bureautique de base, et classement automatique en direct — utilisable simultanément depuis plusieurs postes.

## Liens

- **Test candidat** : `index.html` (page publique, aucune connexion requise)
- **Classement admin** : `admin.html` (protégé par connexion Supabase Auth)

Une fois déployé sur GitHub Pages :
- Candidat : `https://sherlook3d.github.io/recrutement-opk-ambilobe/`
- Admin : `https://sherlook3d.github.io/recrutement-opk-ambilobe/admin.html`

## Backend

Projet Supabase dédié : `opk-ambilobe-recrutement` (réf. `bflspzngwctaochwwjpt`).

Table `candidats` (RLS activé) :
- `anon` peut uniquement **insérer** (les candidats ne se connectent pas)
- `authenticated` (le compte admin) peut **lire** et **supprimer**
- Realtime activé : la page admin reçoit chaque nouveau candidat instantanément, quel que soit le poste d'où il a été soumis

## Créer le compte admin (une seule fois)

1. Ouvrir le [tableau de bord du projet Supabase](https://supabase.com/dashboard/project/bflspzngwctaochwwjpt/auth/users)
2. **Authentication → Users → Add user**
3. Renseigner un e-mail et un mot de passe (à réutiliser sur `admin.html`)
4. Cocher « Auto Confirm User » pour pouvoir se connecter immédiatement

## Résilience hors-ligne

La page candidat tente d'envoyer le résultat immédiatement à la fin du test. En cas de coupure réseau, le résultat est mis en file d'attente localement (`localStorage`) et renvoyé automatiquement dès que la connexion revient (retenté toutes les 8 secondes, et au retour en ligne). Chaque poste génère une étiquette `Poste-XXXX` unique (visible dans la colonne « Poste » du classement) pour tracer la provenance.
