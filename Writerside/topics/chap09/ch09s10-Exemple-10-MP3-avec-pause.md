# Exemple 10: MP3 avec pause

[Code](https://github.com/profdenis/native1/tree/master/chap9/10_mp3_pause)

Ce programme Go est une application de lecteur MP3 écrite en utilisant la boîte à outils GUI Fyne. Voici un
détail des principales fonctions.

Dans la fonction principale, nous avons ces étapes principales :

1. Une nouvelle application et une nouvelle fenêtre sont créées.
2. Il initialise ensuite un canal `playerActionChannel` qui sera utilisé pour la communication entre les composants de
   l'interface et la fonction de lecture.
3. Divers composants de l'interface sont créés, tels que des boutons pour lire, mettre en pause, arrêter et d'autres
   commandes, qui envoient des commandes via `playerActionChannel`.
4. Cette configuration permet à l'interface de fonctionner dans le thread principal tout en jouant des fichiers MP3 dans
   une autre goroutine.
5. Lorsqu'une chanson est sélectionnée pour être jouée, la goroutine correspondante reçoit l'URI du fichier et commence
   à la jouer. Les opérations comme jouer, mettre en pause ou arrêter la chanson sont faites en envoyant les commandes
   correspondantes via le canal `playerActionChannel`.

Passons en revue chaque partie du code :

- Tout d'abord, le type `PlayerAction` et ses valeurs possibles sont définis, ainsi que le canal `playerActionChannel`
  qui sera utilisé pour communiquer ces actions entre la fonction principale et la goroutine de lecture.

- `getOpenButtonFunc` est une fonction qui renvoie une nouvelle fonction qui ouvrira une boîte de dialogue pour choisir
  un fichier MP3 dans le système de fichiers, puis appellera la routine `playSong` pour jouer la chanson sélectionnée.
  Pendant ce processus, toute chanson précédemment en cours de lecture est arrêtée (via le `KillAction`).

- `createActionButton` et `createActionButtonWithIcon` sont des fonctions qui renvoient des boutons qui envoient
  une `PlayerAction` spécifiée au `playerActionChannel` lorsqu'ils sont cliqués.

- `playSong` est une fonction qui renvoie une fonction de goroutine qui joue la chanson sélectionnée. Elle reçoit des
  commandes via `playerActionChannel` pour contrôler la lecture de la chanson (lecture, pause, etc.). Elle utilise la
  bibliothèque beep pour contrôler la lecture audio.

- `getTagsButtonFunc` est une fonction qui renvoie une fonction de bouton qui, lorsqu'on clique dessus, lit les tags
  d'un fichier MP3 et les affiche dans une boîte de dialogue.

- `getCurrentDirURI` est une fonction d'aide utilisée pour obtenir une URI de fichier pour le répertoire courant.

L'architecture globale montre une utilisation typique des goroutines et des canaux, où l'interface graphique reste
réactive en effectuant des opérations potentiellement bloquantes (comme la lecture d'une chanson) dans des goroutines
séparées et en utilisant des canaux pour la communication entre elles.

## `getTagsButtonFunc`

```go
func getTagsButtonFunc(w fyne.Window) func() {
	return func() {
		f, err := os.Open("chap9/09_mp3/Lame_Drivers_-_01_-_Frozen_Egg.mp3")
		if err != nil {
			dialog.ShowError(err, w)
		}
		m, err := tag.ReadFrom(f)
		if err != nil {
			dialog.ShowError(err, w)
		}
		msg := fmt.Sprintf("Format : %s\nTitle : %s\nAlbum : %s\n Artist : %s\nYear : %d",
			m.Format(), m.Title(), m.Album(), m.Artist(), m.Year())
		dialog.ShowInformation("Tags", msg, w)
	}
}
```

La fonction `getTagsButtonFunc` retourne une fonction anonyme. Cette fonction lit le
fichier `mp3` en utilisant la fonction `Open` du package `os` et gère l'erreur potentielle qui pourrait se produire.
Ensuite, elle extrait les métadonnées/tags du fichier mp3 en utilisant la fonction `ReadFrom` du package `tag`, qui
pourrait également provoquer une erreur.

Les métadonnées extraites comprennent le format, le titre, l'album, l'artiste et l'année de la chanson. Ces informations
sont ensuite jointes avec des séparateurs "\n" en utilisant `fmt.Sprintf`, et une boîte de dialogue d'information est
affichée avec ces informations.

Voici une explication de base du code:

1. `func getTagsButtonFunc(w fyne.Window) func()`: C'est la définition de la fonction `getTagsButtonFunc`. Cette
   fonction prend une instance de `fyne.Window` et renvoie une autre fonction.

2. La fonction anonyme renvoyée lit les données mp3. Cela est fait en appelant `os.Open` sur l'emplacement du fichier
   mp3, ce qui renvoie un fichier et une erreur. S'il y a une erreur (le fichier n'a pas pu être ouvert), une boîte de
   dialogue d'erreur est affichée en utilisant `dialog.ShowError`.

3. Ensuite, `tag.ReadFrom(f)` est appelée avec le fichier comme argument, ce qui lit les métadonnées du fichier et
   renvoie une `tag.Metadata` et éventuellement une erreur. S'il y a une erreur (les métadonnées n'ont pas pu être
   lues), une autre boîte de dialogue d'erreur est affichée.

