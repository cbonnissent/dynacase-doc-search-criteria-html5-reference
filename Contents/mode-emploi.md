# Mode d'emploi {#search-criteria-html5:7f3a7070-00a5-4144-bf3c-1cc962e3abdf}

## Dépendances {#search-criteria-html5:14e6e4bb-da9e-4371-b025-2fbbad71de79}

L'intégration du plugin requiert l'ajout dans la page web des composants suivants :

### Javascript {#search-criteria-html5:948e7e99-fadb-49f1-972b-db395b335c33}

* **jquery** : `lib/jquery/ddui/jquery.min.js`
* **underscore** : `lib/underscore/underscore.js`
* **mustache** : `lib/mustache.js/mustache.js`
* **Bootstrap tooltip**: `lib/bootstrap/3/js/tooltip.js`
* **Kendo UI** : `lib/KendoUI/ddui/js/kendo-ddui-builded.js`
* **Document UI : widget** : `DOCUMENT/IHM/widgets/mainWidget-min.js`
* **Search Criteria** : `SEARCH_CRITERIA_HTML5/widgets/main-built.js`

### CSS {#search-criteria-html5:652da046-556c-448d-9f2a-31a0b8c31f30}

* **Bootstrap** : `css/dcp/document/bootstrap.css`
* **Kendo UI** : `css/dcp/document/kendo.css`
* **Document UI** : `css/dcp/document/document.css`
* **Search Criteria** : `css/search_criteria_html5/search_criteria.css`

Les adresses des éléments sont données à titre indicatif et sont valables dans l'optique d'une intégration au sein 
d'une action/application.

## Droits {#search-criteria-html5:ae5c109a-d179-497c-a69c-980469b3b126}

Les utilisateurs devant avoir accès à Search Criteria et ses actions associées doivent avoir l'ACL : *BASIC* de 
l'application *SEARCH_CRITERIA_HTML5*.

## Initialisation {#search-criteria-html5:15ee5449-0a1a-4648-861b-1df7a8e564de}

Idéalement l'initialisation du plugin doit avoir lieu sur l'évènement *ready* de la page en cours pour permettre à la DOM 
et aux dépendances d'être chargées.

L'initialisation de la Search Criteria se fait sur une balise `<div>`. Celle-ci doit être pré-existante à l'initialisation.
On doit ensuite la sélectionner à l'aide de jQuery et l'initialiser.

Exemple d'initialisation des criterias en utilisant une table pour les présenter sur deux colonnes :

    [html]
    <!doctype html>
    <html lang="fr">
    <head>
        <title>Démo</title>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1.5">
        <link rel="icon" href="[DYNACASE_FAVICO]"/>
        <link rel="shortcut icon" href="[DYNACASE_FAVICO]"/>
        <link rel="stylesheet" href="css/dcp/document/bootstrap.css?ws=[WS]">
        <link rel="stylesheet" href="css/dcp/document/kendo.css?ws=[WS]">
        <link rel="stylesheet" href="css/dcp/document/ckeditor.css?ws=[WS]">
        <link rel="stylesheet" href="css/dcp/document/document.css?ws=[WS]">
        <link rel="stylesheet" href="css/search_criteria_html5/search_criteria.css?ws=[WS]"/>
    </head>
    <body>
    <div id="wrapper" class="container-fluid">
        <div id="target" class="row">
    
        </div>
    </div>
    
    <script type="text/javascript" src="lib/jquery/ddui/jquery.min.js?ws=[WS]"></script>
    <script type="text/javascript" src="lib/underscore/underscore.js?ws=[WS]"></script>
    <script type="text/javascript" src="lib/mustache.js/mustache.js?ws=[WS]"></script>
    <script type="text/javascript" src="lib/bootstrap/3/js/tooltip.js?ws=[WS]"></script>
    <script type="text/javascript" src="lib/KendoUI/ddui/js/kendo-ddui-builded.js?ws=[WS]"></script>
    <script type="text/javascript" src="SEARCH_CRITERIA_HTML5/widgets/main-built.js?ws=[WS]"></script>
    <script>
        $("#target").criterias({
            criteriasDef: {
                defaultFam: "ZOO_DEMANDEADOPTION",
                criterias: [
                    {"id": "state"},
                    {"id": "de_idrealised"}
                ]
            }
        });
    </script>
    </body>
    </html>

### Options de configuration {#search-criteria-html5:baf5d0e6-52a6-4ce5-8517-20dc5b20ac91}

Les éléments en gras sont obligatoires.

**criteriasDef**

:   configuration des critères  
    *type* : objet javascript,

*wrapperTemplate*

:   template entourant le critère, ce template génère la DOM qui contient le critère de recherche
    *type* : fonction ou texte (template [Mustache][mustache]).
    *valeur par défaut* : `<div class="dcpCriterias__wrapper" data-id="{{id}}" data-type="{{type}}" data-uuid="{{uuid}}"></div>`
    Si le template est une fonction, il reçoit en entrée un objet décrivant le critère en cours (identifiant, type, identifiant unique) et doit
    retourner une chaîne de caractères correspondant à de la DOM.

*offlineCriteriasDef*

:   La configuration `criteriasDef` n'est pas complétée via une requête sur le serveur
    *type* : Booléen
    *valeur par défaut* : `false`


*note :* aucune option ne peut être modifiée après la création des critères.


