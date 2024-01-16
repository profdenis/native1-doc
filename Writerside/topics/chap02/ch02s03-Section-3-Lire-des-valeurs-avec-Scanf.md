# Section 3 : Lire des valeurs avec Scanf

La fonction `Scanf` est la contrepartie à la fonction `Printf` : au lieu d'imprimer, ou d'écrire, des valeurs sur la
sortie standard (_stdout_), `Scanf` lit des valeurs sur l'entrée standard (_stdin_). Les fonctions `Sscanf` et `Fscanf`
sont semblables à `Scanf`, mais lisent à partir, respectivement, d'une chaîne de caractères et d'un fichier.

Des alternatives à `Scanf` vont être présentées dans la section suivante. `Scanf` est un peu capricieuse, donc il faut
faire attention quand on l'utilise. Cette fonction est basée sur la fonction `scanf` du langage C.

## Exemple 1

Voici un programme qui lit le nom de l'utilisateur sur le _stdin_, suivi de son âge.

```go
package main

import "fmt"

func ex1() {
    var name string
    var age int

    fmt.Print("Veuillez entrer votre nom : ")
    _, err := fmt.Scanf("%s", &name)
    
    if err != nil {
        fmt.Println(err)
    }

    fmt.Print("Veuillez entrer votre âge : ")
    _, err = fmt.Scanf("%d", &age)
    
    if err != nil {
        fmt.Println(err)
    }

    fmt.Printf("Bonjour %s, vous avez %d ans.\n", name, age)
}
```

`Scanf` retourne 2 valeurs : un entier qui contient le nombre d'éléments _scannés_ correctement, et une erreur, qui
pourrait être `nil` s'il n'y a pas d'erreurs. Dans cet exemple, on ignore la première valeur retournée en utilisant `_`
à la place d'une variable. Nous allons utiliser cette valeur dans d'autres exemples plus tard. Après chaque appel, on
vérifie s'il y a des erreurs ou non en comparant `err` à `nil`. Si `err` n'est pas `nil`, alors il y a une erreur et on
écrit cette erreur sur le _stdout_.

Le premier paramètre donné à `Scanf` est le format, similaire au format donné à la fonction `Printf`. Le deuxième
paramètre est l'endroit où l'on veut que la valeur _scannée_ soit placée. Nous devons donner l'adresse d'une variable,
ou un pointeur, sinon, comme on a vu plus tôt, les changements apportés à la variable ne seraient pas visibles à
l'extérieur de la fonction. C'est pourquoi on donne l'adresse de la variable `name` dans le premier cas, et de la
variable `age` dans le deuxième, en utilisant l'opérateur `&`.

### Exécution 1

La sortie dépendra évidemment des valeurs entrées par l'utilisateur. La sortie, incluant les valeurs `Denis` et `22`
entrées sur les 2 premières lignes, ressemblera à ceci :

```
Veuillez entrer votre nom : Denis
Veuillez entrer votre âge : 22
Bonjour Denis, vous avez 22 ans.
```

Notez que le caractère `\n` n'est pas inclus dans la valeur de `name` même si on doit peser sur _enter_ après avoir
entré le nom. La fonction `Scanf` accepte tout les caractères jusqu'à ce qu'elle rencontre la fin de la ligne. C'est le
même processus pour l'âge, sauf que les caractères entrés sont convertis en un nombre entier parce que le format `%d` a
été spécifié.

### Exécution 2

Si on inverse les deux valeurs, on obtient :

```
Veuillez entrer votre nom : 22
Veuillez entrer votre âge : Denis
expected integer
Bonjour 22, vous avez 0 ans.
```

`Scanf` ne peux pas savoir si 22 est un nom approprié ou non, le format `%s` lui demande tout simplement d'accepter une
chaîne de caractères, donc la chaîne `"22"` est acceptée. Il faudra utiliser du code supplémentaire pour valider le
contenu de chaînes de caractères (à voir plus tard).

Mais pour l'âge, la chaîne `"Denis"` ne peut définitivement pas être convertie en nombre entier, donc on obtient
l'erreur `expected integer`. Quand une erreur de la sorte arrive, la variable pointée par le paramètre de `Scanf`, dans
le cas-ci la variable `age`, ne sera pas modifiée, donc la valeur écrite à la fin sera la valeur que la variable avait
avant l'appel à `Scanf`.

### Exécution 3

Qu'arrive-t-il si on n'entre aucun caractère (à part le _enter_) ?

