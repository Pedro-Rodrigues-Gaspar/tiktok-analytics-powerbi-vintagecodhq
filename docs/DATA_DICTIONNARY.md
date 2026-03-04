# Dictionnaire de données — Modèle analytique TikTok

Ce document décrit les tables du modèle Star Schema utilisé pour l’analyse des performances du compte TikTok @VintageCoDHQ. Chaque table est documentée avec le nom des colonnes, le type de données, une description et un exemple de valeur.

---

## Table de faits : Fact Videos

Une ligne par vidéo. Agrège les métriques de performance et les clés vers les dimensions.

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `video_id` | Whole Number (PK) | Identifiant unique de la vidéo TikTok | `40` |
| `Date` | Date (FK) | Clé de la dimension Date (date de publication) | `15032025` |
| `cod_title` | Entier (FK) | Clé de la dimension CoD Title | `Black Ops 2` |
| `hook_type` | Entier (FK) | Clé de la dimension Hook Type | `Nostalgie émotionnelle` |
| `cod_title_id` | Entier (FK) | Clé de la dimension Titre CoD (jeu mentionné) | `3` |
| `Video Label` | Text | Titre artificiel avec concaténation des colonnes cod_title, date, video_id | `CoD 4 - 13 Jun #1` |
| `views` | Entier | Nombre total de vues de la vidéo | `125000` |
| `likes` | Entier | Nombre total de likes | `8500` |
| `comments` | Entier | Nombre total de commentaires | `420` |
| `share` | Entier | Nombre total de partages (shares) | `180` |
| `avg watch time (sec)` | Decimal Number | Temps de vidéo regardée en moyenne | `7,32` |
| `completion_rate` | Decimal Number (%) | Pourcentage de la vidéo regardée en moyenne | `68,5` |
| `video_duration_sec` | Decimal Number | Durée de la vidéo en secondes | `45,5` |


---

## Table de faits : Fact Account Daily

Une ligne par date. Métriques agrégées au niveau compte pour suivre la croissance et l’activité journalière.

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `date` | Date (FK) | Clé de la dimension Date (jour concerné) | `15032024` |
| `Video Views` | Whole Number | Somme des vues de toutes les vidéos pour ce jour | `450000` |
| `Profile Views` | Whole Number | Nombre de vues du profil ce jour-là | `12000` |
| `Likes` | Whole Number | Total des likes reçus sur les vidéos ce jour | `28000` |
| `Comments` | Entier | Total des commentaires reçus ce jour | `1500` |
| `Shares` | Entier | Total des partages ce jour | `890` |


---

## Table de faits : Follower History

Une ligne par date. Métriques agrégées au niveau compte pour suivre le nombre maximum de follower par jour et la différence par rapport au jours précédent.

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `Date` | Date (FK) | Clé de la dimension Date (jour concerné) | `24/02/2025` |
| `Followers` | Whole Number | Followers enregistrés chaque jour | `11869` |
| `Difference in followers from previous day` | Whole Number | Followers gagnés par jour | `502` |


---

## Dimension : Date

Calendrier pour les analyses temporelles (jour, semaine, mois, année, indicateurs).

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `Date` | Date (PK) | Clé surrogate au format JJMMAAAA | `20240315` |
| `Day` | Whole Number | jour du mois | `24` |
| `Day of Week` | Whole Number | Numéro du jour dans la semaine (1-7, lundi commençant par 1) | `3` |
| `Start of Month` | Date | démarrage de la semaine | `24/02/2025` |
| `Month` | Whole Number | Numéro du mois (1–12) | `3` |
| `Month Name` | Texte | Libellé du mois | `Feb` |
| `Week of Month` | Whole Number | Numéro de la semaine du mois (1–5) | `1` |
| `End of Month` | Date | Date fin du mois | `28/02/2024` |


---

## Dimension : Dimension CoD Title

Référence au jeu Call of Duty mentionné ou auquel la vidéo se rapporte.

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `CoD Title` | Text (PK) | Identifiant unique de l'opus Call of Duty | `Black Ops 2` |
| `Series` | Texte | Série de l'opus Call of Duty | `Black Ops` |


---

## Dimension : Dimension Hook Type

Catégorie de l’accroche utilisée en début de vidéo (nostalgie, souvenir, info, etc.).

| Nom de la colonne | Type de données | Description | Exemple de valeur |
|-------------------|-----------------|-------------|-------------------|
| `hook_type` | Text (PK) | Catégorie du Hook utilisé | `Nostalgie émotionnelle` |


*Exemples de valeurs typiques :* 
- Nostalgie émotionnelle : "Si seulement on pouvait revenir à cette époque" 
- Souvenir personnel : "Ces moments inoubliables..."
- Année nostalgique : "Tu te réveilles en 2012 et tes amis sont sur MW2"
- Affirmation : Si tu as reconnu alors t'as connu les meilleures années CoD
- Question : "Tu te rappelles de ce secret sur BO1"
- Comparaison : "Ces maps Avant vs Après"


---

## Schéma des relations (résumé)

- **Fact Performance vidéo** → Dimension Date, Type de contenu, Hook, Titre CoD, Titre (cardinalité 1:* depuis chaque dimension).
- **Fact Compte quotidien** → Dimension Date (cardinalité 1:*).
- Filtres en direction **single** : de la dimension vers la table de faits.

---

*Ce document décrit la structure des tables utilisées dans le projet TikTok Analytics (@VintageCoDHQ). Il doit être mis à jour en cas d'ajout ou de modification des tables et colonnes du modèle de données.*
