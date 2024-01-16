# Exercices

- Créez un projet nommé `exercices` dans lequel vous placerez tous vos exercices, de ce chapitre et des autres
  chapitres.
- Créez un dossier nommé `chap01` à l'intérieur de ce dossier de projet.
- Créez un fichier `main.go` dans ce dossier, qui sera dans le paquet `main`, et qui contiendra la fonction `main`.
- Créez un fichier `exercices.go`, qui sera aussi dans le paquet `main`, qui contiendra les fonctions qui répondent aux
  questions suivantes.
- Créez le fichier `input.go` qui contiendra les fonctions `ReadLine` et `ReadFloat64`, à utiliser pour vous aider à
  répondre aux questions.
    - Vous pouvez appeler les fonctions de la manière suivante : `line, _ := ReadLine()` et `x, _ := ReadFloat64()`.
    - Les entrées/sorties seront présentées en détails dans les prochains chapitres. Pour le moment, les erreurs sont
      ignorées en utilisant `_`.
    - Contenu du fichier `input.go` :

```Go
 package main

import (
	"bufio"
	"io"
	"os"
	"strconv"
)

func ReadLine() (string, error) {
	scanner := bufio.NewScanner(os.Stdin)
	if scanner.Scan() {
		return scanner.Text(), nil
	} else {
		err := scanner.Err()
		if err != nil {
			return "", err
		} else {
			return "", io.EOF
		}
	}
}

func ReadFloat64() (float64, error) {
	line, err := ReadLine()
	if err != nil {
		return 0, err
	}
	x, err := strconv.ParseFloat(line, 64)
	if err != nil {
		return 0, err
	}
	return x, nil
}

```

- Pour chacune des questions suivantes, écrivez une fonction qui effectuera ce qui est demandé.
- Ajoutez un appel à chacune des fonctions dans la fonction `main` pour tester votre code. Vous pouvez commenter les
  appels aux fonctions précédentes dans le `main` pour éviter d'exécuter toutes les fonctions précédentes quand vous
  ajouter une nouvelle fonction.
    - N'effacez pas les appels précédents, gardez-les, mais vous pouvez les commenter.
- Utilisez des nombres réels (type `float64`) pour tous nombres, sauf quand vous devez absolument avoir un nombre
  entier.
    - Vous pouvez convertir in `float64` en `int` de cette façon :

```Go
var f float64 = 3.14
var i int = int(f)
```

## Questions

1. Calculer et afficher la valeur absolue d'un nombre entré par l'utilisateur.
2. Déterminer si le nombre _entier_ entré par l'utilisateur est pair ou impair.
3. Lire trois nombres et imprimer le plus petit de ces trois nombres.
4. Calculer le salaire total d'un employé. On lit en entrée les données concernant son salaire horaire et le nombre
   d'heures travaillées. Si l'employé a travaillé plus de 40 heures, les heures supplémentaires sont
   payées à 1.5 fois le salaire horaire.
5. Lire trois nombres positifs représentant la longueur des côtés d'un triangle. Imprimer :
    - "Scalène" si les trois côtés sont inégaux
    - "Isocèle" si deux des côtés sont égaux
    - "Équilatéral" si les trois côtés sont égaux
6. Un professeur vous fournit trois notes calculées sur 100. Calculer la moyenne et imprimer échec si la note finale est
   inférieure à 60/100. Dans le cas contraire, imprimer la note obtenue par l'étudiant.
7. Lire en entrée une note finale d'un cours. Si la note est plus petite que 0, ou si la note est plus grande que 100,
   alors afficher "Cette note est invalide" et terminer la fonction. Si la note est valide, alors vous devez afficher
   une lettre correspondant à la note selon les conditions suivantes :
    - **E** : plus petite que 60
    - **D** : de 60 à moins que 70
    - **C** : de 70 à moins que 80
    - **B** : de 80 à moins que 90
    - **A** : 90 ou plus
8. Vous devez lire un nombre entre 1 et 10 inclusivement.
    - Si le nombre n'est pas dans cet intervalle, alors afficher *"invalide"*.
    - Si le nombre est valide, alors afficher *"valide"*.
9. Vous devez lire un nombre entre 1 et 10 inclusivement.
    - Si le nombre n'est pas dans cet intervalle, alors afficher *"invalide"* et demander le nombre à nouveau. Vous
      devez vous assurer que le nombre est valide avant de continuer à la prochaine étape. Il n'y aucune limite sur
      le nombre d'essais incorrects.
    - Si le nombre est valide, alors afficher *"valide"*.
10. Vous devez lire un nombre entre 1 et 10 inclusivement.
    - Si le nombre n'est pas dans cet intervalle, alors afficher *"invalide"* et demander le nombre à nouveau. Vous
      devez vous assurer que le nombre est valide avant de continuer à la prochaine étape. Il y a une limite de 3 essais
      incorrects.
    - Si le nombre maximal d'essais incorrects a été atteint, alors afficher *"Nombre maximal d'essais atteint."* et la
      doit se terminer.
    - Si le nombre est valide, alors afficher *"valide"*.
11. Vous devez lire un nombre _entier_ et l'afficher à l'envers. Par exemple, l'utilisateur saisit `123456` et le
    programme affiche `654321`. Pour cela il faudra utiliser la division et le modulo.
12. Vous devez lire un nombre _entier_ et afficher un décompte à partir de ce nombre jusqu'à 0. Lorsque le décompte est
    terminé, afficher "Terminé !" à la place du nombre 0. Par exemple, si le nombre entré est 5, vous devez afficher
    ````
    5
    4
    3
    2
    1
    Terminé !
    ````
13. Pour chacune des questions suivantes, définissez un tableau de nombres entiers de cette
    manière : `numbers := [...]int{2, 5, 3, 4, 7, 1, 7}` . La longueur du tableau et les nombres qu'il contient sont à
    votre choix, mais ils devraient permettre de bien tester les questions. Répondez aux questions dans des fonctions
    différentes. En utilisant une boucle `for` sur les index du tableau,
    1. trouvez la somme et la moyenne de tous les nombres dans le tableau.
    2. affichez tous les nombres pairs contenus dans le tableau.
    3. déterminez si tous les nombres dans le tableau sont positifs ou non. Si tous les nombres sont positifs, alors
       affichez *vrai*, sinon affichez *faux*.
14. Répétez la question précédente, mais en utilisant une boucle `for` et un `range` sur le tableau.