#### Détail de la configuration : {#search-criteria-html5:837ff99a-8b93-4f3e-88e7-e451e1ce6917}

Cette option est en partie complétée via une requête XHR sur le serveur, si vous optez pour un fonctionnement online, 
seul le `defaultFam` et les `id` des `criterias` sont nécessaires.

criteriasDef
:   cet objet de configuration donne les éléments permettant d'établir la configuration des critères.  
    Pour ce faire, il possède deux propriétés :
    
    defaultFam
    :   nom logique de la famille par défaut des critères  
        *type* :chaîne de caractères
    
    criterias
    :   tableau de définition de critères.  
        Chaque élément est un objet composé des éléments suivants :
        
        **id**
        :   nom logique de l'attribut/propriété (les propriétés utilisables sont title et state) contenu dans cette colonne  
            *type* :chaîne de caractères,
        
        famId
        :   famille contenant l'attribut id  
            *type* :chaîne de caractères,  
            Cet élément n'est pas obligatoire si defaultFam est défini ou pour le titre,
        
        label
        :   label affiché devant le critère  
            *type* :chaîne de caractères,
        
        multiplicity
        :   "simple" ou "multiple" indique si le critère est présenté sous forme pour attribut *non multiple* ou *multiple*  
            *type* :chaîne de caractères,
        
        type
        :   indique le type d'attribut ou de propriété sur lequel s'effectue la recherche  
            *type* :chaîne de caractères,
        
        urlSource
        :   url source de l'appel serveur pour les critères de type énuméré, relation, account et state.
            Cette URL doit accepter un paramètre "term" qui contient ce qui est actuellement ajouté dans le champs de recherche par l'utilisateur,  
            et retourner un objet array contenant, pour chaque ligne, un objet avec les trois propriétés key (clef stockée et non affichée), value (affiché à l'utilisateur si cet élément est sélectionné), label (présenté dans la liste déroulante)
        
        defaultValue
        :   valeur par défaut du critère. Le contenu de cet élément doit être de même type que celui du setValue.


*note :* les id de type account et color ne sont pas pris en compte.

## Méthodes associées {#search-criteria-html5:7453aefb-5f41-40fe-a49d-0383d7627224}

Plusieurs méthodes sont associées à l'objet Search Criteria :

destroy
:   Permet de détruire la table.  
    La table est alors supprimée (la balise reste en place, mais son contenu est vidé),

getValues
:   Permet de retrouver les valeurs des critères, cette valeur est retournée sous la forme d'un array javaScript qui 
contient pour chacun de ses éléments les points suivants :
    
    id
    :   identifiant du critère,
    
    kind
    :   type du critère,
    
    type
    :   type de l'attribut/propriété,
    
    multiplicity
    :   indique la multiplicité du critère (multiple ou simple),
    
    operator
    :   opérateur sélectionné,
    
    value
    :   valeur ou array de valeurs

getValue
:   Identique à `getValues` mais prend en entrée l'id d'un critère et ne retourne que la valeur de celui-ci,

setValue
:   Permet d'indiquer la valeur d'un critère. Cette fonction a comme paramètres entrant :
    
    *   id du critère
    *   valeur à donner au critère :
        
        *   chaîne de caractères pour les critères date, text, numérique,
        *   objet javascript avec les propriétés value1 et value2 pour les critères date et numérique avec l'opérateur between,
        *   objet javascript avec les propriétés value et key pour les énuméré, relation, account et state simple et tableaux d'objet javascript avec les propriétés value et key pour les énuméré, relation, account et state multiple

Ces méthodes sont appelées avec la syntaxe suivante :

    [javascript]
    $("#criterias").criterias("fonction_name");

*note :* Suite à l'évolution de la docGrid les critères pourront lui être passée directement et ensuite être utilisés par la grid pour filtrer ses résultats.

## Évènements associés {#search-criteria-html5:cc897a58-ebc1-4eaa-a81c-a7b59ade13c4}

Des évènements sont déclenchés à 2 niveaux :

*   au niveau de criteria
*   au niveau de chacun des critères


###Événements de plus haut niveau {#search-criteria-html5:084efc03-5a6e-4ed4-bd17-29c3c6f47c91}

error
:   déclenché à chaque erreur détectée.  
    Il fournit un objet event et un objet erreur (tableau de messages d'erreurs),

draw
:   déclenché après l'insertion de l'ensemble des critères de recherche

Les évènements peuvent être écoutés de deux manières différentes :

*   en utilisant les fonctionnalités pour s'attacher à un évènement de jQuery  
    Il faut alors préfixer l'évènement cible par le nom du widget en minuscule (dans notre cas *docgrid*)
    
    [javascript]
    $("#criterias").on("criteriaserror", function(e, ui) { console.log(ui);});

*   en s'inscrivant directement à la création du widget :

    [javascript]
    $("#criterias").criterias({ error : function(e, ui) { console.log(ui);}};

###Événements par critère {#search-criteria-html5:1007c56e-cc29-4b16-9924-96a1df843e0c}

change
:   déclenché lorsque la valeur du critère change.  
    Le premier paramètre envoyé est un objet event, le deuxième contient la nouvelle valeur, ou la valeur ajoutée et la 
    liste des nouvelles valeurs pour les critères enum et relation multiple


[mustache]: https://github.com/janl/mustache.js