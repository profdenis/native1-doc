# Section 2 : Les listes chaînées

Une liste chaînée est une structure de données linéaire composée de nœuds qui contiennent des données et un pointeur,
qui pointe vers le prochain nœud dans la séquence. Si le pointeur d'un nœud pointe vers `nil` (ou `null`), cela signifie
qu'il s'agitdu dernier nœud de la liste.

Une liste chaînée a plusieurs utilisations :

1. **Gestion de la mémoire :** Contrairement aux tableaux, les listes chaînées ne sont pas stockées dans des blocs de
   mémoire contigus. Cela les rend particulièrement utiles lorsque vous ne savez pas combien d'éléments seront dans la
   liste et que vous avez besoin d'ajouter ou de supprimer des éléments dynamiquement.

2. **Ajout/Suppression efficace des éléments :** Dans une liste chaînée, ajouter ou supprimer un élément en tête de
   liste (ou possiblement à la fin de la liste) se fait en temps constant, indépendamment de la taille de la liste.

3. **Implémentation d'abstractions de données de haut niveau :** Les listes chaînées peuvent être utilisées pour
   implémenter des structures de données plus complexes comme des piles, des files d'attente, et des tables de hachage.

Il est important de noter que les listes chaînées ont également leurs inconvénients. Par exemple, contrairement aux
tableaux, l'accès à un élément spécifique d'une liste chaînée nécessite généralement de traverser la liste depuis le
début, ce qui peut être lent pour de grandes listes.

## Comparaison des listes chaînées aux tableaux de taille fixe et aux tableaux dynamiques

1. **Tableaux de taille fixe :**
    - _Utilisation_ : Les tableaux de taille fixe sont utiles lorsque la taille des données est connue à l'avance et
      reste constante.
    - _Performance_ : L'accès à un élément spécifique du tableau se fait en temps constant. C'est très rapide, car
      chaque élément est stocké à un emplacement contigu en mémoire. Cependant, l'ajout ou la suppression d'éléments
      peut nécessiter de déplacer plusieurs éléments, ce qui peut prendre beaucoup de temps pour de grands tableaux.

2. **Tableaux dynamiques (par exemple, des slices en Go ou des vecteurs en C++):**
    - _Utilisation_ : Les tableaux dynamiques sont utiles lorsque les données changent dynamiquement en taille. Ils
      ajustent leur taille en allouant automatiquement de la nouvelle mémoire lors de l'ajout d'éléments.
    - _Performance_ : Comme les tableaux à taille fixe, l'accès à un élément spécifique dans un tableau dynamique est
      également en temps constant. L'ajout d'éléments à la fin du tableau est généralement rapide, mais si la capacité
      du tableau est dépassée, une nouvelle mémoire doit être allouée, ce qui peut prendre du temps. De même, l'ajout ou
      la suppression d'éléments au milieu du tableau peut nécessiter de déplacer plusieurs éléments.

3. **Listes chaînées :**
    - _Utilisation_ : Les listes chaînées sont utiles lorsque les données doivent être insérées ou supprimées
      fréquemment. Les listes chaînées sont également utiles pour implémenter d'autres structures de données comme les
      piles et les files d'attente.
    - _Performance_ : L'ajout et la suppression d'éléments au début ou à la fin de la liste se fait en temps constant,
      sans nécessité de déplacer d'autres éléments. Cependant, l'accès à un élément spécifique dans une liste chaînée
      nécessite de traverser la liste depuis le début, ce qui peut être lent pour de grandes listes. De plus,
      contrairement aux tableaux, les listes chaînées nécessitent un espace mémoire supplémentaire pour stocker les
      pointeurs vers les éléments suivants.

En résumé, le choix entre un tableau (fixe ou dynamique) et une liste chaînée dépend largement des spécificités de
l'usage auquel la structure de données est destinée.

## Code

