## Reproduction of resource filtering bug in maven-war-plugin:2.4

Follow the following steps in order to reproduce a bug in version *2.4* of the **maven-war-plugin**.

1. Clone this repository and `cd` into it. Notice that I am using version *2.4* of the **maven-war-plugin** in *pom.xml*, which was the *latest* version of it at publish time.
2. `mvn clean verify`
3. Observe the reproduced bug
	- `cat target/maven-war-plugin-bug-0.0.1-SNAPSHOT/app.properties`. Only the properties that were specified in *pom.xml* itself were filtered into *app.properties*. None of the properties from *default.properties* were filtered into this file.
	- `cat target/classes/app.properties`. Notice here, though, that both the properties from `pom.xml` and `default.properties` made it into `app.properties`.
4. Edit *pom.xml* to change the version of maven-war-plugin from *2.4* to *2.3*.
5. Rerun `mvn clean verify`
6. Observe the bug no longer reproduced
	- `cat target/maven-war-plugin-bug-0.0.1-SNAPSHOT/app.properties` and `cat target/classes/app.properties` should produce the same output.

### Consequence of this bug

Without being able to filter *app.properties* correctly, I will not be able to inject my properties into my Java application (for example, through Dependency Injection frameworks like **Guice**). The version of the file in `target/classes` is not available to my classpath, but the version of the file in `target/maven-war-plugin-bug-0.0.1-SNAPSHOT` is and therefore is where my application will read the properties from.

Until this bug is fixed, I and all other people affected by this bug will be forced to use an older version of the plugin such as *2.3*.