4. Ensuite, les métadonnées sont formatées en une chaîne en utilisant `fmt.Sprintf`. Le message de la chaîne contiendra
   diverses informations des métadonnées, y compris le format, le titre, l'album, l'artiste et l'année.

5. Enfin, une boîte de dialogue d'information est affichée en utilisant `dialog.ShowInformation` avec le titre "Tags" et
   la chaîne formatée précédemment.

Cette fonction anonyme est utilisée comme argument pour créer un bouton. Lorsque ce bouton est pressé, la fonction sera
exécutée, affichant une boîte de dialogue avec les tags du fichier mp3.

## `getOpenButtonFunc`

```go
func getOpenButtonFunc(w fyne.Window, playerActionChannel chan PlayerAction) func() {
	return func() {
		fd := dialog.NewFileOpen(getOpenFileDialogFunc(w, playerActionChannel), w)
		fd.SetFilter(storage.NewExtensionFileFilter([]string{".mp3"}))
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

Cette fonction crée et renvoie une fermeture (closure function) qui s'exécutera lorsque le bouton "Ouvrir" sera cliqué
sur l'interface utilisateur du lecteur MP3. Les détails de la fonction sont :

1. Elle crée un nouveau dialogue d'ouverture de fichier (`dialog.NewFileOpen`) qui s'ouvre lorsque le bouton "Ouvrir"
   sur l'interface utilisateur du lecteur MP3 est cliqué. La fonction retournée
   par `getOpenFileDialogFunc(w, playerActionChannel)` passée en argument à `dialog.NewFileOpen` est un callback qui
   sera exécuté lorsqu'un fichier est sélectionné dans le dialogue.

   `getOpenFileDialogFunc` est responsable de l'arrêt de la lecture de la chanson actuelle (le cas échéant), du
   démarrage de la lecture de la chanson sélectionnée, et de la gestion des erreurs qui peuvent survenir lors de ces
   opérations.

2. Elle définit un filtre (`SetFilter`) pour le dialogue de fichier afin de ne montrer que les fichiers `.mp3`.

3. Elle récupère le répertoire de travail actuel sous forme de URI listable (`getCurrentDirURI()`) et le définit comme
   emplacement de départ (`SetLocation`) pour le dialogue de fichier.

4. S'il y a une erreur en obtenant le répertoire courant, elle affiche un dialogue avec cette erreur.

5. Le dialogue de fichier est alors montré (`fd.Show()`).

## `createActionButton`

```go
func createActionButton(playerActionChannel chan PlayerAction, label string, playerAction PlayerAction) *widget.Button {
	return widget.NewButton(label, func() {
		select {
		case playerActionChannel <- playerAction:
			log.Println(label + "Action")
		default:
			log.Println(label + "Action : Inactif")
		}
	})
}
```

Cette fonction prend trois arguments :

* un `chan PlayerAction` nommé `playerActionChannel` - c'est une sorte de canal de communication par lequel
  les `PlayerAction` sont transmises.
* une `string` nommée `label` - Cela indique l'action que le bouton de l'interface utilisateur effectuera.
* un `PlayerAction` nommé `playerAction` - c'est l'action qui correspond au bouton pressé, par exemple, play, pause,
  etc.

`createActionButton` retourne un bouton (`*widget.Button`) qui est créé en utilisant `widget.NewButton`. Ce bouton
reçoit le `label` comme texte et une fonction anonyme à exécuter lorsque le bouton est cliqué.

La fonction anonyme utilise une déclaration `select` pour essayer d'envoyer `playerAction` dans `playerActionChannel` :

* Si elle peut envoyer le `PlayerAction`, elle enregistre ensuite que l'action avec le `label` fourni a été effectuée (
  avec "Action" ajouté).
* Si elle ne peut pas envoyer le `PlayerAction` (c'est-à-dire si le canal n'est pas prêt à recevoir), alors elle
  enregistre que l'action est inactive (avec "Action : Inactif" ajouté).

Le type `PlayerAction` est défini comme suit :

```go
type PlayerAction int
```

Il s'agit d'un alias de type pour le type `int`. Le but de cet alias de type est de rendre le code plus lisible et
facile à comprendre en associant la valeur `int` à une certaine action du lecteur. Les actions du lecteur sont définies
dans un bloc de constantes comme `iota`, initialisant `PlayAction` avec 0 et incrémentant la valeur pour les constantes
suivantes :

```go
const (
	PlayAction PlayerAction = iota
	PauseAction
	ToggleAction
	StopAction
	KillAction
)
```

## `getOpenFileDialogFunc`

La fonction `getOpenFileDialogFunc` est une fonction d'ordre supérieur qui crée et renvoie une autre fonction. La
fonction qu'elle renvoie est censée traiter un fichier sélectionné par l'utilisateur dans une boîte de dialogue :

```go
func getOpenFileDialogFunc(w fyne.Window, c chan PlayerAction) func(reader fyne.URIReadCloser, err error) {
	return func(reader fyne.URIReadCloser, err error) { ... }
}
```

Cette fonction est conçue pour s'intégrer avec la boîte de dialogue de fichier du framework Fyne (`dialog.NewFileOpen`).

S'il y a une erreur ou si aucun fichier n'est sélectionné (si `reader` est `nil`), la fonction retourne :

```go
if err != nil {
	dialog.ShowError(err, w)
	return
}
if reader == nil {
	log.Println("Annulé")
	return
}
```

Après qu'un fichier a été sélectionné, elle essaie d'envoyer le message `KillAction` dans le canal `c` pour arrêter la
chanson actuellement en lecture :

```go
select {
	case c <- KillAction:
		log.Println("<- KillAction")
	default:
		log.Println("Rien à arrêter")
}
```

Cette partie est dans un bloc `select`, ce qui signifie qu'elle est non bloquante, si le canal `c` n'est pas prêt à
recevoir une nouvelle valeur, elle ne bloquera pas et passera au cas `default`.

Enfin, elle commence à jouer la chanson sélectionnée dans une nouvelle goroutine :

```go
go playSong(reader, c)
```

## `playSong`

```go
func playSong(reader fyne.URIReadCloser, channel chan PlayerAction) {

	streamer, format, err := mp3.Decode(reader)
	if err != nil {
		log.Fatal(err)
	}

	speaker.Init(format.SampleRate, format.SampleRate.N(time.Second/10))

	ctrl := &beep.Ctrl{Streamer: beep.Loop(-1, streamer), Paused: true}

	speaker.Play(ctrl)
	for {
		action := <-channel

		switch action {
		case PlayAction:
			speaker.Lock()
			ctrl.Paused = false
			speaker.Unlock()
		case PauseAction:
			speaker.Lock()
			ctrl.Paused = true
			speaker.Unlock()
		case ToggleAction:
			speaker.Lock()
			ctrl.Paused = !ctrl.Paused
			speaker.Unlock()
		case StopAction:
			speaker.Lock()
			ctrl.Paused = true
			// replace la lecture au début de la chanson
			err := streamer.Seek(0)
			if err != nil {
				log.Println(err)
			}
			speaker.Unlock()
		case KillAction:
			// ferme complètement la lecture de la chanson
			err := streamer.Close()
			if err != nil {
				log.Println(err)
			}
			return
		}
	}
}
```

Voici une explication de la fonction `playSong` :

* Cette fonction reçoit deux arguments : un `URIReadCloser` du package `fyne`, qui fournit les données du fichier audio,
  et un canal de `PlayerAction`, qui est un type personnalisé représentant différentes actions qui peuvent être envoyées
  à cette fonction.

* Elle commence par décoder l'info du Reader en utilisant `mp3.Decode()`. Le décodage peut échouer pour des raisons
  variées, et dans ce cas l'erreur serait enregistrée dans le journal et l'application se terminerait brusquement
  avec `log.Fatal()`.

* Si le décodage réussit, le haut-parleur est initialisé avec `speaker.Init()` en utilisant le taux d'échantillonnage du
  format du fichier mp3.

* Après initialisation, un `Streamer` audio en boucle est créé qui commence initialement en étant `Paused`. Le Speaker
  joue ensuite ce Streamer.

* Une boucle `for` infinie est utilisée pour écouter continuellement les actions du canal `PlayerAction`. En fonction de
  l'action reçue, elle effectue les opérations suivantes :
    * `PlayAction` : La chanson actuelle n'est pas en pause et commencera ou continuera à jouer.
    * `PauseAction` : La chanson est mise en pause.
    * `ToggleAction` : L'état de la chanson est basculé entre pause et jeu.
    * `StopAction` : La chanson est mise en pause et remise à son début.
    * `KillAction` : Le `Streamer` de la chanson est fermé et la fonction retourne, mettant ainsi fin à la lecture de la
      chanson.