[Code listes native1 ](https://github.com/profdenis/native1/tree/master/chap7/list)

Dans ce code Go, une implémentation d'une liste simplement chaînée est fournie.

Il y a deux structures définies : `intNode` et `IntList`. `intNode` est la structure de base, représentant un nœud dans
la liste, qui contient une valeur entière et a un attribut `next` qui pointe vers le nœud suivant dans la
liste. `Intlist` représente la liste elle-même qui stocke les nœuds de départ et de fin (pointeurs sur `intNode`), et la
taille (nombre d'éléments) de la liste.

La fonction `NewIntList` crée une nouvelle liste d'entiers avec les éléments donnés. Elle utilise des paramètres
variadiques, permettant d'inputer plusieurs entiers sans avoir besoin de les emballer dans un slice. Les `...int` dans
la signature de la fonction dénotent ce paramètre variadique.

Les fonctions `AddStart` et `AddEnd` sont des méthodes qui permettent d'ajouter des éléments au début et à la fin de la
liste respectivement.

`Insert` permet d'insérer un élément à une position particulière dans la liste. Une erreur est renvoyée si la position
est invalide.

La fonction `IsEmpty` vérifie si la liste est vide en vérifiant si le nœud de départ est nul.

La fonction `Size` renvoie la taille de la liste.

La méthode `String` fournit une représentation en chaîne de la liste. Elle utilise `strings.Builder` pour concaténer
efficacement les chaînes.

Les méthodes `RemoveStart` et `RemoveEnd` suppriment le premier et le dernier élément de la liste et les renvoient. Dans
le cas où il n'y a pas d'éléments à supprimer, elles renvoient une erreur.

La méthode `Remove` supprime un élément à une position particulière. Comme `Insert`, elle renvoie une erreur si la
position est invalide.

Il y a aussi plusieurs tests fournis pour garantir le bon fonctionnement des différentes méthodes. Ces tests utilisent
le package `testing` de Go pour exécuter et vérifier les tests.

### `IsEmpty`

Vérifie si la liste est vide ou non.

```go
func (list *IntList) IsEmpty() bool {
	return list.start == nil
}
```

Si la liste ne contient aucun élément, alors elle ne contient aucun noeud, donc `list.start` ne peut pointer vers un
noeud de la liste puisqu'il y en a aucun, et on n'a pas le choix de faire pointer `list.start` vers `nil`.

### `AddStart`

Ajoute au début de la liste.

Voici une explication de la méthode `AddStart` :

1. Un nouveau `intNode` est créé avec `element` comme valeur et le début existant de la liste comme noeud `next`.
2. Le `start` de `IntList` est mis à jour vers ce nouveau noeud.
3. La taille de `IntList` est augmentée de 1.

```go
func (list *IntList) AddStart(element int) {
	newStart := &intNode{value: element, next: list.start} // Etape 1
	list.start = newStart // Etape 2
	list.size++ // Etape 3
}
```

En essence, `AddStart` ajoute un élément au début de la liste, repoussant les éléments actuels une position en arrière.

Votre cas de test associé `TestIntList_AddStart` confirme ce comportement. Il crée des listes avec des entrées fournies,
puis appelle `AddStart` avec un élément de test, et vérifie que la liste résultante est celle attendue. Par exemple,
dans le cas `TestIntList_AddStart_WithNonEmptyList`, une liste est créée avec les éléments (2, 3, 4), puis `AddStart`
est appelée avec `1`. La liste attendue après cette opération est (1, 2, 3, 4), ce qui signifie que `1` a été ajouté
avec succès au début.

### `AddEnd`

Ajoute à la fin de la liste.

- La fonction `AddEnd` correspond à une structure de données de liste chaînée. Elle prend un entier comme argument et
  l'ajoute comme un nœud à la fin de la liste.
- Le `struct IntList` a trois champs : `start`, `end` et `size`. `start` et `end` sont des pointeurs vers le premier et
  le dernier nœuds de la liste respectivement, tandis que `size` représente le nombre d'éléments dans la liste.
- La méthode commence par vérifier si `list.start` est `nil`. Si c'est le cas, cela signifie que la liste est vide. Dans
  ce cas, elle crée un nouveau nœud dont la `value` est égale à `element`, et les pointeurs `start` et `end` de la liste
  pointent vers ce seul nœud.
- Si la liste n'est pas vide (`list.start` n'est pas `nil`), elle crée un nouveau nœud dont la `value` est `element`
  et `next` est `nil` par défaut. Le champ `next` du nœud `end` actuel est mis à jour pour pointer vers ce nouveau nœud.
  Ensuite, `list.end` est réaffecté à ce nouveau nœud, ajoutant ainsi effectivement l'élément à la fin de la liste.
- Enfin, elle incrémente `list.size` pour refléter l'ajout du nouveau nœud à la liste.

Voici le segment de code correspondant :

```go
func (list *IntList) AddEnd(element int) {
	if list.start == nil {
		list.start = &intNode{value: element}
		list.end = list.start
	} else {
		list.end.next = &intNode{value: element}
		list.end = list.end.next
	}
	list.size++
}
```

Les autres méthodes de liste définies dans votre code effectuent différents opérations sur ce type `IntList`, telles que
ajouter un élément au début de la liste (`AddStart`), supprimer un élément du début de la liste (`RemoveStart`), insérer
un élément à une position donnée (`Insert`), et ainsi de suite.

`intNode` est un struct représentant un nœud dans la liste avec une valeur et un pointeur vers le nœud suivant.

### `Insert`

Ajouter un élément à une position spécifique dans la liste chaînée.

```go
func (list *IntList) Insert(element int, pos int) error {
```

Cette ligne commence la définition d'une fonction nommée `Insert` sur le type de structure `IntList`. Elle prend deux
paramètres : un entier `element` qui sera inséré, et un entier `pos` qui est la position dans la liste où l'élément sera
inséré. La fonction 'Insert' renvoie une erreur si quelque chose se passe mal.

L'instruction switch suivante couvre différents scénarios de `pos`.

```go
	switch {
	case pos < 0 || pos > list.size:
		return errors.New("invalid position")
	case pos == 0:
		list.AddStart(element)
	case pos == list.size:
		list.AddEnd(element)
	default:
		current := list.start
		for i := 0; i < pos-1; i++ {
			current = current.next
		}
		newNode := &intNode{value: element, next: current.next}
		current.next = newNode
		list.size++
	}
```

* `case pos < 0 || pos > list.size:` vérifie si la position spécifiée est inférieure à 0 ou supérieure à la taille de la
  liste. Si l'une ou l'autre de ces conditions est satisfaite, alors il n'est pas possible d'insérer le nouvel élément à
  cette position, et une erreur est renvoyée.
* `case pos == 0:` gère le scénario où la position pour insérer l'élément est 0. Cela signifie que le nouvel élément
  doit être ajouté au début de la liste. La fonction `AddStart` est appelée ici pour ajouter le nouvel élément en début
  de liste.
* `case pos == list.size:` gère le scénario où la position pour insérer l'élément est égale à la taille de la liste.
  Cela signifie que le nouvel élément doit être ajouté à la fin de la liste. La fonction `AddEnd` est appelée ici pour
  ajouter le nouvel élément à la fin de la liste.
* `default:` couvre toutes les autres positions possibles à l'intérieur de la liste. Il parcourt la liste jusqu'à la
  position correcte et insère le nouveau nœud.

Enfin,

```go
	return nil
}
``` 

Cette ligne indique que la fonction a réussi, car elle s'exécute uniquement si aucune erreur ne s'est produite
auparavant.

### `RemoveStart`

Supprime le premier élément dans la liste liée.

Voici une explication plus détaillée :

```go
func (list *IntList) RemoveStart() (int, error) {
	if list.start == nil {
		return 0, errors.New("empty list, cannot remove")
	}
```

D'abord, la méthode vérifie si la liste est vide en vérifiant si `list.start` est `nil`. Si c'est le cas, une erreur est
renvoyée indiquant qu'on ne peut pas supprimer un élément d'une liste vide.

```go
element := list.start.value
	list.start = list.start.next
	list.size--
```

Ensuite, si la liste n'est pas vide, la méthode extrait la valeur du premier élément pour la retourner plus tard et met
à jour le point de départ de la liste pour passer à l'élément suivant dans la liste. La `size` de la liste est
décrémentée de 1.

```go
if list.size == 0 {
    list.end = nil
}
return element, nil
```

Après cela, si la taille de la liste devient maintenant 0, la fin de la liste est également mise à nil. Enfin, la
méthode retourne la valeur de l'élément de départ supprimé et aucune erreur puisque l'opération a réussi. S'il y a un
problème avec la liste (comme si elle est vide), l'erreur sera non nulle.

### `RemoveEnd()`

Supprime le dernier élément dans la liste liée.

```go
func (list *IntList) RemoveEnd() (int, error) {
	var element int
	// liste vide
	if list.end == nil {
		return element, errors.New("liste vide, impossible de supprimer")
	}
	if list.end == list.start {
		// seulement 1 élément dans la liste
		element = list.end.value
		list.start = nil
		list.end = nil
	} else {
		//au moins 2 éléments dans la liste
		newEnd := list.start
		for newEnd.next != list.end {
			newEnd = newEnd.next
		}
		element = list.end.value
		list.end = newEnd
		list.end.next = nil
	}
	list.size--
	return element, nil
}
```

* La fonction essaie de supprimer un élément de la fin de la liste.
* Si la liste est vide (`list.end == nil`), elle renvoie une erreur indiquant "liste vide, impossible de supprimer".
* S'il n'y à qu'un seul élément dans la liste (`list.start == list.end`), elle supprime l'élément, puis
  définir `list.start` & `list.end` à `nil`.
* S'il y a plus d'un élément, elle parcourt la liste jusqu'à ce qu'elle trouve l'avant-dernier
  élément (`newEnd.next != list.end`). Elle supprime alors le dernier élément en définissant `list.end` au `newEnd`,
  c'est-à-dire, l'avant-dernier devient le nouvel élément de fin. Le suivant de `list.end` est défini à `nil`,
  supprimant toute référence au dernier élément.
* À la fin, la taille de la liste est diminuée de 1.

### `Remove`

Enlever un élément à une position (un index) spécifié.

```go
func (list *IntList) Remove(pos int) (int, error) {
	switch {
	case pos < 0 || pos >= list.size:
		return 0, errors.New("invalid position")
	case pos == 0:
		return list.RemoveStart()
	case pos == list.size-1:
		return list.RemoveEnd()
	default:
		current := list.start
		for i := 0; i < pos-1; i++ {
			current = current.next
		}
		element := current.next.value
		current.next = current.next.next
		list.size--
		return element, nil
	}
}
```

Cette fonction supprime un élément de `IntList` à une position spécifiée, `pos`, et renvoie cette valeur ainsi que toute
erreur qui pourrait survenir.

1. Si `pos` est inférieur à 0 ou supérieur ou égal à la taille de la liste, la fonction renvoie une erreur, car la
   position est invalide.
2. Si `pos` est 0, la méthode `RemoveStart()` est appelée, qui supprime le premier nœud de la liste, et renvoie sa
   valeur.
3. Si `pos` équivaut à `list.size - 1`, la méthode `RemoveEnd()` est appelée qui supprime le dernier élément de la
   liste, et renvoie sa valeur.
4. Si `pos` est une position valide autre que 0 ou la fin de la liste, la fonction parcours la liste jusqu'à ce qu'elle
   atteigne `pos - 1` (le nœud juste avant celui que nous voulons supprimer) puis élimine le nœud `pos` en liant le
   pointeur `next` de `pos - 1` au nœud suivant de `pos` (`pos + 1`), contournant ainsi le nœud `pos`.
5. La taille de la liste est décrémentée de 1, signifiant qu'un nœud a été supprimé.
6. La valeur du nœud supprimé est retournée avec `nil` symbolisant qu'il n'y a pas eu d'erreurs.