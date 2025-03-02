---
lab:
  title: "Labo\_09c\_: Implémenter Azure Kubernetes Service"
  module: Administer PaaS Compute Options
---

# Labo 09c : Implémenter Azure Kubernetes Service
# Manuel de labo de l’étudiant

## Scénario du labo

Contoso dispose d’un certain nombre d’applications multiniveau qu’il n’est pas possible d’exécuter à l’aide d’Azure Container Instances. Pour déterminer si elles peuvent être exécutées sous la forme de charges de travail conteneurisées,vous devez envisager d’utiliser Kubernetes comme orchestrateur de conteneurs. Pour réduire encore la charge de gestion, vous allez tester Azure Kubernetes Service, notamment son expérience de déploiement simplifiée et ses fonctionnalités de montée en charge.

**Remarque :** Une **[simulation de labo interactive](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2015)** est disponible et vous permet de progresser à votre propre rythme. Il peut exister de légères différences entre la simulation interactive et le labo hébergé. Toutefois, les concepts et idées de base présentés sont identiques. 

## Objectifs

Dans ce labo, vous allez :

+ Tâche 1 : Inscrire les fournisseurs de ressources Microsoft.Kubernetes et Microsoft.KubernetesConfiguration
+ Tâche 2 : Déployer un cluster Azure Kubernetes Service
+ Tâche 3 : Déployer des pods dans le cluster Azure Kubernetes Service
+ Tâche 4 : Mettre à l’échelle des charges de travail conteneurisées dans le cluster de service Azure Kubernetes

## Durée estimée : 40 minutes

## Diagramme de l'architecture

![image](../media/lab09c.png)

### Instructions

## Exercice 1

## Tâche 1 : Inscrire les fournisseurs de ressources Microsoft.Kubernetes et Microsoft.KubernetesConfiguration

Dans cette tâche, vous allez inscrire les fournisseurs de ressources nécessaires pour déployer un cluster Azure Kubernetes Services.

