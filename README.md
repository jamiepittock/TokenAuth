# Token Auth (WIP)

### Setup
```public/index.php``` needs ```header("Access-Control-Allow-Origin: *");header("Access-Control-Allow-Headers: Authorization");``` at the top. ```// TODO: make it so only API files need this```

Apache users also need to add the following to their ```public/.htaccess``` file
```
RewriteEngine On
RewriteCond %{HTTP:Authorization} ^(.*)
RewriteRule .* - [e=HTTP_AUTHORIZATION:%1]
```

### Template Tags
```{% requireJwt %}```
Template requires a valid JWT token (sent either by GET or POST variables with the key ```jwt```) to access / use.

```{% tokenAuthLogin %}```
Turn template into a login point. POSTing a ```loginName``` and ```password``` will return a JWT, or error if one occurs.
Attempting to access the template via non-POST means will result in a 400 error.

```{% tokenAuthPost %}```
Catch-all POST request handler. Routes requests according to their specified action (```act``` in the post request). Will throw a 400 error if not accessed via POST.

```{% returnJson %}```
Sets the Content-Type header to application/json.


### Example Get API
```
{% requireJwt %}
{% returnJson %}
{% set data = ['hello','world'] %}
{{ data|json_encode()|raw }}
```