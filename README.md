# Project-on-Hinernate-Crud-Operation
//Depending class
package com.qsp;

import java.util.List;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToMany;

@Entity
public class Teacher {
	@Override
	public String toString() {
		return "Teacher [id=" + id + ", name=" + name + ", student=" + student + "]";
	}
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private int id;
	private String name;
	@OneToMany(cascade = {CascadeType.PERSIST,CascadeType.REMOVE,CascadeType.MERGE})
	private List<Student>student;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public List<Student> getStudent() {
		return student;
	}
	public void setStudent(List<Student> student) {
		this.student = student;
	}
	public int getId() {
		return id;
	}

}

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
//Dependent Class
package com.qsp;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToMany;

@Entity
public class Student {
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", mobno=" + mobno + "]";
	}

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private int id;
	private String name;
	private long mobno;
	
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public long getMobno() {
		return mobno;
	}
	public void setMobno(long mobno) {
		this.mobno = mobno;
	}
//	public Teacher getTeacher() {
//		return teacher;
//	}
//	public void setTeacher(Teacher teacher) {
//		this.teacher = teacher;
	
	public int getId() {
		return id;
	}
	

}

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

//All the main logic to perform the crud opration
package com.qsp;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class Save {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("vikas");
		EntityManager entityManager = entityManagerFactory.createEntityManager();

		Teacher teacher = new Teacher();
		teacher.setName("nandesh sir");

		Student student = new Student();
		student.setName("saurabh");
		student.setMobno(645767574l);

		Student student1 = new Student();
		student1.setName("tiger");
		student1.setMobno(945757574l);

		ArrayList<Student> list = new ArrayList<Student>();
		list.add(student);
		list.add(student1);

		teacher.setStudent(list);

		System.out.println("enter the number");
		System.out.println("1.for save");
		System.out.println("2.for update");
		System.out.println("3.for delete");
		System.out.println("4.for exist");

		int choice = sc.nextInt();

		do {
			switch (choice) {
			case 1:
				System.out.println("save the data"); {
				entityManager.getTransaction().begin();
				entityManager.persist(teacher);
				entityManager.getTransaction().commit();
				break;

			}
			case 2:
				System.out.println("Welcome to update"); {
				System.out.println("what do you want to upadate ");
				System.out.println("For Teacher data press.1");
				System.out.println("For Student data press.2");
				int ch = sc.nextInt();
				switch (ch) {
				case 1:
					System.out.println("enter the id of the teacher whos data you have to update"); {
					int id = sc.nextInt();
					Teacher teacher2 = entityManager.find(Teacher.class, id);
					if (teacher2 != null) {
						System.out.println(teacher2.getId());
						System.out.println(teacher2.getName());

						System.out.println("1.for update name");
						int uid = sc.nextInt();
						System.out.println("entr the new name ");
						String name = sc.next();

						teacher2.setName(name);
						entityManager.getTransaction().begin();
						entityManager.merge(teacher2);
						entityManager.getTransaction().commit();
						break;
					}
					break;

				}
				case 2:
					System.out.println("what you want update"); {
					List<Student> list2 = teacher.getStudent();
					System.out.println(list2);
					System.out.println("enter the id whoes data to be updated");

					for (Student stu : list2) {
						int id = sc.nextInt();
						if (stu.getId() == id) {
							System.out.println(stu);
							System.out.println("what you want to update");
							System.out.println("1.name");
							System.out.println("2.for mobono");
							int subSwitch = sc.nextInt();
							switch (subSwitch) {
							case 1: {
								System.out.println("enter the new mobno");
								long mob = sc.nextLong();
								stu.setMobno(mob);

								entityManager.getTransaction().begin();
								entityManager.merge(stu);
								entityManager.getTransaction().commit();
								break;

							}

							case 2: {
								System.out.println("enter the new name");
								String name = sc.next();
								stu.setName(name);
								entityManager.getTransaction().begin();
								entityManager.merge(stu);
								entityManager.getTransaction().commit();
								break;

							}
							}
						}

					}

					int ssc = sc.nextInt();
					switch (ssc) {
					case 1: {

					}
					case 2: {

					}
					}

				}
				}

			}
			case 3:
				System.out.println("delete the data"); {

				entityManager.getTransaction().begin();
				entityManager.remove(teacher);
				entityManager.getTransaction().commit();
				break;
			}

			}

		} while (choice != 4);

	}

}
