# JPA

## Descripción
Aplicación que demuestra el uso de Spring Data JPA para almacenar y recuperar objetos `Customer` en una base de datos H2 en memoria.

## Objetivo
Construir una aplicación que:
- Almacene POJOs (Plain Old Java Objects) en una base de datos relacional
- Implemente operaciones CRUD usando Spring Data JPA
- Demuestre consultas personalizadas con JPA

## Requisitos
- **Java 17** o superior
- **Maven 3.5+** o Gradle 7.5+
- **Spring Boot 3.x**
- **IDE**: IntelliJ IDEA, Eclipse, o VSCode
- **Tiempo estimado**: 15 minutos

## Estructura del Proyecto
![]()

## 🚀 Inicialización del Proyecto

### Opción 1: Spring Initializr
1. Navegar a [https://start.spring.io](https://start.spring.io)
2. Seleccionar:
   - **Project**: Maven
   - **Language**: Java
   - **Spring Boot**: 3.x
3. Agregar dependencias:
   - Spring Data JPA
   - H2 Database
4. Generar y descargar el proyecto

### Opción 2: Clonar desde GitHub
```bash
git clone https://github.com/hakki17/JPA
cd JPA
```

## Implementación

### 1. Entidad Customer
```java
@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    private String firstName;
    private String lastName;
    
    // Constructores, getters y toString
}
```

**Anotaciones importantes:**
- `@Entity`: Marca la clase como entidad JPA
- `@Id`: Define la clave primaria
- `@GeneratedValue`: ID generado automáticamente

### 2. Repositorio CustomerRepository
```java
public interface CustomerRepository extends CrudRepository<Customer, Long> {
    List<Customer> findByLastName(String lastName);
    Customer findById(long id);
}
```

**Características:**
- Extiende `CrudRepository` para operaciones CRUD básicas
- Métodos personalizados declarados por nombre
- Spring Data JPA genera la implementación automáticamente

### 3. Clase Principal
```java
@SpringBootApplication
public class AccessingDataJpaApplication {
    
    @Bean
    public CommandLineRunner demo(CustomerRepository repository) {
        return (args) -> {
            // Guardar clientes
            repository.save(new Customer("Jack", "Bauer"));
            repository.save(new Customer("Chloe", "O'Brian"));
            
            // Buscar todos
            repository.findAll().forEach(customer -> {
                log.info(customer.toString());
            });
            
            // Buscar por ID
            Customer customer = repository.findById(1L);
            
            // Buscar por apellido
            repository.findByLastName("Bauer").forEach(bauer -> {
                log.info(bauer.toString());
            });
        };
    }
}
```

## Ejecutar la Aplicación

### Con Maven:
```bash
# Ejecutar directamente
./mvnw spring-boot:run

# O construir JAR y ejecutar
./mvnw clean package
java -jar target/gs-accessing-data-jpa-0.1.0.jar
```

### Con Gradle:
```bash
# Ejecutar directamente
./gradlew bootRun

# O construir JAR y ejecutar
./gradlew build
java -jar build/libs/gs-accessing-data-jpa-0.1.0.jar
```

## Salida Esperada

![]()

## Operaciones Demostradas

1. **Create**: `repository.save(new Customer(...))`
2. **Read All**: `repository.findAll()`
3. **Read by ID**: `repository.findById(1L)`
4. **Query personalizada**: `repository.findByLastName("Bauer")`
5. **Delete**: Heredado de `CrudRepository`

## Conceptos Clave

### Spring Data JPA
- Simplifica el acceso a datos con JPA
- Genera implementaciones de repositorio automáticamente
- Reduce código boilerplate

### Anotaciones Importantes
- `@SpringBootApplication`: Configuración automática de Spring Boot
- `@Entity`: Define entidad JPA
- `@Id` y `@GeneratedValue`: Configuración de clave primaria
- `@Bean`: Define beans de Spring

### Base de Datos H2
- Base de datos en memoria
- Configuración automática con Spring Boot
- Ideal para desarrollo y pruebas

## Aprendizajes
- Configuración básica de JPA con Spring Boot
- Creación de entidades y repositorios
- Consultas derivadas del nombre del método
- Uso de CommandLineRunner para pruebas
