# <p align='center'>  Attaque par dictionnaire</p>
![image](https://user-images.githubusercontent.com/85867562/179609398-142ea052-9352-4e83-970b-a0ce3ade0261.png)

## INTRODUCTION DE L'IDÉE DU PROJET
<p>
Une attaque par dictionnaire est une technique d'attaque par force brute pour laquelle les attaquants utilisent des mots et des phrases courantes, comme ceux d'un dictionnaire, pour deviner des mots de passe. Parce que les gens utilisent souvent des mots de passe simples et faciles à retenir, et pour plusieurs de leurs comptes, les attaques par dictionnaire ont des chances d'aboutir sans nécessité la mobilisation de beaucoup de ressources. Une attaque par dictionnaire est une variante d'attaque par force brute, sauf qu'elle utilise une liste prédéfinie de mots de passe potentiellement très utilisés et ayant une plus forte probabilité de succès .
</p>

## Objectifs du projet 

1. paralléliser le code séquentiel.
2. Expliquation du contenu du code (séquentiel + paralléle).
3. Comparaison de la version séquentielle avec la version paralléle au niveau du temps de l’exécution.
4. Faire varier les paramétres de votre code (nombre de threads,concurrence...) et voir son impacte sur le temps d’exécution (Vaux mieu faire celà dans un tableau).

## Le code séquentiel
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <time.h>
#include <omp.h>

int main(int argc, char **argv)
{
        FILE *dictionary;
        time_t tstart,tend;
        char str[200];
        char *user_input;
        char string[200];
        int i,j=1,k;
        int found;

        found=0;
        user_input=argv[1];//stocker le texte chiffre entrer par l'utilisateur
        //ouvrir le dictionaire en mode lecture
        dictionary =fopen("C:/Users/Hsaine/Desktop/S6/calculparallelleetapplicationreparties/projet-dictionnay attack/darkc0de.lst","r") ;
        /*un dictionnaire de taille 213558*/
        //dictionary =fopen("C:/Users/Hsaine/Desktop/S6/calculparallelleetapplicationreparties/projet-dictionnay attack/words.english","r") ;
        //debut du compteur du temps
        time(&tstart);
       
        
        for ( k = 1; k < 1707657; k++)
        {
         //pour chaque iteration on charge le mot k du dictionaire dans str
           fgets(str,sizeof(str),dictionary);
            
           printf("itration: %s\n",str);
            j++;
            //calcul du lenght de str(le mot du dictionaire)
            int len_str=strlen(str)-1;
            //calcul du lenght de user_input
            int len_userinput = strlen(user_input)-1;
            int key;
            if(str[len_str]=='\n')
            str[len_str] = 0;
            //boucles imbriquers pour tester le mot chiffrer avec le user_input
            //traverser le cles possibles
            for(key=-20;key<=20;key++)
               {
                   //faire le traitement pour tout les lettres du user_input
                for (i=0 ; i < len_userinput; i++){
              //dechiffrement de chaque element du user_input en utilisant le dechiffrement par decalage       
                     string[i]=user_input[i]+key;

                    
                                              }
            //comparaison entre le texte clair obtenu et le mot extrait du dictionaire                        
                    if(strcmp(string ,str)==0){
            //affichage si trouve une egalite
                     printf("Found %s\n",str);
                     printf("key %d\n",key*-1);
                      found++;
                                              }
                }
          }
          //si il ne trouve aucune combinaison faire l'affichae suivent 
        if(found==0)
        printf("Not Found \n");
        //fermer le dictionnaire
        fclose(dictionary);
 //arreter le compteur du temps
time(&tend);
    time_t duration;
    duration = tend - tstart;
    struct tm *dur;
    dur = gmtime(&duration);
    //afficher le temps du execution
    printf("\nDuration: \n");
    printf("%d sec\n", dur->tm_sec);
    printf("%d min\n", dur->tm_min);
    printf("%d hour(s)\n", dur->tm_hour);
    printf("%d day(s)\n", dur->tm_mday-1);
    
	
	return 0;
}
```
## Explication de code séquentiel

<ol>
  <li>Stocker le mot chiffré entré par l'utilisateur dans la variable "user_imput".</li>
  <li>Ouvrir le dictionnaire.</li>
  <li>Entrer dans une boucle pour extraire les mots dudictionnaire.</li>
  <li> Pour chaque mot du dictionnaire :
    <ol>
      <li>ccrypter en ajoutant une" clé à user_imput"commençant par la clé -20.</li>
      <li>Comparaison entre cette dernière combinaison avec ce mot:Si ils sont égaux => afficher le mot et la clé.</li>
      <li>Incrémentation de la clé et refaire chaque fois lemême test.</li>
      Si aucun mot n'a été trouvé :affichage " non trouvé"
      Fermer le dictionnaire
    </ol>
  </li>
</ol>

## Le code paralléle
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <time.h>
#include <omp.h>

int main(int argc, char **argv)
{
        FILE *dictionary;
        time_t tstart,tend;
        char str[200];
        char *user_input;
        char string[200];
        int i,j=1,k;
        int found;

        found=0;
        user_input=argv[1];//stocker le texte chiffre entrer par l'utilisateur
       /*un dictionnaire de taille 213558*/
        //dictionary =fopen("C:/Users/Hsaine/Desktop/S6/calculparallelleetapplicationreparties/projet-dictionnay attack/words.english","r") ;
        //ouvrir le dictionaire en mode lecture de taille 1707657
        dictionary =fopen("C:/Users/Hsaine/Desktop/S6/calculparallelleetapplicationreparties/projet-dictionnay attack/darkc0de.lst","r") ;
       
        //debut du compteur du temps
        time(&tstart);
       /*dans ce cas un thread qui a terminÃ© un bloc de calculs reÃ§oit le bloc dâ€™itÃ©rations suivant de notre code. 
        pour parallÃ©liser la boucle,On a utilisÃ© la directive "#pragma omp parallel for schedule(dynamic)" 
        et on a les passÃ©s comme paramÃ¨tres :schedule: qui gÃ¨re la distributions des itÃ©rations de la boucle 
        Ã  travers les threads et comme clause:dynamic
       */

       #pragma omp parallel for schedule(dynamic) 
        for ( k = 1; k < 1707657; k++)
        {
         //pour chaque iteration on charge le mot k du dictionaire dans str
           fgets(str,sizeof(str),dictionary);
           /*aprÃ¨s l'appel de printf qui affichent les mots de dictionnaire,
            le nombre de threads exÃ©cutant dans  cette rÃ©gion est 1. */
           omp_set_num_threads(1);
           printf("itration: %s\n",str);
            j++;
            //calcul du lenght de str(le mot du dictionaire)
            int len_str=strlen(str)-1;
            //calcul du lenght de user_input
            int len_userinput = strlen(user_input)-1;
            int key;
            if(str[len_str]=='\n')
            str[len_str] = 0;
            //boucles imbriquers pour tester le mot chiffrer avec le user_input
            //traverser le cles possibles
            #pragma omp parallel for num_threads(2) /*afin que l'index de la premiÃ¨re  boucle for est divisÃ© entre les threads,*/
            for(key=-20;key<=20;key++)
               {
                   //faire le traitement pour tout les lettres du user_input
                for (i=0 ; i < len_userinput; i++){
              //dechiffrement de chaque element du user_input en utilisant le dechiffrement par decalage       
                     string[i]=user_input[i]+key;

                    
                                              }
            //comparaison entre le texte clair obtenu et le mot extrait du dictionaire                        
                    if(strcmp(string ,str)==0){
            //affichage si trouve une egalite
                     printf("Found %s\n",str);
                     printf("key %d\n",key*-1);
                      found++;
                                              }
                }
          }
          //si il ne trouve aucune combinaison faire l'affichae suivent 
        if(found==0)
        printf("Not Found \n");
        //fermer le dictionnaire
        fclose(dictionary);
 //arreter le compteur du temps
time(&tend);
    time_t duration;
    duration = tend - tstart;
    struct tm *dur;
    dur = gmtime(&duration);
    //afficher le temps du execution
    printf("\nDuration: \n");
    printf("%d sec\n", dur->tm_sec);
    printf("%d min\n", dur->tm_min);
    printf("%d hour(s)\n", dur->tm_hour);
    printf("%d day(s)\n", dur->tm_mday-1);
    
	
	return 0;
}
              
      
```

## Explication de code paralléle

* Dans notre code on trois boucles for, une boucle for externe(globale) responsable de parcourt des mots de dictionnaire, et deux boucles imbriqués internes pour prendre le mot et le chiffrer et le comparer.
* Pour paralléliser la boucle externe ,On utilisé la directive **#pragma omp parallel for schedule(dynamic)** et on a passé comme paramètres **schedule** qui gère la distributions des itérations de la boucle à travers les threads et comme **clause dynamic**.
*  **#pragma omp parallel for schedule(dynamic)** dans ce cas un thread qui a terminé un bloc de calculs reçoit le bloc d’itérations suivant de notre code.
* **omp_set_num_threads(1)**, après l'appel de printf qui affichent les mots de dictionnaire,le nombre de threads exécutant dans cette région est 1.
* Pour paralléliser les deux boucles internes de notre code on a utilisé la directive suivante: **#pragma omp parallel for num_threads(2)** afin que l'index de la première boucle for est divisé entre les threads, on a essaye de paralléliser l'intégrité de les deux boucles en ajoutant la
**collapse clause** pour réduire le temps d'exécution mais ça ne marche pas car le parallélisme d'une boucle for conditionnelle est interdite.

## Le dictionnaire 
vous trouvez là le lien de dictionnaire utilisé  [dictionnaire](darkc0de.lst).

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


<h1 align="center">Contact </h1>
  
 Email: 
<hsaineayoub77@gmail.com>

[mon linkedin](https://www.linkedin.com/in/ayoub-hsaine-a3a1b3197/)




