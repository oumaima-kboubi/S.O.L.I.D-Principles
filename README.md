# S.O.L.I.D-Principles

Théorisé en 2002 par Robert C. Martin dans son ouvrage Agile Software Development, Principles, Patterns and Practices, l’acronyme SOLID est un moyen mnémotechnique pour retenir 5 grands principes applicables au développement d’applications logicielles pour les rendre plus faciles à comprendre, à maintenir et à faire évoluer.

Ces 5 principes sont :
* S comme Single Responsibility Principle 
* O comme Open/Closed Principle
* L comme Liskov Substitution Principle
* I comme Interface Segregation Principle
* D comme Dependency Inversion Principle.

Pour plus de détails consulter le fichier ```pdf``` dans le dossier ```Compte Rendu```

## 📖Single Responsibility Principle 
Le principe SRP pour "Single Responsibility Principle" énonce qu’une classe ne devrait avoir qu’une et une seule raison de changer.

L’idée ici est de faire en sorte qu’une classe ne soit responsable que d’une seule fonction de votre application, et que cette responsabilité soit complètement encapsulée  dans la classe.

L’objectif du principe SRP est de réduire la complexité de votre projet.

### ✒️Explication de l'exemple de  ```SRP```

⭕Avant la modification du code la classe CarManager s'occupe de la récupération des noms des voitures et de la recherche de la meilleure voiture ainsi que la récupuration d'une des voitures de la base de données par son id.

=> ⚠️La classe CarManager a plusieurs responsabilités! 
Aprés la modification, on a dévisé cette classe en plusieurs classes chacunes à responsabilité limitée comme suit:
* La classe CarDao s'occupe de la récupération des données de la base de données.
* La classe CarFormatter s'occupe de la transformation de l'objet Car en une chaine contenant les caractéristiques de la voiture concernée.
* La classe CarRater s'occupe de la récupération de la meilleure voiture.

✔️Aprés cette modification chaque classe a une seule responsabilité, le code est plus facile à lire, à maintenir et à tester.

## 📖Open/Closed Principle

Le second principe SOLID est l’"Open/Closed Principle" (OCP) : les classes d’un projet devraient être ouvertes à l’extension, mais fermées à la modification.

L’idée de fond derrière ce principe est la suivante : ajouter de nouvelles fonctionnalités ne devrait pas casser les fonctionnalités déjà existantes.

Changer le comportement d’une classe existante pour l’adapter à un nouveau besoin il vaut mieux étendre cette classe et en adapter son comportement : elle est donc ouverte à l’extension et fermée à la modification.

L’intérêt de faire cela, c’est d’éviter de casser ou d’introduire des bugs dans une application qui fonctionne correctement.

### ✒️Explication de l'exemple de  ```OCP```

⭕Avant la modification du code la classe ResourceType contient une énumération des types de ressource ("TIME_SLOT" ou bien "SPACE_SLOT"), La classe RessourceAllocator s'occupe des traitements de toutes les ressources.

=> ⚠️Lors de l'ajout d'un nouveau type, il faut modifier la classe de RessourceAllocator se qui peut engendrer des problèmes dans l'application! 

✔️Aprés la modification, on a dévisé cette classe comme suit:

* L'interface Resource contient les méthodes génériques qui vont être utilisées par les diffrents types de ressources.
* La classe SpaceResource et TimeResource implémente l'interface générique Resource, donc par défaut elle doit redéfinir les méthodes de l'interface selon la spécificité de son type "Space" ou "Time".
* La classe ResourceAllocator utilise la méthode de type spécifique passé en paramètre comme Resource de façon générique (surclassement)

=>De cette façon, pour ajouter un nouveau type de ressource il suffit d'implémenter l'interface Resource et redefinir les méthodes nécessaires selon la spécifité du nouveau type et puis passer l'instance de la nouvelle classe en paramètre au RessourceAllocator pour exécuter ses méthodes spécifiques.

## 📖Liskov Substitution Principle

Le troisième principe SOLID met en valeur non pas une, mais deux brillantes développeuses. En effet, le principe de substitution a été formulé pour la première fois par Barbara Liskov et Jeannette Wing dans un article intitulé Family Values : A Behavioral Notion of Subtyping [EN] en 1994.

Il complète le second principe : il doit être possible de substituer une classe "parente" par l’une de ses classes enfants. Pour cela, nous devons garantir que les classes enfants auront le même comportement que la classe qu’elles étendent.

Une classe devrait implémenter une interface, notamment si elle a pour objectif d’être étendue.

Si nous lançons des exceptions, il vaut mieux avoir une classe d’exception par type d’erreur, puis étendre celle-ci pour chaque cas d’erreur. Nous nous assurons dans ce cas que le code qui dépend de notre implémentation sera en capacité de gérer proprement les cas d’erreur.

Pour résumer:

Le principe de substitution de Liskov reprend le principe Open/Closed et l'applique au cas particulier de l'héritage de classes : si une classe enfant est une implémentation valide, alors une classe parent doit également l'être (et vice versa).

