# RHBK Bulk Load
Questo progetto è progettato per facilitare la creazione di utenti in massa in Red Hat Build of Keycloak (RHBK), consentendo test di prestazioni estensive del sistema.

## Importante
Questo progetto non è affiliato a Red Hat e non è un'iniziativa ufficiale. È un'iniziativa personale progettata per aiutarmi a testare RHBK. Usa questo progetto a tuo rischio. Non è consigliato per l'uso in un ambiente di produzione.


## Requisiti
* OpenJDK Runtime Environment (Red_Hat-21.0.2.0.13-1) (build 21.0.2+13-LTS)
* Apache Maven 3.9.6 (Red Hat 3.9.6-6)
* podman version 5.0.3
* OpenShift 4.14 
* RHBK 24-12
* oc client
* Suported databases:
    * PostgreSQL
    * MySQL
    * MariaDB
    * Oracle
    * Microsoft SQL Server

## Sommario
* [Compilando e Executando](#compilando-e-executando)
    * [Compilando o código fonte](#compilando-o-código-fonte)
    * [Construindo a imagem do contêiner](#construindo-a-imagem-do-contêiner)
    * [Executando o contêiner](#executando-o-contêiner)
* [Executando o contêiner utilizando podman](#executando-o-contêiner-utilizando-podman)
* [Executando o contêiner utilizando OpenShift](#executando-o-contêiner-utilizando-openshift)


## Compilare ed Eseguire
Questo progetto è architettato per funzionare all'interno di un contenitore. Le seguenti istruzioni ti guideranno attraverso il processo di compilazione del codice sorgente, costruzione di un'immagine del contenitore ed esecuzione del contenitore. Se preferisci, puoi prendere l'immagine da quay.io e saltare i passaggi di compilazione del codice sorgente e costruzione dell'immagine del contenitore.


### Compilazione del codice sorgente
Ora che hai gli strumenti necessari installati, puoi compilare il codice sorgente. Per farlo, esegui il seguente comando:
```shell
$ mvn clean package -Pnative
```
```console
[INFO] Scanning for projects...
[WARNING] 
[WARNING] Some problems were encountered while building the effective model for org.acme:rhbk-bulk-load:jar:1.0.0-SNAPSHOT
[WARNING] 'dependencies.dependency.(groupId:artifactId:type:classifier)' must be unique: io.quarkus:quarkus-arc:jar -> duplicate declaration of version (?) @ line 81, column 21
[WARNING] 
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING] 
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
[WARNING] 
[INFO] 
[INFO] ----------------------< org.acme:rhbk-bulk-load >-----------------------
[INFO] Building rhbk-bulk-load 1.0.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[WARNING] The artifact io.quarkus:quarkus-resteasy-reactive:jar:3.12.3 has been relocated to io.quarkus:quarkus-rest:jar:3.12.3: Update the artifactId in your project build file. Refer to https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.9 for more information.
[INFO] 
[INFO] --- clean:3.2.0:clean (default-clean) @ rhbk-bulk-load ---
[INFO] Deleting /home/parraes/rhbk-bulk-load/target
[INFO] 
[INFO] --- resources:3.3.1:resources (default-resources) @ rhbk-bulk-load ---
[INFO] Copying 1 resource from src/main/resources to target/classes
[INFO] 
[INFO] --- quarkus:3.12.3:generate-code (default) @ rhbk-bulk-load ---
[INFO] 
[INFO] --- compiler:3.13.0:compile (default-compile) @ rhbk-bulk-load ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 3 source files with javac [debug release 21] to target/classes
[INFO] 
[INFO] --- quarkus:3.12.3:generate-code-tests (default) @ rhbk-bulk-load ---
[INFO] 
[INFO] --- resources:3.3.1:testResources (default-testResources) @ rhbk-bulk-load ---
[INFO] skip non existing resourceDirectory /home/parraes/rhbk-bulk-load/src/test/resources
[INFO] 
[INFO] --- compiler:3.13.0:testCompile (default-testCompile) @ rhbk-bulk-load ---
[INFO] No sources to compile
[INFO] 
[INFO] --- surefire:3.2.5:test (default-test) @ rhbk-bulk-load ---
[INFO] No tests to run.
[INFO] 
[INFO] --- jar:3.3.0:jar (default-jar) @ rhbk-bulk-load ---
[INFO] Building jar: /home/parraes/rhbk-bulk-load/target/rhbk-bulk-load-1.0.0-SNAPSHOT.jar
[INFO] 
[INFO] --- quarkus:3.12.3:build (default) @ rhbk-bulk-load ---
[WARNING] [io.quarkus.deployment.steps.NativeImageAllowIncompleteClasspathAggregateStep] The following extensions have required native-image to allow run-time resolution of classes: {quarkus-jdbc-oracle}. This is a global requirement which might have unexpected effects on other extensions as well, and is a hint of the library needing some additional refactoring to better support GraalVM native-image. In the case of 3rd party dependencies and/or proprietary code there is not much we can do - please ask for support to your library vendor. If you incur in any problem with other Quarkus extensions, please try reproducing the problem without these extensions first.
[WARNING] [io.quarkus.deployment.pkg.steps.NativeImageBuildStep] Cannot find the `native-image` in the GRAALVM_HOME, JAVA_HOME and System PATH. Install it using `gu install native-image` Attempting to fall back to container build.
[INFO] [io.quarkus.deployment.pkg.steps.JarResultBuildStep] Building native image source jar: /home/parraes/rhbk-bulk-load/target/rhbk-bulk-load-1.0.0-SNAPSHOT-native-image-source-jar/rhbk-bulk-load-1.0.0-SNAPSHOT-runner.jar
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildStep] Building native image from /home/parraes/rhbk-bulk-load/target/rhbk-bulk-load-1.0.0-SNAPSHOT-native-image-source-jar/rhbk-bulk-load-1.0.0-SNAPSHOT-runner.jar
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildContainerRunner] Using podman to run the native image builder
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildContainerRunner] Pulling builder image 'quay.io/quarkus/ubi-quarkus-mandrel-builder-image:jdk-21'
7ddf4bd2bd1be664e1dbd1edbc0ba9934a51fb732dc528d30acec21085545c36
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildStep] Running Quarkus native-image plugin on MANDREL 23.1.4.0 JDK 21.0.4+7-LTS
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildRunner] podman run --env LANG=C --rm --user 1000:1000 --userns=keep-id -v /home/parraes/rhbk-bulk-load/target/rhbk-bulk-load-1.0.0-SNAPSHOT-native-image-source-jar:/project:z --name build-native-zIYLW quay.io/quarkus/ubi-quarkus-mandrel-builder-image:jdk-21 -J-Dsun.nio.ch.maxUpdateArraySize=100 -J-Djava.util.logging.manager=org.jboss.logmanager.LogManager -J-Dcom.mysql.cj.disableAbandonedConnectionCleanup=true -J-Dlogging.initial-configurator.min-level=500 -J-Dvertx.logger-delegate-factory-class-name=io.quarkus.vertx.core.runtime.VertxLogDelegateFactory -J-Dvertx.disableDnsResolver=true -J-Dio.netty.leakDetection.level=DISABLED -J-Dio.netty.allocator.maxOrder=3 -J-Duser.language=en -J-Duser.country=US -J-Dfile.encoding=UTF-8 --features=io.quarkus.jdbc.postgresql.runtime.graal.SQLXMLFeature,io.quarkus.runner.Feature,io.quarkus.runtime.graal.DisableLoggingFeature -J--add-exports=java.security.jgss/sun.security.krb5=ALL-UNNAMED -J--add-exports=java.security.jgss/sun.security.jgss=ALL-UNNAMED -J--add-opens=java.base/java.text=ALL-UNNAMED -J--add-opens=java.base/java.io=ALL-UNNAMED -J--add-opens=java.base/java.lang.invoke=ALL-UNNAMED -J--add-opens=java.base/java.util=ALL-UNNAMED -H:+UnlockExperimentalVMOptions -H:BuildOutputJSONFile=rhbk-bulk-load-1.0.0-SNAPSHOT-runner-build-output-stats.json -H:-UnlockExperimentalVMOptions --strict-image-heap -H:+UnlockExperimentalVMOptions -H:+AllowFoldMethods -H:-UnlockExperimentalVMOptions -J-Djava.awt.headless=true --no-fallback -H:+UnlockExperimentalVMOptions -H:+ReportExceptionStackTraces -H:-UnlockExperimentalVMOptions -H:+AddAllCharsets --enable-url-protocols=http --enable-monitoring=heapdump -H:+UnlockExperimentalVMOptions -H:-UseServiceLoaderFeature -H:-UnlockExperimentalVMOptions -J--add-exports=org.graalvm.nativeimage/org.graalvm.nativeimage.impl=ALL-UNNAMED --exclude-config com\.oracle\.database\.jdbc /META-INF/native-image/native-image\.properties --exclude-config com\.oracle\.database\.jdbc /META-INF/native-image/reflect-config\.json --exclude-config io\.netty\.netty-codec /META-INF/native-image/io\.netty/netty-codec/generated/handlers/reflect-config\.json --exclude-config io\.netty\.netty-handler /META-INF/native-image/io\.netty/netty-handler/generated/handlers/reflect-config\.json rhbk-bulk-load-1.0.0-SNAPSHOT-runner -jar rhbk-bulk-load-1.0.0-SNAPSHOT-runner.jar
========================================================================================================================
GraalVM Native Image: Generating 'rhbk-bulk-load-1.0.0-SNAPSHOT-runner' (executable)...
========================================================================================================================
For detailed information and explanations on the build output, visit:
https://github.com/oracle/graal/blob/master/docs/reference-manual/native-image/BuildOutput.md
------------------------------------------------------------------------------------------------------------------------
Warning: Feature class oracle.nativeimage.NativeImageFeature is annotated with the deprecated annotation @AutomaticFeature. Support for this annotation will be removed in a future version of GraalVM. Applications should register a feature using the option --features=oracle.nativeimage.NativeImageFeature
[1/8] Initializing...                                                                                    (5.5s @ 0.34GB)
 Java version: 21.0.4+7-LTS, vendor version: Mandrel-23.1.4.0-Final
 Graal compiler: optimization level: 2, target machine: x86-64-v3
 C compiler: gcc (redhat, x86_64, 8.5.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 5 user-specific feature(s):
 - com.oracle.svm.thirdparty.gson.GsonFeature
 - io.quarkus.jdbc.postgresql.runtime.graal.SQLXMLFeature
 - io.quarkus.runner.Feature: Auto-generated class by Quarkus from the existing extensions
 - io.quarkus.runtime.graal.DisableLoggingFeature: Disables INFO logging during the analysis phase
 - oracle.nativeimage.NativeImageFeature
------------------------------------------------------------------------------------------------------------------------
 3 experimental option(s) unlocked:
 - '-H:+AllowFoldMethods' (origin(s): command line)
 - '-H:BuildOutputJSONFile' (origin(s): command line)
 - '-H:-UseServiceLoaderFeature' (origin(s): command line)
------------------------------------------------------------------------------------------------------------------------
Build resources:
 - 26.49GB of memory (43.1% of 61.51GB system memory, determined at start)
 - 16 thread(s) (100.0% of 16 available processor(s), determined at start)
[2/8] Performing analysis...  [******]                                                                  (27.9s @ 2.02GB)
   18,968 reachable types   (89.1% of   21,291 total)
   30,507 reachable fields  (56.0% of   54,507 total)
  101,604 reachable methods (58.6% of  173,323 total)
    5,555 types,   666 fields, and 5,046 methods registered for reflection
       68 types,    89 fields, and    56 methods registered for JNI access
        4 native libraries: dl, pthread, rt, z
[3/8] Building universe...                                                                               (4.6s @ 2.32GB)
[4/8] Parsing methods...      [**]                                                                       (2.7s @ 2.57GB)
[5/8] Inlining methods...     [***]                                                                      (1.8s @ 2.79GB)
[6/8] Compiling methods...    [*****]                                                                   (24.7s @ 1.91GB)
[7/8] Layouting methods...    [***]                                                                      (5.6s @ 2.53GB)
[8/8] Creating image...       [***]                                                                      (6.7s @ 2.80GB)
  47.98MB (41.21%) for code area:    68,264 compilation units
  68.04MB (58.44%) for image heap:  669,046 objects and 236 resources
 411.73kB ( 0.35%) for other data
 116.41MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 origins of code area:                                Top 10 object types in image heap:
  13.47MB java.base                                           15.21MB byte[] for code metadata
   6.99MB m.oracle.database.jdbc.ojdbc11-23.3.0.23.09.jar      7.60MB byte[] for java.lang.String
   3.60MB java.xml                                             6.70MB char[]
   2.26MB com.mysql.mysql-connector-j-8.3.0.jar                6.43MB int[]
   1.83MB c.microsoft.sqlserver.mssql-jdbc-12.6.1.jre11.jar    4.91MB java.lang.Class
   1.78MB c.f.jackson.core.jackson-databind-2.17.2.jar         4.84MB java.lang.String
   1.57MB svm.jar (Native Image)                               2.67MB long[]
   1.48MB org.mariadb.jdbc.mariadb-java-client-3.4.0.jar       1.63MB byte[] for general heap data
   1.47MB org.postgresql.postgresql-42.7.3.jar                 1.62MB byte[] for embedded resources
   1.35MB jdk.proxy4                                           1.59MB com.oracle.svm.core.hub.DynamicHubCompanion
  11.69MB for 101 more packages                               14.84MB for 5022 more object types
------------------------------------------------------------------------------------------------------------------------
Recommendations:
 HEAP: Set max heap for improved and more predictable memory usage.
 CPU:  Enable more CPU features with '-march=native' for improved performance.
------------------------------------------------------------------------------------------------------------------------
                       7.0s (8.6% of total time) in 923 GCs | Peak RSS: 3.96GB | CPU load: 10.60
------------------------------------------------------------------------------------------------------------------------
Produced artifacts:
 /project/rhbk-bulk-load-1.0.0-SNAPSHOT-runner (executable)
 /project/rhbk-bulk-load-1.0.0-SNAPSHOT-runner-build-output-stats.json (build_info)
========================================================================================================================
Finished generating 'rhbk-bulk-load-1.0.0-SNAPSHOT-runner' in 1m 20s.
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildRunner] podman run --env LANG=C --rm --user 1000:1000 --userns=keep-id -v /home/parraes/rhbk-bulk-load/target/rhbk-bulk-load-1.0.0-SNAPSHOT-native-image-source-jar:/project:z --entrypoint /bin/bash quay.io/quarkus/ubi-quarkus-mandrel-builder-image:jdk-21 -c objcopy --strip-debug rhbk-bulk-load-1.0.0-SNAPSHOT-runner
[INFO] [io.quarkus.deployment.QuarkusAugmentor] Quarkus augmentation completed in 91365ms
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:36 min
[INFO] Finished at: 2024-07-29T16:03:58-03:00
[INFO] ------------------------------------------------------------------------
```
### Costruire un'immagine del contenitore
Ora costruiamo l'immagine del contenitore utilizzando il seguente comando:
```shell
$ podman build -f src/main/docker/Dockerfile.native -t quay.io/parraes/rhbk-bulk-load:v0.0.1 .
```
```console
STEP 1/7: FROM registry.access.redhat.com/ubi8/ubi-minimal:8.9
STEP 2/7: WORKDIR /work/
--> Using cache c3ba86097b9fdef2febaf09977d17232b32b4119e090bbc8d1bc6201e27a9184
--> c3ba86097b9f
STEP 3/7: RUN chown 1001 /work     && chmod "g+rwX" /work     && chown 1001:root /work
--> Using cache 91791add5b6124be81c1dd71aafe2b44938ac01f9fd0cd0b2540a3a046cf91f4
--> 91791add5b61
STEP 4/7: COPY --chown=1001:root target/*-runner /work/application
--> Using cache fefa8cd3c0f1676649f5093655086a1c6ffeb391d8dca95870376af0a5e5e069
--> fefa8cd3c0f1
STEP 5/7: EXPOSE 8080
--> Using cache 4223fee1ab13d15eb7a116abe1afe7571d975521eb3a4f90b8e47f63d46206bb
--> 4223fee1ab13
STEP 6/7: USER 1001
--> Using cache 04c5d3dc51bd483540085a39dee7f2e902f59d7ab41615a9effe681dde16913e
--> 04c5d3dc51bd
STEP 7/7: ENTRYPOINT ["./application", "-Dquarkus.http.host=0.0.0.0"]
--> Using cache bd13282927f95dd19714ba92e38b770529c31f8aff0d971fb18b24123b4644ef
COMMIT quay.io/parraes/rhbk-bulk-load:v0.0.1
--> bd13282927f9
Successfully tagged quay.io/parraes/rhbk-bulk-load:v0.0.1
bd13282927f95dd19714ba92e38b770529c31f8aff0d971fb18b24123b4644ef
```
### Esecuzione del contenitore
Ora eseguiamo il contenitore, puoi scegliere tra podman o OpenShift per l'esecuzione.

#### Esecuzione del contenitore utilizzando podman
```shell
$ podman run -d \
    -p 9000:9000 \
    -e CONFIG.DATASOURCE.DRIVER="<driver-name>" \
    -e CONFIG.DATASOURCE.URL="<jdbc_url>" \
    -e CONFIG.DATASOURCE.USERNAME="<db_username>" \
    -e CONFIG.DATASOURCE.PASSWORD="<db_password>" \
    -e CONFIG.KEYCLOAK.REALM="<rhbk_realm>" \
    --name rhbk-bulk-load-multi-db  \
    quay.io/parraes/rhbk-bulk-load:v0.0.1
```
* CONFIG.DATASOURCE.DRIVER è il driver del database, può essere postgres, mysql, mariadb, oracle o mssql.
* CONFIG.DATASOURCE.URL è l'URL del database, può essere jdbc:postgresql://hostname:port_number/database_name, jdbc:mysql://hostname:port_number/database_name, jdbc:mariadb://hostname:port_number/database_name, jdbc:oracle:thin:@hostname:port_number:database_name o jdbc:sqlserver://hostname:port_number;databaseName=database_name.
* CONFIG.DATASOURCE.USERNAME è il nome utente del database.
* CONFIG.DATASOURCE.PASSWORD è la password del database.
* CONFIG.KEYCLOAK.REALM è il realm del RHBK.

#### Esecuzione del contenitore utilizzando OpenShift
Ora eseguiamo il contenitore utilizzando OpenShift, puoi usare il seguente comando per creare un deployment e un servizio:
```shell
$ oc new-app --name=sso-bulk-load-multi-db \
    -e CONFIG.DATASOURCE.DRIVER="<driver-name>" \
    -e CONFIG.DATASOURCE.URL="<jdbc_url>" \
    -e CONFIG.DATASOURCE.USERNAME="<db_username>" \
    -e CONFIG.DATASOURCE.PASSWORD="<db_password>" \
    -e CONFIG.KEYCLOAK.REALM="<rhbk_realm>" \
    quay.io/parraes/rhbk-bulk-load:v0.0.1
```
* CONFIG.DATASOURCE.DRIVER è il driver del database, può essere postgres, mysql, mariadb, oracle o mssql.
* CONFIG.DATASOURCE.URL è l'URL del database, può essere jdbc:postgresql://hostname:port_number/database_name, jdbc:mysql://hostname:port_number/database_name, jdbc:mariadb://hostname:port_number/database_name, jdbc:oracle:thin:@hostname:port_number:database_name o jdbc:sqlserver://hostname:port_number;databaseName=database_name.
* CONFIG.DATASOURCE.USERNAME è il nome utente del database.
* CONFIG.DATASOURCE.PASSWORD è la password del database.
* CONFIG.KEYCLOAK.REALM è il realm del RHBK.

Esporre il servizio alla rete esterna:
```shell
oc expose svc/rhbk-bulk-load-multi-db
```

Ottieni la rotta del servizio:
```shell
oc get route rhbk-bulk-load-multi-db
```

## Utilizzo dell'API per creare utenti
Ora utilizziamo l'API per creare utenti, puoi usare il seguente comando per creare utenti:
```shell
curl -X 'POST' \
  'http://<hostname>:<port>/bulk/insert/10000' \
  -H 'accept: text/plain' \
  -d ''     
```

Questo comando creerà 10000 utenti nel RHBK.

## Utilizzo dell'API per eliminare gli utenti
Ora utilizziamo l'API per eliminare gli utenti, puoi usare il seguente comando per eliminare gli utenti:
```shell
curl -X 'DELETE' \
  'http://<hostname>:<port>bulk/delete/admin' \
  -H 'accept: text/plain'    
```
Tutti gli utenti saranno eliminati dal RHBK, ad eccezione dell'utente amministratore.
``
