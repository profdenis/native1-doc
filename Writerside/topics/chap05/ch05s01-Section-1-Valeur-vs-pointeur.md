# Section 1 : Valeur vs. pointeur

Voici un exemple de structure pour représenter un point en 2 dimensions, chaque dimension étant un nombre réel
(type `float64`).

```go
type Point struct {
	X float64
	Y float64
}

func NewPoint(x float64, y float64) *Point {
	return &Point{X: x, Y: y}
}

func section1a() {
	p := Point{2, 5}
	fmt.Println(p)

	pPtr := &p
	fmt.Printf("%p %v\n", pPtr, *pPtr)

	pPtr = &Point{4, 3}
	fmt.Printf("%p %v\n", pPtr, *pPtr)
	
	pPtr = new(Point)
	fmt.Printf("%p %v\n", pPtr, *pPtr)
	pPtr.X = 1
	pPtr.Y = 6
	fmt.Printf("%p %v\n", pPtr, *pPtr)
	
	pPtr = NewPoint(-1, 5.5)
	fmt.Printf("%p %v\n", pPtr, *pPtr)
}
```

Sortie :

```go
{2 5}
0xc0000a6020 {2 5}
0xc0000a6050 {4 3}
0xc0000a6070 {0 0}
0xc0000a6070 {1 6}
0xc0000a60a0 {-1 5.5}
```

La variable `p` est de type valeur, elle contient un point, qui est composé de 2 `float64`. L'espace utilisé par cette
variable est la somme de l'espace utilisé par ses composantes, donc 128 bits dans ce cas-ci. En général, l'espace
utilisé pourrait être plus grand que la somme de l'espace utilisé par ses composantes si les composantes sont de
différentes tailles (voir les détails plus loin). La variable `pPtr` est un pointeur vers un `Point`.

![struct valeur](Structure_240121_155544.jpg)

On peut créer un nouveau point de cette façon : `pPtr = &Point{4, 3}`. Notez l'utilisation du `&` devant `Point{4, 3}`.
On crée un nouveau point, et on place l'adresse du nouveau point dans `pPtr`. On ne touche pas au premier point ici, on
remplace seulement l'adresse mémoire contenu dans la variable `pPtr` par une nouvelle adresse. On a donc 2 points
différents en mémoire à ce moment. Vérifiez dans la sortie, les adresses sont différentes. Les adresses vont être fort
probablement différentes des adresses données plus haut.

![struct pointeur](Structure2_240204_201808.jpg)

On peut également créer un nouveau point avec `new`. Le problème avec `new`, c'est qu'on ne peut pas spécifier les
valeurs des différents champs de cette façon. On doit donc modifier les valeurs par défaut des champs après avoir créé
la structure avec `new`. En Go, lorsqu'une variable est de _type pointeur vers une structure_, il n'est pas nécessaire
de déréférencer le pointeur avec l'astérisque `*` avant d'accéder à ces champs. Si on devait le faire, on utiliserait
`(*pPtr).X = 1` à la place de `pPtr.X = 1`. Il est très commun de manipuler des pointeurs vers des structures, donc les
créateurs de Go ont décidé d'introduire la notation avec le `.` non seulement directement sur les structures, mais aussi
sur les pointeurs vers des structures.

Il est également possible de déclarer une fonction pour créer un nouveau point, similaire à un constructeur dans un
langage orienté-objet. Dans GoLand, il est possible de générer un constructeur automatiquement avec le raccourci
`alt-ins`. Cette fonction démontre également qu'il est possible de spécifier la valeur des champs d'une structure par
leurs noms, suivis d'un `:` et des valeurs de champ. Si on ne spécifie pas les noms des champs dans l'initialisation, on
doit placer les valeurs des champs dans le même ordre que leurs déclarations.

Il faut noter que dans cet exemple, après avoir créé un nouveau point avec `new` et avoir placé son adresse dans `pPtr`,
nous perdons l'adresse du point `{4 3}`, qui ne sera plus accessible du tout par le programme. Similairement, après
avoir créé un nouveau point avec le constructeur, nous allons perdre le point `{1 6}` parce que nous perdons son
adresse. En général, il serait préférable de réutiliser les points déjà créés pour éviter d'allouer de la mémoire pour
rien. En Go, comme dans d'autres langages tels que Java et C#, il y a un vidangeur (_garbage collector_) qui va
récupérer cet espace mémoire, mais ce procédé n'est pas gratuit, il pourrait y avoir un impact sur la performance dans
certain cas. Dans la plupart des cas, l'impact sera faible, mais de bonnes pratiques en termes de gestion de la mémoire
est préférable. Les langages qui n'ont pas de vidangeur, comme le C et le C++, ne pourront pas récupérer la mémoire
perdue, ce qui créera une fuite de mémoire (_memory leak_).

![new(Point)](Structure3_240204_205941.jpg)