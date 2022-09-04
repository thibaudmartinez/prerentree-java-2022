---
marp: true
paginate: true
theme: 'dauphine'
---

<!-- _class: lead -->

<!-- _header: M1 Informatique • Pré-rentrée 2022 • Programmation Java -->

## Chapitre 1
# Structures de programmation de base (1)

<br>

Thibaud Martinez 
thibaud.martinez@dauphine.psl.eu

<!-- _footer: ![width:300](../images/logo-dauphine.png) -->

---

## Java "Hello, World!"

```java
public class PremierProgramme {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); 
    }
}
```


* Dans un programme Java, tout fait partie d'une classe.

* :warning: Le nom du fichier doit être le même que celui de la classe publique, avec l'extension `.java`, soit `PremierProgramme.java`.

* `main` est le point d'entrée du programme. L'exécution du code débutera par cette méthode. 

---

## Commentaires

```java
// ceci est un commentaire sur une seule ligne
```

```java
/* ceci est un commentaire 
sur plusieurs 
lignes */
```

---

## Expressions et instructions 

* Une **expression** est une combinaison de variables, d'opérateurs et d'appels de méthodes construite selon la syntaxe du langage et qui s'évalue à une seule valeur. Par exemple `1 * 2 * 3`.

* Une **instruction** constitue une unité d'exécution complète. 
Par exemple `int i = 10;`

    **Les instructions se terminent par un `;`.**

---

<style scoped>
p {
    font-size: 0.9rem
}

table {
    font-size: 0.65em;
}
</style>

## Types de données

Java est un langage **fortement typé** ➔ chaque variable doit avoir un type déclaré. 

Il existe **8 types élémentaires**.

| Type      | Taille (bits) | Description                                                       | Valeurs                                                  |
|-----------|---------------|-------------------------------------------------------------------|----------------------------------------------------------|
| `boolean` |       1       | Une valeur logique                                                |                     `true` ou `false`                    |
| `byte`    |       8       | Un octet signé                                                    |                        -128 à 127                        |
| `char`    |       16      | Un caractère Unicode                                              |                      \u0000 à \uFFFF                     |
| `short`   |       16      | Un entier court signé                                             |                      -32768 à 32767                      |
| `int`     |       32      | Un entier signé                                                   |            -2<sup>31</sup> à 2<sup>31</sup>-1            |
| `long`    |       64      | Un entier long                                                    |            -2<sup>63</sup> à 2<sup>63</sup>-1            |
| `float`   |       32      | Un nombre à virgule flottante de 32 bits signé (simple précision) |     2<sup>-149</sup> à (2-2<sup>-23)·2<sup>127</sup>     |
| `double`  |       64      | Un nombre à virgule flottante de 64 bits signé (double précision) | 2<sup>-1075</sup> à (2-2<sup>-52</sup>)·2<sup>1023</sup> |

---

## Variables

### Déclaration

```
<type> <nom>;
```

```java
double salaire;
int jourDeVacances;
long population;
boolean doitContinuer;
```

:sparkles: Il est d'usage d'écrire le nom des variables en ***lower camelCase***. 

---

### Initialisation

Après avoir **déclaré** une variable, on doit l'**initialiser** au moyen d'une instruction d'affectation.

```java
int jourDeVacances;   // déclaration
jourDeVacances = 12;  // initialisation
```

```java
int jourDeVacances = 12;
```

:warning: On ne peut pas utiliser la valeur d'une variable non initialisée.

---

### Inférence de type

Depuis Java 10, le compilateur peut déduire le type de la variable de sa valeur d'initialisation.

```java
var taux = 4.29;    // taux est un double
var lettre = 'A';   // lettre est un char
```

<!-- _footer: '[JEP 286: Local-Variable Type Inference](https://openjdk.org/jeps/286)' -->

---

## Constantes

Les constantes sont des variables dont la valeur ne change pas.

On utilise le mot clé `final` pour définir une constante.

```
final <type> <nom>;
```

```java
final double PI = 3.141592;
```

:sparkles: On écrit le nom des constantes en SCREAMING_SNAKE_CASE.

---

## Transtypage ou _cast_

Il s'agit de convertir le type d'une variable ou une valeur donnée.

* la conversion peut avoir lieu **implicitement** si le type de originel est "moins fort" que le type visé.

```java
int x = 9;
double y = x;
```

* dans les autres cas, la conversion doit être **explicite**.

