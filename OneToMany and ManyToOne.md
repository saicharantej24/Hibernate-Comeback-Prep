# One To Many:
    One stud uses multiple laptops
Stud.java:
```java
package org.example;


import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;

import java.util.List;

@Entity
public class Stud {
    @Id
    private int roll;

    private String name;
    private int marks;
    @OneToMany
    private List<Laptop> laptops;



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

    public List<Laptop> getLaptops() {
        return laptops;
    }

    public void setLaptops(List<Laptop> laptops) {
        this.laptops = laptops;
    }

    @Override
    public String toString() {
        return "Stud{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                ", laptops=" + laptops +
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

import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Laptop l1=new Laptop();
        l1.setLid(100);
        l1.setBrand("hp");
        l1.setModel("auth");
        l1.setRam(260);

        Laptop l2=new Laptop();
        l2.setLid(101);
        l2.setBrand("Acer");
        l2.setModel("New");
        l2.setRam(720);

        Stud s1=new Stud();
        s1.setRoll(1);
        s1.setName("saibsct");
        s1.setMarks(100);
        s1.setLaptops(Arrays.asList(l1,l2));


        SessionFactory sf = new Configuration()
                .addAnnotatedClass(org.example.Stud.class)
                .addAnnotatedClass(org.example.Laptop.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();

        Transaction tx=session.beginTransaction();
        session.persist(l1);
        session.persist(l2);
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
    alter table if exists Stud_Laptop 
       drop constraint if exists FK26yorbpe92i095eil4g77efjc
Hibernate: 
    alter table if exists Stud_Laptop 
       drop constraint if exists FKctx7hqqlyd5w13gf22j211kgs
Hibernate: 
    drop table if exists Laptop cascade
Hibernate: 
    drop table if exists Stud cascade
Hibernate: 
    drop table if exists Stud_Laptop cascade
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
        marks integer not null,
        roll integer not null,
        name varchar(255),
        primary key (roll)
    )
Hibernate: 
    create table Stud_Laptop (
        Stud_roll integer not null,
        laptops_lid integer not null unique
    )
Hibernate: 
    alter table if exists Stud_Laptop 
       add constraint FK26yorbpe92i095eil4g77efjc 
       foreign key (laptops_lid) 
       references Laptop
Hibernate: 
    alter table if exists Stud_Laptop 
       add constraint FKctx7hqqlyd5w13gf22j211kgs 
       foreign key (Stud_roll) 
       references Stud
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
        Laptop
        (brand, model, ram, lid) 
    values
        (?, ?, ?, ?)
Hibernate: 
    insert 
    into
        Stud
        (marks, name, roll) 
    values
        (?, ?, ?)
Hibernate: 
    insert 
    into
        Stud_Laptop
        (Stud_roll, laptops_lid) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        Stud_Laptop
        (Stud_roll, laptops_lid) 
    values
        (?, ?)
Stud{roll=1, name='saibsct', marks=100, laptops=[Laptop{lid=100, brand='hp', model='auth', ram=260}, Laptop{lid=101, brand='Acer', model='New', ram=720}]}

Process finished with exit code 0
```
```
In pgadmin a new tab called stud_laptop created
     stud_roll laptops_lid
1       1       100
2       1       101
```
    But we dont want to create new table instead we want to show it in laptop table itslef:
# OneToMany and ManyToOne
Stud.java:
```java
package org.example;


import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;

import java.util.List;

@Entity
public class Stud {
    @Id
    private int roll;

    private String name;
    private int marks;
    @OneToMany(mappedBy="st")     // we are telling Stud not to create table as it laptop will do by passing the object st
    private List<Laptop> laptops;



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

    public List<Laptop> getLaptops() {
        return laptops;
    }

    public void setLaptops(List<Laptop> laptops) {
        this.laptops = laptops;
    }

    @Override
    public String toString() {
        return "Stud{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                ", laptops=" + laptops +
                '}';
    }
}
```
Laptop.java:
```java
package org.example;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.ManyToOne;

@Entity
public class Laptop {
    @Id
    private int lid;
    private String brand;
    private String model;
    private int ram;
    @ManyToOne
    private Stud st;

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

    public Stud getSt() {
        return st;
    }

    public void setSt(Stud st) {
        this.st = st;
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

import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Laptop l1=new Laptop();
        l1.setLid(100);
        l1.setBrand("hp");
        l1.setModel("auth");
        l1.setRam(260);

        Laptop l2=new Laptop();
        l2.setLid(101);
        l2.setBrand("Acer");
        l2.setModel("New");
        l2.setRam(720);

        Stud s1=new Stud();
        s1.setRoll(1);
        s1.setName("saibsct");
        s1.setMarks(100);
        s1.setLaptops(Arrays.asList(l1,l2));

        // to not see a new table
        l1.setSt(s1);
        l2.setSt(s1);

        SessionFactory sf = new Configuration()
                .addAnnotatedClass(org.example.Stud.class)
                .addAnnotatedClass(org.example.Laptop.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();

        Transaction tx=session.beginTransaction();
        session.persist(l1);
        session.persist(l2);
        session.persist(s1);
        tx.commit();
        Stud s2=session.find(Stud.class,1);
        System.out.println(s2);
        session.close();
        sf.close();
    }
}
```
