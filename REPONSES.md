
# TP6 – Variables d'environnement, Inputs et Outputs

## Q1. Pourquoi la variable n'est-elle pas disponible dans la step qui l'a définie, mais seulement dans les suivantes ?

GITHUB_ENV est lu par GitHub Actions après l'exécution de la step. Les variables sont donc disponibles uniquement dans les steps suivantes du même job.

## Q2. Quelle est la portée de GITHUB_ENV ?

GITHUB_ENV permet de partager les variables d'environnement entre les steps d'un même job. La variable APP_NAME n'est donc pas disponible dans le job verify-env.

## Q3. Que se passe-t-il si on écrit > au lieu de >> dans GITHUB_OUTPUT ?

L'opérateur >> ajoute les données sans supprimer le contenu précédent. L'opérateur > écrase le contenu précédent. Le deuxième output remplace donc le premier si on utilise > deux fois.

## Q4. Pourquoi GITHUB_ENV ne suffit-il pas ici, alors que GITHUB_OUTPUT fonctionne entre jobs ?

GITHUB_ENV partage les variables entre les steps d'un même job. GITHUB_OUTPUT permet de créer des outputs au niveau d'une step, de les exposer au niveau du job et de les récupérer dans un autre job grâce à needs.

## Q5. Que se passe-t-il si on retire needs: build du job deploy ?

Le job deploy peut s'exécuter en parallèle avec build. Il ne dispose plus de la dépendance permettant de récupérer les outputs de build avec needs.build.outputs.full_version.



# TP7 – Actions tierces et actions personnalisées

## Q1. Pourquoi actions/checkout n'a-t-il pas besoin de with alors que setup-node en a besoin ?

actions/checkout utilise des paramètres par défaut pour récupérer le dépôt courant. Il n'est donc pas nécessaire de préciser with dans notre cas.

actions/setup-node a besoin de with pour connaître la version de Node.js à installer. Ici, nous utilisons node-version: 20.

## Q2. Pourquoi le job verify ne retrouve-t-il pas report.txt sans l'artefact, alors que les deux jobs partagent le même dépôt ?

Chaque job s'exécute sur un runner indépendant. Le fichier créé dans le job build n'existe donc pas automatiquement dans le job verify.

L'artefact permet de transférer le fichier entre les deux jobs.


## Q6. Pourquoi actions/checkout doit-il s'exécuter avant l'appel d'une action locale ?

actions/checkout récupère les fichiers du dépôt sur le runner.

L'action locale se trouve dans le dépôt, dans .github/actions/build-info/action.yml. Il faut donc récupérer le dépôt avant de pouvoir utiliser cette action.

## Q7. Pourquoi shell: bash est-il obligatoire sur les steps run d'une action composite ?

Dans une action composite, chaque step qui utilise run doit préciser son shell.

GitHub doit connaître l'interpréteur à utiliser pour exécuter les commandes. Ici, nous utilisons bash.
