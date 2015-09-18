# Goal #
The aim of this project is to provide a reusable configuration module

# Sample #

Consider this template :

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean name="scheduledTimeTask.${server}" class="org.springframework.scheduling.timer.ScheduledTimerTask">
        <property name="timerTask" ref="monitoringTask.${server}"/>

        <property name="delay" value="1000"/>
        <property name="period" value="1000"/>
    </bean>
    <bean name="timer.${server}" class="org.springframework.scheduling.timer.TimerFactoryBean">
        <property name="scheduledTimerTasks">
            <list>
                <ref bean="scheduledTimeTask.${server}"/>
            </list>
        </property>
    </bean>
    <bean name="monitoringTask.${server}" class="be.hikage.springtemplate.MonitoringTimerTask">
           <property name="url" value="http://${server}/ping"/>
    </bean>


</beans>
```


You can use this template like this :

```
<hikage:import-template location="classpath:be/hikage/springtemplate/template-good.xml">
        <hikage:variable name="server" value="google.com"/>
    </hikage:import-template>
```

The imported configuration  corresponds to this configuration :

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean name="scheduledTimeTask.google.com" class="org.springframework.scheduling.timer.ScheduledTimerTask">
        <property name="timerTask" ref="monitoringTask.google.com"/>

        <property name="delay" value="1000"/>
        <property name="period" value="1000"/>
    </bean>
    <bean name="timer.google.com" class="org.springframework.scheduling.timer.TimerFactoryBean">
        <property name="scheduledTimerTasks">
            <list>
                <ref bean="scheduledTimeTask.google.com"/>
            </list>
        </property>
    </bean>
    <bean name="monitoringTask.google.com" class="be.hikage.springtemplate.MonitoringTimerTask">
           <property name="url" value="http://google.com/ping"/>
    </bean>


</beans>
```

However, the model becomes really interesting is when multiple occurrence:

```

<!-- One for Google -->
<hikage:import-template location="classpath:be/hikage/springtemplate/template-good.xml">
        <hikage:variable name="server" value="google.com"/>
    </hikage:import-template>

<!-- One for Hikage.be -->
<hikage:import-template location="classpath:be/hikage/springtemplate/template-good.xml">
        <hikage:variable name="server" value="hikage.be"/>
    </hikage:import-template>


<!-- One for SpringFramework -->
<hikage:import-template location="classpath:be/hikage/springtemplate/template-good.xml">
        <hikage:variable name="server" value="springframework.org"/>
</hikage:import-template>
```

Now, if a modification must be made, there is only one file to modify.