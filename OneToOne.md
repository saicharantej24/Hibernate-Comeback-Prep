# One to One
> Each stud have 1 laptop
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
    @OneToOne
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

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class Laptop {
    @Id
    private int lid;
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

    public int getLid() {
        return lid;
    }

    public void setLid(int lid) {
        this.lid = lid;
    }

    @Override
    public String toString() {
        return "Laptop{" +
                "lid=" + lid +
                ", brand='" + brand + '\'' +
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
        l1.setLid(100);
        l1.setBrand("hp");
        l1.setModel("auth");
        l1.setRam(260);

        Stud s1=new Stud();
        s1.setRoll(1);
        s1.setName("saibsct");
        s1.setMarks(100);
        s1.setLaptop(l1);


        SessionFactory sf = new Configuration()
                .addAnnotatedClass(org.example.Stud.class)
                .addAnnotatedClass(org.example.Laptop.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();

        Transaction tx=session.beginTransaction();
        session.persist(l1);
        session.persist(s1);
        tx.commit();
        Stud s2=session.find(Stud.class,1);
        System.out.println(s2);
        session.close();
        sf.close();
    }
}

```
Output:
```
Hibernate: 
    set client_min_messages = WARNING
Hibernate: 
    alter table if exists Stud 
       drop constraint if exists FK7vwa8i81tjns7yhi5xpe06waj
Hibernate: 
    drop table if exists Laptop cascade
Hibernate: 
    drop table if exists Stud cascade
Hibernate: 
    create table Laptop (
        lid integer not null,
        ram integer not null,
        brand varchar(255),
        model varchar(255),
        primary key (lid)
    )
Hibernate: 
    create table Stud (
        laptop_lid integer unique,
        marks integer not null,
        roll integer not null,
        name varchar(255),
        primary key (roll)
    )
Hibernate: 
    alter table if exists Stud 
       add constraint FK7vwa8i81tjns7yhi5xpe06waj 
       foreign key (laptop_lid) 
       references Laptop
Hibernate: 
    insert 
    into
        Laptop
        (brand, model, ram, lid) 
    values
        (?, ?, ?, ?)
Hibernate: 
    insert 
    into
        Stud
        (laptop_lid, marks, name, roll) 
    values
        (?, ?, ?, ?)
Stud{roll=1, name='saibsct', marks=100, laptop=Laptop{lid=100, brand='hp', model='auth', ram=260}}
```
```
Q: Why in output student craeted 1st why noot laptop since stud depends on laptop?(my own)
Ans:
Hibernate (Hibernate) creates tables based on metadata processing order, not foreign-key dependency order.

So:

Entity scanning order

addAnnotatedClass(...) order

Internal metadata sorting

👉 These decide which CREATE TABLE is logged first, not relationship direction.
🔍 Why it looks like Stud first (but isn’t meaningful)

You have:

.addAnnotatedClass(Stud.class)
.addAnnotatedClass(Laptop.class)


Hibernate may:

Process Stud metadata first

Log its DDL first

Delay FK constraint until both tables exist

That’s why you see:

create table Stud (...)
create table Laptop (...)
alter table Stud add foreign key ...
This is normal and correct.

🧱 How Hibernate avoids FK problems
Hibernate splits DDL into phases:

1️⃣ Create tables without foreign keys
2️⃣ Create referenced tables
3️⃣ Add foreign key constraints using ALTER TABLE

That’s why you see:

alter table Stud add constraint ...


So even if Stud table is created first:

No FK exists yet

DB is safe

Constraint is added later
🧠 One-line explanation

Hibernate creates tables first, relationships later — so log order is meaningless.
```
