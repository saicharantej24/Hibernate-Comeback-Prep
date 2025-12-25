
# Embeddable
Stud.java:
```java
package org.example;

import jakarta.persistence.*;

@Entity
public class Stud {
    @Id
    private int roll;

    private String name;
    private int marks;
    @Embedded
    private Laptop laptop;



    public int getRoll() {
        return roll;
    }

    public int getMarks() {
        return marks;
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
    public Laptop getLaptop() {
        return laptop;
    }

    public void setLaptop(Laptop laptop) {
        this.laptop = laptop;
    }
    @Override
    public String toString() {
        return "Stud{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                ", laptop=" + laptop +
                '}';
    }
}
```
Laptop.java:
```java
package org.example;

import jakarta.persistence.Embeddable;

@Embeddable
public class Laptop {
    private String brand;
    private String model;
    private int ram;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getModel() {
        return model;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public int getRam() {
        return ram;
    }

    public void setRam(int ram) {
        this.ram = ram;
    }

    @Override
    public String toString() {
        return "laptop{" +
                "brand='" + brand + '\'' +
                ", model='" + model + '\'' +
                ", ram=" + ram +
                '}';
    }
}

```
Main.java:
```java
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Main {
    public static void main(String[] args) {
        Laptop l1=new Laptop();
        l1.setBrand("hp");
        l1.setModel("auth");
        l1.setRam(260);

        Stud s1=new Stud();
        s1.setRoll(1);
        s1.setName("saibsct");
        s1.setMarks(100);
        s1.setLaptop(l1);


        SessionFactory sf = new Configuration().
                addAnnotatedClass(org.example.Stud.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();

        Transaction tx=session.beginTransaction();
        session.persist(s1);
        tx.commit();
        Stud s2=session.find(Stud.class,1);
        System.out.println(s2);
        session.close();
        sf.close();
    }
}
```
```
Output:
Hibernate: 
    set client_min_messages = WARNING
Hibernate: 
    drop table if exists Stud cascade
Hibernate: 
    create table Stud (
        marks integer not null,
        ram integer,
        roll integer not null,
        brand varchar(255),
        model varchar(255),
        name varchar(255),
        primary key (roll)
    )
Hibernate: 
    insert 
    into
        Stud
        (brand, model, ram, marks, name, roll) 
    values
        (?, ?, ?, ?, ?, ?)
Stud{roll=1, name='saibsct', marks=100, laptop=laptop{brand='hp', model='auth', ram=260}}
```

