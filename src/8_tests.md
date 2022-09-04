---
marp: true
paginate: true
theme: 'dauphine'
---

<!-- _class: lead -->

<!-- _header: M1 Informatique • Pré-rentrée 2022 • Programmation Java -->

## Chapitre 8
# Documentation,<br>tests unitaires et gestion des dépendances
<br>

Thibaud Martinez 
thibaud.martinez@dauphine.psl.eu

<!-- _footer: ![width:300](../images/logo-dauphine.png) -->

---

## Documentation

* Explique ce que fait le code, comment l'utiliser et pourquoi il est écrit comme il l'est. 
* Incontournable pour la **collaboration** et pour que le projet soit **maintenable**.
* Conserver la documentation proche du code documenté, ou même directement dans le code, facilite les mises à jour.

---

### Javadoc

Le JDK contient un outil très utile, appelé `javadoc`, qui génère une **documentation HTML** à partir des fichiers sources.

En ajoutant au code source des commentaires spéciaux `/**`, on peut facilement générer une documentation avec `javadoc`.

<!-- _footer: '[Javadoc Tool](https://www.oracle.com/java/technologies/javase/javadoc-tool.html#javadocdocuments)' -->

---

### Documenter une classe

```java
/**
* Un objet {@code Carte} représente une carte à jouer, telle que
* "Reine de cœur". Une carte a une couleur (Carreau, Coeur,
* Pique ou Trèfle) et une valeur (1 = As, 2 . . . 10, 11 = Valet,
* 12 = Reine, 13 = Roi)
*/
public class Carte {
    // ...
}
```

---

### Documenter une méthode

```java
/**
* Augmente le salaire d'un employé.
* @param pourcentage le pourcentage d'augmentation du salaire (par exemple, 0.1 signifie 10 %).
* @return le montant de l'augmentation
*/
public double augmenteSalaire(double pourcentage) {
       double augmentation = salaire * pourcentage;
       salaire += augmentation;
       return salaire;
}
```

---

### Documenter des variables

Il n'est nécessaire de documenter **que les membres publics**, ce qui signifie généralement des constantes statiques. 

```java
/**
* La couleur "Coeur"
*/
public static final int HEARTS = 1;
```

---

### Générer la documentation

En supposant que les classes sont toutes dans le dossier `src/`. La commande suivante génèrera la documentation pour tout le code source et stockera le résultat dans un dossier `doc`. 

```bash
javadoc -d doc src/*
```

---

## Test unitaires

Les **tests unitaires** sont une méthode de test de logiciels par laquelle des **unités individuelles** de code source telles que des méthodes sont testées pour déterminer si elles fonctionnent comme attendues.

→ On teste des unités de code **en isolation**.

→ Il existe d'autres types de tests : tests d'intégration, tests système...

---

### JUnit 

**JUnit** est un framework pour écrire et exécuter des tests unitaires en Java. Il est actuellement à la **version 5**.

<br>

Il comprend 3 sous-projets :

* **JUnit Platform** : sert de base au lancement de tests sur la JVM.
* **JUnit Jupiter** : fournit des utilitaires pour écrires des tests unitaires.
* **JUnit Vintage** : pour lancer des tests correspondants à des versions antérieurs de JUnit.

<!-- _footer: '[JUnit 5](https://junit.org/junit5/)' -->

---

### Utilisation de JUnit

Supposons qu'on souhaite tester la classe suivante :

```java
package exemple;

public class Calculatrice {
    public static int addition(int a, int b) {
        return a + b;
    }

    public static int division(int a, int b) {
        return a / b;
    }
}
```

---

On écrit le test unitaire suivant :

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import exemple.Calculator;
import org.junit.jupiter.api.Test;

class CalculatriceTests {
    private final Calculatrice calc = new Calculatrice();

    @Test
    void addition() {
        assertEquals(2, calc.addition(1, 1));
    }
}
```

---

### Définir des tests avec JUnit

* Généralement, une classe de tests unitaires est associée à une classe;
* on ajoute une annotation `@Test` pour indiquer que la méthode est un test;
* le nom de la méthode est libre;
* la méthode doit renvoyer `void` et ne prends pas de paramètres. 

---

### Assertions

Les assertions sont des méthodes utilitaires qui permettent de déclarer que des conditions doivent être remplies pour que le test soit réussi.

* [assertTrue](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertTrue(boolean))
* [assertFalse](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertFalse(boolean))
* [assertNull](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertNull(java.lang.Object))
* [assertNotNull](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertNotNull(java.lang.Object))
* [assertEquals](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertEquals(byte,byte))
* ...


<!-- _footer: '[Assertions](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html)' -->

---

### Faire échouer un test

On peut utiliser l'assertion [`fail`](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#fail(java.lang.String)) pour faire échouer un test.

```java
import static org.junit.jupiter.api.Assertions.fail;
import exemple.Calculatrice;
import org.junit.jupiter.api.Test;

class CalculatriceTests {
    private final Calculatrice calc = new Calculatrice();

    @Test
    void addition() {
        int res = calc.addition(1, 1);
        if (res < 0) {
            fail("Un nombre négatif n'est pas attendu");
        }
    }
}
```

---

<style scoped>
p {
    font-size: 0.9rem;
}

code {
    font-size: 0.8rem;
}
</style>

### Avant et après les tests

On peut exécuter des instructions avant et après les tests grâce à  
[`@BeforeAll`](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/AfterAll.html), [`@BeforeEach`](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/BeforeEach.html), [`@AfterEach`](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/AfterEach.html), [`@AfterAll`](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/AfterAll.html).


```java
import org.junit.jupiter.api.*;

