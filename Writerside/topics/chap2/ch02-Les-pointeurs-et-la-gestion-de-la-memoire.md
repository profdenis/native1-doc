# Chapitre 2 : Les pointeurs

Dans le langage Go, il y a 2 types de variables :

- les **variables de type valeur**, et
- les **variables de type pointeur**.

**Variables de type valeur :**

En Go, **tous les types sont par défaut des types valeur**. Cela signifie que lorsque vous attribuez une variable à une
autre, la "valeur" est copiée. Si vous modifiez la nouvelle variable, l'originale ne sera pas affectée.

```go
var a int = 5
b := a  // b = 5, copie de la valeur de a
b = 10  // la modification de b n'affecte pas a
```

Dans cet exemple, lorsque vous changez la valeur de `b`, `a` reste le même. Cela est dû au fait que `b` est une copie
de `a`.

**Variables de type pointeur :**

Un pointeur, d'autre part, pointe vers l'emplacement mémoire d'une autre variable. Lorsque vous modifiez la valeur à
laquelle pointe un pointeur, vous modifiez également la valeur originale.

```go
var a int = 5
var b *int = &a  // b pointe vers la mémoire de a
*b = 10          // nous changeons la valeur de a à travers b
```

Maintenant, si vous modifiez `*b`, `a` change aussi, parce que `b` est un pointeur vers `a`.

Les pointeurs sont utiles lorsque vous voulez partager une variable entre plusieurs fonctions, ou lorsque vous
travaillez avec de grands ensembles de données que vous ne voulez pas copier.

Ces exemples sont seulement utilisés en guise d'introduction. Ce chapitre est consacré aux pointeurs et à la gestion de
la mémoire. Il y aura donc beaucoup plus d'exemples et d'explications pour clarifier ces concepts dans les sections qui
suivent.

La mémoire peut être allouée de 2 façons :

- l'allocation statique de la mémoire, et
- l'allocation dynamique de la mémoire.

**L'allocation statique de la mémoire :**

Dans l'allocation statique, la taille et la position de la mémoire à allouer sont déterminées avant l'exécution du
programme. Les variables déclarées globalement et localement dans une fonction sont des exemples de cette allocation. En
Go, lorsque vous déclarez une variable comme suit :

```go
var num int
```

La mémoire nécessaire pour stocker une valeur entière est automatiquement allouée. Cette allocation est appelée statique
car la taille de la mémoire allouée est fixe et ne peut pas être modifiée pendant l'exécution.

**L'allocation dynamique de la mémoire :**

Au contraire, l'allocation dynamique de la mémoire se produit pendant l'exécution du programme et permet d'allouer la
quantité de mémoire nécessaire en temps réel. En Go, cette allocation est réalisée en utilisant le mot-clé `new`
ou `make`. Par exemple :

```go
ptr := new(int)
```

Dans ce cas, Go alloue la mémoire nécessaire pour stocker un entier et renvoie un pointeur vers cette mémoire. De même,
la fonction `make` est utilisée pour allouer de la mémoire pour les _slices_, les _maps_ et les _channels_. Cette
allocation est dite dynamique parce que la quantité de mémoire peut varier pendant l'exécution.

Il est à noter que contrairement à certains autres langages de programmation, Go gère automatiquement la _libération_
(ou _"désallocation"_) de la mémoire à l'aide d'un **garbage collector** (_ramasse-miettes_ ou _vidangeur_), de sorte
que les développeurs n'ont pas à se soucier de la libération de la mémoire une fois qu'elle n'est plus nécessaire.

Dans d'autres langages comme le C et le C++, la libération de la mémoire est manuelle, ce qui est une source importante
de problèmes potentiels, notamment les _fuites de mémoire_ (_memory leaks_). Il est très difficile dans les applications
moindrement complexes de s'assurer de l'absence de fuites de mémoire dans un langage qui utilise la libération manuelle.
Il est plus difficile de tester une application qui utilise un langage avec la libération manuelle de la mémoire.

Par contre, l'utilisation d'un _garbage collector_ peut affecter la performance d'un programme dans certaines
situations. Mais en général, cet impact est mineur. Dans la grande majorité des cas, les risques qu'amène la libération
manuelle de la mémoire sont trop grand et beaucoup plus significatif que les inconvénients de l'utilisation d'un
_garbage collector_.

Nous reviendrons sur ce sujet plus tard.
