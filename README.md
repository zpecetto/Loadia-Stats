# Loadia-Stats

Statistiques de jeux vidéo via Loadia, avec n8n.

## Workflow

[Jeux vidéo.json](Workflow/Jeux%20vid%C3%A9o.json) contient 20 nœuds.

Extraction de **Edit Fields** à **Merge12** : contrôle de session et reconnexion, statistiques du site, ludothèque, table Jeux vidéo et Google Sheets. Les compteurs de collection, souhaits, jeux joués et backlog sont conservés, ainsi que plateformes, temps de jeu, notes, genres, studios et périodes de sortie.

## Configuration du service

Renseigner `cookie_loadia`, `mail_loadia` et `mdp_loadia` dans **Edit Fields**. Configurer Browserless et Discord. Les URL de statistiques et de ludothèque restent celles du service Loadia.

## Démarrage et configuration

Chaque extraction planifiée reprend le début d’Ultime : **Schedule Trigger**, **Date & Time**, **Configuration Globale**, **Loop Over Items4**, **HTTP Request1**, **If5**. La branche d’échec conserve **Restauration Tunnel1**, **Wait1** et son retour dans la boucle.

1. Importer le JSON dans n8n. L’export reste désactivé tant que la configuration n’est pas terminée.
2. Remplacer l’URL `https://YOUR_N8N_HOST.example.invalid/` de **HTTP Request1** par celle de votre instance. Le succès est déterminé par un HTTP 200.
3. Reconnecter les credentials SSH du nœud de restauration et adapter la commande ngrok à votre installation. Sans tunnel, remplacer cette commande par votre propre mécanisme de restauration ou retirer explicitement cette branche.
4. Adapter l’horaire du planificateur : l’export reprend le vendredi à 17 h. Choisir le fuseau horaire de l’instance ou du workflow.
5. Remplacer les champs `YOUR_...`, renseigner les cookies privés lorsque nécessaires et sélectionner les credentials des services utilisés.
6. Créer les tables décrites dans [DataTables](DataTables/README.md), puis les sélectionner dans chaque nœud Data Table. Les CSV sont vides, avec leurs seuls en-têtes.
7. Pour les sorties Sheets, créer un onglet **N8N** à partir de [GoogleSheets/N8N.csv](GoogleSheets/N8N.csv), sélectionner votre document et vos credentials dans tous les nœuds Sheets. Les intitulés des colonnes et les colonnes de correspondance doivent rester identiques.
8. Vérifier une exécution complète avant activation. Les branches de collecte peuvent vider et reconstruire leurs tables cibles ; utiliser des tables dédiées.

Les identifiants de documents, tables, dossiers, comptes, cookies, tokens, données épinglées et historiques d’exécution personnels ont été retirés. Les identifiants internes des nœuds ont été régénérés. Les workflows séparés et le workflow complet sont des alternatives : éviter de lancer simultanément plusieurs versions qui reconstruisent les mêmes tables ou contrôlent le même tunnel.

## Vérification

Le JSON, les connexions, les références entre nœuds, la syntaxe JavaScript, les expressions complètes et les entrées des nœuds Merge ont été contrôlés localement. Aucun service personnel ni workflow de production n’a été exécuté. Les credentials et l’intégration réelle doivent être vérifiés dans l’instance cible.
