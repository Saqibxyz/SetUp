OneToOneMapping
```package org.hibernate;

import jakarta.persistence.*;

@Entity
public class Student {
    @Id
    @GeneratedValue (strategy = GenerationType.AUTO)
    int roll;
    String name;
    int age;
    @OneToOne
    Laptop laptop;

    public Laptop getLaptop() {
        return laptop;
    }

    public void setLaptop(Laptop laptop) {
        this.laptop = laptop;
    }

    public Student() {
    }
// for invalid student
    public Student(int id,String name, int age) {
        this.roll=id;
        this.name = name;
        this.age = age;
    }

    public int getRoll() {
        return roll;
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

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "roll=" + roll +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", laptop=" + laptop +
                '}';
    }
}


```
```
package org.hibernate;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Laptop {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    int id;
    String name;
    int ram;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getRam() {
        return ram;
    }
    public void setRam(int ram) {
        this.ram = ram;
    }
    @Override
    public String toString() {
        return "Laptop{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", ram=" + ram +
                '}';
    }
}
```
```package org.hibernate;

import org.hibernate.cfg.Configuration;

import java.util.Optional;

public class Main {
    public static void main(String[] args) {

        Laptop laptop=new Laptop();
        laptop.setName("Fujitsu");
        laptop.setRam(32);
        Student student=new Student();
        student.setName("Saqib");
        student.setAge(23);
        student.setLaptop(laptop);

       try( SessionFactory sessionFactory=new Configuration().
               configure().addAnnotatedClass(Student.class).addAnnotatedClass(Laptop.class)
               .buildSessionFactory()) {

          try ( Session session=sessionFactory.openSession();){
              Transaction transaction=session.beginTransaction();
              session.persist(student);
              session.persist(laptop);
              transaction.commit();
              Student student1=session.find(Student.class,1);
              System.out.println(student1);

          }




       }


    }
}
```
