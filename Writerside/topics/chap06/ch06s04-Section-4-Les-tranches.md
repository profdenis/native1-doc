# Section 4 : Les tranches

Les tranches (_slices_) sont une abstraction au-dessus des tableaux. Elles se composent de trois composants :

1. un pointeur vers un tableau,
2. une longueur et
3. une capacité.

Lorsqu'une tranche est créée, un tableau sous-jacent est également créé. La tranche pointe vers ce tableau, et la taille
de la tranche ne peut jamais dépasser la taille du tableau sous-jacent.

Voici un exemple de création de tranche :

```go
s := make([]int, 5, 10) // Crée une tranche de longueur 5 et de capacité 10
```

Dans cet exemple, un tableau de 10 éléments est créé dans le tas, et une tranche de longueur 5 est créée qui pointe vers
les cinq premiers éléments de ce tableau. Les cinq éléments restants sont réservés pour permettre à la tranche de
s'étendre.

Il est important de noter que les tranches, étant une structure de données dynamique, sont **presque toujours allouées
sur le tas**, du moins pour le tableau sous-jacent, même si leur taille est connue au moment de la compilation.

Mais comme toujours en Go, les détails précis de l'allocation de mémoire peuvent dépendre de nombreux facteurs et
peuvent être gérés par le _runtime_ Go. Par exemple, pour les tranches très petites, Go pourrait décider de les allouer
sur la pile au lieu du tas pour des raisons de performances. Cependant, ce sont des détails d'implémentation qui ne sont
généralement pas exposés à l'utilisateur.

## Comment les tranches gèrent la mémoire

Lorsque vous créez une tranche, Go alloue dynamiquement un tableau sous-jacent et renvoie une tranche qui fait référence
à une partie de ce tableau. Si vous augmentez la taille de la tranche au-delà de la capacité de ce tableau, Go alloue un
nouveau tableau, copie les éléments existants dans le nouveau tableau, puis met à jour la tranche pour qu'elle pointe
vers le nouveau tableau. Ce processus est appelé **redimensionnement dynamique**.

Par exemple, lorsque vous utilisez `append` pour ajouter un élément à une tranche, si la tranche a de la capacité
disponible dans son tableau sous-jacent, l'élément sera ajouté dans cet espace. Si la tranche est déjà au maximum de sa
capacité, un nouveau tableau sera créé avec une capacité doublée, tous les éléments existants seront copiés, et le
nouvel élément sera ajouté à la suite.

Il convient de noter que lorsqu'une tranche est redimensionnée, l'ancien tableau sous-jacent reste en mémoire jusqu'à ce
qu'il soit éliminé par le _garbage collector_ de Go. Cela peut entraîner une utilisation non optimale de la mémoire si
le
tableau sous-jacent d'une tranche est grand et que seul un petit sous-ensemble de celui-ci est encore nécessaire. Pour
éviter une telle situation, vous pourriez utiliser la fonction `copy` et `runtime.GC` de Go pour copier la tranche dans
un petit tableau sous-jacent et forcer le _garbage collector_ à éliminer l'ancien grand tableau.

En résumé, les tranches en Go fournissent une abstraction flexible et facile à utiliser pour travailler avec des
séquences de données, mais il est important de comprendre comment elles gèrent la mémoire sous-jacente pour éviter les
écueils potentiels.

### Tranches de structures vs. tranches de pointeurs vers des structures

Travailler avec des tranches de structures ou des tranches de pointeurs vers des structures peut avoir un
impact significatif sur la façon dont la mémoire est utilisée et gérée.

Considérons un exemple où `Person` est une structure avec quelques champs. Si vous créez une tranche de `Person`,
chaque `Person` dans la tranche est stockée en tant que valeur. Cela signifie que si vous passez des éléments de la
tranche à une fonction ou si vous les copiez dans une nouvelle tranche, vous créez une copie de ces structures. Cette
copie représente un coût en termes de performances et d'utilisation de la mémoire, surtout si les structures sont
grandes.

```go
people := []Person{
    {Name: "John", Age: 30},
    {Name: "Jane", Age: 25},
}
```

D'autre part, si vous créez une tranche de pointeurs vers des structures `Person`, vous ne stockez que les adresses
mémoire des structures dans la tranche. Lorsque vous passez ces éléments à une fonction ou les copiez dans une nouvelle
tranche, vous copiez seulement les pointeurs, pas les structures elles-mêmes. Cela peut être plus efficace en termes
d'utilisation de la mémoire et de performance, surtout pour les grandes structures. De plus, puisque vous travaillez
avec des pointeurs, toute modification apportée à la structure à travers le pointeur sera reflétée dans toute votre
application, car tous les pointeurs se réfèrent au même emplacement mémoire.

```go
people := []*Person{
    &Person{Name: "John", Age: 30},
    &Person{Name: "Jane", Age: 25},
}
```

Cependant, il convient de noter que travailler avec des pointeurs peut compliquer la gestion de la mémoire. Par exemple,
si une structure est supprimée à un endroit, mais que des pointeurs vers cette structure existent toujours ailleurs dans
le programme, cela peut entraîner des bugs subtils et difficiles à repérer.

Il n'y a pas de règle stricte pour déterminer quand utiliser des tranches de structures ou des tranches de pointeurs à
des structures. Cela dépend principalement de vos besoins spécifiques. Si la taille de votre structure est petite et que
les effets de la copie de structures ne sont pas significatifs, les tranches de structures peuvent suffire. Si les
structures sont grandes ou si vous avez besoin de modifications en place sur la structure à travers le programme, les
tranches de pointeurs vers des structures peuvent être préférables.
