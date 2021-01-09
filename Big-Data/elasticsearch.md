<style>
body {
  margin: auto;
  width: 50%;
  min-width: 700px;
}
h1, h2, h2 {
  color: #EF4F99;
}
.index_entry {
  font-weight: bold;
  font-size: 1.25em;
  color: hsl(332, 83%, 50%);
  margin-bottom: 0;
}
</style>
<h1>ElasticSearch</h1>

- [Installation & Configuration](#installation--configuration)
  - [Install ElasticSearch as a service](#install-elasticsearch-as-a-service)
  - [Configuration](#configuration)
- [Modules and plugins](#modules-and-plugins)
- [Glossary](#glossary)

# Installation & Configuration

## Install ElasticSearch as a service

Download the application as a `.zip` file, extract the content and change to the `./bin` directory of the install folder.

Install the service:

```
elasticsearch-7.10.0\bin> .\elasticsearch-service.bat install
Installing service      :  "elasticsearch-service-x64"
Using JAVA_HOME (64-bit):  "C:\Program Files\RedHat\java-15-openjdk-15.0.1-1"
...
The service 'elasticsearch-service-x64' has been installed.
```

Start the service:

```
elasticsearch-7.10.0\bin> .\elasticsearch-service.bat start
```

Test that _ElasticSearch_ is running:

```PowerShell
> $param = @{
   Uri = "localhost:9200/?pretty"
   Method = "GET"
}
> Invoke-RestMethod @param
```

Or simply:

```PowerShell
> Invoke-RestMethod http://localhost:9200


name         : LYN-L129
cluster_name : elasticsearch
cluster_uuid : SUTKhBHBSiudy3D-WDakaw
version      : @{number=7.10.0; build_flavor=default; build_type=zip; build_hash=51e9d6f22758d0374a0f3f5c6e8f3a7997850f96; build_date=2020-11-09T21:30:33.964949Z; build_snapshot=False; lucene_version=8.7.0; minimum_wire_compatibility_version=6.8.0;
               minimum_index_compatibility_version=6.0.0-beta1}
tagline      : You Know, for Search
```

## Configuration

The configuration files for ElasticSearch are in `./config/`. The main configuration file is `elasticsearch.yml`

As ElasticSearch is a Java application, file `jvm.options` provides settings for the Java Virtual Machine. One useful setting is the _heap size_.

ElasticSearch uses _Log4J_ as logging system. The config file of _Log4J_ is `log4j2.properties`.

The `./config/` also contains admin configuration files about _users_ and _roles_. Administration task can be carried out from **Kibana**.

# Modules and plugins

**Modules** are in the folder `./modules` of the install directory. For instance **X-Pack** features are part of the default modules.

**Plugins** are in the folder `./plugins` of the install directory. This directory is initially empty. Modules are part of ElasticSearch while plugins are for customization. Plugins can be removed but modules cannot.


# Glossary

<p class="index_entry">Cluster</p>
A _cluster_ is a collection of related nodes which stores the complete database. By default, _clusters_ are completely independent of each others. A _cluster_ is automatically created when the first node is created. A cluster exposes its own REST API.

<p class="index_entry">Index</p>
Every document is stored logically within an _index_. An _index_ is therefore a _collection_ of documents.

<p class="index_entry">Node</p>
A _node_ is an instance of ElasticSearch which stores data. Each _node_ stores a part of data which is the key for distributed data processing. Any number of _nodes_ can be started on a given machine/instance which can be useful for development purposes.