```java
double a = 6.56;
int b = (int)a;
```

<!-- _footer: '[Conversions and Promotions](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html)' -->

---

<style scoped>
table {
    font-size: 0.7em;
}
</style>


## Opérateurs

Les opérateurs permettent de combiner des valeurs.

### Opérateurs arithmétiques

* addition : `2 + 3`
* soustraction : `2 - 3`
* multiplication : `2 * 3`
* division : `2 / 3`
* modulo (reste de la division entière): `5 % 2`

:warning: L'opérateur `/` désigne une division entière si les deux arguments sont des entiers, et une division en virgule flottante sinon.

---

Il est courant de réaliser une opération avec la valeur d'une variable puis d'affecter le résultat de cette opération à cette même variable.

Les opérateurs `+=`, `-=`, `*=`, `/=`, `%=` répondent à ces cas d'usage.

```java
int i = 1;

i += 2  // i vaut 3
i -= 1  // i vaut 2
i *= 5  // i vaut 10
i /= 2  // i vaut 5
i %= 2  // i vaut 1
```


---

<style scoped>
p {
    font-size: 0.9rem
}

</style>

### Incrémentation et décrementation

Ajouter ou retirer 1 à une variable est une opération courante en programmation, il existe les opérateurs unaires `++` et `--` pour réaliser ces opérations.

```java
int n = 12;
n++; // n vaut 13
n--; // n vaut 12
```

#### Opérateurs _prefix_ et _postfix_

`++` et `--` peuvent être placés avant la variable (opérateur _prefix_) ou après (opérateur _postfix_). Dans le premier cas la valeur de la variable est modifiée avant d'être renvoyée, dans le second, elle est modifiée après.

```java
int m = 7;
int n = 7;
int a = 2 * ++m; // a vaut 16 et m vaut 8
int b = 2 * n++; // b vaut 14 et n vaut 8
```

---

### Opérateurs booléens

* égalité : `x == 9`
* inégalité : `x != 9`
* inférieur : `x < 9 `
* inférieur ou égal : `x <= 9`
* supérieur : `x > 9`
* supérieur ou égal : `x >= 9`
* et : `y && z`
* ou : `y || z`
* négation logique : `!x`

---

### Évaluation _short-circuit_

Les opérateurs `&&` et `||` sont évalués selon une logique _"short-circuit"_: le deuxième argument n'est évalué que si le premier argument ne suffit pas à déterminer la valeur de l'expression.

Ce comportement peut être utilisé pour éviter les erreurs.

```java
x != 0 && 1 / x > x + y     // pas d'erreur de division par 0
```

---

### Opérateur ternaire

```java
condition ? expression1 : expression2
```

L'expression prend la valeur de l'`expression1` si la `condition` est vraie et celle de l'`expression2` sinon.

#### Exemple


```java
int valeur = (1 < 2) ? 3 : 4;   // valeur = 3   
int minimum = x < y ? x : y;    // minimum entre x et y
```

---

### Expressions _switch_

Pour choisir entre plusieurs valeurs, on peut utiliser une expression _switch_ (à partir de Java 14).

```java
int codeTaille = 2;

char taille = switch (codeTaille) {
    case 0 -> 'S';
    case 1 -> 'M';
    case 2 -> 'L';
    default -> '?';
};
// taille = 'L'
```

<!-- _footer: '[JEP 361: Switch Expressions](https://openjdk.org/jeps/361)' -->

---

### Opérateurs _bitwise_

Ces opérateurs agissent directement sur les **bits**.

* complément : `~` → fait de chaque `0` un `1` et de chaque `1` un `0`.
* décalage à gauche signé : `<<` → décale la séquence de bits vers la gauche
* décalage à droite signé : `>>` → décale la séquence de bits vers la droite
* et : `&`
* ou exclusif (XOR) : `^`
* ou inclusif (OR) : `|` 

Ils sont relativement peu utilisés et servent essentiellement pour optimiser les calculs.


<!-- _footer: '[Bitwise operation](https://en.wikipedia.org/wiki/Bitwise_operation)' -->

---

## Structures de contrôle

---

### Bloc et portée des variables

Un **bloc** consiste en un certain nombre d'instructions Java, entourées d'une paire d'**accolades**.

Les blocs définissent la **portée** des variables.

```java
public static void main(String[] args) {
    int n; 
    {
        int k;
        // ...
    } // k est défini et utilisable jusqu'à ce point
}
```

---

