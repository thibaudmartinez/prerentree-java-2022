---
marp: true
paginate: true
theme: 'dauphine'
---

<!-- _class: lead -->

<!-- _header: M1 Informatique • Pré-rentrée 2022 • Programmation Java -->

## Chapitre 6
# Généricité

<br>

Thibaud Martinez 
thibaud.martinez@dauphine.psl.eu

<!-- _footer: ![width:300](../images/logo-dauphine.png) -->

---

## Définition

La **généricité** ou **programmation générique** consiste à écrire un code qui peut être réutilisé pour des objets de nombreux types différents.

Les types (classes et interfaces) peuvent être des paramètres lors de la définition de classes, interfaces et méthodes.

Ainsi, la programmation générique fait appel à des **paramètres de type (_type parameters_)**.

---

## Exemple : ArrayList

La classe [`ArrayList`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/ArrayList.html) définit une liste redimensionnable (contrairement aux tableaux (_arrays_) pour lesquels la taille est fixée).

`ArrayList` est une **classe générique** : la classe prend en paramètre un type qui définit le type des objets de la liste.

```java
ArrayList<String> files = new ArrayList<String>();
```

Le compilateur va vérifier qu'on n'insère pas d'objets de mauvais type.

```java
files.add(new File("...")); // can only add String objects to an ArrayList<String>
```

:boom: Le code ci-dessus ne compilera pas.

---

## Définir une classe générique

Une classe générique est une classe comportant un ou plusieurs paramètres de types.

```java
public class Pair<T> {
    private T premier;
    private T second;

    public Pair(T premier, T second) {
        this.premier = premier;
        this.second = second;
    }

    public T getPremier() { return premier; }
    public T getSecond() { return second; }

    public void setPremier(T newValue) { premier = newValue; }
    public void setSecond(T newValue) { second = newValue; }
}
```

---

Une classe générique peut avoir plus d'un paramètres de type.

```java
public class Pair<T, U> { ... }
```

<br>

:sparkles: Il est courant d'utiliser des lettres majuscules pour les variables de type, et de les garder courtes. 

---

## Instancier des classes génériques

```java
Pair<String> p1 = new Pair<String>("Foo", "Bar");
```

<br>

On peut également utiliser la syntaxe _"diamond"_.

```java
Pair<String> p2 = new Pair<>("Foo", "Bar");
```

<br>

Enfin, il est possible d'utiliser l'**inférence de type** lorsqu'on instancie une classe générique.

```java
var p3 = new Pair<String>("Foo", "Bar");
```

---

## Définir une méthode générique

On peut également aussi définir une méthode unique avec des paramètres de type.

```java
class Tableau {
    public static <T> T trouverMilieu(T[] a) {
        return a[a.length / 2];
    }
}
```

<br>

Il est possible de définir des méthodes génériques, à la fois dans les **classes ordinaires** et dans les **classes génériques**.

---

## Appel d'une méthode générique

```java
String[] l = {"Steven", "Georges", "Francis"};
String milieu = Tableau.<String>trouverMilieu(l);
```

<br>

Dans ce cas (et dans la plupart des cas), on peut **omettre** le paramètre de type `<String>` de l'appel de méthode. Le compilateur dispose de suffisamment d'informations pour **déduire** la méthode que l'on souhaite appeler.

```java
milieu = Tableau.trouverMilieu(l);
```

---

## Types élémentaires et généricité

```java
int[] nombres = {1, 2, 3, 4, 5};
int milieu = Tableau.<int>trouverMilieu(nombres);
```

:boom: Le code ci-dessus ne compile pas. Il n'est possible d'utiliser la généricité **qu'avec des objets** et donc **pas avec les types élémentaires**.


→ La solution : **utiliser des _wrapper classes_**.

```java
Integer[] nombres = {1, 2, 3, 4, 5};
Integer milieu = Tableau.<Integer>trouverMilieu(nombres);
```

<br>

:information_source: Cette limitation est due au fait, qu'en Java, la généricité est implémentée avec la méthode d'[effacement de type (**_type erasure_**)](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html). 

---

## _Wrapper classes_

Les **classes enveloppes (_wrapper classes_)** permettent d'utiliser des types de données élémentaires en tant qu'objets.

Ce sont les classes suivantes : `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`, `Boolean` et `Character`.

<br>

---

## _Autoboxing_

C'est la **conversion automatique** que le compilateur effectue entre les types primitifs et les objets des _wrapper classes_ correspondantes.

L'_autoboxing_ est appliqué quand une variable de type élémentaire est :

* affectée à une variable de la _wrapper class_ correspondante;

    ```java
    Character ch = 'a';
    ```

* passée en tant qu'argument à une méthode qui attend un objet de la  _wrapper class_ correspondante.

    ```java
    var l = new ArrayList<Integer>();
    l.add(10);
    ```

---

## Bornes pour les paramètres de type

Les **bornes (_bounds_)** permettent de **restreindre les types** qui peuvent être utilisés comme paramètres de type dans un type générique.

Par exemple, une méthode peut n'accepter que des instances de `Number` ou de ses sous-classes.

```java
public <T extends Number> void afficheEntier(T n){
    System.out.println(n.intValue());
}
```

---

## Bornes multiples

Un paramètre de type peut avoir plusieurs bornes.

```java
<T extends B1 & B2 & B3>
```

Un paramètre de type avec plusieurs bornes est un **sous-type** de tous les types énumérés dans la limite. 

:warning: Si l'une des bornes est une classe, elle doit être spécifiée en premier.

```java
class A { }
interface B { }
interface C { }

class D <T extends A & B & C> { }
```

---

## Généricité et héritage

On considère la hiérarchie de classes suivantes :

```java
public class A { }
public class B extends A { }
public class C extends A { }
```

<br>

:boom: On ne peut pas écrire : 

```java
ArrayList<A> listA = new ArrayList<B>();
```

Autrement dit, `ArrayList<A>` n'est pas un sous-type de `ArrayList<B>`. 

---

## _Wildcard types_

Le mot-clé _wildcard_ `?` permet de résoudre ce problème.

```java
ArrayList< ? extends A> listA = new ArrayList<B>();
```

<br>

`ArrayList< ? extends A>` désigne une liste d'objets qui sont des instances de la classe `A`, ou des sous-classes de `A` (par exemple `B` et `C`).

→ on pourra appeler des méthodes de `A` sur les éléments de `listA`.