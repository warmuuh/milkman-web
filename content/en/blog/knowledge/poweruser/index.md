---
title: "Poweruser Tips For Milkman"
linkTitle: "Poweruser Tips For Milkman"
date: 2024-06-07
description: >
  Use the full power of Milkman with these tips and tricks

---

This post should be a compilation of more complicated use-cases that fully uses the possibilities provided by Milkman. It should be a collection of tips and tricks that are not immediately obvious, but can be very useful in certain situations.

# AWS

As Milkman can interact with Sql and NoSql databases, it can also interact with AWS services. This is done by using the AWS SDK for Java. To use the SDK, you need to add the SDK to your project. See following sections for examples.

## AWS RDS

AWS RDS is just a SQL database in the cloud. To connect to it, you need to add the AWS SDK to your installation. As Milkman is using JDBC for sql connections, you can use [AWS Advanced JDBC Wrapper](https://github.com/aws/aws-advanced-jdbc-wrapper) to your `/plugin` directory, additionally to your usual JDBC driver (like postgres or mysql jdbc conncetor) and use it like this:

```
jdbc:aws-wrapper:postgresql://database-pg-name.cluster-XYZ.us-east-2.rds.amazonaws.com:5432/connectionSample
```

Another option to connect to (access-restricted) RDS instances is to use a SSH tunnel. See [SSL Jump Hosts](#ssl-jump-hosts) for more information.

See [AWS SSO](#aws-sso) for how to authenticate with AWS via SSO.

## AWS DynamoDB

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

## AWS SSO
To support authentification via AWS SSO, simply drop the [SSO Artifact](https://mvnrepository.com/artifact/software.amazon.awssdk/sso) into your `/plugin` directory. Then you can use the following code to authenticate:

```bash
# in terminal:
aws sso login --profile my_profile
```

All aws-related functionality in milkman will pick up the active login and use it for opening connections.


# Jump Hosts via Socks Proxy

For connecting to databases that are not directly accessible from your machine, you can use a SSH tunnel. Usually, you need to manually open a tunnel via `ssh` and use the tunnel as a proxy (socks-proxy) for your connection. Milkman can do this in a best-effort way, as some drivers are natively supporting this. It is currently supported for:

* Jdbc Connections: appending `socksProxyPort`, `socksProxyHost` and `socksProxyRemoteDns` to the connection string
* Cassandra
* Others: system properties `socksProxyPort` and `socksProxyPort` are set. this is respected by some drivers and http clients

# Jump Hosts in JDBC Connections

If you use jdbc, an easier method is to [jdbc-sshj](https://github.com/bokysan/jdbc-sshj) which opens a tunnel when needed. The JDBC-string then looks a bit more complicated:

```yaml
# connecting to an aws rds instance via a jump host
# {{host}} and {{port}} are placeholders that are replaced by the jdbc-sshj driver
jdbc:sshj-native://<user>@<jump-host>?remote=<aws-endpoint>:5432;;;jdbc:postgresql://{{host}}:{{port}}/<database>?user=<user>&password=<password>
```

# Client Certificates / mTLS
# Tests as Request-Sequence
# Scripting Tips&Tricks
## Printing JWT to Console
## Custom Authentication
# External Files
# Libraries
# Custom Templates
