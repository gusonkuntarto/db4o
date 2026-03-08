# `db4o-8.0-java`: Database for Objects (Java Version 8.0)

This repository serves as a community archive and distribution point for **db4o version 8.0**, specifically compiled for and compatible with **Java 8.0**.

db4o (Database for Objects) is a pioneer in the object database space, designed to provide seamless, native object persistence for Java and .NET applications without the complexity of Object-Relational Mapping (ORM) layers like Hibernate.

---

> **Note on Project Status:** This is an *archival and compatibility maintenance* repository. The original commercial entity behind db4o (Versant, later Actian) discontinued the product. This version (8.0) is widely considered the last stable, feature-complete release before the project shifted focus. This fork ensures it remains usable for legacy systems and educational purposes requiring Java 8.

---

## 🌟 Key Features of db4o 8.0

As highlighted in our [Mental Schema: Modern Database Architecture](https://www.google.com/search?q=image_2.png) diagram, db4o represents the **Object-Oriented Systems (OODBMS)** paradigm:

1. **Native Object Persistence:** Store any complex Java object graph directly, respecting encapsulation, inheritance, and object identity (OID).
2. **No ORM Mismatch:** Eliminates the "impedance mismatch" between objects and tables. Your application model *is* your database model.
3. **Embeddable:** A single, small JAR file that runs co-located within your Java application JVM. No separate server installation required.
4. **Flexible Querying:**
* **QBE (Query by Example):** Create a prototype object, and db4o finds matches.
* **SODA (Simple Object Data Access):** A powerful, low-level API for dynamic query construction.
* **Native Queries:** (Historically a key feature, though performance may vary on modern JVMs compared to optimized SODA).



---

## 🚀 Getting Started

### Prerequisites

* Java Development Kit (JDK) 8.0.

### Installation

Since this is an archived project, it is not available on Maven Central. You must manually include the JAR file in your project's classpath.

1. **Download:** Navigate to the `releases` tab of this repository and download the `db4o-8.0-java.jar`.
2. **Add to Project:**
* **For simple projects:** Add the JAR to your system `CLASSPATH`.
* **For Maven/Gradle projects (Local install):** Install the JAR to your local repository:
```bash
mvn install:install-file \
   -Dfile=path/to/db4o-8.0-java.jar \
   -DgroupId=com.db4o \
   -DartifactId=db4o-java \
   -Dversion=8.0 \
   -Dpackaging=jar

```


* **Then add the dependency:**
```xml
<dependency>
    <groupId=com.db4o</groupId>
    <artifactId=db4o-java</artifactId>
    <version>8.0</version>
</dependency>

```





## 💻 Usage Example

Here is a basic example of opening a database, storing an object, and retrieving it using Java 8 syntax.

```java
import com.db4o.Db4oEmbedded;
import com.db4o.ObjectContainer;
import com.db4o.ObjectSet;
import com.db4o.query.Query;

import java.util.List;

// Simple definition of a persistent class
class Pilot {
    private String name;
    private int points;

    public Pilot(String name, int points) {
        this.name = name;
        this.points = points;
    }

    public String getName() { return name; }
    public int getPoints() { return points; }

    @Override
    public String toString() {
        return String.format("Pilot[Name: %s, Points: %d]", name, points);
    }
}

public class Db4oJava8Example {
    public static void main(String[] args) {
        String dbFile = "formula1.db4o";

        // 1. Open (or create) the embedded database container
        try (ObjectContainer db = Db4oEmbedded.openFile(Db4oEmbedded.newConfiguration(), dbFile)) {

            // 2. Store objects
            Pilot p1 = new Pilot("Michael Schumacher", 100);
            Pilot p2 = new Pilot("Rubens Barrichello", 99);
            db.store(p1);
            db.store(p2);
            System.out.println("Stored pilots.");

            // 3. Query using SODA (the powerful query API)
            System.out.println("\nQuerying for pilots with > 99 points:");
            Query query = db.query();
            query.constrain(Pilot.class);
            query.descend("points").constrain(99).greater();

            ObjectSet<Pilot> result = query.execute();
            listResult(result);

            // 4. Query using QBE (Query By Example - simple syntax)
            System.out.println("\nQuerying using QBE for 'Rubens':");
            Pilot proto = new Pilot("Rubens Barrichello", 0); // Prototype, matching only name
            ObjectSet<Pilot> qbeResult = db.queryByExample(proto);
            listResult(qbeResult);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Ensure connection is closed. The try-with-resources handles this
            // but manual closing (db.close()) is often required in complex legacy apps.
        }
    }

    public static <T> void listResult(List<T> result) {
        System.out.println("Result count: " + result.size());
        for (T item : result) {
            System.out.println(item);
        }
    }
}

```

---

## 🗺️ Role in "Modern Database Architecture"

This repository directly supports the **"Mental Schema"** coursework (visualized below).

* **db4o** represents the core learning objective for **Section I: Object-Oriented Systems (OODBMS)**.
* By maintaining Java 8 compatibility, it provides a stable environment to master **QBE**, **SODA**, and the vital competency of **Conceptual $\rightarrow$ Physical mapping** without needing to configure ORM tools.

You can download the full conceptual map [here](https://www.google.com/search?q=image_2.png).

---

## 📄 License and Provenance

This repository distributes binary artifacts (`.jar` files) and associated documentation derived from the original db4o project.

The original db4o project was distributed under a dual license: a commercial license and the **GNU General Public License (GPL)**. This maintenance fork adheres to the **G GPL v2** (or later) license terms.

Unless otherwise noted, modifications in this repository are licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).
