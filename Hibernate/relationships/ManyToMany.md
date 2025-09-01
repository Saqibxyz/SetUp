>ManyToMany
```package org.hibernate;

import jakarta.persistence.*;

import java.util.List;

@Entity
public class Student {
    @Id
    @GeneratedValue (strategy = GenerationType.AUTO)
    int roll;
    String name;
    int age;
    @ManyToMany(mappedBy = "students")
    List<Laptop> laptops;

    public List<Laptop> getLaptops() {
        return laptops;
    }

    public void setLaptops(List<Laptop> laptops) {
        this.laptops = laptops;
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
                ", laptop=" + laptops +
                '}';
    }
}

```
```
package org.hibernate;

import jakarta.persistence.*;

import java.util.List;

@Entity
public class Laptop {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    int id;
    String name;
    int ram;
    @ManyToMany
    List<Student> students;

    public List<Student> getStudents() {
        return students;
    }

    public void setStudents(List<Student> students) {
        this.students = students;
    }

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

import java.util.Arrays;
import java.util.Optional;

public class Main {
    public static void main(String[] args) {

        Laptop laptop1=new Laptop();
        laptop1.setName("Fujitsu");
        laptop1.setRam(32);
        /// ////
        Laptop laptop2=new Laptop();
        laptop2.setName("Dell");
        laptop2.setRam(8);
        /// ///////
        Student student=new Student();
        student.setName("Saqib");
        student.setAge(23);
        /// /////////
        Student student2=new Student();
        student.setName("Zahid");
        student.setAge(20);
        /// //////
        laptop1.setStudents(Arrays.asList(student,student2));
        student.setLaptops(Arrays.asList(laptop1,laptop2));

       try( SessionFactory sessionFactory=new Configuration().
               configure().addAnnotatedClass(Student.class).addAnnotatedClass(Laptop.class)
               .buildSessionFactory()) {

          try ( Session session=sessionFactory.openSession();){
              Transaction transaction=session.beginTransaction();
              session.persist(student);
              transaction.commit();
              Student student1=session.find(Student.class,1);
              System.out.println(student1);

          }




       }


    }
}
```