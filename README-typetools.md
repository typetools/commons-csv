This is a version of Commons CSV that is annotated with type annotations for the Checker Framework.


To build this project
---------------------

```
mvn -B -Dmaven.test.skip=true package
```

This creates file
`target/commons-csv-VERSION.jar`


To update to a newer version of the upstream library
----------------------------------------------------

In the upstream repository, find the commit corresponding to a public release.

Commons CSV version 1.6 is commit 2596fdeebcab53fe459c481990bf1dec838128a5

Pull in that commit:
```
git pull https://github.com/apache/commons-csv <commitid>
```

Update the PACKAGE environment variable below.

Use latest Checker Framework version by changing `pom.xml`.


To upload to Maven Central
--------------------------

# Set a new Maven Central version number in file cfMavenCentral.xml.

PACKAGE=commons-csv-0.0.1

# Compile, and create Javadoc jar file
mvn -B -Dmaven.test.skip=true package
mvn source:jar
mvn javadoc:javadoc && (cd target/site/apidocs && jar -cf ${PACKAGE}-javadoc.jar org)

## This does not seem to work for me:
# -Dhomedir=/projects/swlab1/checker-framework/hosting-info

mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/${PACKAGE}.jar \
&& \
mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/${PACKAGE}-sources.jar -Dclassifier=sources \
&& \
mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/site/apidocs/${PACKAGE}-javadoc.jar -Dclassifier=javadoc