1. Connectez-vous au [portail Azure](https://portal.azure.com).

1. Dans le portail Azure, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Lorsque vous êtes invité à sélectionner **Bash** ou **PowerShell**, sélectionnez **PowerShell**.

    >**Remarque** : Si c’est la première fois que vous démarrez **Cloud Shell** et que vous voyez le message **Vous n’avez aucun stockage monté**, sélectionnez l’abonnement que vous utilisez dans ce labo, puis sélectionnez **Créer un stockage**.

1. Dans le volet Cloud Shell, exécutez la commande suivante pour inscrire les fournisseurs de ressources Microsoft.Kubernetes et Microsoft.KubernetesConfiguration.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Kubernetes

   Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
   ```

1. Fermez le volet Cloud Shell.

## Tâche 2 : Déployer un cluster Azure Kubernetes Service

Dans cette tâche, vous allez déployer un cluster Azure Kubernetes Services à l’aide du portail Azure.

1. Dans le portail Azure, recherchez **Services Kubernetes**, puis, dans le panneau **Services Kubernetes**, cliquez sur **+ Créer**, puis cliquez sur **+ Créer un cluster Kubernetes**.

1. Sous l’onglet **De base** du volet **Créer un cluster Kubernetes**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Abonnement | le nom de l’abonnement Azure que vous utilisez dans ce labo |
    | Resource group | le nom d’un nouveau groupe de ressources **az104-09c-rg1** |
    | Configuration prédéfinie du cluster | **Dev/Test ($)** |
    | Nom du cluster Kubernetes | **az104-9c-aks1** |
    | Région | le nom d’une région dans laquelle vous pouvez approvisionner un cluster Kubernetes |
    | Zones de disponibilité | **Aucune** (décochez toutes les cases) |
    | Version de Kubernetes | accepter la valeur par défaut |
    | Disponibilité du serveur d’API | accepter la valeur par défaut |
    | Taille du nœud | accepter la valeur par défaut |
    | Méthode de mise à l’échelle | **Manuel** |
    | Nombre de nœuds | **1** |

1. Cliquez sur **Suivant : Pools de noeuds >** , puis sous l’onglet **Pools de noeuds** du volet **Créer un cluster Kubernetes**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Activer les nœuds virtuels | **Désactivé** (valeur par défaut) |

1. Cliquez sur **Suivant : Accès >** , puis, sous l’onglet **Accès** du volet **Créer un cluster Kubernetes**, laissez les paramètres avec leurs valeurs par défaut :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Identité des ressources | **Identité managée affectée par le système** |
    | Méthode d'authentification | **Comptes locaux avec RBAC Kubernetes** |

1. Cliquez sur **Suivant : Mise en réseau >** , puis, dans l’onglet **Mise en réseau** du volet **Créer un cluster Kubernetes**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Configuration réseau | **Kubenet** |
    | Préfixe du nom DNS | **n’importe quel préfixe DNS global unique valide** |

1. Cliquez sur **Suivant : Intégrations >** , puis, sous l’onglet **Intégrations** du volet **Créer un cluster Kubernetes**, spécifiez les paramètres suivants (conservez les valeurs par défaut pour les autres) :

    | Paramètre | Valeur |
    | ---- | ---- |
    | Supervision de conteneur | **Désactiver** |
    | Activer les règles d’alerte recommandées | **Décochez la case** |
    
1.  Cliquez sur **Vérifier + Créer**, vérifiez que la validation a réussi, puis cliquez sur **Créer**.

    >**Remarque** : Dans les scénarios de production, il est préférable d’activer la supervision. En l’occurrence, la supervision est désactivée parce qu’elle n’est pas couverte dans le labo.

    >**Remarque** : Attendez la fin du déploiement. Cela devrait prendre environ 10 minutes.

## Tâche 3 : Déployer des pods dans le cluster Azure Kubernetes Service

Dans cette tâche, vous allez déployer un pod dans le cluster Azure Kubernetes Service.

1. Dans le volet de déploiement, cliquez sur le lien **Accéder à la ressource**.

1. Dans le volet du service Kubernetes **az104-9c-aks1**, dans la section **Paramètres**, cliquez sur **Pools de nœuds**.

1. Dans le volet **az104-9c-aks1 - Pools de nœuds**, vérifiez que le cluster se compose d’un seul pool avec un seul nœud.

1. Dans le portail Azure, ouvrez **Azure Cloud Shell** en cliquant sur l’icône située en haut à droite du portail Azure.

1. Basculez **Azure Cloud Shell** vers **Bash** (arrière-plan noir).

1. Dans le volet Cloud Shell, exécutez la commande suivante pour récupérer les informations d’identification qui vous permettront d’accéder au cluster AKS :

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour vérifier la connectivité au cluster AKS :

    ```sh
    kubectl get nodes
    ```

1. Dans le volet **Cloud Shell**, passez en revue la sortie et vérifiez que le nœud dont se compose le cluster à ce stade est à l’état **Prêt**.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour déployer l’image **nginx** à partir de Docker Hub :

    ```sh
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **Remarque** : Veillez à utiliser des lettres minuscules lors de la saisie du nom du déploiement (nginx-deployment)

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour vérifier qu’un pod Kubernetes a été créé :

    ```sh
    kubectl get pods
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour identifier l’état du déploiement :

    ```sh
    kubectl get deployment
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour rendre le pod disponible à partir d’Internet :

    ```sh
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour déterminer si une adresse IP publique a été approvisionnée :

    ```sh
    kubectl get service
    ```

1. Réexécutez la commande jusqu’à ce que la valeur de la colonne **EXTERNAL-IP** correspondant à l’entrée **nginx-deployment** passe de **\<pending\>** à une adresse IP publique. Notez l’adresse IP publique dans la colonne **EXTERNAL-IP** en regard de **nginx-deployment**.

1. Ouvrez une fenêtre de navigateur et accédez à l’adresse IP que vous avez obtenue à l’étape précédente. Vérifiez que la page du navigateur affiche le message **Bienvenue dans nginx !** « Hello World ! ».

## Tâche 4 : Mettre à l’échelle des charges de travail conteneurisées dans le cluster de service Azure Kubernetes

Dans cette tâche, vous allez effectuer une montée en charge horizontale du nombre de pods, puis du nombre de nœuds de cluster.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour mettre à l’échelle le déploiement en augmentant le nombre de pods à 2 :

    ```sh
    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour vérifier le résultat de la mise à l’échelle du déploiement :

    ```sh
    kubectl get pods
    ```

    > **Remarque** : Passez en revue la sortie de la commande et vérifiez que le nombre de pods est passé à 2.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour effectuer un scale-out du cluster en augmentant le nombre de nœuds à 2 :

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks scale --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER --node-count 2
    ```

    > **Remarque** : Attendez la fin de l’approvisionnement du nœud supplémentaire. Ceci peut prendre environ 3 minutes. En cas d’échec, exécutez la commande `az aks scale`.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour vérifier le résultat de la mise à l’échelle du cluster :

    ```sh
    kubectl get nodes
    ```

    > **Remarque** : Passez en revue la sortie de la commande et vérifiez que le nombre de nœuds est passé à 2.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour mettre à l’échelle le déploiement :

    ```sh
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour vérifier le résultat de la mise à l’échelle du déploiement :

    ```sh
    kubectl get pods
    ```

    > **Remarque** : Passez en revue la sortie de la commande et vérifiez que le nombre de pods est passé à 10.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour passer en revue la distribution des pods sur les nœuds du cluster :

    ```sh
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **Remarque** : Passez en revue la sortie de la commande et vérifiez que les pods sont distribués sur les deux nœuds.

1. Dans le volet **Cloud Shell**, exécutez la commande suivante pour supprimer le déploiement :

    ```sh
    kubectl delete deployment nginx-deployment
    ```

1. Fermez le volet **Cloud Shell**.

## Nettoyer les ressources

>**Remarque** : N’oubliez pas de supprimer toutes les nouvelles ressources Azure que vous n’utilisez plus. La suppression des ressources inutilisées vous évitera d’encourir des frais inattendus.

>**Remarque** :  Ne vous inquiétez pas si les ressources de laboratoire ne peuvent pas être immédiatement supprimées. Parfois, les ressources ont des dépendances et leur suppression prend plus de temps. Il s’agit d’une tâche d’administrateur courante pour surveiller l’utilisation des ressources. Il vous suffit donc de consulter régulièrement vos ressources dans le portail pour voir comment se passe le nettoyage. 

1. Dans le Portail Azure, ouvrez la session shell **Bash** dans le volet **Cloud Shell**.

1. Listez tous les groupes de ressources créés dans les labos de ce module en exécutant la commande suivante :

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].name" --output tsv
   ```

1. Supprimez tous les groupes de ressources que vous avez créés dans les labos de ce module en exécutant la commande suivante :

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**Remarque** : La commande s’exécute de façon asynchrone (comme déterminé par le paramètre --no-wait). Par conséquent, vous serez en mesure d’exécuter une autre commande Azure CLI immédiatement après au cours de la même session Bash, mais la suppression réelle du groupe de ressources prendra quelques minutes.

## Révision

Dans cet exercice, vous avez :

+ Déployé un cluster Azure Kubernetes Service
+ Déployé des pods dans le cluster Azure Kubernetes Service
+ Mis à l’échelle des charges de travail conteneurisées dans le cluster de service Azure Kubernetes
