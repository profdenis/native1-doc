# Section 2 : Méthodes

Voici le contenu du fichier `point.go`, qui contient le même définition de la structure `Point` et le même constructeur
que dans la section précédente, mais qui contient en plus quelques méthodes définies sur `Point`.

[point.go sur Github](https://github.com/profdenis/native1/tree/master/chap5/point.go)

```go
package main

import (
	"fmt"
	"math"
)

type Point struct {
	X float64
	Y float64
}

func NewPoint(x float64, y float64) *Point {
	return &Point{X: x, Y: y}
}

// String convertir le Point en String pour implémenter l'interface Stringer
func (p *Point) String() string {
	return fmt.Sprintf("Point(%f, %f)", p.X, p.Y)
}

// Add additionne 2 points et retourne le résultat dans un 3e point
func (p *Point) Add(other *Point) *Point {
	return &Point{p.X + other.X, p.Y + other.Y}
}

// Subtract soustrait 2 point et retourne le résultat dans un 3e point
func (p *Point) Subtract(other *Point) *Point {
	return &Point{p.X - other.X, p.Y - other.Y}
}

// Distance calcule et retourne la distance Euclidienne entre 2 point
func (p *Point) Distance(other *Point) float64 {
	return math.Sqrt(math.Pow(other.X-p.X, 2) + math.Pow(other.Y-p.Y, 2))
}

// Scale mise à l'échelle d'un Point par un facteur et retourne le résultat dans un nouveau point
func (p *Point) Scale(factor float64) *Point {
	return &Point{p.X * factor, p.Y * factor}
}
```

Les méthodes sont un type de fonctions particulières : ce sont des fonctions définies sur des structures. C'est le même
concept que les méthodes dans d'autres langages comme Java et C#, mais définies sur des structures à la place des
classes.

## Est-ce que Go est orienté-objet ?

Go n'est pas un langage orienté-objet dans le même sens que Java et C# le sont parce que Go ne supporte pas
l'héritage entre classes (ou structures), mais à part pour cette différence, les structures et les classes sont des
concepts très similaires. Go supporte les interfaces et la composition de structures et d'interfaces en plus des
méthodes, mais pas l'héritage entre classes et le polymorphisme, donc c'est pourquoi Go n'est pas considéré comme étant
orienté-objet par certains qui considèrent l'héritage entre classes et le polymorphisme comme étant obligatoire pour qu'
un langage soit qualifié d'orienté-objet. Il n'existe aucune définition exacte de ce qu'est un langage orienté-objet,
donc dépendamment de la définition utilisée, Go peut être considéré comme orienté-objet ou non.

## Comment définir une méthode

Comme présenté dans l'exemple plus haut, les méthodes sont définies en dehors de la définition de la structure. Une
méthode est une fonction avec un élément de plus : un _receveur_ est ajouté entre le mot `func` et le nom de la
fonction. Le receveur défini sur quel type de structure la méthode peut être appelée. Le receveur peut être de type
valeur ou pointeur. Le receveur est comme un paramètre supplémentaire sur une fonction, donc les mêmes règles
s'appliquent au receveur qu'aux autres paramètres d'une fonction :

- si le receveur est de type valeur, alors le contenu de la structure sur laquelle la méthode est appelée sera copiée
  dans une variable locale temporaire pour l'exécution de la méthode.
- si le receveur est un pointeur, alors le contenu de la structure ne sera pas copié parce que l'adresse de la structure
  sera donnée à la méthode, donc les modifications apportées à la structure dans la méthode seront visibles à
  l'extérieur de la méthode.

## Comment appeler une méthode

Dans cet exemple, on commence par créer 2 points avec le constructeur, et ensuite, on appelle la méthode `Add` sur le
premier point, en donnant le 2e point en argument. Pour appeler une méthode sur une structure, il faut utiliser le `.`
directement précédé d'une structure ou d'un pointeur vers une structure, suivi du nom de la méthode et de ses arguments.

```go
p1 := NewPoint(1, 2)
p2 := NewPoint(3, 4)
fmt.Println(p1)
fmt.Println(p2)

p3 := p1.Add(p2)
fmt.Println(p3)
```

Sortie :

```go
Point(1.000000, 2.000000)
Point(3.000000, 4.000000)
Point(4.000000, 6.000000)
```

## Conversion pour l'impression

La méthode `String` définie sur la structure `Point` est spéciale car elle permet la conversion de la structure en
chaîne de caractères pour la sortie, sur le terminal dans cet exemple-ci. Quand on appelle `fmt.Println` (ou d'autres
fonctions d'impression) sur un point, si la méthode `String()` qui retourne une `string` est définie, alors elle sera
utilisée pour la conversion du point en chaîne de caractères. Cette fonction est définie dans l'interface `Stringer`.
En Java, la méthode `toString` joue le même rôle.

Si cette méthode n'est pas définie, alors la sortie sera :

```go
&{1 2}
&{3 4}
&{4 6}
```

Par défault, si la méthode String n'est pas définie, alors la sortie commencera par `&` si on a un pointeur vers une
structure, et entre accolades `{}`, on aura la valeur de tous les champs séparés par des espaces.
