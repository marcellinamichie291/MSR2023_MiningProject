:toc:

= jMolecules – Architectural abstractions for Java

A set of libraries to help developers implement domain models in distraction-free, plain old Java.

== Ideas behind jMolecules

- Explicitly express architectural concepts for easier code reading and writing.
- Keep domain-specific code free from technical dependencies. Reduce boilerplate code.
- Automatically generate documentation and validate implementation structures and your architecture.

== Goals

1. Make developers life easier.
2. Express that a piece of code (a package, class, or method) implements an architectural concept.
3. Make it easy for the human reader to determine what kind of architectural concepts a given piece of code is.
4. Allow tool integration:
  a. Augmentation of the code. (Tooling examples: ByteBuddy with Spring and JPA integrations).
  b. Check for architectural rules. (Tooling examples: jQAssistant, ArchUnit).

== Use Case: Express DDD concepts

Example: A banking domain.

=== Using the Annotation-Based Model

[source,java]
----
import org.jmolecules.ddd.annotation.*;

@Entity
class BankAccount {

    @Identity
    final IBAN iban;

    /* ... */

}

@ValueObject
class IBAN { /* ... */ }

@ValueObject
record Currency { /* ... */ }

@Repository
class Accounts { /* ... */ }
----

When we take Ubiquitous Language serious, we want names (for classes, methods, etc.) that only contain words from the domain language.
That means the titles of the building blocks should not be part of the names.
So in a banking domain we don't want `BankAccountEntity`, `CurrencyVO` or even `AccountRepository` as types.
Instead, we want `BankAccount`, `Currency` and `Accounts` – like in the example above.

Still, we want to express that a given class (or other architectural element) is a special building block; i.e. uses a design pattern.
jMolecules provide a set of standard annotations for the building blocks known from DDD.

=== Using the Type-Based Model

As an alternative to the above mentioned annotations, jMolecules also provides a set of interfaces, largely based on the ideas presented in John Sullivan's series https://scabl.blogspot.com/p/advancing-enterprise-ddd.html["Advancing Enterprise DDD"].
They allow expressing relationships between the building blocks right within the type system, so that the compiler can help to verify model correctness and the information can also be processed by Java reflection more easily.

* `Identifier` -- A type to represent types that are supposed to act as identifiers.
* `Identifiable<ID>` -- Anything that's exposing an identifier.
* `Entity<T extends AggregateRoot<T, ?>, ID> extends Identifiable<ID>` -- An entity, declaring to which `AggregateRoot` it belongs and which identifier it exposes.
* `AggregateRoot<T extends AggregateRoot<T, ID>, ID extends Identifier> extends Entity<T, ID>` -- an aggregate root being an `Entity` belonging to itself exposing a dedicated `Identifier`
* `Association<T extends AggregateRoot<T, ID>, ID extends Identifier> extends Identifiable<ID>` -- an explicit association to a target `AggregateRoot`.

This arrangement gives guidance to modeling and allows to easily verify the following rules, potentially via reflection:

* Enforced, dedicated identifier types per aggregate to avoid identifiers for different aggregates mixed up.
* `AggregateRoot` must only refer to `Entity` instances that were declared to belong to it.
* ``AggregateRoot``s and ``Entity``s must only refer to other `AggregateRoots` via `Association` instances.

For automated verification and runtime technology integration see https://github.com/xmolecules/jmolecules-integrations#jmoleculestechnology-integrations[jMolecules Integrations].

=== Available Libraries
* link:jmolecules-ddd[`jmolecules-ddd`] -- annotations and interfaces to express DDD building blocks (value objects, entities, aggregate roots etc.) in code.
* link:jmolecules-events[`jmolecules-events`] -- annotations and interfaces to express the concept of events in code.
* link:kmolecules-ddd[`kmolecules-ddd`] -- Kotlin-based flavor of `jmolecules-ddd` to mitigate Kotlin/Java interop issues for the type based model.

== Use Case: Expressing architectural concepts
jMolecules provides annotations to describe higher-level architectural concepts following the styles of Layered, Onion and Hexagonal Architecture.
They allow you to mark a package as a layer, ring, or containing ports and adapters.

