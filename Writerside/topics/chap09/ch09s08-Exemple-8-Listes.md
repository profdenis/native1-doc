# Exemple 8 : Listes

[Code](https://github.com/profdenis/native1/tree/master/chap9/08_listes)

La fonction `main` :
Cette fonction initialise une instance d'application en utilisant `app.New()`, et crée une nouvelle fenêtre intitulée
_"Exemple 08: listes"_. Elle crée ensuite des onglets interactifs sur la fenêtre en utilisant la fonction `makeTabs`,
définit le contenu de la fenêtre sur ces onglets, définit la taille de la fenêtre et enfin, affiche la fenêtre et
démarre la boucle d'événements de l'application.

La fonction `makeTabs(win fyne.Window) *container.AppTabs { ... }` :
Cette fonction crée deux listes : une pour des nombres et une pour des personnes. Ces listes sont passées
à `container.NewAppTabs()`, qui crée des onglets pour chaque liste. La fonction `container.NewTabItem()` est utilisée
pour créer des onglets individuels à partir de ces listes.

La fonction `makeNumbersList() *container.Scroll { ... }` :
Cette fonction crée une liste déroulante de nombres. Le tableau de nombres est utilisé pour définir les éléments de la
liste. Chaque élément de la liste est une étiquette affichant un nombre du tableau, et la sélection d'un élément
incrémente le nombre correspondant.

La fonction `makePersonList(win fyne.Window) *container.Scroll { ... }` :
Cette fonction est similaire à `makeNumbersList()`, mais elle crée une liste déroulante de personnes à la place. Chaque
personne est affichée comme une étiquette dans la liste, et la sélection d'une personne affiche une boîte de dialogue
d'information avec le nom de la personne sélectionnée.

## Fonction `makeNumbersList()`

La fonction `makeNumbersList()` dans le code Go fourni est utilisée pour créer une liste déroulante de nombres. Voici
une explication des différentes parties de la fonction :

1. La fonction `makeNumbersList()` renvoie un pointeur vers un `container.Scroll`, qui est un type de conteneur
   déroulant dans la bibliothèque Fyne.

2. Un tableau d'entiers appelé `numbers` est initialisé avec un ensemble de nombres.

3. La fonction `widget.NewList` est utilisée pour créer un nouveau widget de liste. Cette fonction accepte trois
   arguments :

    1. Une fonction qui renvoie le nombre d'éléments dans la liste, dans ce cas la longueur de `numbers`.

    2. Une fonction qui renvoie une nouvelle instance de l'objet qui constituera chaque élément de la liste. Dans ce
       cas, elle renvoie une étiquette avec le texte _"Item"_.

    3. Une fonction qui est appelée pour mettre à jour les propriétés d'un élément lorsqu'il devient visible. Ici, cette
       fonction définit le texte de l'étiquette de chaque élément pour afficher son index numérique dans la liste et la
       valeur correspondante de la tranche `numbers`.

4. `list.OnSelected` est un gestionnaire d'événements qui est déclenché lorsqu'un élément de liste est sélectionné. Ici,
   il est défini pour incrémenter la valeur dans la tranche `numbers` à l'index de l'élément sélectionné, puis
   rafraîchir l'affichage de l'élément.

5. Le widget de liste est placé dans un conteneur déroulant en utilisant `container.NewVScroll(list)` et est renvoyé
   depuis la fonction.

## Fonction `makePersonList()`

Semblable à `makeNumbersList()`, `makePersonList()` crée également un widget de liste mais à la place des éléments
numériques, les éléments `Person` sont créés.

Décortiquons en détail la fonction `makePersonList`:

```go
func makePersonList(win fyne.Window) *container.Scroll {
```

Il s'agit d'une déclaration de fonction `makePersonList` qui prend un paramètre `fyne.Window` et renvoie
un `*container.Scroll`.

```go
people := []Person{
	{Name: "John", Age: 30},
	{Name: "Jane", Age: 25},
	{Name: "Alice", Age: 35},
}
```

Un tableau de type `Person` avec trois éléments est déclaré et initialisé.

```go
list := widget.NewList(
	func() int {
		return len(people)
	},
	func() fyne.CanvasObject {
		return widget.NewLabel("Personne")
	},
	func(id widget.ListItemID, item fyne.CanvasObject) {
		person := people[id]
		item.(*widget.Label).SetText(fmt.Sprintf("Nom : %s, Âge : %d", person.Name, person.Age))
	},
)
```

Un `widget.List` est créé.

- La première fonction appelle `len(people)` pour obtenir le nombre d'éléments de la liste.

- La deuxième fonction crée un nouvel objet étiquette pour chaque élément.

- La troisième fonction définit le texte pour chaque étiquette afin d'afficher le nom et l'âge de la personne.

```go
list.OnSelected = func(id widget.ListItemID) {
	out := fmt.Sprintf("Personne sélectionnée : %s", people[id].Name)
	dialog.ShowInformation(fmt.Sprintf("Personne %d", id), out, win)
}
```

Une fonction de gestion d'événement, pour quand un élément est sélectionné, est définie. Cette fonction affiche une
boîte de dialogue avec le nom de la personne sélectionnée.

```go
return container.NewVScroll(list)
```

La fonction se termine en créant un nouveau conteneur de défilement vertical pour la liste et en le renvoyant.

