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
<h1>Kibana</h1>

- [Installation & Configuration](#installation--configuration)
- [Requests](#requests)

# Installation & Configuration

Download the application as a `.zip` file, extract the content and change to the `./bin` directory of the install folder. Before making the install be sure that _ElasticSearch_ is installed and running.

Start _Kibana_ with the appropriate script:

```
> .\kibana.bat
...
log   [08:12:33.038] [info][listening] Server running at http://localhost:5601
log   [08:12:34.310] [info][server][Kibana][http] http server running at http://localhost:5601
```

The Kibana client is served at the default URL [`http://localhost:5601`](http://localhost:5601)

# Requests

For sending requests to an ElasticSearch cluster's API, go to *DEV Tools*> *Console* .