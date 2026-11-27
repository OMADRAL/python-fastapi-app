## Q1. Combien de jobs sont générés ? Donnez la liste des combinaisons.

La matrice génère 6 jobs, car elle combine 2 systèmes
d'exploitation (Ubuntu et Windows) avec 3 versions de Python
(3.11, 3.12 et 3.13).

Les combinaisons sont :

- Ubuntu + Python 3.11
- Ubuntu + Python 3.12
- Ubuntu + Python 3.13
- Windows + Python 3.11
- Windows + Python 3.12
- Windows + Python 3.13

## Question Q2 – Combien de jobs au total ?
Correction : 6 jobs.

Explication :
1. La matrice initiale génère 6 jobs.
2. exclude supprime Windows + Python 3.11 : il reste 5 jobs.
3. include ajoute Ubuntu + Python 3.14 : on obtient 6 jobs.
\[6-1+1=6\]

## Question Q3 – Que se passerait-il si on tentait d'exclure Python 3.14 ? Pourquoi ?
Correction :

Python 3.14 resterait dans la matrice.

En effet, exclude est appliqué avant include. Au moment où GitHub Actions applique exclude, la combinaison Ubuntu + Python 3.14 n'existe pas encore dans la matrice initiale.

Ensuite, include ajoute cette combinaison.
Donc, même si on écrit :
exclude:
  - os: ubuntu-latest
    python-version: "3.14"


la combinaison ajoutée ensuite par include sera toujours présente.

## Question Q4 – Quelle est la différence observée ? Dans quel cas choisir true ou false ?
Correction :
fail-fast: true	fail-fast: false
Comportement par défaut	Désactive l'arrêt anticipé
Un échec peut annuler les autres jobs en cours ou en attente	Les autres jobs continuent
Exécution potentiellement plus rapide	Permet de récupérer tous les résultats

Quand utiliser true ?

Lorsqu'on souhaite arrêter rapidement les tests après un échec important afin d'économiser du temps et des ressources.

Quand utiliser false ?

Lorsqu'on souhaite obtenir les résultats de toutes les combinaisons, notamment pour tester la compatibilité d'une application sur plusieurs systèmes et versions de Python.