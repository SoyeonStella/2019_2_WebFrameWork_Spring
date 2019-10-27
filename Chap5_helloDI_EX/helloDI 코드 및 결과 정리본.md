# 의존성 주입 실습 코드 (helloDI)  

##  패키지 : helloDI/src/main/java/kr.ac.hansung  

#### AnimalType.java  

~~~ java
package kr.ac.hansung;

public interface AnimalType {
	public void sound();
}  
~~~

#### Cat.java  

~~~java
package kr.ac.hansung;

import lombok.Setter;

@Setter
public class Cat implements AnimalType {

	private String myName;

	@Override
	public void sound() {

		System.out.println("Cat name = " + myName + " : Meow");
		
	}

}  
~~~

#### Dog.java  

~~~java
package kr.ac.hansung;

import lombok.Setter;

@Setter
public class Dog implements AnimalType {
	

	private String myName;

	@Override
	public void sound() {
		
		System.out.println("Dog name = " + myName + " : BowWow");
		
	}

	

}  
~~~

#### PetOwner.java  

~~~java
package kr.ac.hansung;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class PetOwner {

	@Autowired //wired by type
	@Qualifier(value="qf_cat") //모호성 해결, value="qf_dog"로 하면 id_dog 빈이 주입 됨.
	private AnimalType animal;
	
	public void play() {
		animal.sound();
	}
}  
~~~

#### MainApp.java  

~~~java
package kr.ac.hansung;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {

	public static void main(String[] args) {

		ClassPathXmlApplicationContext context =
				new ClassPathXmlApplicationContext("kr/ac/hansung/conf/animal.xml");
	
		PetOwner person = (PetOwner)context.getBean("id_petOwner");
	
		person.play();
		
		context.close();
	}

}  
~~~



## 패키지 : helloDI/src/main/java/kr.ac.hansung.conf  

#### animal.xml  

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
	
	<bean id="id_dog" class="kr.ac.hansung.Dog">
		<qualifier value="qf_dog"></qualifier>
		<property name="myName" value="poodle"></property>
	</bean>
	
	<bean id="id_cat" class="kr.ac.hansung.Cat">
		<qualifier value="qf_cat"></qualifier>
		<property name="myName" value="bella"></property>
	</bean>
	
	<bean id="id_petOwner" class="kr.ac.hansung.PetOwner">
	</bean>

	<context:annotation-config></context:annotation-config>
</beans>
~~~



## 결과 

~~~console
Cat name = bella : Meow
~~~