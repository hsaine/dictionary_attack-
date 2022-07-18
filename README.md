# <p align='center'>dictionary_attack</p>
![image](https://user-images.githubusercontent.com/85867562/179609398-142ea052-9352-4e83-970b-a0ce3ade0261.png)

## INTRODUCTION DE L'IDÉE DU PROJET
<p>
Une attaque par dictionnaire est une technique d'attaque par force brute pour laquelle les attaquants utilisent des mots et des phrases courantes, comme ceux d'un dictionnaire, pour deviner des mots de passe. Parce que les gens utilisent souvent des mots de passe simples et faciles à retenir, et pour plusieurs de leurs comptes, les attaques par dictionnaire ont des chances d'aboutir sans nécessité la mobilisation de beaucoup de ressources. Une attaque par dictionnaire est une variante d'attaque par force brute, sauf qu'elle utilise une liste prédéfinie de mots de passe potentiellement très utilisés et ayant une plus forte probabilité de succès .
</p>

## Objectif de projet 

1. paralléliser le code séquentiel.
2. Expliquation du contenu du code (séquentiel + paralléle).
3. Comparaison de la version séquentielle avec la version paralléle au niveau du temps de l’exécution.
4. Faire varier les paramétres de votre code (nombre de threads,concurrence...) et voir son impacte sur le temps d’exécution (Vaux mieu faire celà dans un tableau).

## Le code séquentiel

## Le code paralléle

## Tableau de comparaison des resultas

| La taille du dictionnaire      |  Le temps d'exécution du code séquentiel |  Le temps d'exécution du code parallel      |  Le gain du parallisme     |
| :---        |    :----:   |          :----:  |    :----:   |  
| 1707656      | 9 min 20 s       | 6 min 38 s  | 30%  |
| 213558   | 1 min 7 s        | 44 s      |  33% |

### Explication 
<p>l'utilisation de cette méthode pour paralléliser notre code séquentiel nous permet de le paralléliser avec un gain de 30% à 40% qui nous pouvons considérer comme un résultat important suivent à les instructions lourdes utiliser dans le code(lire à partir de dictionnaire ,les deux boucles imbriquées, une boucle conditionnelle ...).</p>

## Conclusion 
Ce projet nous a permet de familiariser avec **OPENMP** et bien maitriser les différents directives de cette API standard pour les systèmes parallèles à mémoire partagée.
Étant donné que les attaques par dictionnaire s'appuient sur des mots couramment utilisés comme mots de passe, une politique de mots de passe forte permet déjà de bien se protéger contre ces attaques. Il faut encourager les utilisateurs à créer des mots de passe uniques - idéalement une combinaison de mots aléatoires avec des symboles et des chiffres - à ne pas les réutiliser ou les partager.

## Contact 

