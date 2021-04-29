# Explore the possibility of using JFrog as the storage back end for Prometheus and Loki

CCFS-607 A determination is made on the possibility of using JFrog as the storage solution

## Actual situation
Prometheus data is stored locally and Loki logs are stored on Google Cloud, the idea is to be **cloud agnostic**.

## Prometheus
Prometheus' official documentation includes an Integrations section that lists must of the Remote Endpoints and Storage, listed [here](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage)

## Possible Solutions
* JFrog Artifactory REST API - [link](https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API#ArtifactoryRESTAPI-DeployArtifact)

```json
PUT /libs-release-local/my/jar/1.0/jar-1.0.jar
{
    "uri": "http://localhost:8081/artifactory/libs-release-local/my/jar/1.0/jar-1.0.jar",
    "downloadUri": "http://localhost:8081/artifactory/libs-release-local/my/jar/1.0/jar-1.0.jar",
    "repo": "libs-release-local",
    "path": "/my/jar/1.0/jar-1.0.jar",
    "created": ISO8601 (yyyy-MM-dd'T'HH:mm:ss.SSSZ),
    "createdBy": "userY",
    "size": "1024", //bytes
    "mimeType": "application/java-archive",
    "checksums":
    {
        "md5" : string,
        "sha1" : string
    },
    "originalChecksums":{
        "md5" : string,
        "sha1" : string
    }
}
```
* Use an alternative technology listed in the integrations that Prometheus offers for remote endpoints and storage
* Try out [Prom-migrator](https://github.com/timescale/promscale/tree/master/cmd/prom-migrator) - tool for migrating data between remote storage systems adding the end points for deploying artifacts to the JFrog repository

## Loki
Loki's documentation on storage lists all supported stores, unfortunately, JFrog isn't one of them, the list is found in [here](https://grafana.com/docs/loki/latest/operations/storage/). There, at the moment, **Loki doesn't support JFrog as a storage service.**

### Supported stores for index:
* Single Store (botldb-shipper)
* Amazon DynamoDB
* Google Bigtable
* Apache Cassandra
* BoltDB

### Supported for the chunks:
* Amazon DynamoDB
* Google Bigtable
* Apache Cassandra
* Amazon S3
* Google Cloud Storage
* Filesystem

## Conclusion
For Prometheus, it might be possible to integrate JFrog Artifactory as a storage service, trying out its API, or trying out the Prom-migrator tool which may also be helpful for these purposes. On the other hand, for Loki, it is not possible to use JFrog as a storage solution.

## Resources
* [Prometheus integrations for endpoints and storage](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage)
* [JFrog API to deploy an artifact](https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API#ArtifactoryRESTAPI-DeployArtifact)
* [Loki Storage](https://grafana.com/docs/loki/latest/operations/storage/)

