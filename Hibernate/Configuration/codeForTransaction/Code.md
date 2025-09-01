>Sample code for transaction
```
package org.hibernate;

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