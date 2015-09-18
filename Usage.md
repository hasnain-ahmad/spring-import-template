# Introduction #




# Getting stated #

## Create the template ##

First, you have to create a Spring Template.
This is actually a standard configuration file, but with some own features and restrictions.

**You can use some variables in configuration. A variable is defined like this**${variable} All non-nested beans **MUST** have dynamic name (name contain a variable)

```
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://ww.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- A Bean must use variable in his name -->
	<bean name="simple-${env}" class="be.hikage.springtemplate.SimpleBean">
	
		<!-- You can used variable in constructor value -->
		<constructor-arg value="constructorData.${env}"/>

		<!-- You can used variable in propertyPlaceHolder too -->
		<constructor-arg value="${${env}.constructorValue}"/>
		
		<!-- You can use variable in property -->
		<property name="propertyValue" value="propertyData.${env}"/>
		<property name="externalizedPropertyValue" value="${${env}.propertyValue}"/>
	</bean>
		
	<!-- A Bean must use variable in his name -->
	<bean name="container-${env}" class="be.hikage.springtemplate.ContainerBean">
		<property name="bean" ref="simple-${env}"/>
		<property name="innerAnonymous">
			<!-- A inner bean is not affected by name restriction -->
			<bean  class="be.hikage.springtemplate.SimpleBean">
				<constructor-arg value=""/>
				<constructor-arg value=""/>
			</bean>
		</property>
	</bean>
</beans>
```


## Import the template in other configuration ##

To import template, you must use the dedicated namespace
```
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:hikage="http://www.hikage.be/schema/import-template" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.hikage.be/schema/import-template http://www.hikage.be/schema/import-template/import-template.xsd">
        

	<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="location" value="classpath:be/hikage/springtemplate/test-config-good.properties"/>
	</bean>
	
	<!--Usage  -->
	<hikage:import-template location="classpath:be/hikage/springtemplate/template-good.xml">
	
		<!-- Replace variable ${env} by dev -->
		<hikage:variable name="env" value="dev"/>
	</hikage:import-template>
</beans>
```