---
title: "Poweruser Tips For Milkman"
linkTitle: "Poweruser Tips For Milkman"
date: 2024-06-07
description: >
  Use the full power of Milkman with these tips and tricks

---

This post should be a compilation of more complicated use-cases that fully uses the possibilities provided by Milkman. It should be a collection of tips and tricks that are not immediately obvious, but can be very useful in certain situations.

## AWS

As Milkman can interact with Sql and NoSql databases, it can also interact with AWS services. This is done by using the AWS SDK for Java. To use the SDK, you need to add the SDK to your project. See following sections for examples.

### AWS RDS

AWS RDS is just a SQL database in the cloud. To connect to it, you need to add the AWS SDK to your installation. As Milkman is using JDBC for sql connections, you can use [AWS Advanced JDBC Wrapper](https://github.com/aws/aws-advanced-jdbc-wrapper) to your `/plugin` directory, additionally to your usual JDBC driver (like postgres or mysql jdbc conncetor) and use it like this:

```
jdbc:aws-wrapper:postgresql://database-pg-name.cluster-XYZ.us-east-2.rds.amazonaws.com:5432/connectionSample
```

Another option to connect to (access-restricted) RDS instances is to use a SSH tunnel. See [SSL Jump Hosts](#jump-hosts-in-jdbc-connections) for more information.

See [AWS SSO](#aws-sso) for how to authenticate with AWS via SSO.

### AWS DynamoDB

AWS DynamoDB is a NoSQL database in the cloud. To connect to it, you need to add the AWS SDK to your installation. As Milkman is using [JNoSql](https://jnosql.org/) for NoSQL connections, you can use the [jnosql-dynamodb-document Driver](https://github.com/warmuuh/jnosql-dynamodb-document) to your `/plugin` directory and use it like this:

```yaml
#Parameters
jnosql.document.provider: com.github.warmuuh.jnosql.dynamodb.DynamoDBDocumentConfiguration
jnosql.dynamodb.region: eu-central-1
jnosql.dynamodb.profile: my_profile
jnosql.dynamodb.prefix: some-prefix-
jnosql.dynamodb.selectmode: scan
```

See [AWS SSO](#aws-sso) for how to authenticate with AWS via SSO.

### AWS SSO
To support authentification via AWS SSO, simply drop the [SSO Artifact](https://mvnrepository.com/artifact/software.amazon.awssdk/sso) into your `/plugin` directory. Then you can use the following code to authenticate:

```bash
# in terminal:
aws sso login --profile my_profile
```

All aws-related functionality in milkman will pick up the active login and use it for opening connections.


## Jump Hosts via Socks Proxy

For connecting to databases that are not directly accessible from your machine, you can use a SSH tunnel. Usually, you need to manually open a tunnel via `ssh` and use the tunnel as a proxy (socks-proxy) for your connection. Milkman can do this in a best-effort way, as some drivers are natively supporting this. It is currently supported for:

* Jdbc Connections: appending `socksProxyPort`, `socksProxyHost` and `socksProxyRemoteDns` to the connection string
* Cassandra
* Others: system properties `socksProxyPort` and `socksProxyPort` are set. this is respected by some drivers and http clients

## Jump Hosts in JDBC Connections

If you use jdbc, an easier method is to [jdbc-sshj](https://github.com/bokysan/jdbc-sshj) which opens a tunnel when needed. The JDBC-string then looks a bit more complicated:

```yaml
# connecting to an aws rds instance via a jump host
# {{host}} and {{port}} are placeholders that are replaced by the jdbc-sshj driver
jdbc:sshj-native://<user>@<jump-host>?remote=<aws-endpoint>:5432;;;jdbc:postgresql://{{host}}:{{port}}/<database>?user=<user>&password=<password>
```

## Client Certificates / mTLS

With rising security requirements, more and more services require client certificates for authentication. Milkman can handle this by adding the client certificate to the request. To do this, you need to add the client certificate to the list of known certificates in Milkman preferences. After that, you can select the certificate in the request settings (cog-wheel next to the http request method).

## Tests as Request-Sequence
`Test` requests can be used to create a sequence of requests that are executed in order. This can be used to handle a more complex scenario with multiple dependent requests. To use data of one request in the next one, you can use `scripting` component.

These requests are running in isolation, so any change to environment variables (if not specified differently in the properties of the Test) are not propagated to the current active environment.

### Example:

Request 1: e.g. a request that creates a user and returns it in response body
```javascript
var user = mm.response.body.body;
mm.setEnvironmentVariable("user", user)
console.log("user: " + user)
```

Request 2: use the user to login and get a token, using the user from the previous request

```javascript
//request body
grant_type=password
&client_id=...
&client_secret=...
&scope=openid
&username={{js:escape("{{user}}")}}
&password=password

// script to extract the token:
var body = JSON.parse(mm.response.body.body)
milkman.setEnvironmentVariable("user-token", body.access_token)
milkman.setEnvironmentVariable("user-rt", body.refresh_token)
```

## Scripting Tips&Tricks

Because you can load external javascript files that can then be used in the scripting part, the scripting component is way more powerful than without using that capability.

### Using Chai.js

```javascript
// preload script: https://cdnjs.cloudflare.com/ajax/libs/chai/4.3.4/chai.min.js
// then you can use chai.js in your scripts:

chai.should();
var body = JSON.parse(milkman.response.body.body)
body.id.should.equal(mm.getEnvironmentVariable("userId"));


var headers = milkman.response.headers.entries;
var tokenHeader = null;
for (var i = 0; i < headers.length; i++) {
    if (headers[i].name === 'x-token')
      tokenHeader = headers[i];
}


should.not.equal(tokenHeader, null);
tokenHeader.value.should.match(/^0.*$/) //is it according to new format?
```

### Using external files

Sometimes, you might want to use content from external files in your requests, such as a csv-file loaded from external. Out-of-the-box, Milkman does not support this, but you can use the following workaround:

```javascript
//in an external preload script, e.g. "~/.milkman-scripts/loadFile.js"
function loadFile(path) {
 var pathObj = java.nio.file.Paths.get(path)
 var bytesObj = java.nio.file.Files.readAllBytes(pathObj);
 var bytes = Java.from(bytesObj) // converting to JavaScript
 return String.fromCharCode.apply(null, bytes)
}
//then using that as preload script in milkman: file:///Users/.../.milkman-scripts/loadfile.js

//and for example in the request body, use it like this:
{
  "csvFile": "{{js:base64(loadFile('/path/to/test.csv'))}}"
}
```

### Printing to Console
Sometimes, scripting can be used to add debugging information to your requests, for example, you can print the information of a JWT token to console:

```javascript
// using pre-load script: https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.0.0/crypto-js.min.js

// using custom pre-load script:

function parseJwt (token) {
    try {
		var words = CryptoJS.enc.Base64.parse(token.split('.')[1]);
		var textString = CryptoJS.enc.Utf8.stringify(words);
      return JSON.parse(textString);
    } catch (e) {
      return null;
    }
};

// in scripting of the request:

var body = JSON.parse(mm.response.body.body)
console.log(JSON.stringify(parseJwt(body.access_token), null, 2))
```


## Libraries

In lot of companies, many services exist and sometimes, a directory with all service specifications exist. Milkman can make use of these directories (currently, only apis.guru format is supported, open tickets to request more formats) to import service definitions. This allows you to import your company's services and use them in your requests with just a few clicks.

## Custom Templates

If you want to export requests in a specific format, you can create custom templates. These templates are written in [mustache](https://mustache.github.io/) with whitespace control extensions. Some examples of http exports:

### WGet

Useful because some alpine docker images dont have curl installed but busybox wget

```mustache
wget -O- {{#headers.entries-}}{{#enabled-}}
--header="{{&name}}: {{&value}}"
{{_/enabled}}{{/headers.entries_}}
{{&url}}
```

### Spring Rest Template

If you program and need to implement the request in java / spring

```mustache
RestTemplate restTemplate = new RestTemplate();

Headers headers = new Headers();
{{#headers.entries}}{{#enabled-}}
headers.add("{{&name}}", "{{&value}}");
{{/enabled}}{{/headers.entries-}}
{{#body.body-}}
String body = "{{&.}}";
{{-/body.body}}
HttpEntity<String> request = new HttpEntity<>(
{{-#body.body-}}
body,
{{-/body.body-}}
headers);
ResponseEntity<String> response = restTemplate
  .exchange("{{url}}", HttpMethod.{{httpMethod}}, request, String.class);

String responseBody = response.getBody();
```
