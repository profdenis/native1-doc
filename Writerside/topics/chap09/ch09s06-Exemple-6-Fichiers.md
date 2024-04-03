# Exemple 6 : Fichiers

[Code](https://github.com/profdenis/native1/tree/master/chap9/06_fichiers)

Ce programme Go est un éditeur Markdown simple utilisant la boîte à outils GUI Fyne.

La fonction `main` est le point d'entrée de l'application Go. Elle initialise une nouvelle application Fyne `a`, crée
une nouvelle fenêtre `w`, crée deux entrées `src` et `dst` avec la fonction `makeSrcDstEntries`, et arrange ces entrées
à l'intérieur d'un _container_ `split` horizontal.

Le contenu de la fenêtre `w` est défini par les _containers_ qui contiennen des boutons et le container `split`.
Deux boutons, _"Ouvrir"_ et _"Enregistrer"_ sont responsables de l'appel des fonctions `getOpenButtonFunc` et une
fonction anonyme une fois cliqués, respectivement, qui gèrent les opérations de lecture/écriture.
Le `w.Resize(fyne.NewSize(800, 600))` ajuste la taille de la fenêtre et `w.ShowAndRun()` fait apparaître la fenêtre et
exécute la boucle principale de l'événement de l'application.

```go
func getOpenButtonFunc(w fyne.Window, src *widget.Entry, dst *widget.RichText) func() {...}
```

La fonction `getOpenButtonFunc` met en place la fonctionnalité du bouton "Ouvrir", qui ouvrira une boîte de dialogue de
fichier où les utilisateurs peuvent choisir un fichier markdown (".md") à charger dans la fenêtre Fyne.

```go
func getOpenFileDialogFunc(w fyne.Window, src *widget.Entry, dst *widget.RichText) func(reader fyne.URIReadCloser, err error) {...}
```

La fonction `getOpenFileDialogFunc` est utilisée pour gérer l'ouverture du fichier sélectionné à partir de la boîte de
dialogue. Elle lit le fichier (en supposant un fichier markdown) et définit le contenu à `src` et `dst`. Elle imprime
également toutes les erreurs rencontrées.

```go
func makeSrcDstEntries() (*widget.Entry, *widget.RichText) {...}
```

La fonction `makeSrcDstEntries` définit les entrées `src` et `dst` en tant que widgets Fyne : `src` est un champ
d'entrée multiligne et `dst` est un champ RichText. La propriété `src.OnChanged` est définie à une fonction pour mettre
à jour `dst` chaque fois que le contenu de `src` change (en supposant un contenu markdown).

```go
func getCurrentDirURI() (fyne.ListableURI, error) {...}
```

La fonction `getCurrentDirURI` est utilisée pour obtenir l'URI du répertoire courant. Elle est appelée à la fois par la
fonction `getOpenButtonFunc` ainsi que par la fonction "Enregistrer" pour définir l'emplacement initial des boîtes de
dialogue de fichier.

La dernière fonction anonyme associée au bouton _"Enregistrer"_ est utilisée pour gérer l'enregistrement des fichiers.
Lorsque le bouton est cliqué, une boîte de dialogue d'enregistrement de fichier s'ouvre et permet aux utilisateurs de
choisir un emplacement pour sauvegarder un fichier. Il crée ou écrase un fichier .md avec le texte actuellement dans le
widget d'entrée `src`.

## `getOpenButtonFunc` et `getOpenFileDialogFunc` plus en détails

```go
func getOpenButtonFunc(w fyne.Window, src *widget.Entry, dst *widget.RichText) func() {
	return func() {
		fd := dialog.NewFileOpen(getOpenFileDialogFunc(w, src, dst), w)
		fd.SetFilter(storage.NewExtensionFileFilter([]string{".md"}))
		listableUri, err := getCurrentDirURI()
		if err != nil {
			dialog.ShowError(err, w)
			return
		}
		fd.SetLocation(listableUri)
		fd.Show()
	}
}
```

Ici, `getOpenButtonFunc` retourne une fonction qui, lorsqu'elle est invoquée, crée une boîte de dialogue de fichier en
appelant `dialog.NewFileOpen()`. La boîte de dialogue créée sera utilisée pour ouvrir un fichier `.md` sélectionné par
l'utilisateur.

- `getOpenFileDialogFunc(w, src, dst)` est une fonction de rappel pour gérer l'ouverture du fichier.
- `fd.SetFilter(storage.NewExtensionFileFilter([]string{".md"}))` restreint les fichiers visibles à ceux ayant une
  extension `.md`.
- `getCurrentDirURI()` est utilisé pour obtenir l'URI du répertoire de travail actuel et le définir comme emplacement
  par défaut pour la boîte de dialogue de fichier.
- `fd.Show()` affichera la boîte de dialogue de fichier à l'utilisateur.

La fonction de rappel pour gérer l'ouverture du fichier est définie comme suit :

```go
func getOpenFileDialogFunc(w fyne.Window, src *widget.Entry, dst *widget.RichText) func(reader fyne.URIReadCloser, err error) {
	return func(reader fyne.URIReadCloser, err error) {
		if err != nil {
			dialog.ShowError(err, w)
			return
		}
		if reader == nil {
			log.Println("Annulé")
			return
		}
		data, err := io.ReadAll(reader)
		if err != nil {
			fmt.Println(err)
		}
		src.SetText(string(data))
		dst.ParseMarkdown(string(data))
	}
}
```

Cette fonction retourne une autre fonction qui fait ce qui suit lorsqu'elle est invoquée :

- Elle vérifie et affiche toute erreur potentielle.
- Si `reader` est `nil`, elle consigne `"Annulé"`.
- Elle lit toutes les données du fichier, fixe le texte comme contenu de `src` (un `widget.Entry`), et transforme le
  texte en format Markdown dans `dst` (un `widget.RichText`), pour le prévisualiser dans l'autre partie de la fenêtre de
  l'application.

Les fonctions anonymes pour le bouton _"Enregistrer"_ sont similaires en termes de structure, mais la fonction est
inverse, c.-à-d. que le texte contenu dans `src` est enregistré dans le fichier sélectionné à la place d'être lu et
affiché.