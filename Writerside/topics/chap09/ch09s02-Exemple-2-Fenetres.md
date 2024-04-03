# Exemple 2 : Fenêtres

[Code](https://github.com/profdenis/native1/tree/master/chap9/02_fenetres)

Explications bloc par bloc :

```go
package main
import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
)
```

Ici, nous avons la déclaration du package principal, et il importe les packages Fyne nécessaires pour créer une
application GUI.

```go
func main() {
	a := app.New()
```

La fonction `main` est le point d'entrée du programme Go. `app.New()` crée une nouvelle application Fyne.

```go
	w := a.NewWindow("Allo")
	w.SetContent(widget.NewLabel("Allo"))
	w.SetMaster()
	w.Show()
```

Une fenêtre intitulée _"Allo"_ est créée, avec une étiquette qui dit aussi _"Allo"_. `setMaster()` indique que si cette
fenêtre est fermée, l'application entière doit se fermer. `Show()` rend la fenêtre visible.

```go
	w2 := a.NewWindow("Plus grande")
	button := widget.NewButton("Ouvrir", func() {
		w3 := a.NewWindow("Trois")
		w3.SetContent(widget.NewLabel("Trois"))
		w3.Resize(fyne.NewSize(100, 100))
		w3.Show()
	})
	w2.SetContent(button)
	w2.Resize(fyne.NewSize(200, 200))
	w2.Show()
```

Ce bloc crée une autre fenêtre intitulée _"Plus grande"_, et la fenêtre contient un bouton disant _"Ouvrir"_. Lorsqu'on
clique dessus, le bouton ouvre une troisième fenêtre intitulée _"Trois"_. La taille des fenêtres est également définie
ici.

```go
	a.Run()
}
```

Enfin, `a.Run()` exécute la boucle d'événement de l'application Fyne qui se poursuivrait jusqu'à ce que toutes les
fenêtres soient fermées ou que la fonction `Quit()` soit appelée.