[source,java]
----
import org.jmolecules.architecture.layered.*;

@DomainLayer
package org.acmebank.domain;

@ApplicationLayer
package org.acmebank.application;
----
That way, all classes in the respective package are considered to be part of the annotated layer, ring, or considered a port / adapter.

Alternatively, classes can be annotated directly:

[source,java]
----
import org.jmolecules.architecture.layered.*;

@DomainLayer
@Entity
public class BankAccount { /* ... */ }

@ApplicationLayer
@Service
public class TransferMoney { /* ... */ }
----

Currently, annotations for Layered, Onion and Hexagonal Architecture exist.

=== Available Libraries

* link:jmolecules-architecture[`jmolecules-architecture`] -- annotations to express architectural styles in code.
** link:jmolecules-architecture/jmolecules-cqrs-architecture[`jmolecules-cqrs-architecture`] -- CQRS architecture
*** `@Command`
*** `@CommandDispatcher`
*** `@CommandHandler`
*** `@QueryModel`
** link:jmolecules-architecture/jmolecules-layered-architecture[`jmolecules-layered-architecture`] -- Layered architecture
*** `@DomainLayer`
*** `@ApplicationLayer`
*** `@InfrastructureLayer`
*** `@InterfaceLayer`
** link:jmolecules-architecture/jmolecules-onion-architecture[`jmolecules-onion-architecture`] -- Onion architecture
*** **Classic**
**** `@DomainModelRing`
**** `@DomainServiceRing`
**** `@ApplicationServiceRing`
**** `@InfrastructureRing`
*** **Simplified** (does not separate domain model and services)
**** `@DomainRing`
**** `@ApplicationRing`
**** `@InfrastructureRing`
** link:jmolecules-architecture/jmolecules-hexagonal-architecture[`jmolecules-hexagonal-architecture`] -- Hexagonal architecture
*** `@Application`
*** `@(Primary|Secondary)Adapter`
*** `@(Primary|Secondary)Port`

== Use Case: Generate Technical Boilerplate Code

The jMolecules annotations and interfaces can be used to generate technical code needed to express the concept in a certain target technology.

=== Available Libraries

* https://github.com/xmolecules/jmolecules-integrations[Spring, Data JPA, Data MongoDB, Data JDBC, and Jackson integration] -- to make code using jMolecules annotations work out of the box in those technologies.

== Use Case: Verify and Document Architecture

The jMolecules concepts expressed in code can be used to verify rules that stem from the concepts' definitions and  generate documentation.

=== Available Libraries

* https://github.com/jqassistant-contrib/jqassistant-jmolecules-plugin[jQAssistant plugin] -- to verify rules applying to the different architectural styles, DDD building blocks, CQRS and events. Also creates PlantUML diagrams from the information available in the codebase.
* https://github.com/xmolecules/jmolecules-integrations/tree/main/jmolecules-archunit[ArchUnit rules] -- allow to verify relationships between DDD building blocks.
* https://github.com/odrotbohm/moduliths[Moduliths] -- supports detection of jMolecules components, DDD building blocks and events for module model and documentation purposes (see http://odrotbohm.de/2021/07/moduliths-1.1-released/[blog post] for more information).

== Installation
To use jMolecules in your project just declare a dependency to it.
Release binaries are available from the Maven central repository.

=== Maven

[source,xml]
----
<dependency>
  <groupId>org.jmolecules</groupId>
  <artifactId>jmolecules-ddd</artifactId>
  <version>1.4.0</version>
</dependency>
----

=== Gradle

[source,groovy]
----
repositories {
  mavenCentral()
}
dependencies {
  implementation("org.jmolecules:jmolecules-ddd:1.4.0")
}
----

== Developer information

=== Release instructions

* `mvn release:prepare -DscmReleaseCommitComment="$ticketId - Release version $version." -DscmDevelopmentCommitComment="$ticketId - Prepare next development iteration."`
* `mvn release:perform -Dgpg.keyname=$keyname`
