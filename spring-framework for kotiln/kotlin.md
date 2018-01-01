# 1. Kotlin

======

[Kotlin](https://kotlinlang.org) is a statically-typed language targeting the JVM (and other platforms) which allows writing concise and elegant code while providing very good [interoperability](https://kotlinlang.org/docs/reference/java-interop.html) with existing libraries written in Java.

코틀린은 JVM 및 기타 플랫폼을 목표으로 한 정적 타입 언어이다. 코틀린은 자바로 작성된 라이브러리와 아주 좋은 상호 운용성을 제공하면서도(자바 라이브러리를 호출하여 코틀린에서 사용 가능함) 간결하고 우아한 코드로 작성할수 있는 언어이다.

The Spring Framework provides first-class support for Kotlin that allows developers to write Kotlin applications almost as if the Spring Framework was a native Kotlin framework.

Spring Framework 가 네이티브 코틀린 프레임워크처럼 개발자가 코트린 어플리케이션으로 작성할수 있도록 first-class 지원을 한다.

## 1.1. Requirements(필요사항들)
------------

Spring Framework supports Kotlin 1.1+ and requires [`kotlin-stdlib`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-stdlib) (or one of its [`kotlin-stdlib-jre7`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-stdlib-jre7) / [`kotlin-stdlib-jre8`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-stdlib-jre8) variants) and [`kotlin-reflect`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-reflect) to be present on the classpath. They are provided by default if one bootstraps a Kotlin project on [start.spring.io](https://start.spring.io/#!language=kotlin).

Spring Framework은 Kotlin 1.1 이상을 지원하고, kotlin-stdlib(kotlin-stdlib-jre7 또는 kotlin-stdlib-jre8)를 필요로 하고, kotlin-reflect 가 classpath 에 있어야 한다. 그것들은 기본적으로  start.spring.io에 코틀린 프로젝트 생성시에 기본적으로 제공된다.


## 1.2. Extensions[확장]
----------

Kotlin [extensions](https://kotlinlang.org/docs/reference/extensions.html) provide the ability to extend existing classes with additional functionality. The Spring Framework Kotlin APIs make use of these extensions to add new Kotlin specific conveniences to existing Spring APIs. 

코틀린 extensions은  존재하는 클래스들에 추가적으로 함수적인 기능들을 확장하도록 한다. Spring Framework Kotlin API는 기존의 Spring API 들에게 새로운 코틀린의 특수한 편리성(언어적 특성에 따른 편리함)을 더할수 있도록 한다.

{doc-root}/spring-framework/docs/{spring-version}/kdoc-api/spring-framework/\[Spring Framework KDoc API\] lists and documents all the Kotlin extensions and DSLs available.

{doc-root}/spring-framework/docs/{spring-version}/kdoc-api/spring-framework/\[Spring Framework KDoc API\]에
 Kotlin extension들과 그리고 DSL 들을 정리(리스트화)하였다.


Note

Keep in mind that Kotlin extensions need to be imported to be used. This means for example that the `GenericApplicationContext.registerBean` Kotlin extension will only be available if `import org.springframework.context.support.registerBean` is imported. That said, similar to static imports, an IDE should automatically suggest the import in most cases. 

Kotlin extensions은 사용하기 위해서는 import 되어야 함을 명심해라. 이것은 예를 들면 `GenericApplicationContext.registerBean` Kotlin extension은 `import org.springframework.context.support.registerBean`가 import가 되어야 사용할수 있음을 의미한다. 이걸 말하자면, static 임포트도, 마찬가지로 개발도구에서 대부분의 경우에도 자동적으로 되어야 함을 말한다.

For example, [Kotlin reified type parameters](https://kotlinlang.org/docs/reference/inline-functions.html#reified-type-parameters) provide a workaround for JVM [generics type erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html), and Spring Framework provides some extensions to take advantage of this feature. This allows for a better Kotlin API `RestTemplate`, the new `WebClient` from Spring WebFlux and for various other APIs.

예를들면, [Kotlin reified type parameters]
은 JVM [generics type erasure] 에 대한 제 2의 해결책을 제공하고 Spring Framework에서 이러한 특징을 이점을 취하면서 익스텐션을 제공한다. 이러한 점은 Spring WebFlux 와 다양한 또다른 API들로부터 좀 더 좋은 Kotlin API `RestTemplate`, 새로운 `WebClient` 를 제공해준다.

Note

Other libraries like Reactor and Spring Data also provide Kotlin extensions for their APIs, thus giving a better Kotlin development experience overall.
Reactor 와  Spring Data 와 같은 또 다른 라이브러리는 Kotlin extensione들을 제공한다, 즉 전반적인 좀더 나은 코틀린 개발 경험을 주어지게 한다.


To retrieve a list of `Foo` objects in Java, one would normally write:

자바에서 Foo 객체로 리스트를 반환한다면, 보통 아래처럼 작성한다:

    Flux<User> users  = client.get().retrieve().bodyToFlux(User.class)

Whilst with Kotlin and Spring Framework extensions, one is able to write:

Kotiln과 Spring Framework extensionsd을 가지고 , 아래처럼 코드를 작성할수있다.:

    val users = client.get().retrieve().bodyToFlux<User>()
    // or (both are equivalent)
    val users : Flux<User> = client.get().retrieve().bodyToFlux()

As in Java, `users` in Kotlin is strongly typed, but Kotlin’s clever type inference allows for shorter syntax.
자바와 , 코틀린에서는 `users` 는 강한 타입핑이 되지만 , 코틀린은  더 짧은 문법으로 명확한 타입 추론을 허용한다.

## 1.3. Null-safety
-----------

One of Kotlin’s key features is [null-safety](https://kotlinlang.org/docs/reference/null-safety.html) \- which cleanly deals with `null` values at compile time rather than bumping into the famous `NullPointerException` at runtime. This makes applications safer through nullability declarations and expressing "value or no value" semantics without paying the cost of wrappers like `Optional`. (Kotlin allows using functional constructs with nullable values; check out this [comprehensive guide to Kotlin null-safety](http://www.baeldung.com/kotlin-null-safety).)

Although Java does not allow one to express null-safety in its type-system, Spring Framework now provides [null-safety of the whole Spring Framework API](core.html#null-safety) via tooling-friendly annotations declared in the `org.springframework.lang` package. By default, types from Java APIs used in Kotlin are recognized as [platform types](https://kotlinlang.org/docs/reference/java-interop.html#null-safety-and-platform-types) for which null-checks are relaxed. [Kotlin support for JSR 305 annotations](https://github.com/Kotlin/KEEP/blob/jsr-305/proposals/jsr-305-custom-nullability-qualifiers.md) \+ Spring nullability annotations provide null-safety for the whole Spring Framework API to Kotlin developers, with the advantage of dealing with `null` related issues at compile time.

Note

Libraries like Reactor or Spring Data provide null-safe APIs leveraging this feature.

The JSR 305 checks can be configured by adding the `-Xjsr305` compiler flag with the following options: `-Xjsr305={strict|warn|ignore}`.

For kotlin versions 1.1.50+, the default behavior is the same to `-Xjsr305=warn`. The `strict` value is required to have Spring Framework API full null-safety taken in account but should be considered experimental since Spring API nullability declaration could evolve even between minor releases and more checks may be added in the future).

Note

Generic type arguments, varargs and array elements nullability are not supported yet, but should be in an upcoming release, see [this dicussion](https://github.com/Kotlin/KEEP/issues/79) for up-to-date information.

## 1.4. Classes & Interfaces
--------------------

Spring Framework supports various Kotlin constructs like instantiating Kotlin classes via primary constructors, immutable classes data binding and function optional parameters with default values.

Spring Framework는  다양한 코틀린 생성자들을 지원한다. 예를들면 primary constructors 통한 코트린 클래스들과 데이터 바인딩을 한 불변 클래스들 그리고 기본 값을 가진  optional parameters 함수.

Kotlin parameter names are recognized via a dedicated `KotlinReflectionParameterNameDiscoverer` which allows finding interface method parameter names without requiring the Java 8 `-parameters` compiler flag enabled during compilation.

코틀린 파라미터는 수정시에 자바 8의 `-parameters` 컴파이러 옵션 활성을 하지 않고도 인터페이스 메서드 파라미터 이름을 찾을수 있도록 만들어진 `KotlinReflectionParameterNameDiscoverer`을 통해서 인식된다.

[Jackson Kotlin module](https://github.com/FasterXML/jackson-module-kotlin) which is required for serializing / deserializing JSON data is automatically registered when found in the classpath and a warning message will be logged if Jackson and Kotlin are detected without the Jackson Kotlin module present.

Json 데이터를 serializing / deserializing 을 필요로 하여 만들어진 
[Jackson Kotlin module](https://github.com/FasterXML/jackson-module-kotlin)은 자동적으로 등록한다.
classpath 에서 

## 1.5. Annotations
-----------

Spring Framework also takes advantage of [Kotlin null-safety](https://kotlinlang.org/docs/reference/null-safety.html) to determine if a HTTP parameter is required without having to explicitly define the `required` attribute. That means `@RequestParam name: String?` will be treated as not required and conversely `@RequestParam name: String` as being required. This feature is also supported on the Spring Messaging `@Header` annotation.

In a similar fashion, Spring bean injection with `@Autowired` or `@Inject` uses this information to determine if a bean is required or not. `@Autowired lateinit var foo: Foo` implies that a bean of type `Foo` must be registered in the application context while `@Autowired lateinit var foo: Foo?` won’t raise an error if such bean does not exist.

Note

If you are using bean validation on classes with [primary constructor properties](https://kotlinlang.org/docs/reference/classes.html#constructors), make sure to use [annotation use-site targets](https://kotlinlang.org/docs/reference/annotations.html#annotation-use-site-targets) as described in [this Stack Overflow response](https://stackoverflow.com/a/35853200/1092077).

## 1.6. Bean definition DSL
-------------------

Spring Framework 5 introduces a new way to register beans in a functional way using lambdas as an alternative to XML or JavaConfig (`@Configuration` and `@Bean`). In a nutshell, it makes it possible to register beans with a lambda that acts as a `FactoryBean`. This mechanism is very efficient as it does not require any reflection or CGLIB proxies.

In Java, one may for example write:

    	GenericApplicationContext context = new GenericApplicationContext();
    	context.registerBean(Foo.class);
    	context.registerBean(Bar.class, () -> new Bar(context.getBean(Foo.class))
    );

Whilst in Kotlin with reified type parameters and `GenericApplicationContext` Kotlin extensions one can instead simply write:

    val context = GenericApplicationContext().apply {
    	registerBean<Foo>()
    	registerBean { Bar(it.getBean<Foo>()) }
    }

In order to allow a more declarative approach and cleaner syntax, Spring Framework provides a {doc-root}/spring-framework/docs/{spring-version}/kdoc-api/spring-framework/org.springframework.context.support/-bean-definition-dsl/\[Kotlin bean definition DSL\] It declares an `ApplicationContextInitializer` via a clean declarative API which enables one to deal with profiles and `Environment` for customizing how beans are registered.

    fun beans() = beans {
    	bean<UserHandler>()
    	bean<Routes>()
    	bean<WebHandler>("webHandler") {
    		RouterFunctions.toWebHandler(
    			ref<Routes>().router(),
    			HandlerStrategies.builder().viewResolver(ref()).build()
    		)
    	}
    	bean("messageSource") {
    		ReloadableResourceBundleMessageSource().apply {
    			setBasename("messages")
    			setDefaultEncoding("UTF-8")
    		}
    	}
    	bean {
    		val prefix = "classpath:/templates/"
    		val suffix = ".mustache"
    		val loader = MustacheResourceTemplateLoader(prefix, suffix)
    		MustacheViewResolver(Mustache.compiler().withLoader(loader)).apply {
    			setPrefix(prefix)
    			setSuffix(suffix)
    		}
    	}
    	profile("foo") {
    		bean<Foo>()
    	}
    }

In this example, `bean<Routes>()` is using autowiring by constructor and `ref<Routes>()` is a shortcut for `applicationContext.getBean(Routes::class.java)`.

This `beans()` function can then be used to register beans on the application context.

    val context = GenericApplicationContext().apply {
    	beans().initialize(this)
    	refresh()
    }

Note

This DSL is programmatic, thus it allows custom registration logic of beans via an `if` expression, a `for` loop or any other Kotlin constructs.

See [spring-kotlin-functional beans declaration](https://github.com/sdeleuze/spring-kotlin-functional/blob/master/src/main/kotlin/functional/Beans.kt) for a concrete example.

Note

Spring Boot is based on Java Config and [does not provide specific support for functional bean definition yet](https://github.com/spring-projects/spring-boot/issues/8115), but one can experimentally use functional bean definitions via Spring Boot’s `ApplicationContextInitializer` support, see [this Stack Overflow answer](https://stackoverflow.com/questions/45935931/how-to-use-functional-bean-definition-kotlin-dsl-with-spring-boot-and-spring-w/46033685#46033685) for more details and up-to-date information.

Web
---

### 1.7.1. WebFlux Functional DSL

Spring Framework now comes with a {doc-root}/spring-framework/docs/{spring-version}/kdoc-api/spring-framework/org.springframework.web.reactive.function.server/-router-function-dsl/\[Kotlin routing DSL\] that allows one to leverage the [WebFlux functional API](web-reactive.html#webflux-fn) for writing clean and idiomatic Kotlin code:

    router {
    	accept(TEXT_HTML).nest {
    		GET("/") { ok().render("index") }
    		GET("/sse") { ok().render("sse") }
    		GET("/users", userHandler::findAllView)
    	}
    	"/api".nest {
    		accept(APPLICATION_JSON).nest {
    			GET("/users", userHandler::findAll)
    		}
    		accept(TEXT_EVENT_STREAM).nest {
    			GET("/users", userHandler::stream)
    		}
    	}
    	resources("/**", ClassPathResource("static/"))
    }

Note

This DSL is programmatic, thus it allows custom registration logic of beans via an `if` expression, a `for` loop or any other Kotlin constructs. That can be useful when routes need to be registered depending on dynamic data (for example, from a database).

See [MiXiT project routes](https://github.com/mixitconf/mixit/tree/bad6b92bce6193f9b3f696af9d416c276501dbf1/src/main/kotlin/mixit/web/routes) for a concrete example.

### 1.7.2. Kotlin Script templates

As of version 4.3, Spring Framework provides a [ScriptTemplateView](http://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/view/script/ScriptTemplateView.html) to render templates using script engines that supports [JSR-223](https://www.jcp.org/en/jsr/detail?id=223). Spring Framework 5 goes even further by extending this feature to WebFlux and supporting [i18n and nested templates](https://jira.spring.io/browse/SPR-15064).

Kotlin provides similar support and allows the rendering of Kotlin based templates, see [this commit](https://github.com/spring-projects/spring-framework/commit/badde3a479a53e1dd0777dd1bd5b55cb1021cf9e) for details.

This enables some interesting use cases - like writing type-safe templates using [kotlinx.html](https://github.com/Kotlin/kotlinx.html) DSL or simply using Kotlin multiline `String` with interpolation.

This can allow one to write Kotlin templates with full autocompletion and refactoring support in a supported IDE:

    import io.spring.demo.*
    
    """
    ${include("header")}
    <h1>${i18n("title")}</h1>
    <ul>
    ${users.joinToLine{ "<li>${i18n("user")} ${it.firstname} ${it.lastname}</li>" }}
    </ul>
    ${include("footer")}
    """

See [kotlin-script-templating](https://github.com/sdeleuze/kotlin-script-templating) example project for more details.

## 1.8. Spring projects in Kotlin
-------------------------

This section provides focus on some specific hints and recommendations worth knowing when developing Spring projects in Kotlin.

이 세션에서는 코틀린으로 Spring 프로젝트 개발할때, 알아야 추천과 몇몇 특정 힌트들을 제공하는데 치중합니다.

### 1.8.1. Final by default(기본적으로 Final)

By default, [all classes in Kotlin are `final`](https://discuss.kotlinlang.org/t/classes-final-by-default/166). The `open` modifier on a class is the opposite of Java’s `final`: it allows others to inherit from this class. This also applies to member functions, in that they need to be marked as `open` to be overridden.

기본적으로, 모든 클래스는 코틀린에서는 final 이다. `open` 이라는 클래스의 수정자는 자바의 `final` 과 반대된다. 이것은 또한 이것은 멤버 함수들로 적용시키고 ,`open` 으로 오버로딩되는것이 표시되어야 함이 필요로한다.


Whilst Kotlin’s JVM-friendly design is generally frictionless with Spring, this specific Kotlin feature can prevent the application from starting, if this fact is not taken in consideration. This is because Spring beans are normally proxied by CGLIB - such as `@Configuration` classes - which need to be inherited at runtime for technical reasons. The workaround was to add an `open` keyword on each class and member functions of Spring beans proxied by CGLIB such as `@Configuration` classes, which can quickly become painful and is against the Kotlin principle of keeping code concise and predictable.


코트린의 JVM 친화적인 설계는 일반적으로 스프링과 충돌하지는 않지만, 어떤 고려도 하지 않는다면  이러한 코틀린의 특징은 어플리케이션 구동을 막을수있다. 이런 연유로 Spring beans 들은 보통 CGLIB 의한 프록시화 됩니다.  `@Configuration` 클래스들과 같이 - 이것은 기술적 이유로 런타임시에 상속을 필요로 합니다. 차선책은 `open` 키워드를 각각의 클래스나  `@Configuration` 클래스들와 같이 CGLIB에 의해서 프록시화 된 스프링 beans 들을 멤버 함수들에 추가하는 방법이 있습니다. 이것은 아주 고통스러운 방법이자 코틀린의 코드의 간결성과 예측성을 원칙에  반대하는 것입니다.

Fortunately, Kotlin now provides a [`kotlin-spring`](https://kotlinlang.org/docs/reference/compiler-plugins.html#kotlin-spring-compiler-plugin) plugin, a preconfigured version of `kotlin-allopen` plugin that automatically opens classes and their member functions for types annotated or meta-annotated with one of the following annotations:

운좋게도,다음 어노테이션 중에 하나의 메타어노테이션 된것과 어노테이션 타입에 대해서 클래스들과 멤버 함수들을 자동적으로 open 시키는  `kotlin-allopen` plugin 로 미리 설정된 코틀린은 현재 [`kotlin-spring`] 플러그인을 제공합니다.


*   `@Component`
    
*   `@Async`
    
*   `@Transactional`
    
*   `@Cacheable`
    

Meta-annotations support means that types annotated with `@Configuration`, `@Controller`, `@RestController`, `@Service` or `@Repository` are automatically opened since these annotations are meta-annotated with `@Component`.

Meta-annotations 지원은 `@Component` 타입 어노테이션과 같이 메타 어노테이션이 되어지기 때문에  `@Configuration`, `@Controller`, `@RestController`, `@Service` 또는 `@Repository` 는 자동적으로 open 되어진다.
   
[start.spring.io](http://start.spring.io/#!language=kotlin) enables it by default, so in practice you will be able to write your Kotlin beans without any additional `open` keyword, like in Java.

[start.spring.io](http://start.spring.io/#!language=kotlin)은 기본적으로 이것이 가능하나, 그래서 실제적으로 우리는 추가적인 `open` 키워드 없이도 자바처럼 코틀린 Bean들을 작성할수 있다.

### 1.8.2. Using immutable class instances for persistence(영속성을 위한 불변 클래스 인스턴스들의 사용)

In Kotlin, it is very convenient and considered best practice to declare read-only properties within the primary constructor, as in the following example:

코틀린에서는, 다음 예시처럼 primary 생성자 내에서 읽기전용 속성들을 선언하는것은 아주 간결하고 , 아주 좋은 방법이다.

    class Person(val name: String, val age: Int)

You can optionally add [the `data` keyword](https://kotlinlang.org/docs/reference/data-classes.html) to make the compiler automatically derive the following members from all properties declared in the primary constructor:

우리는 선택적으로 `data` 키워드를 primary 생성자에서 선언된 모든 속성들을 다음과 같은 멤버들을 얻을수 있도록 컴파일 하도록 추가할 수 있다.

*   equals()/hashCode() pair
    
*   toString() of the form "User(name=John, age=42)"
    
*   componentN() functions corresponding to the properties in their order of declaration
    
*   copy() function
    

This allows for easy changes to individual properties even if `Person` properties are read-only:
만약 `Person` 의 속성을 읽기전용으로 한다면 개개의 속성들을 변경을 쉽게 하도록 한다.

    data class Person(val name: String, val age: Int)
    
    val jack = Person(name = "Jack", age = 1)
    val olderJack = jack.copy(age = 2)

Common persistence technologies such as JPA require a default constructor, preventing this kind of design. Fortunately, there is now a workaround for this ["default constructor hell"](https://stackoverflow.com/questions/32038177/kotlin-with-jpa-default-constructor-hell) since Kotlin provides a [kotlin-jpa](https://kotlinlang.org/docs/reference/compiler-plugins.html#kotlin-jpa-compiler-plugin) plugin which generates synthetic no-arg constructor for classes annotated with JPA annotations.

JPA 같은 공통 영속성 기술들은 기본 생성자를 이러한 종류의 설계를 막기 위해서 필요로한다.운 좋게도, 현재 "기본 생성자 지옥"을 위한 대안이 있습니다. 그 대안인 kotlin-jpa 는 JPA 어노테이션을 가진 모든 클래스의 문법적인 매개변수 없는 생성자를 생성하는 라이브러리이다.

If you need to leverage this kind of mechanism for other persistence technologies, you can configure the [kotlin-noarg](https://kotlinlang.org/docs/reference/compiler-plugins.html#how-to-use-no-arg-plugin) plugin.

만약 당신이 또다른 영속성 기술들의 이러한 메커니즘에 영향을 주는 것이 필요하다면, 당신은 `kotlin-noarg` 플러그인으로 설정가능한다.

Note

As of the Kay release train, Spring Data supports Kotlin immutable class instances and does not require the `kotlin-noarg` plugin if the module leverages Spring Data object mappings (like with MongoDB, Redis, Cassandra, etc).
Spring Data 는 코틀린의 불변 객체 인스턴스를 지원하고,만약 모듈이 Spring Data 맵핑(MongoDB, Redis, Cassandra 등과 같이 )이라면  `kotlin-noarg` 플러그인을 필요로 하지 않는다.


### 1.8.3. Injecting dependencies(의존성 주입하기)

Our recommendation is to try and favor constructor injection with `val` read-only (and non-nullable when possible) [properties](https://kotlinlang.org/docs/reference/properties.html).

우리의 권고는 `val` 과 같은 읽기 전용 속성 생성자 주입으로 할것을 더 선호한다.

    @Component
    class YourBean(
    	private val mongoTemplate: MongoTemplate,
    	private val solrClient: SolrClient
    )

Note

As of Spring Framework 4.3, classes with a single constructor have their parameters automatically autowired, that’s why there is no need for an explicit `@Autowired constructor` in the example shown above.

Spring Framework 4.3에서는 , 하나의 생성자를 가진 모든 클래스들은 자동적으로 autowired 되고 , 이러 이유로 명시적으로 위에 보는 것과 같이 `@Autowired constructor` 할 필요는 없다. 


If one really needs to use field injection, use the `lateinit var` construct, i.e.,

예를들면, 만약 필드 주입을 사용하는 것이 정말 필요하다면 , `lateinit var` 생성자를 구성하라.

    @Component
    class YourBean {
    
    	@Autowired
    	lateinit var mongoTemplate: MongoTemplate
    
    	@Autowired
    	lateinit var solrClient: SolrClient
    }

### 1.8.4. Injecting configuration properties(설정 속성들 주입하기)

In Java, one can inject configuration properties using annotations like `@Value("${property}")`, however in Kotlin `$` is a reserved character that is used for [string interpolation](https://kotlinlang.org/docs/reference/idioms.html#string-interpolation).

자바에서는, `@Value("${property}")` 와 같은 어노테이션을 사용하여 설정 속성들을 주입할수 있지만, 코틀린에서 `$`은  [string interpolation] 위해서 사용하는 예약어이다.

Therefore, if one wishes to use the `@Value` annotation in Kotlin, the `$` character will need to be escaped by writing `@Value("\${property}")`.

그러므로 , 만약 `@Value` 어노테이션을 사용하길 원한다면, `$` 문자는 `@Value("\${property}")` 에 쓰이도록 이스케이스 처리되어야 한다.

As an alternative, it is possible to customize the properties placeholder prefix by declaring the following configuration beans:


대체적으로, 다음 bean들에서 선선된 속성의 접두어 지정은 커스텀마이징이 가능하다.

    @Bean
    fun propertyConfigurer() = PropertySourcesPlaceholderConfigurer().apply {
    	setPlaceholderPrefix("%{")
    }

Existing code (like Spring Boot actuators or `@LocalServerPort`) that uses the `${…​}` syntax, can be customised with configuration beans, like as follows:

`${…​}` 가 문법이 존재하는 코드 (Spring Boot actuators 또는 @LocalServerPort 처럼)는 다음과 같이 설정 Beans 을 가지고 커스터마이징이 가능하다.

    @Bean
    fun kotlinPropertyConfigurer() = PropertySourcesPlaceholderConfigurer().apply {
    	setPlaceholderPrefix("%{")
    	setIgnoreUnresolvablePlaceholders(true)
    }
    
    @Bean
    fun defaultPropertyConfigurer() = PropertySourcesPlaceholderConfigurer()

Note

If Spring Boot is being used, then [`@ConfigurationProperties`](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-typesafe-configuration-properties) instead of `@Value` annotations can be used, but currently this only works with nullable `var` properties (which is far from ideal) since immutable classes initialized by constructors are not yet supported. See these issues about [`@ConfigurationProperties` binding for immutable POJOs](https://github.com/spring-projects/spring-boot/issues/8762) and [`@ConfigurationProperties` binding on interfaces](https://github.com/spring-projects/spring-boot/issues/1254) for more details.


스프링 부트에서 사용하게된다면 ,  `@Value` 어노테이션 대신에 `@ConfigurationProperties` 으로 사용할수 있고 , 생성자에 의해서 초기화 되는 immutable  클래스들은 아직 지원되지 않기 때문에 null 가능한  `var` 속성들을 가지고 현재 동작하고 있다. 이 이슈는 [`@ConfigurationProperties` immutable POJOs를 위한 바인딩](https://github.com/spring-projects/spring-boot/issues/8762) 그리고 [`@ConfigurationProperties` 의 인터페이스 바인딩](https://github.com/spring-projects/spring-boot/issues/1254) 에서 볼수 있다.

### 1.8.5. Annotation array attributes[어노테이션 배열 속성들]

Kotlin annotations are mostly similar to Java ones, but array attributes - which are extensively used in Spring - behave differently. As explained in [Kotlin documentation](https://kotlinlang.org/docs/reference/annotations.html) unlike other attributes, the `value` attribute name can be omitted and when it is an array attribute it is specified as a `vararg` parameter.

코틀린 어노테이션은 주로 자바처럼 유사하기도 하지만 속성은 배열이다. 이것은 

To understand what that means, let’s take `@RequestMapping`, which is one of the most widely used Spring annotations as an example. This Java annotation is declared as:

이것이 무엇을 의미하는지를 이해하지 위해서  스프링 어노테이션으로 널리 사용되는 `@RequestMapping` 을 가지고 예시를 들어보자. 이것은 자바 어노테이션으로 선언되었다: 

    public @interface RequestMapping {
    
    	@AliasFor("path")
    	String[] value() default {};
    
    	@AliasFor("value")
    	String[] path() default {};
    
    	RequestMethod[] method() default {};
    
    	// ...
    }

The typical use case for `@RequestMapping` is to map a handler method to a specific path and method. In Java, it is possible to specify a single value for the annotation array attribute and it will be automatically converted to an array.

That’s why one can write `@RequestMapping(value = "/foo", method = RequestMethod.GET)` or `@RequestMapping(path = "/foo", method = RequestMethod.GET)`.

However, in Kotlin, one will have to write `@RequestMapping("/foo", method = arrayOf(RequestMethod.GET))`. The variant using `path` is not recommended as it need to be written `@RequestMapping(path = arrayOf("/foo"), method = arrayOf(RequestMethod.GET))`.

A workaround for this specific `method` attribute (the most common one) is to use a shortcut annotation such as `@GetMapping` or `@PostMapping`, etc.

Note

Reminder: If the `@RequestMapping` `method` attribute is not specified, all HTTP methods will be matched, not only the `GET` methods.

Improving the syntax and consistency of Kotlin annotation array attributes is discussed in [this Kotlin language design issue](https://youtrack.jetbrains.com/issue/KT-11235).

### 1.8.6. Testing

#### Per class lifecycle

Kotlin allows one to specify meaningful test function names between backticks, and as of JUnit 5 Kotlin test classes can use the `@TestInstance(TestInstance.Lifecycle.PER_CLASS)` annotation to enable a single instantiation of test classes which allows the use of `@BeforeAll` and `@AfterAll` annotations on non-static methods, which is a good fit for Kotlin.

It is now possible to change the default behavior to `PER_CLASS` thanks to a `junit-platform.properties` file with a `junit.jupiter.testinstance.lifecycle.default = per_class` property.

    class IntegrationTests {
    
      val application = Application(8181)
      val client = WebClient.create("http://localhost:8181")
    
      @BeforeAll
      fun beforeAll() {
        application.start()
      }
    
      @Test
      fun `Find all users on HTML page`() {
        client.get().uri("/users")
            .accept(TEXT_HTML)
            .retrieve()
            .bodyToMono<String>()
            .test()
            .expectNextMatches { it.contains("Foo") }
            .verifyComplete()
      }
    
      @AfterAll
      fun afterAll() {
        application.stop()
      }
    }

#### Specification-like tests

It is possible to create specification-like tests with JUnit 5 and Kotlin.

    class SpecificationLikeTests {
    
      @Nested
      @DisplayName("a calculator")
      inner class Calculator {
         val calculator = SampleCalculator()
    
         @Test
         fun `should return the result of adding the first number to the second number`() {
            val sum = calculator.sum(2, 4)
            assertEquals(6, sum)
         }
    
         @Test
         fun `should return the result of subtracting the second number from the first number`() {
            val subtract = calculator.subtract(4, 2)
            assertEquals(2, subtract)
         }
      }
    }

#### `WebTestClient` type inference issue in Kotlin

`WebTestClient` is not usable yet in Kotlin due to a [type inference issue](https://youtrack.jetbrains.com/issue/KT-5464) which is expected to be fixed as of Kotlin 1.3. You can watch [SPR-16057](https://jira.spring.io/browse/SPR-16057) for up-to-date information. Meanwhile, the proposed alternative is to use directly `WebClient` with its Reactor and Spring Kotlin extensions to perform integration tests on an embedded WebFlux server.

## 1.9. Getting started
---------------

### 1.9.1. start.spring.io

The easiest way to start a new Spring Framework 5 project in Kotlin is to create a new Spring Boot 2 project on [start.spring.io](https://start.spring.io/#!language=kotlin).

It is also possible to create a standalone WebFlux project as described in [this blog post](https://spring.io/blog/2017/08/01/spring-framework-5-kotlin-apis-the-functional-way).

### 1.9.2. Choosing the web flavor

Spring Framework now comes with 2 different web stacks: [Spring MVC](web.html#mvc) and [Spring WebFlux](web-reactive.html#spring-web-reactive).

Spring WebFlux is recommended if one wants to create applications that will deal with latency, long-lived connections, streaming scenarios or simply if one wants to use the web functional Kotlin DSL.

For other use cases, especially if you are using blocking technologies like JPA, Spring MVC and its annotation-based programming model is a perfectly valid and fully supported choice.

## 1.10. Resources
---------

*   [Kotlin language reference](http://kotlinlang.org/docs/reference/)
    
*   [Kotlin Slack](http://slack.kotlinlang.org/) (with a dedicated #spring channel)
    
*   [Try Kotlin in your browser](https://try.kotlinlang.org/)
    
*   [Kotlin blog](https://blog.jetbrains.com/kotlin/)
    
*   [Awesome Kotlin](https://kotlin.link/)
    

### 1.10.1. Blog posts

*   [Developing Spring Boot applications with Kotlin](https://spring.io/blog/2016/02/15/developing-spring-boot-applications-with-kotlin)
    
*   [A Geospatial Messenger with Kotlin, Spring Boot and PostgreSQL](https://spring.io/blog/2016/03/20/a-geospatial-messenger-with-kotlin-spring-boot-and-postgresql)
    
*   [Introducing Kotlin support in Spring Framework 5.0](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0)
    
*   [Spring Framework 5 Kotlin APIs, the functional way](https://spring.io/blog/2017/08/01/spring-framework-5-kotlin-apis-the-functional-way)
    

### 1.10.2. Examples

*   [spring-boot-kotlin-demo](https://github.com/sdeleuze/spring-boot-kotlin-demo): regular Spring Boot + Spring Data JPA project
    
*   [mixit](https://github.com/mixitconf/mixit): Spring Boot 2 + WebFlux + Reactive Spring Data MongoDB
    
*   [spring-kotlin-functional](https://github.com/sdeleuze/spring-kotlin-functional): standalone WebFlux + functional bean definition DSL
    
*   [spring-kotlin-fullstack](https://github.com/sdeleuze/spring-kotlin-fullstack): WebFlux Kotlin fullstack example with Kotlin2js for frontend instead of JavaScript or TypeScript
    
*   [spring-petclinic-kotlin](https://github.com/spring-petclinic/spring-petclinic-kotlin): Kotlin version of the Spring PetClinic Sample Application
    

### 1.10.3. Tutorials

*   [Creating a RESTful Web Service with Spring Boot](https://kotlinlang.org/docs/tutorials/spring-boot-restful.html)
    

### 1.10.4. Issues

Here is a list of pending issues related to Spring + Kotlin support.

#### Spring Framework

*   [Unable to use WebTestClient with mock server in Kotlin](https://jira.spring.io/browse/SPR-16057)
    
*   [Support null-safety at generics, varargs and array elements level](https://jira.spring.io/browse/SPR-15942)
    
*   [Add support for Kotlin coroutines](https://jira.spring.io/browse/SPR-15413)
    

#### Spring Boot

*   [Improve Kotlin support](https://github.com/spring-projects/spring-boot/issues/5537)
    
*   [Allow `@ConfigurationProperties` binding for immutable POJOs](https://github.com/spring-projects/spring-boot/issues/8762)
    
*   [Allow `@ConfigurationProperties` binding on interfaces](https://github.com/spring-projects/spring-boot/issues/1254)
    
*   [Provide support for Kotlin KClass parameter in `SpringApplication.run()`](https://github.com/spring-projects/spring-boot/issues/8511)
    
*   [Expose the functional bean registration API via `SpringApplication`](https://github.com/spring-projects/spring-boot/issues/8115)
    

#### Kotlin

*   [Better generics null-safety support](https://github.com/Kotlin/KEEP/issues/79)
    
*   [Parent issue for Spring Framework support](https://youtrack.jetbrains.com/issue/KT-6380)
    
*   [Smart cast regression with open classes](https://youtrack.jetbrains.com/issue/KT-20283)
    
*   [JSR-223 application classpath not available when using Java 9](https://youtrack.jetbrains.com/issue/KT-18833)
    
*   [Support "::foo" as a short-hand syntax for bound callable reference to "this::foo"](https://youtrack.jetbrains.com/issue/KT-15667)
    
*   [Allow specifying array annotation attribute single value without arrayOf()](https://youtrack.jetbrains.com/issue/KT-11235)
    
*   [Kotlin requires type inference where Java doesn’t](https://youtrack.jetbrains.com/issue/KT-5464)
    
*   [Impossible to pass not all SAM argument as function](https://youtrack.jetbrains.com/issue/KT-14984)
    
*   [Apply JSR 305 meta-annotations to generic type parameters](https://youtrack.jetbrains.com/issue/KT-19592)
    
*   [Provide a way for libraries to avoid mixing Kotlin 1.0 and 1.1 dependencies](https://youtrack.jetbrains.com/issue/KT-18398)
    
*   [Support JSR 223 bindings directly via script variables](https://youtrack.jetbrains.com/issue/KT-15125)
    
*   [Kotlin Runtime library warning with kotlin-script-util dependency](https://youtrack.jetbrains.com/issue/KT-16780)
    
*   [Move incremental compilation message from Gradle’s warning to info logging level](https://youtrack.jetbrains.com/issue/KT-18765)
    
*   [Support all-open and no-arg compiler plugins in Kotlin Eclipse plugin](https://youtrack.jetbrains.com/issue/KT-15467)