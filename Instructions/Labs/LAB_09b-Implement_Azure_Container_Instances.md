---
lab:
  title: "Labo\_09b\_: Implémenter Azure Container Instances"
  module: Administer PaaS Compute Options
---

# Labo 09b : Implémenter Azure Container Instances
# Manuel de labo de l’étudiant

## Scénario du labo

Contoso souhaite trouver une nouvelle plateforme pour ses charges de travail virtualisées. Vous avez identifié différentes images conteneur qui peuvent être exploitées pour atteindre cet objectif. Étant donné que vous souhaitez réduire la charge de gestion des conteneurs, vous prévoyez d’évaluer l’utilisation d’Azure Container Instances pour le déploiement d’images Docker.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, vous allez :

- Tâche 1 : Déployer une instance Azure Container Instances en utilisant une image Docker
- Tâche 2 : Passer en revue les fonctionnalités d’Azure Container Instances

## Durée estimée : 20 minutes

## Diagramme de l'architecture

![image](../media/lab09b.png)

### Instructions

## Exercice 1

## Tâche 1 : Déployer une instance Azure Container Instances en utilisant une image Docker

Dans cette tâche, vous allez créer une nouvelle instance de conteneur pour l’application web.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le portail Azure, recherchez **instances de conteneur** puis, dans le panneau **Instances de conteneur**, cliquez sur **+ Créer**.

1. Sous l’onglet **Informations de base** du volet **Créer une instance de conteneur**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Resource group | le nom d’un nouveau groupe de ressources **az104-09b-rg1** |
    | Nom du conteneur | **az104-9b-c1** |
    | Région | le nom d’une région dans laquelle vous pouvez approvisionner des instances de conteneur Azure |
    | Source d’image | **Images du guide de démarrage rapide** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Cliquez sur **Suivant : Mise en réseau >**  puis, sous l’onglet **Mise en réseau** du volet **Créer une instance de conteneur**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | --- | --- |
    | Étiquette du nom DNS | tout nom d’hôte DNS unique valide et global |

    >**Remarque** : Votre conteneur sera accessible au public à l’adresse : dns-name-label.region.azurecontainer.io. Si vous recevez un message d’erreur **Étiquette de nom DNS indisponible**, spécifiez une autre valeur.

1. Cliquez sur **Suivant : > Avancé >** , passez en revue les paramètres sous l’onglet **Avancé** du panneau **Créer une instance de conteneur** sans apporter de modifications, cliquez sur **Vérifier + Créer**, vérifiez que la validation a réussi et cliquez sur **Créer**.

    >**Remarque** : Attendez la fin du déploiement. Ce processus prend environ 3 minutes.

    >**Remarque** : Pendant que vous attendez, regardez l’[exemple de code derrière l’application simple](https://github.com/Azure-Samples/aci-helloworld). Pour l’afficher, parcourez le dossier de l’application \\.

## Tâche 2 : Passer en revue les fonctionnalités d’Azure Container Instances

Dans cette tâche, vous allez examiner le déploiement de l’instance de conteneur.

1. Dans le volet de déploiement, cliquez sur le lien **Accéder à la ressource**.

1. Dans le volet **Vue d’ensemble** de l’instance de conteneur, vérifiez que l’**État** indiqué est **En cours d’exécution**.

1. Copiez la valeur du **nom de domaine complet** de l’instance de conteneur, ouvrez un nouvel onglet de navigateur et accédez à l’URL correspondante.

1. Vérifiez que la page **Bienvenue dans Azure Container Instances** s’affiche.

1. Fermez le nouvel onglet du navigateur, revenez Dans le portail Azure, dans la section **Paramètres** du volet d’instance de conteneur, cliquez sur **Conteneurs**, puis sur **Journaux**.

1. Vérifiez que les entrées de journal représentant la requête HTTP GET générée en affichant l’application dans le navigateur apparaissent.

## Nettoyer les ressources

>**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées vous évitera d’encourir des frais inattendus.

>**Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et leur suppression prend plus de temps. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le portail Azure, ouvrez la session **PowerShell** dans le volet **Cloud Shell**.

    >**Remarque** : le stockage Cloud Shell doit être créé pour que ces commandes fonctionnent. 

1. Listez tous les groupes de ressources créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Supprimez tous les groupes de ressources que vous avez créés dans les labos de ce module en exécutant la commande suivante :

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Remarque** : La commande s’exécute de façon asynchrone (tel que déterminé par le paramètre -AsJob). Vous pourrez donc exécuter une autre commande PowerShell immédiatement après au cours de la même session PowerShell, mais la suppression effective du groupe de ressources peut prendre quelques minutes.

## Révision

Dans ce labo, vous avez :

- Déployé une image Docker à l’aide d’Azure Container Instances
- Passé en revue les fonctionnalités d’Azure Container Instances
