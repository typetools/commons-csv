This is a version of Commons CSV that is annotated with type annotations for the Checker Framework.


To build this project
---------------------

```
mvn -B -Dmaven.test.skip=true package
```

This creates file
`target/commons-csv-VERSION.jar`.


To update to a newer version of the upstream library
----------------------------------------------------

At https://github.com/apache/commons-csv/releases ,
find the commit corresponding to a public release.

Commons CSV version 1.7 is commit a227a1e2fb61ff5f192cfd8099e7e6f4848d7d43

Pull in that commit:
```
git pull https://github.com/apache/commons-csv <commitid>
```

Update the PACKAGE environment variable below.

Use the latest Checker Framework version by changing `pom.xml`.


To upload to Maven Central
--------------------------

This must be done on a CSE machine, which has access to the necessary passwords.

# Set the version number:
#  * in file cfMavenCentral.xml
#  * in file pom.xml (if different from upstream)
#  * environment variable PACKAGE below

PACKAGE=commons-csv-1.6 && \
mvn -B -Dmaven.test.skip=true package && \
mvn source:jar && \
mvn javadoc:javadoc && (cd target/site/apidocs && jar -cf ${PACKAGE}-javadoc.jar org)

## This does not seem to work for me:
# -Dhomedir=/projects/swlab1/checker-framework/hosting-info

[ ! -z "$PACKAGE" ] && \
mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/${PACKAGE}.jar \
&& \
mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/${PACKAGE}-sources.jar -Dclassifier=sources \
&& \
mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=cfMavenCentral.xml -Dgpg.publicKeyring=/projects/swlab1/checker-framework/hosting-info/pubring.gpg -Dgpg.secretKeyring=/projects/swlab1/checker-framework/hosting-info/secring.gpg -Dgpg.keyname=ADF4D638 -Dgpg.passphrase="`cat /projects/swlab1/checker-framework/hosting-info/release-private.password`" -Dfile=target/site/apidocs/${PACKAGE}-javadoc.jar -Dclassifier=javadoc

# Complete the release at https://oss.sonatype.org/#stagingRepositories