:warning: Vous ne pouvez pas déclarer des variables locales de même nom dans deux blocs imbriqués.

```java
public static void main(String[] args) {
    int n; 
    {
        int k;
        int n; // Error: variable n is already defined...
    }
}
```

---

## Instructions conditionnelles

Elles permettent d'exécuter un bloc d'instructions si une condition est remplie.


```java
if (<condition>) {
    <instruction>
}
```

```java
if (ventes >= objectif) {
    bonus = 100; 
}
```

---

### _if-else_

**Si** la condition est remplie ... **sinon** ...

```java
if (<condition>) {
    <instruction>
} else {
    <instruction>
}
```

```java
if (ventes >= objectif) {
    bonus = 100; 
else {
    bonus = 0; 
}
```

--- 

### _if-else-if_

**Si** la condition est remplie ... **sinon si une autre condition** est remplie ...


```java
if (vente >= 2 * objectif) {
    bonus = 1000;
} else if (vente >= objectif) {
    bonus = 100; 
}
```

---

### _switch_

La construction _if-else_ peut s'avérer lourde lorsqu'on doit traiter plusieurs alternatives pour la même expression. _switch_ offre une alternative plus compacte.

```java
int codeTaille = 2;

switch (codeTaille) {
    case 0 -> { System.out.println('S'); }
    case 1 -> { System.out.println('M'); }
    case 2 -> { System.out.println('L'); }
    default -> { System.out.println('?'); }
};
```

:sparkles: Lorsque c'est possible, on préfera utiliser des expressions _switch_ (voir diapositive précédente).

--- 

## Boucle _while_

La boucle _while_ exécute une instruction **tant qu'une condition est vraie**.

```java
while (<condition>) {
    <instruction>
}
```

:warning: La boucle while ne s'exécutera jamais si la condition est fausse au départ.

```java
while (solde < objectif) {
    solde += paiement;
    double interets = solde * tauxInteret;
    balance += interets;
    annees++;
}
```

---

## Boucle _do-while_

Pour s'assurer qu'un bloc d'instructions est exécuté au moins une fois.

```java
do {
    <instruction>
} while (<condition>);
```

```java
do {
    solde += paiement;
    double interets = solde * tauxInteret; 
    solde += interet;
    years++;
}
while (solde < objectif);
```

---

### Boucle _for_

```
for (<initialisation>; <condition d'arrêt>; <mise à jour>) {
    <instruction>
}
```

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

---

## _Break_

L'instruction _break_ est utilisée pour sortir d'une boucle.

```java
while (annees <= 100) {
    solde += paiement;
    double interets = solde * tauxInteret; 
    solde += interet;
    
    if (solde >= objectif) {
        break;
    }
    
    years++;
}
```

---

## Méthodes

Une méthode Java joue un rôle similaire à une fonction dans un langage comme C. Il s'agit d'une séquence d'instructions qui effectue une tâche spécifique, empaqueté comme une unité et qui renvoie une valeur.

```java
public static <type de retour> <nom> (<paramètres>) {
    <instructions>
}
```

_Le sens des mots clés `public` et `static` sera vu plus tard dans le cours._

Si la méthode ne renvoie rien, son type de retour est `void`.

```java
public static void afficheNombre() {
    System.out.println(42)
}
```

---

Pour retourner une valeur, on utilise le mot-clé `return`.

```java
public static int additionne(int a, int b) {
    return a + b;
}
```

L'appel de la méthode s'effectue de la façon suivante :

```java
int resultat = additionne(9, 14)
```

`9` et `14` constituent les **arguments** passés lors de l'appel de la méthode.

---

### Signature de la méthode et surcharge

Une méthode est identifiée par son **nom** et le **nombre, type et ordre de ses paramètres**. Ces éléments définissent la **signature de la méthode**.

Ainsi, deux fonctions peuvent avoir le même nom mais des paramètres différents.

```java
public static int additionne(int a, int b) {
    return a + b;
}

public static double additionne(double a, double b) {
    return a + b;
}
```

C'est ce qu'on appelle la **surcharge des méthodes**.

---

### Passage par valeur

Le passage des arguments de type élémentaire se fait **par valeur**, c'est-à-dire que la méthode n'opére pas sur les valeurs des variables en arguments mais sur des **copies des valeurs**.


```java
public static void mtd(int n) {
    n = n + 1;
}

int v = 10;
mtd(v);
// v vaut toujours 10
```