D'un point de vue fonctionnel, cela revient à formaliser un contrat sur nos objets au sujet de leur implémentation, c'est à dire le comportement de leurs fonctions.

### ✒️Explication de l'exemple de  ```LSP```

⭕Avant la modification du code la classe Duck précise deux méthodes qui sont quack et swim.

La classe ElectronicDuck hérite son comportement de la classe Duck donc implécitement les méthodes quack et swim mais en ajoutant une exception selon le fonctionement particulier de cette classe.

=> ⚠️On ne peut pas remplacer la classe parente Duck par la classe fille ElectronicDuck car cette dernière lance une exception et sa classe mére ne lance aucune exception lors de son fonctionnement!

✔️Aprés la modification, on a procédé comme suit:

* L'interface IDuck contient le type générique ayant les méthodes quack et swim ainsi que la classe d'exception spécifique à ce type IDuckException qui hérite de la calsse générique des exceptions Exception.
* Les deux classes ElectronicDuck et Duck implémentent l'interface IDuck, la classe ElectronicDuck spécifie une classe d'exception lorsque le duck n'est pas en marche DuckIsOffException qui hérite de la classe d'exception IDuckException précisée dans l'interface IDuck.
* Les méthodes de la classe Pool reçoit comme paramètre l'interface IDuck, alors on peut passer n'importe quel type de classe implémentant cette interface et utilisé sa classe d'exception spécifique

=> ⚠️De cette façon, On peut remplacer la classe parente par sa classe enfant. 

## 📖Interface Segregation Principle

Le quatrième principe SOLID commence donc par la lettre I pour "Interface Segregation Principle".

L’idée ici est d’éviter d’avoir des interfaces aux multiples responsabilités et de les redécouper en multiples interfaces qui ont elles une seule responsabilité qui peut se traduire par l’implémentation de plusieurs méthodes.

ce principe revisite le premier principe (Single Responsibility Principle) en l’appliquant cette fois à nos interfaces.

### ✒️Explication de l'exemple de  ```ISP```

⭕Avant la modification du code l'interface Door contient plusieurs méthodes spécifIques à la porte avec d'autre méthode comme "timeOutCallback()" et "proximityCallback()" Donc la classe SensingDoor en implementant l'interface Door est forcée de définir la méthode "timeOutCallback()". Contrairement à la classe qui TimesDoor qui doit définir la méthode "proximityCallback()" !

=> ⚠️L'interface Door n'a pas une responsabilité unique donc elle ne peut être implementée par les deux types de portes "SensingDoor" et "TimedDoor".

✔️Aprés la modification, on enlève ces deux méthodes "timeOutCallback()" et "proximityCallback()" de l'inteface Door et on ajoute deux interfaces TimerClient qui contient la première méthode et l'interface SensorClient qui contient la deuxième méthode. Donc chaque type de porte implemente l'interface qui convient à son type ainsi que l'interface de base Door

## 📖Dependency Inversion Principle

Le cinquième et dernier chapitre du principe SOLID est le principe d’inversion de dépendances : "Dependency Inversion Principle".

Les classes de haut niveau ne devraient pas dépendre directement des classes de bas niveau, mais d’abstractions.

En générale, les classes de haut niveau ne devraient pas dépendre directement des classes de bas niveau, mais d’abstractions. donc ce sont les classes de bas niveau qui doivent dépendre des contrats de haut niveau

* Les classes de bas niveau qui implémentent des fonctionnalités de base : écrire dans un fichier, se connecter à une base de données, retourner une réponse HTTP...
* Les classes de haut niveau qui concernent le métier de l'application ("business logic") : la gestion des coûts de livraison, par exemple.

### ✒️Explication de l'exemple de  ```DIP```

⭕Avant la modification du code la classe EncodingModule contient deux méthodes d'encodage "encodeWithFiles()" et "encodeBasedOnNetworkAndDatabase()". La classe EncodingModuleClient dépend entièrement du la classe EncodingModule

=> ⚠️Il existe un couplage trés fort entre les deux classes!

✔️Aprés la modification, on a procédé comme suit:

* On ajoute deux interfaces "IRead" et "IWriter"
* On ajoute deux classe pour chaque type d'encodage qui permettent de lire et d'écrire :
* MyFileWriter et MyDatabaseWriter pour écrire des données sur un fichier ou bien dans la base de donnée
* MyFileReader et MyNetworkReader pour lire les données soit d'un fichier ou bien du réseau
* La classe EncodingModule contient une méthode générique "encode()" qui a comme paramètres des interfaces IWriter et IReader, donc on peut utiliser l'une des deux méthodes d'encodage selon le besion avec un couplage faible entre les classes
* Dans la classe EncodingModuleClient on va préciser le type d'encodage dont on a besoin en passant les données nécessaires pour les méthodes génériques et laisser le soin d'instancier les objets nécessaires au programme.

