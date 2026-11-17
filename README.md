# Mini-SOC avec Wazuh

Projet de laboratoire pour me former au rôle d'analyste SOC niveau 1 : déployer un SIEM open source, collecter des logs, écrire des règles de détection et superviser les alertes.

## Architecture

- **Serveur Ubuntu** : Wazuh manager, indexer et dashboard
- **Agent Wazuh** sur un PC Windows 11

## Logs collectés (Windows)

- Security
- System
- PowerShell (Microsoft-Windows-PowerShell/Operational)

La journalisation des scripts PowerShell (ScriptBlockLogging) a été activée pour générer les événements 4104.

## Règles de détection personnalisées

| Règle | Description | Niveau |
|---|---|---|
| 100001 | Connexion suspecte : 5 échecs de connexion en 2 minutes | 10 |
| 100002 | Exécution de script PowerShell anormal | 12 |

Les règles sont écrites en XML (la règle 2 utilise une expression régulière).

## Tests

J'ai simulé les comportements surveillés (plusieurs mauvais mots de passe, commande PowerShell contenant des mots suspects) : les deux règles se sont déclenchées. Voir les captures d'écran à la racine du dépôt.

## Tableau de bord

Un tableau de bord « Mini-SOC L1 » regroupe trois visualisations : alertes par gravité, top des règles, alertes dans le temps.

## Problème rencontré

L'agent Windows s'est arrêté à cause d'une balise `<localfile>` mal placée dans `ossec.conf` (erreur « Invalid element: rootcheck »). Diagnostic grâce au fichier `ossec.log`, puis correction de la balise.

## Contenu du dépôt

- `les fichiers .png : captures d'écran.
- `local_rules.xml` : les deux règles
