# Section 3 : Le vidangeur

Le _vidangeur_ (_ramasse-miettes_, _garbage collector_) (GC) de Go a pour objectif de libérer automatiquement l'espace
mémoire qui n'est plus utilisé par un programme, c'est-à-dire les objets qui ne sont plus accessibles par le code.

Le GC de Go est un GC à balayage concurrent, ce qui signifie qu'il peut fonctionner en même temps que votre code sans
avoir besoin de l'arrêter complètement (ce qu'on appelle un "stop-the-world" GC).

Le processus de GC se déroule en trois phases principales :

1. **Phase de marquage (Mark)**: Le GC commence à la racine de l'arbre d'objets (par exemple, les variables globales,
   les piles de chaque goroutine, etc.) et suit chaque pointeur, marquant chaque objet atteint comme étant en cours
   d'utilisation. Cette phase est réalisée de manière concurrente avec l'exécution du programme, avec une courte pause
   au début et à la fin de la phase.

2. **Phase d'assistance (Assist)**: Pendant cette phase, le code de l'application aide le GC en participant activement à
   la phase de marquage. L'application consacre une partie de son temps CPU à la collecte des déchets,
   proportionnellement à l'allocation de mémoire qu'elle effectue.

3. **Phase de balayage (Sweep)**: Le GC parcourt tous les objets en mémoire et libère ceux qui n'ont pas été marqués
   comme étant en cours d'utilisation. Cela libère de l'espace pour de nouvelles allocations. Cette phase est également
   effectuée en arrière-plan, de manière concurrente avec l'application.

Le GC de Go a été conçu pour minimiser l'impact sur les performances de l'application. Il est également possible de
contrôler le GC de manière plus fine par le biais de réglages et de la fonction `runtime.GC()` qui force une collecte de
déchets.

Cependant, les détails exacts du fonctionnement du GC en Go peuvent varier entre les versions du langage et sont
sujettes à des optimisations continuelles par les développeurs de Go.
