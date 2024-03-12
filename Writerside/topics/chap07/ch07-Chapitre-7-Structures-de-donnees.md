# Chapitre 7 : Structures de données

Les structures de données sont une manière d’organiser, de stocker et de manipuler des données afin qu'elles soient
efficacement accessibles et utilisables. Elles forment la base de presque tous les systèmes logiciels et algorithmes,
car la manière dont les données sont structurées peut avoir un effet significatif sur la performance et sur le
fonctionnement du programme.

Il existe plusieurs types de structures de données, qui peuvent être classés de nombreuses manières. Toutefois, on peut
les diviser en deux catégories principales : les structures de données linéaires et non linéaires.

- Les structures de données **linéaires** sont celles dans lesquelles les éléments sont arrangés d'une manière
  séquentielle et chaque élément est connecté au précédent et au suivant. Quelques exemples de ce type sont les
  tableaux, les listes chaînées, les piles et les files d'attente.

    - Les **tableaux** sont des structures de données qui stockent des éléments du même type dans une séquence contiguë.

    - Les **listes chaînées** sont similaires, mais chaque élément (noeud) contient un lien vers le prochain noeud,
      permettant un accès plus flexible et dynamique aux éléments.

    - Les **piles** et les **files d'attente** sont des structures de données qui imposent un ordre spécifique pour
      l'ajout et la suppression d'éléments, généralement _dernier entré, premier sorti_ (**LIFO**) pour les piles et
      _premier entré, premier sorti_ (**FIFO**) pour les files.

- Les structures de données **non linéaires** sont celles dans lesquelles les éléments ne sont pas disposés de façon
  linéaire. Ils incluent les arbres, les graphes, les hachages et d'autres types.

    - Les **arbres** sont composés de noeuds liés où un noeud parent peut avoir de multiples noeuds enfants, mais chaque
      noeud enfant ne peut avoir qu'un seul noeud parent.

    - Les **graphes** étendent cette idée en permettant aux noeuds d'avoir des liens vers plusieurs autres noeuds.

    - Les **dictionnaires** ou **tableaux associatifs** (ou _maps_ en anglais) stockent des éléments dans des
      emplacements basés sur une clé dérivée d'une fonction de hachage, permettant un accès direct aux valeurs en
      utilisant ces clés.

Choisir la bonne structure de données dépend du problème spécifique que vous êtes en train de résoudre. Par exemple, si
vous avez besoin d'un accès constant à des éléments par leur index, un tableau serait approprié, mais si vous avez
besoin d'ajouter et de supprimer fréquemment des éléments, une liste chaînée ou une pile pourrait être préférable.
Comprendre la nature de ces différentes structures de données et la façon dont elles fonctionnent vous permettra de
concevoir des logiciels plus efficaces, plus propres et plus adaptés à vos objectifs.

## Structures de données en Go

Le langage Go offre plusieurs types de structures de données intégrés sans avoir besoin d'importer de paquets
supplémentaires. Les principaux sont :

1. **Les tableaux** : Un tableau en Go est une collection ordonnée d'éléments de même type. Sa taille doit être
   connue au moment de la déclaration.
    ```go
    var arr [10]int // Un tableau contenant 10 entiers.
    ```

2. **Les chaînes de caractères (`string`)** : Une _string_ est une séquence de caractères. En Go, les chaînes de
   caractères sont codées en UTF-8 et peuvent contenir n'importe quel caractère Unicode.
    ```go
    s := "hello, world"
    ```

3. **Les tranches (tableaux dynamiques)** : Les slices sont similaires aux tableaux, mais leur taille peut changer. Ils
   fournissent un moyen plus flexible de travailler avec des collections de même type.
    ```go
    sl := []int{1, 2, 3} // Une slice d'entiers.
    ```

4. **Les dictionnaires (tableaux associatifs, ou _maps_)** : En Go, une map est une structure de données qui associe des
   valeurs à des clés uniques, similaires aux dictionnaires en Python, aux `HashMap` ou `TreeMap` en Java, ou aux objets
   en JavaScript.
    ```go
    m := map[string]int{ // Une map qui associe des strings à des entiers.
        "one": 1,
        "two": 2,
    }
    ```