# Spring

[Зачем мы используем Spring? Почему его так любят?](#зачем-мы-используем-spring-почему-его-так-любят)

[Inversion of Control](#inversion-of-control)

[Dependency Injection](#dependency-injection)

[@Autowired](#autowired)

[@Qualifier vs @Primary](#qualifier-vs-primary)

[Что такое Bean?](#что-такое-bean)

[Скоуп бинов. Web скоуп](#скоуп-бинов-web-скоуп)

[Жизненный цикл Bean. Вопросы связанные с классами входящими в жизненный цикл бина](#жизненный-цикл-bean-вопросы-связанные-с-классами-входящими-в-жизненный-цикл-бина)

[@ComponentScan](#componentscan)

[@Conditional в Spring](#conditional-в-spring)

[@Service, @Repository, @Component](#service-repository-component)

[@Controller и @RestController](#controller-и-restcontroller)

[@ResponseBody vs ResponseEntity](#responsebody-vs-responseentity)

[Spring MVC. Что это? Какие проблемы решает?](#spring-mvc-что-это-какие-проблемы-решает)

[Парадигма AOP](#парадигма-aop)

[Циклическая зависимость](#циклическая-зависимость)

[Spring Boot](#spring-boot)

[Spring JDBC](#spring-jdbc)

[Spring Data JPA](#spring-data-jpa)

[Блокировка по умолчанию в Spring](#блокировка-по-умолчанию-в-spring)

[@Transactional. Как работает?](#transactional-как-работает)

[@Async](#async)

[Starter в Spring. Плюсы и минусы](#starter-в-spring-плюсы-и-минусы)

[Spring Criteria API](#spring-criteria-api)

[@Scheduled](#scheduled)

[Понятие Controller и Servlet в Java. Это одно и то же? Если разное, в чем отличия?](#понятие-controller-и-servlet-в-java-это-одно-и-то-же-если-разное-в-чем-отличия)

[Что такое объект Filter? В какой момент вызывается?](#что-такое-объект-filter-в-какой-момент-вызывается)

[Что такое ApplicationContext?](#что-такое-applicationcontext)

[Зачем нужны аннотации @RequestParam и @PathVariable?](#зачем-нужны-аннотации-requestparam-и-pathvariable)

[@ConfigurationProperties? ConfigurationProperties vs Value](#configurationproperties-configurationproperties-vs-value)

[@Value. Что это?](#value-что-это)

[Как работает DispatcherServlet?](#как-работает-dispatcherservlet)

[Варианты обработки Exceptions в Spring](#варианты-обработки-exceptions-в-spring)

[@Configuration. Является Configuration компонентом? Какую доп логику Configuration реализует?](#configuration-является-configuration-компонентом-какую-доп-логику-configuration-реализует)

[@PostConstruct](#postconstruct)

[Dependency management в Spring boot](#dependency-management-в-spring-boot)

[Spring Security, как хранить пароль пользователя?](#spring-security-как-хранить-пароль-пользователя)

[Spring Profiles как с ними работать?](#spring-profiles-как-с-ними-работать)

[Виды proxy в Spring](#виды-proxy-в-spring)

[@Lookup](#lookup)

# Зачем мы используем Spring? Почему его так любят?

Spring — это один из самых популярных фреймворков для разработки приложений на Java. Его используют благодаря широкому
набору возможностей, гибкости и модульности, что упрощает разработку корпоративных приложений.

[К оглавлению](#Spring)

# Inversion of Control

Это принцип, при котором мы передаем управление созданием и настройкой объектов Spring-у

[К оглавлению](#Spring)

# Dependency Injection

Dependency Injection (внедрение зависимостей) — это способ предоставления объекту его зависимостей извне, вместо того
чтобы объект создавал их самостоятельно. DI упрощает управление зависимостями и делает код более модульным, тестируемым
и легко расширяемым.

### Dependency Injection vs dependency lookup

| Критерий              | Dependency Injection                                              | Dependency Lookup                                       |
|-----------------------|-------------------------------------------------------------------|---------------------------------------------------------|
| Суть                  | Зависимости предоставляются извне (контейнером или фреймворком).  | Объект сам запрашивает зависимости (чаще из контекста). |
| Простота тестирования | Легче тестировать, зависимости можно заменять mock-объектами.     | Требует мокирования контекста или фабрики для тестов.   |
| Уровень контроля      | Контейнер управляет зависимостями, объект их не создаёт.          | Объект сам решает, как получить зависимости.            |
| Пример                | Spring внедряет зависимость через аннотации или XML-конфигурацию. | Вручную вызов метода ApplicationContext.getBean(...).   |

### Какие способы внедрения зависимостей знаешь?

| Способ             | Описание                                                                    | Плюсы                                                                               | Минусы                                                                                  | Примеры использования                                         |
|--------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------|
| Через конструктор  | Зависимости передаются при создании объекта.                                | Гарантия неизменяемости. Обязательные зависимости явно видны. Высокая тестируемость | Неудобно для большого числа зависимостей. Не подходит для опциональных зависимостей     | Когда зависимости обязательны и не изменяются после создания. |
| Через setter       | Зависимости устанавливаются с помощью метода после создания объекта.        | Подходит для опциональных зависимостей. Зависимости можно изменять в runtime.       | Возможна ситуация, когда объект используется без установленной зависимости. Менее явный | Когда зависимости опциональны или могут изменяться.           |
| Через поля (field) | Зависимости внедряются напрямую в поля объекта, обычно с помощью аннотаций. | Минимум кода. Простота при использовании фреймворков (Spring, CDI).                 | Сложнее тестировать. Нарушение принципа явности зависимостей. Отсутствие контроля.      | Быстрое внедрение в Spring через @Autowired.                  |

### Почему лучше использовать конструктор?

+ Неизменяемость: Зависимости устанавливаются один раз при создании объекта, и их невозможно изменить. 
+ Обязательность зависимостей: Устанавливает чёткое требование, что объект не может существовать без переданных зависимостей. 
+ Тестируемость: Проще создавать mock-объекты для тестов. 
+ Явность: Видно, какие зависимости требуются для работы объекта.

### Когда пригодится способ внедрения через setter?

+ Опциональные зависимости: Зависимость не обязательна для работы объекта и используется только в некоторых сценариях. 
+ Динамическое изменение зависимостей: Когда требуется сменить зависимость во время выполнения программы. 
+ Библиотеки или старый код: Для интеграции с кодом, где нельзя контролировать процесс создания объектов.

[К оглавлению](#Spring)

# @Autowired

Автоматически внедряет бины. Аннотацией @Autowired помечают:

+ Сеттер 
+ Поле 
+ Конструктор

[К оглавлению](#Spring)

# @Qualifier vs @Primary

Если есть два одинаковых бина (по типу и имени) Spring не знает какой именно использовать и выдает exception. Если над
одним из этих бинов установлена @Primary, то его использовать предпочтительнее. Но если нам нужно использовать в работе
оба этих бина, можно над каждым поставить @Qualifier и задать имя, для идентификации этих бинов (работает только с
@Autowired)

| Аннотация  | Описание                                                 | Когда использовать                                                 |
|------------|----------------------------------------------------------|--------------------------------------------------------------------|
| @Primary   | Указывает бин по умолчанию при выборе среди нескольких.  | Когда нужен бин, который используется чаще всего.                  |
| @Qualifier | Явно указывает, какой бин использовать среди нескольких. | Когда нужно внедрить конкретный бин, несмотря на наличие @Primary. |

[К оглавлению](#Spring)

# Что такое Bean? 

Bean в Spring — это объект, управляемый контейнером Spring IoC. Это основной строительный блок приложения, который
регистрируется и конфигурируется контейнером.

Аннотация @Bean используется для указания метода, который возвращает объект(бин) и эти бины в дальнейшем можно внедрять
в другие компоненты.

Аннотация @Bean используется для явного объявления бина в Java-конфигурации (в классе, помеченном как @Configuration).

Также можно использовать методы по умолчанию для определения бинов. Это позволяет создавать конфигурации бинов путем
реализации интерфейсов с определениями бинов в методах по умолчанию.

```java
public interface BaseConfig {
    @Bean
    default TransferServiceImpl transferService() {
        return new TransferServiceImpl();
    }
}

@Configuration
public class AppConfig implements BaseConfig {
}
```

### Что такое BeanDefinition?

BeanDefinition — это метаданные, описывающие, как должен быть создан, конфигурирован и управляем конкретный бин.

Он содержит:
+ Класс бина. 
+ Его скоуп (например, singleton или prototype). 
+ Зависимости. 
+ Методы инициализации и уничтожения.

### Как Spring создает бины?

+ Чтение конфигурации: Spring считывает конфигурацию из @Configuration, XML или аннотаций на классах.
+ Создание BeanDefinition: На основе конфигурации Spring формирует BeanDefinition для каждого бина. 
+ Инициализация бина: Вызывается конструктор или фабричный метод. Выполняется внедрение зависимостей (через конструктор, поля или сеттеры).
+ Обработка бина: Постобработка через BeanPostProcessor (например, для работы с @Autowired или @PostConstruct).
+ Добавление в контейнер: Готовый бин помещается в ApplicationContext.

### Как конфигурируется бин?

+ Аннотации (@Bean, @Component, @Value, @Scope).
+ XML-конфигурацию.
+ Java-код (в классе с @Configuration).
+ Средства типа @PostConstruct и @PreDestroy для управления жизненным циклом.

### В чем проблема того, что мы не можем final поле сделать в Bean-е?

Если в бине есть final поле, то оно должно быть инициализировано в момент создания объекта. Spring может не знать
значения для этого поля на этапе создания объекта, так как внедрение зависимостей происходит после вызова конструктора (
например, через @Autowired).

### Можно вставить Bean в статическое поле?

Нет, Spring не поддерживает внедрение зависимостей в статические поля напрямую. Решение: Используйте вручную сеттер или
вызовите контекст через ApplicationContext.

### Как сделать ленивую инициализацию бина?

Используйте аннотацию @Lazy.

### Какие есть способы донастройки бина?

Spring предоставляет несколько способов для настройки (customization) бинов перед их использованием в приложении.

| Метод                                 | Описание                                                      |
|---------------------------------------|---------------------------------------------------------------|
| @PostConstruct                        | Метод вызывается после создания бина и внедрения зависимостей |
| @PreDestroy                           | Метод вызывается перед удалением бина                         |
| InitializingBean#afterPropertiesSet() | Метод вызывается после установки свойств                      |
| DisposableBean#destroy()	             | Метод вызывается перед уничтожением бина                      |
| @Bean(initMethod, destroyMethod)	     | Указывает методы инициализации и удаления                     |
| BeanPostProcessor                     | Позволяет изменять бины до и после инициализации              |
| ApplicationContextAware               | Дает доступ к ApplicationContext внутри бина                  |

1. @PostConstruct и @PreDestroy

Аннотации, которые позволяют выполнить код перед инициализацией бина и перед его уничтожением.

````
@Component
public class MyBean {
    @PostConstruct
    public void init() {
        System.out.println("Бин создан!");
}

    @PreDestroy
    public void destroy() {
        System.out.println("Бин будет удален!");
    }
}

Вывод:

Бин создан!
Бин будет удален!
```` 

2. Реализация InitializingBean и DisposableBean

Интерфейсы, которые позволяют управлять жизненным циклом бина.

````
@Component
public class MyBean implements InitializingBean, DisposableBean {
    @Override
    public void afterPropertiesSet() {
        System.out.println("Бин инициализирован!");
}

    @Override
    public void destroy() {
        System.out.println("Бин уничтожается!");
    }
}
````

Используется редко, так как @PostConstruct и @PreDestroy удобнее.

3. @Bean(initMethod, destroyMethod)

В @Configuration можно указать методы инициализации и уничтожения бина.

````
@Configuration
public class AppConfig {
    @Bean(initMethod = "init", destroyMethod = "cleanup")
    public MyBean myBean() {
        return new MyBean();
    }
}

class MyBean {
    public void init() {
        System.out.println("Инициализация через initMethod");
}

    public void cleanup() {
        System.out.println("Удаление через destroyMethod");
    }
}

Вывод:
Инициализация через initMethod
Удаление через destroyMethod
````

4. BeanPostProcessor

Позволяет изменять бины до и после их инициализации.

````
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        System.out.println("Before init: " + beanName);
    return bean;
}

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        System.out.println("After init: " + beanName);
        return bean;
    }
}

Вывод:
Before init: myBean
After init: myBean
````

5. ApplicationContextAware

Дает доступ к ApplicationContext внутри бина.

````
@Component
public class MyBean implements ApplicationContextAware {
private ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) {
        this.applicationContext = applicationContext;
        System.out.println("ApplicationContext установлен!");
    }
}
````

Полезно для получения других бинов внутри класса.

### Как создать несколько бинов одного типа Singleton?

По умолчанию в Spring бины создаются в единственном экземпляре (Singleton). Однако, можно создать несколько бинов одного типа.

1. Разные имена в @Bean

````
@Configuration
public class AppConfig {
    @Bean("bean1")
    public MyBean myBean1() {
        return new MyBean("Первый бин");
}

    @Bean("bean2")
    public MyBean myBean2() {
        return new MyBean("Второй бин");
    }
}

class MyBean {
    private String name;

    public MyBean(String name) {
        this.name = name;
    }
}
````

Теперь можно получить два разных бина:

````
@Autowired
@Qualifier("bean1")
private MyBean myBean1;
````

2. Использование @Primary и @Qualifier

Если есть несколько бинов одного типа, можно указать приоритетный бин с @Primary.

````
@Component
@Primary
public class FirstService implements MyService {}

@Component
public class SecondService implements MyService {}
````

Теперь @Autowired будет использовать FirstService.
Если нужен SecondService, то используем @Qualifier:

````
@Autowired
@Qualifier("secondService")
private MyService myService;
````

3. Использование @Profile

Можно загружать разные бины в зависимости от профиля окружения.

````
@Component
@Profile("dev")
public class DevBean implements MyBean {}

@Component
@Profile("prod")
public class ProdBean implements MyBean {}
````

Запуск с -Dspring.profiles.active=dev создаст DevBean, а с -Dspring.profiles.active=prod — ProdBean.

### Где хранятся бины?

Контейнер Spring – ApplicationContext

Spring создает и хранит бины в контейнере IoC (ApplicationContext). Когда Spring загружается, он:

+ Сканирует классы с аннотациями @Component, @Service, @Repository. 
+ Создает бины и помещает их в ApplicationContext. 
+ Управляет их жизненным циклом (инициализация, использование, удаление).

### Как получить бин из ApplicationContext?

````
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
MyBean myBean = context.getBean(MyBean.class);
````

### Как вывести все бины?
````
String[] beanNames = context.getBeanDefinitionNames();
for (String beanName : beanNames) {
System.out.println(beanName);
}
````

Выведет все бины, зарегистрированные в контейнере.

[К оглавлению](#Spring)

# Скоуп бинов. Web скоуп

+ Singleton

  + Для каждого бина создается только один экземпляр на весь контекст приложения и переиспользуется везде, где он требуется 
  + Используется по-умолчанию 
  + Жизненный цикл бина начинается при загрузке контекста, и заканчивается при его завершении

+ Prototype

  + Каждый раз, когда запрашивается бин, создается новый экземпляр 
  + Бины со скоупом prototype не уничтожаются Spring, так как они выходят из его контроля сразу после создания 
  + НЕ ХРАНЯТСЯ в контексте Spring-а

Web скоуп:

| Скоуп       | Описание                                                                                           | Жизненный цикл                                               |
|-------------|----------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| request     | Создаётся один экземпляр бина на каждый HTTP-запрос                                                | Бин существует только в рамках одного HTTP-запроса.          |
| session     | Создаётся один экземпляр бина на каждую HTTP-сессию                                                | Бин живёт в течение одной HTTP-сессии.                       |
| application | Создаётся один экземпляр бина на весь ServletContext (аналогично синглтону, но для веб-приложения) | Бин существует в рамках всего времени работы веб-приложения. |
| websocket   | Создаётся один экземпляр бина на каждую WebSocket-сессию.                                          | Бин живёт в течение времени существования WebSocket-сессии   |

### Как создать свой Скоуп

1. Реализуйте интерфейс Scope:
````
public class CustomScope implements Scope {
    private Map<String, Object> beans = new HashMap<>();

    @Override
    public Object get(String name, ObjectFactory<?> objectFactory) {
        return beans.computeIfAbsent(name, k -> objectFactory.getObject());
    }

    @Override
    public Object remove(String name) {
        return beans.remove(name);
    }
    // Реализуйте другие методы...
}
````

2. Зарегистрируйте его в контексте:

````
@Bean
public static CustomScopeConfigurer customScopeConfigurer() {
    CustomScopeConfigurer configurer = new CustomScopeConfigurer();
    configurer.addScope("customScope", new CustomScope());
    return configurer;
}
````

[К оглавлению](#Spring)

# Жизненный цикл Bean. Вопросы связанные с классами входящими в жизненный цикл бина

Spring управляет созданием, инициализацией и уничтожением бинов в рамках контейнера IoC. Этот жизненный цикл можно разделить на несколько этапов.

`Создание бина -> Внедрение зависимостей -> Постобработка -> Инициализация -> Использование -> Уничтожение`

![beanLifeCycle.png](../../../resources/beanLifeCycle.png)

+ Сначала мы предоставляем наши Bean Definitions IoC контейнеру 
+ IoC-контейнер сортирует Bean Definitions чтобы сначала создавать те, которые не имеют зависимостей, и затем те, которые зависят от других бинов 
+ Затем итерируется по всем Bean Definitions и инициализирует их 
+ Далее вызываются PostConstruct методы 
+ Перед и после инициализации вызываются методы BeanPostProcessor 
+ На выходе получаем готовый бин. Если у этого бина скоуп singleton - храним его в ассоциативном массиве IoC контейнера. Если же другой скоуп - бин сразу же возвращается 
+ Для всех бинов, которые находятся в IoC контейнере, перед его завершением вызывается @PreDestroy метод

| Этап                   | Описание                                                                                                                 | Методы/Аннотации                                                                                     |
|------------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Создание бина          | Spring создает экземпляр бина, используя конструктор или фабричный метод                                                 | Конструктор бина (или фабричный метод).                                                              |
| Внедрение зависимостей | После создания, Spring внедряет все необходимые зависимости (через конструктор, поля, или сеттеры).                      | @Autowired, @Inject, конструкторы, сеттеры.                                                          |
| Постобработка          | Бин проходит через постобработчики перед инициализацией. Это этап, на котором можно изменить состояние бина.             | BeanPostProcessor — postProcessBeforeInitialization, postProcessAfterInitialization.                 |
| Инициализация          | Если бин реализует интерфейсы InitializingBean или использует аннотации @PostConstruct, вызываются методы инициализации. | afterPropertiesSet(), @PostConstruct, пользовательские методы инициализации через @Bean(initMethod). |
| Использование          | Бин доступен для использования в приложении. Это этап, когда бин полностью готов для работы.                             | Использование бина через @Autowired или прямые вызовы методов.                                       |
| Уничтожение            | Когда контейнер Spring уничтожает бин (особенно для синглтонов), вызываются методы очистки.                              | destroy(), @PreDestroy, пользовательские методы уничтожения через @Bean(destroyMethod).              |

[К оглавлению](#Spring)

# @ComponentScan

[К оглавлению](#Spring)

# @Conditional в Spring

[К оглавлению](#Spring)

# @Service, @Repository, @Component

[К оглавлению](#Spring)

# @Controller и @RestController

[К оглавлению](#Spring)

# @ResponseBody vs ResponseEntity

[К оглавлению](#Spring)

# Spring MVC. Что это? Какие проблемы решает?

[К оглавлению](#Spring)

# Парадигма AOP

[К оглавлению](#Spring)

# Циклическая зависимость

[К оглавлению](#Spring)

# Spring Boot

[К оглавлению](#Spring)

# Spring JDBC

[К оглавлению](#Spring)

# Spring Data JPA

[К оглавлению](#Spring)

# Блокировка по умолчанию в Spring

[К оглавлению](#Spring)

# @Transactional. Как работает?

[К оглавлению](#Spring)

# @Async

[К оглавлению](#Spring)

# Starter в Spring. Плюсы и минусы

[К оглавлению](#Spring)

# Spring Criteria API

[К оглавлению](#Spring)

# @Scheduled

[К оглавлению](#Spring)

# Понятие Controller и Servlet в Java. Это одно и то же? Если разное, в чем отличия?

[К оглавлению](#Spring)

# Что такое объект Filter? В какой момент вызывается?

[К оглавлению](#Spring)

# Что такое ApplicationContext?

[К оглавлению](#Spring)

# Зачем нужны аннотации @RequestParam и @PathVariable?

[К оглавлению](#Spring)

# @ConfigurationProperties? ConfigurationProperties vs Value

[К оглавлению](#Spring)

# @Value. Что это?

[К оглавлению](#Spring)

# Как работает DispatcherServlet?

[К оглавлению](#Spring)

# Варианты обработки Exceptions в Spring

[К оглавлению](#Spring)

# @Configuration. Является Configuration компонентом? Какую доп логику Configuration реализует?

[К оглавлению](#Spring)

# @PostConstruct

[К оглавлению](#Spring)

# Dependency management в Spring boot

[К оглавлению](#Spring)

# Spring Security, как хранить пароль пользователя?

[К оглавлению](#Spring)

# Spring Profiles как с ними работать?

[К оглавлению](#Spring)

# Виды proxy в Spring

[К оглавлению](#Spring)

# @Lookup

[К оглавлению](#Spring)