class StandardTests {
    @BeforeAll
    static void initAll() {}

    @BeforeEach
    void init() {}

    @AfterEach
    void tearDown() {}

    @AfterAll
    static void tearDownAll() {}
}
```

---

### Tester les exceptions

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;
import exemple.Calculatrice;
import org.junit.jupiter.api.Test;

class CalculatriceTests {
    private final Calculatrice calc = new Calculatrice();
    
    @Test
    void testException() {
        Exception exception = assertThrows(ArithmeticException.class, () ->
            calc.division(1, 0));
        assertEquals("/ by zero", exception.getMessage());
    }
}
```
---

### Exécuter des tests JUnit

On utilise le [_Console Launcher_](https://junit.org/junit5/docs/current/user-guide/#running-tests-console-launcher) pour lancer les tests JUnit depuis la console.

La dernière version peut être obtenue depuis [Maven Central](https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/).

En supposant qu'on travaille avec l'arborescence de fichiers suivante :

```
projet
└── exemple
    ├── Calculatrice.java
    └── CalculatriceTests.java
```

On peut lancer les tests avec :

```
javac -cp '.:junit-platform-console-standalone-1.9.0.jar' exemple/*.java
java -jar junit-platform-console-standalone-1.9.0.jar -cp . --scan-classpath
```


---

## Distribuer et récupérer des logiciels en Java

### Archives JAR

Les logiciels Java sont très souvent distribués sous forme d'**archives JAR** (_Java Archive_).

→ Un fichier JAR peut contenir à la fois des fichiers de classe et d'autres types de fichiers tels que des fichiers image et son. De plus, les fichiers JAR sont compressés, en utilisant le format de compression ZIP bien connu.

---

### Créer des archives JAR

On utilise l'outil `jar` pour créer des fichiers JAR.

```
jar cvf <jarFileName> <file1> <file2> . . .
```

Par exemple :

```
jar cvf ClassesCalculatrice.jar *.class icon.gif
```

---

### Créer des archives JAR exécutables

On peut utiliser l'option `e` de la commande `jar` pour spécifier le point d'entrée du programme, c'est-à-dire la classe qu'on spécifie normalement lorsqu'on appelle le lanceur de programme java.

```
jar cvfe MonProgramme.jar monpackage.MonProgramme <fichiers à ajouter>
```

On peut ensuite démarrer le programme :

```
java -jar MonProgramme.jar
```

---

### Utiliser des bibliothèques externes sous forme de JAR

On peut vouloir utiliser une bibliothèque distribuée en JAR. Pour pouvoir utiliser les classes présentes dans l'archive, il faut l'ajouter au _classpath_.

:information_source: Le _classpath_ est le chemin à partir duquel l'environnement d'exécution Java recherche les classes et autres fichiers de ressources.

```
javac -cp '.:org.exemple.jar' MaClasse.java
java -cp '.:org.exemple.jar' MaClasse
```

<!-- _footer: '[Setting the Class Path](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/classpath.html)' -->

---

### Maven

[Maven](https://maven.apache.org/what-is-maven.html) est un outil pour construire et gérer des projets en Java.

Il permet notamment de **gérer les dépendances du projets**, c'est-à-dire d'importer et d'utiliser des bibliothèques externes. Il simplifie aussi l'**exécution de tests JUnit**.

<br>

:sparkles: Maven propose [une structure de répertoires particulière](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html). Il est suggéré de respecter cette structure pour organiser vos projets.

---

### Créer un projet Maven

Une fois [Maven installé](https://maven.apache.org/install.html), on peut créer un nouveau projet :

```
mvn archetype:generate \
    -DgroupId=com.mycompany.app \
    -DartifactId=my-app \
    -DarchetypeArtifactId=maven-archetype-quickstart \
    -DarchetypeVersion=1.4 \
    -DinteractiveMode=false
```

<br>

Le fichier `pom.xml` (pour [_Project Object Model_](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contient les informations sur le projet et les détails de configuration utilisés par Maven pour construire le projet.

<!-- _footer: '[Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)' -->

---

<style scoped>
p {
    font-size: 0.9rem;
}

code {
    font-size: 0.7rem;
}
</style>

### Ajouter des dépendances

Pour ajouter des bibliothèques à un projet, on ajoutera les informations sur celles-ci entre les balises `<dependencies></dependencies>` dans `pom.xml`.

Par exemple, pour ajouter [JUnit 5](https://junit.org/junit5/docs/current/user-guide/): 

```xml
<dependencies>
<dependency>
     <groupId>org.junit.jupiter</groupId>
     <artifactId>junit-jupiter-engine</artifactId>
     <version>5.9.0</version>
     <scope>test</scope>
</dependency>
<dependency>
     <groupId>org.junit.platform</groupId>
     <artifactId>junit-platform-runner</artifactId>
     <version> 1.9.0</version>
     <scope>test</scope>
</dependencies>
```

:information_source: Les autres bibliothèques disponibles sont listées sur [Maven Central Repository](https://search.maven.org/). 

---

### Compiler le code

```
mvn compile
```

Le résultat de la compilation se trouve dans le répertoire `target/`.

### Exécuter les tests

```
mvn test
```

<!-- _footer: '[Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/index.html)' -->
