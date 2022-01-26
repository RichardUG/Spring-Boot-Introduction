# Spring-Boot-Introduction

# Creación proyecto con spring

> Podemos hacer uso de la herrramienta que nos brinda la pagina web de spring boot [Spring initializr](https://start.spring.io/) para crear la aplicación, o crearlo de manera manual
>
> ![](/img/captura.PNG)

# Creación aplicación web

> Vamos a crear el archivo java ```src/main/java/com/example/springboot/HelloController.java``` y en el vamos a escribir lo siguiente

> ```java
> package com.example.springboot;
>
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.RestController;
>
> @RestController
> public class HelloController {
>
>	@GetMapping("/")
> 	public String index() {
> 		return "Greetings from Spring Boot!";
> 	}
>
> }
>```

# Crear aplicación de clase

> Vamos a crear el archivo java ```src/main/java/com/example/springboot/Application.java``` y en el vamos a escribir lo siguiente

> ```java
> package com.example.springboot;
>
> import java.util.Arrays;
> import org.springframework.boot.CommandLineRunner;
> import org.springframework.boot.SpringApplication;
> import org.springframework.boot.autoconfigure.SpringBootApplication;
> import org.springframework.context.ApplicationContext;
> import org.springframework.context.annotation.Bean;
>
> @SpringBootApplication
> public class Application {
>
> 	public static void main(String[] args) {
> 		SpringApplication.run(Application.class, args);
> 	}
>
> 	@Bean
> 	public CommandLineRunner commandLineRunner(ApplicationContext ctx) {
> 		return args -> {
>
> 			System.out.println("Let's inspect the beans provided by Spring Boot:");
> 
> 			String[] beanNames = ctx.getBeanDefinitionNames();
> 			Arrays.sort(beanNames);
> 			for (String beanName : beanNames) {
> 				System.out.println(beanName);
> 			}
>
> 		};
> 	}
>
> }
>```

# Ejecutar aplicación

> Corremos el siguiente comando ```mvnw spring-boot:run``` en nuestra terminal y obtenemos lo siguiente
>
> ```
> Let's inspect the beans provided by Spring Boot:
> application
> beanNameHandlerMapping
> defaultServletHandlerMapping
> dispatcherServlet
> embeddedServletContainerCustomizerBeanPostProcessor
> handlerExceptionResolver
> helloController
> httpRequestHandlerAdapter
> messageSource
> mvcContentNegotiationManager
> mvcConversionService
> mvcValidator
> org.springframework.boot.autoconfigure.MessageSourceAutoConfiguration
> org.springframework.boot.autoconfigure.PropertyPlaceholderAutoConfiguration
> org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration
> org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$DispatcherServletConfiguration
> org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$EmbeddedTomcat
> org.springframework.boot.autoconfigure.web.ServerPropertiesAutoConfiguration
> org.springframework.boot.context.embedded.properties.ServerProperties
> org.springframework.context.annotation.ConfigurationClassPostProcessor.enhancedConfigurationProcessor
> org.springframework.context.annotation.ConfigurationClassPostProcessor.importAwareProcessor
> org.springframework.context.annotation.internalAutowiredAnnotationProcessor
> org.springframework.context.annotation.internalCommonAnnotationProcessor
> org.springframework.context.annotation.internalConfigurationAnnotationProcessor
> org.springframework.context.annotation.internalRequiredAnnotationProcessor
> org.springframework.web.servlet.config.annotation.DelegatingWebMvcConfiguration
> propertySourcesBinder
> propertySourcesPlaceholderConfigurer
> requestMappingHandlerAdapter
> requestMappingHandlerMapping
> sourceHandlerMapping
> mpleControllerHandlerAdapter
> mcatEmbeddedServletContainerFactory
> wControllerHandlerMapping
> ```

> Ahora ejecutamos el siguiente comando ```curl localhost:8080``` para consultar hacia lo que se encuentra en localhost en el puerto 8080 en este momento, obtenemos como resultado 
> ![](/img/resultado1.PNG)

# Pruebas de unidad

> Agregamos la siguiente dependencia a nuestro pom para poder ejecutar las pruebas
>
> ```xml
> <dependency>
> 	<groupId>org.springframework.boot</groupId>
> 	<artifactId>spring-boot-starter-test</artifactId>
> 	<scope>test</scope>
> </dependency>
>``` 

> Vamos a crear el archivo java ```src/test/java/com/example/springboot/HelloControllerTest.java``` y en el vamos a escribir lo siguiente

> ```java
> package com.example.springboot;
>
> import static org.hamcrest.Matchers.equalTo;
> import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
> import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
> import org.junit.jupiter.api.Test;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
> import org.springframework.boot.test.context.SpringBootTest;
> import org.springframework.http.MediaType;
> import org.springframework.test.web.servlet.MockMvc;
> import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
> 
> @SpringBootTest
> @AutoConfigureMockMvc
> public class HelloControllerTest {
> 
> 	@Autowired
> 	private MockMvc mvc;
> 
> 	@Test
> 	public void getHello() throws Exception {
> 		mvc.perform(MockMvcRequestBuilders.get("/").accept(MediaType.APPLICATION_JSON))
> 				.andExpect(status().isOk())
> 				.andExpect(content().string(equalTo("Greetings from Spring Boot!")));
> 	}
> }
>```

> Ahora crearemos el archivo java ```src/test/java/com/example/springboot/HelloControllerIT.java``` y en el vamos a escribir lo siguiente

> ```java
> package com.example.springboot;
>
> import org.junit.jupiter.api.Test;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.boot.test.context.SpringBootTest;
> import org.springframework.boot.test.web.client.TestRestTemplate;
> import org.springframework.http.ResponseEntity;
> import static org.assertj.core.api.Assertions.assertThat;
>
> @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
> public class HelloControllerIT {
>
> 	@Autowired
> 	private TestRestTemplate template;
> 
>     @Test
>     public void getHello() throws Exception {
>         ResponseEntity<String> response = template.getForEntity("/", String.class);
>         assertThat(response.getBody()).isEqualTo("Greetings from Spring Boot!");
>     }
> }
>```

# Agregar serviscios de producción

> Agregamos la siguiente dependencia al pom para poder usar los servicios de actuator 
>
> ```
> <dependency>
> 	<groupId>org.springframework.boot</groupId>
> 	<artifactId>spring-boot-starter-actuator</artifactId>
> </dependency>
> ```

> Ahora ejecutamos ```mvnw spring-boot:run``` para correr la aplicación 
>
> ```
> management.endpoint.configprops-org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointProperties
> management.endpoint.env-org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointProperties
> management.endpoint.health-org.springframework.boot.actuate.autoconfigure.health.HealthEndpointProperties
> management.endpoint.logfile-org.springframework.boot.actuate.autoconfigure.logging.LogFileWebEndpointProperties
> management.endpoints.jmx-org.springframework.boot.actuate.autoconfigure.endpoint.jmx.JmxEndpointProperties
> management.endpoints.web-org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointProperties
> management.endpoints.web.cors-org.springframework.boot.actuate.autoconfigure.endpoint.web.CorsEndpointProperties
> management.health.diskspace-org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthIndicatorProperties
> management.info-org.springframework.boot.actuate.autoconfigure.info.InfoContributorProperties
> management.metrics-org.springframework.boot.actuate.autoconfigure.metrics.MetricsProperties
> management.metrics.export.simple-org.springframework.boot.actuate.autoconfigure.metrics.export.simple.SimpleProperties
> management.server-org.springframework.boot.actuate.autoconfigure.web.server.ManagementServerProperties
> ```
>
> Ahora ejecutamos el comando ```curl localhost:8080/actuator/health``` en nuestra terminal para saber el estado de ese puerto y obtenemos
>
> ![](/img/resultado2.PNG)
>
> Finalmente ejecutamos el comando ```curl -X POST localhost:8080/actuator/shutdown``` en nuestra terminal para terminar la conexión y obtenemos
>
> ![](/img/resultado3.PNG)

## Autor
[Richard Santiago Urrea Garcia](https://github.com/RichardUG)

## Licencia & Derechos de Autor
**©** Richard Santiago Urrea Garcia, Estudiante de Ingeniería de Sistemas de la Escuela Colombiana de Ingeniería Julio Garavito

Licencia bajo la [GNU General Public License](/LICENSE).
