# ManyToMany
Stud.java:
```java
package org.example;


import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.ManyToMany;
import jakarta.persistence.OneToMany;

import java.util.List;

@Entity
public class Stud {
    @Id
    private int roll;

    private String name;
    private int marks;
    @ManyToMany
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
import jakarta.persistence.ManyToMany;
import jakarta.persistence.ManyToOne;

import java.util.List;

@Entity
public class Laptop {
    @Id
    private int lid;
    private String brand;
    private String model;
    private int ram;
    @ManyToMany
    private List<Stud> sts;

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


    public List<Stud> getSts() {
        return sts;
    }

    public void setSts(List<Stud> sts) {
        this.sts = sts;
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

        Laptop l3=new Laptop();
        l3.setLid(107);
        l3.setBrand("Dell");
        l3.setModel("upgrad");
        l3.setRam(1080);

        Stud s1=new Stud();
        s1.setRoll(1);
        s1.setName("saibsct");
        s1.setMarks(100);
        Stud s2=new Stud();
        s2.setRoll(11);
        s2.setName("sai");
        s2.setMarks(76);
        Stud s3=new Stud();
        s3.setRoll(21);
        s3.setName("bsct");
        s3.setMarks(40);



        s1.setLaptops(Arrays.asList(l1,l2));
        s2.setLaptops(Arrays.asList(l3,l2));
        s3.setLaptops(Arrays.asList(l1));

        l1.setSts(Arrays.asList(s1,s2));
        l2.setSts(Arrays.asList(s1,s3));
        l3.setSts(Arrays.asList(s2));



        SessionFactory sf = new Configuration()
                .addAnnotatedClass(org.example.Stud.class)
                .addAnnotatedClass(org.example.Laptop.class)
                .configure().buildSessionFactory();
        Session session = sf.openSession();

        Transaction tx=session.beginTransaction();
        session.persist(l1);
        session.persist(l2);
        session.persist(l3);
        session.persist(s1);
        session.persist(s2);
        session.persist(s3);
        tx.commit();
        Stud s5=session.find(Stud.class,11);
        System.out.println(s2);
        session.close();
        sf.close();
    }
}

```
```
But new 2 tables stud_laptop and laptop_stud also created
so add this:@ManyToMany(mappedBy="sts")
to stop students to craete a table now only 1 table will be there that is laptops_stud created by laptops
```
