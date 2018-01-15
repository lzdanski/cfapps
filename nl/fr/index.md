---

copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Création d'applications Cloud Foundry
{: #creating_cloud_foundry_apps}

Avec {{site.data.keyword.Bluemix}}, vous pouvez créer votre application dans la console {{site.data.keyword.Bluemix_notm}}. Vous pouvez ensuite décider de continuer à vous servir de la console, d'utiliser l'interface de ligne de commande cf ou {{site.data.keyword.jazzhub_title}} pour développer, suivre, planifier et déployer votre application.
{:shortdesc}

Quand vous créez une application dans {{site.data.keyword.Bluemix_notm}}, vous pouvez commencer par le module de démarrage. Un *module de démarrage* est un modèle qui inclut des services prédéfinis et du code d'application configuré avec un pack de construction particulier. Il existe deux types de module de démarrage : les conteneurs boilerplate et les contextes d'exécution.

Un *conteneur boilerplate* regroupe une application, ainsi qu'un environnement d'exécution et des services prédéfinis qui lui sont associés pour un domaine particulier. Par exemple, le conteneur boilerplate Mobile Cloud inclut un contexte d'exécution Node.js, ainsi que les services Mobile Data, Mobile Application Security et Push. Il inclut également un logiciel SDK et des exemples d'application pour vous permettre de commencer à développer des applications mobiles qui accèdent à ces services.

Un *contexte d'exécution* est un ensemble de ressources utilisé pour exécuter une application. {{site.data.keyword.Bluemix_notm}} fournit des environnements d'exécution sous forme de conteneurs pour différents types d'application. Les environnements d'exécution sont intégrés sous forme de packs de construction dans {{site.data.keyword.Bluemix_notm}} ; ils sont configurés automatiquement et ne nécessitent pratiquement aucune maintenance.

Pour commencer à créer une application, procédez comme suit :
  1. Cliquez sur **Catalogue** dans la barre d'outils IBM Cloud.
  2. Cliquez sur **Applications Cloud Foundry** et choisissez un contexte d'exécution. Suivez l'expérience guidée pour spécifier un nom et sélectionnez comment vous voulez coder. Cliquez sur **Créer**.
  3. Une fois que vous avez terminé l'expérience guidée, cliquez sur **Présentation**.
  5. Vous pouvez ajouter un service à votre application en cliquant sur **Créer une connexion** dans la présentation de l'application sur le tableau de bord. Parcourez et sélectionnez des services dans le catalogue ou accédez à la fin du catalogue et cliquez sur
**Services expérimentaux {{site.data.keyword.Bluemix_notm}}** pour examiner ces services. Vous pouvez aussi
utiliser l'interface de
ligne de commande cf. Voir Options pour apprendre à utiliser les
applications.
  6. Dans la page Présentation, accédez à la carte "Continuous Delivery" et cliquez sur **Afficher la chaîne d'outils**. La source de votre application est sauvegardée dans un référentiel qui est hébergé dans Bluemix. Une chaîne d'outils ouverte utilisant ce référentiel et un pipeline de distribution afin de développer et de déployer votre application sont également créés. Pour plus d'informations sur le service Continuous Delivery, voir <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Initiation à Continuous Delivery</a>.

**Remarque :** si un service que vous liez à une application tombe en panne, il se peut que l'application s'arrête ou présente des
erreurs. {{site.data.keyword.Bluemix_notm}} ne redémarre pas automatiquement l'application pour assurer la
reprise suite à ces problèmes. Envisagez de coder votre application de sorte à identifier les indisponibilités, les exceptions et les pannes de connexion pour assurer
la reprise. Pour plus d'informations, voir la rubrique de traitement des incidents intitulée Les applications ne sont pas redémarrées automatiquement.

## Options pour la gestion des applications

Une fois que vous avez créé votre application, vous disposez de quelques options pour continuer à ajouter des services, construire l'application et la déployer :

<dl><dt>Interface de ligne de commande cf</dt>
<dd>Utilisez l'<a href="https://github.com/cloudfoundry/cli#getting-started">interface de ligne de commande cf</a> pour mettre à jour votre application, créer une instance de service ou lier le service à votre application. Vous pouvez également utiliser l'interface de ligne de commande cloud-cli pour créer, mettre à jour et supprimer des offres de services.</dd>
<dt>Interface utilisateur {{site.data.keyword.Bluemix_notm}}</dt>
<dd>Utilisez l'<a href="https://console.bluemix.net/dashboard/apps">interface utilisateur</a> {{site.data.keyword.Bluemix_notm}} pour générer votre application, et effectuer diverses activités comme choisir les services et les contextes d'exécution à combiner pour résoudre vos problèmes métier. </dd>
<dt>{{site.data.keyword.contdelivery_full}}</dt>
<dd>Utilisez {{site.data.keyword.contdelivery_short}} pour automatiser les générations, les tests unitaires, les déploiements, etc. Editez et insérez du code via l'interface IDE Web enrichie. Créez des chaînes d'outils pour activer les intégrations d'outils prenant en charge vos tâches de développement, de déploiement et d'opérations. Le service Continuous Delivery inclut Delivery Pipeline, Eclipse Orion Web IDE et Git Repos and Issue Tracking. Pour plus d'informations, voir <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Initiation à Continuous Delivery</a>.</dd>
</dl>

## Conseils

Tenez compte des conseils suivants lorsque vous développez vos applications Web :

<dl><dt>Persistance</dt>
<dd>Ne spécifiez pas d'emplacement de stockage local pour vos applications. Même si une seule instance est en cours d'exécution, chaque instance de votre application peut être à tout moment redémarrée et déplacée vers une autre machine virtuelle, notamment pour des raisons d'équilibrage de charge. Tout
élément stocké en local est effacé lorsque l'application est déplacée ou supprimée. Utilisez l'un des services de magasin de données {{site.data.keyword.Bluemix_notm}} pour la
persistance.</dd>
<dt>Limites de ressources</dt>
<dd>Prenez connaissance des limites relatives aux quantités de ressources qu'un compte d'essai peut utiliser. Ces limites sont les suivantes :
<table style="width:100%">
<caption>Tableau 1. Limites de ressources {{site.data.keyword.Bluemix_notm}} pour un compte d'essai</caption>
  <th>Type de ressource</th>	<th>Quantité maximale</th>
<tr><td>Nombre de services utilisés par toutes les applications</td> <td>10</td>
<tr><td>Mémoire utilisée dans toutes les applications</td> <td>	2 G</td>
<tr><td>Nombre de routes</td> <td>500</td>
</table>
</dd>
</dl>
