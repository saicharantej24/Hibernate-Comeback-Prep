# Hibernate-Comeback-Prep

# Hibernate
# Standard file: Stud.java
```java
package org.example;

import jakarta.persistence.*;

@Entity
public class Stud {
    @Id
    private int roll;
    private String name;
    private int marks;

    public int getRoll() {
        return roll;
    }

    public int getMarks() {
        return marks;
    }

    @Override
    public String toString() {
        return "Stud{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                '}';
    }

    public void setMarks(int marks) {
        this.marks = marks;
    }

    public void setRoll(int roll) {
        this.roll = roll;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
```java
Main.java:
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Main {
    public static void main(String[] args) {

        Stud s1 = new Stud();
        s1.setRoll(4);
        s1.setName("tej");
        s1.setMarks(104);

        //Configuration cfg = new Configuration();
        //cfg.configure();
        //cfg.addAnnotatedClass(Stud.class); 
        // SessionFactory sf = cfg.buildSessionFactory();
        // single lone code
        SessionFactory sf =new Configuration().
                addAnnotatedClass(org.example.Stud.class)
                .configure().buildSessionFactory();
                Session session = sf.openSession();

        Transaction tx=session.beginTransaction();

        session.beginTransaction();
        session.persist(s1);
        session.getTransaction().commit();

        session.close();
        sf.close();

        System.out.println("Saved: " + s1);
    }
}
```
# Fetching date:
```java
Main.java:
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Main {
    public static void main(String[] args) {

        Stud s1 = new Stud();
        s1.setRoll(4);
        s1.setName("tej");
        s1.setMarks(104);

        //Configuration cfg = new Configuration();
        //cfg.configure();
        //cfg.addAnnotatedClass(Stud.class);

        // SessionFactory sf = cfg.buildSessionFactory();
        // single lone code
        SessionFactory sf = new Configuration().
                addAnnotatedClass(org.example.Stud.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();
        Stud s2=null;
        s2=session.find(Stud.class,1);
        System.out.println(s2);
        session.close();
        sf.close();
    }
}

```
```text
Output:  
Hibernate:  
    select
        s1_0.roll,
        s1_0.marks,
        s1_0.name 
    from
        Stud s1_0 
    where
        s1_0.roll=?
Stud{roll=1, name='sai', marks=1}
```
# Update data
```java
Main.java:
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Main {
    public static void main(String[] args) {

        Stud s1 = new Stud();
        s1.setRoll(5);
        s1.setName("saitej");
        s1.setMarks(105);
        SessionFactory sf = new Configuration().
                addAnnotatedClass(org.example.Stud.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();
        Transaction tx=session.beginTransaction();
        session.merge(s1);
        tx.commit();
        session.close();
        sf.close();
    }
}
```
# Deleting data
```java
Main.java:
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Main {
    public static void main(String[] args) {

        Stud s1 = new Stud();

        SessionFactory sf = new Configuration().
                addAnnotatedClass(org.example.Stud.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();
        s1=session.find(Stud.class,2);
        Transaction tx=session.beginTransaction();
        session.remove(s1);
        tx.commit();
        session.close();
        sf.close();
    }
}
```
# changing table and column names
>> Here we making changes in Standard file- Stud.java:
```java
Stud.java:
package org.example;

import jakarta.persistence.*;

@Entity
@Table(name="new_name:Demo")
public class Stud {
    @Id
    private int roll;
    @Column(name="new_col")
    private String name;
    @Transient   // Now it will not be stored in db
    private int marks;

    public int getRoll() {
        return roll;
    }

    public int getMarks() {
        return marks;
    }

    @Override
    public String toString() {
        return "Stud{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                '}';
    }

    public void setMarks(int marks) {
        this.marks = marks;
    }

    public void setRoll(int roll) {
        this.roll = roll;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
```text
Output:
Hibernate: 
    create table "new_name:Demo" (
        roll integer not null,
        new_col varchar(255),
        primary key (roll)
    )
```


