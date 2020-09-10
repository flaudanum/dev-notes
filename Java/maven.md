# Maven

## Common tasks

### Check for dependency updates

The `display-dependency-updates` goal will check all the dependencies used in 
your project and display a list of those dependencies with newer versions 
available. This should not require a preliminary install (*to be confirmed*).

<b>Example:</b><br/>
From within the Maven project root path, use the command:
```
$ mvn versions:display-dependency-updates
[INFO] Scanning for projects...
Downloading from central: ...
...
[INFO] The following dependencies in Dependencies have newer versions:
...
```

## Management of Java ARchives

### Exclude source content from a JAR

This can be carried with the plugin `maven-jar-plugin`. The element `<exclude>`

```xml
<configuration>
    <excludes>
        <exclude>com/domain/package/.*</exclude>
    </excludes>
</configuration>
```

## GLOSSARY

### Maven property

You can use Maven properties in a pom.xml file or in any resource that is being 
processed by the Maven Resource plugin’s filtering features. A property is 
always surrounded by `${` and `}`. For example, to reference the 
`project.version` property, one would write `${project.version}`.

There are some implicit properties available in any Maven project, these 
implicit properties are:

<dl>
<dt><code>project.*</code></dt>
<dd>
Maven Project Object Model (POM). You can use the <code>project.*</code> prefix 
to reference values in a Maven POM.
</dd>
<dt><code>settings.*</code></dt>
<dd>
Maven Settings. You use the <code>settings.*</code> prefix to reference values 
from your Maven Settings in ~/.m2/settings.xml.
</dd>
<dt><code>env.*</code></dt>
<dd>
Environment variables like PATH and M2_HOME can be referenced using the 
<code>env.*</code> prefix.
</dd>
</dl>

Any property which can be retrieved from the `System.getProperty()` method can 
be referenced as a Maven property. 

In addition to the implicit properties listed above, a Maven POM, Maven 
Settings, or a Maven Profile can define a set of arbitrary, user-defined 
properties.

More details [here](https://books.sonatype.com/mvnref-book/reference/resource-filtering-sect-properties.html).

### User property

This is the name of the *Maven property* that can be used to set a plugin 
parameter. This allows configuring a plugin from outside the `<configuration>` 
section. Note that this only works if the parameter is not specified in the 
`<configuration>`.