```
Veuillez entrer votre nom : 
unexpected newline
Veuillez entrer votre âge : 
unexpected newline
Bonjour , vous avez 0 ans.
```

On obtient l'erreur `unexpected newline`. `Scanf` ne modifiera pas les variables `name` et `age` à travers leurs
pointeurs si les valeurs entrées sont vides, donc la sortie inclut la valeur que ces variables avaient les appels
à `Scanf`. Puisque ces variables n'avaient pas été explicitement initialisées, elles ont leurs valeurs zéros,
c'est-à-dire `""` et `0` respectivement.

### Exécution 4

Une particularité de l'interaction avec le _stdin_ est qu'il faut entrer un changement de ligne pour que les caractères
entrés soient traités par `Scanf`. Cette fonction va alors lire chaque caractère entré et va essayer de les faire
correspondre au format donné à la fonction. Quand `Scanf` rencontre un caractère blanc (un _whitespace character_, comme
un espace `' '`, un changement de ligne `'\n'`, une tabulation `'\t'`), ça indique la fin d'un format. Par exemple, la
chaîne `"1 100"` représente 2 nombres pour `Scanf` (_un_ et _cent_), et non pas le nombre _mille cent_.

Dans notre exemple, on peut entrer les deux valeurs sur la même ligne. Le premier appel à `Scanf` va lire jusqu'à
l'espace entre le `'s'` et le premier `'2'`, et le deuxième appel à `Scanf` va lire le nombre 22 jusqu'au changement de
ligne. Ça va fonctionner, mais la sortie sera moins belle.

```
Veuillez entrer votre nom : Denis 22
Veuillez entrer votre âge : Bonjour Denis, vous avez 22 ans.
```

Des exemples avec une meilleure validation des entrées seront présentés un peu plus loin dans la prochaine section.

### Exécution 5

Quand `Scanf` essaie de lire un nombre entier, la fonction va essayer de lire des chiffres, et quand un caractère qui
n'est pas un chiffre est rencontré, ça va terminer la lecture du nombre entier. En général, `Scanf` va essayer de lire
autant de caractères possible pour les faire correspondre au format.

```
Veuillez entrer votre nom : Denis
Veuillez entrer votre âge : 2.2
Bonjour Denis, vous avez 2 ans.
```

Dans ce cas-ci, `Scanf` arrête au point, donc l'âge est 2. `Scanf` n'est pas parfait, mais elle a basée sur la
fonction `scanf` du langage de programmation C. Le but des développeurs du langage Go était de copier le comportement du
langage C dans ce cas-ci, pour être compatible.

## Exemple 2

Cet exemple est semblable au précédent, mais le nom et l'âge doivent être entrés sur la même ligne, séparés par un
espace. Il n'y a qu'un seul appel à `Scanf` ici parce que le format est "%s %d", qui demande d'avoir une chaîne de
caractères suivie d'un nombre entier sur la même ligne.

```go
func ex2() {
	var name string
	var age int

	fmt.Print("Veuillez entrer votre nom suivi de votre âge, séparés par un espace : ")
	n, err := fmt.Scanf("%s %d", &name, &age)

	if err != nil {
		fmt.Println(err)
	}

	if n != 2 {
		fmt.Println("Vous devez entrer votre nom suivi de votre, séparés par un espace")
		return
	}

	fmt.Printf("Bonjour %s, vous avez %d ans.\n", name, age)
}
```

Une autre différence dans cet exemple est la vérification du nombre d'éléments lus correctement, ici placé dans la
variable `n`. Dans cet exemple, `n` devrait être 2. L'utilisation de `n` donne une façon différente de détecter les
erreurs. En pratique, on utilise soit `err`, soit `n`, selon les besoins, mais pas nécessairement les deux à la fois.

### Exécution 1 {id="ex-cution-1_1"}

Entrée : `Denis 22\n`

```
Veuillez entrer votre nom suivi de votre âge, séparés par un espace : Denis 22
Bonjour Denis, vous avez 22 ans.
```

### Exécution 2 {id="ex-cution-2_1"}

Entrée : `Denis\n`

```
Veuillez entrer votre nom suivi de votre âge, séparés par un espace : Denis
newline in input does not match format
Vous devez entrer votre nom suivi de votre, séparés par un espace
```

### Exécution 3 {id="ex-cution-3_1"}

Entrée : `Denis abc\n`

```
Veuillez entrer votre nom suivi de votre âge, séparés par un espace : Denis abc
expected integer
Vous devez entrer votre nom suivi de votre, séparés par un espace
```