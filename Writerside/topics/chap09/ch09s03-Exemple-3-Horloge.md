# Exemple 3 : Horloge

[Code](https://github.com/profdenis/native1/tree/master/chap9/03_horloge)

Cet exemple est une application Go qui affiche l'heure actuelle dans une fenêtre. L'heure est mise à jour chaque
seconde.

Voici une brève explication de chaque partie :

```go
package main
import (
	"fyne.io/fyne/v2/app"
	"fyne.io/fyne/v2/widget"
	"time"
)
```

Ceci définit le package `main` et importe les bibliothèques nécessaires : `fyne.io/fyne/v2/app` pour créer
l'application, `fyne.io/fyne/v2/widget` pour les widgets de l'interface utilisateur, et `time` pour obtenir et formater
l'heure actuelle.

```go
func main() {
	a := app.New()
	w := a.NewWindow("Clock")
	clock := widget.NewLabel("")
	updateTimeLabel(clock)
	w.SetContent(clock)
```

Dans la fonction `main`, une nouvelle application Fyne est instanciée avec `app.New()`. Ensuite, une fenêtre intitulée
_"Clock"_ est créée avec `a.NewWindow("Clock")`. Un `widget` de type `Label`, qui affichera l'heure actuelle, est créé
avec `widget.NewLabel("")` et son texte est défini avec la fonction `updateTimeLabel(clock)`. Ce label est ensuite
défini comme le contenu de la fenêtre.

```go
	go func() {
		for range time.Tick(time.Second) {
			updateTimeLabel(clock)
		}
	}()
```

Ceci est une goroutine, qui s'exécute simultanément avec la fonction principale. Elle utilise une boucle `for ... range`
pour attendre chaque `tick` (chaque seconde) de `time.Tick(time.Second)`. Chaque fois qu'elle reçoit un `tick`, elle met
à jour le texte de l'étiquette avec l'heure actuelle en utilisant la fonction `updateTimeLabel(clock)`.

```go
	w.ShowAndRun()
}
```

`w.ShowAndRun()` affiche la fenêtre et exécute la boucle d'événements de l'application. Cela permet à l'application de
rester en cours d'exécution jusqu'à ce que la fenêtre soit fermée par l'utilisateur.

```go
func updateTimeLabel(clock *widget.Label) {
	formatted := time.Now().Format("L'heure est 03:04:05")
	clock.SetText(formatted)
}
```

Dans la fonction `updateTimeLabel`, l'heure actuelle est récupérée avec `time.Now()`, puis formatée en une chaîne
_"L'heure est 03:04:05"_ avec `Format("L'heure est 03:04:05")`. La chaîne formatée est ensuite définie comme texte pour
l'étiquette `clock` avec `clock.SetText(formatted)`.