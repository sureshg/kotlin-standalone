# [Avian](https://github.com/ReadyTalk/avian) example usage in gradle build

Kotlin+java.nio hello world http server example

## Requirements

You will need the following environment to build the project:

* OpenJDK >= 1.7 (`jar`,`javac` in PATH)
* Ensure JAVA_HOME is pointing to the OpenJDK
* Build tools (`make`,`gcc`,`g++` in PATH)
* Git (`git` in PATH)
* Bash (`bash` in PATH)

## How to build

* This is what i did on a fresh CentOS VM running in [Digital Ocean](https://www.digitalocean.com/)
```bash
# CentOS Linux release 7.2.1511 (Core)
# Linux sureshg-centos-sfo1 3.10.0-327.18.2.el7.x86_64 #1 SMP Thu May 12 11:03:55 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

$ yum -y install gcc gcc-c++ zlib-devel git java-1.8.0-openjdk-devel

# Fix for jni_md.h error - http://stackoverflow.com/a/24996278/416868
$ ln -s /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.111-2.b15.el7_3.x86_64/include/linux/jni_md.h  /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.111-2.b15.el7_3.x86_64/include/jni_md.h
$ ln -s /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.111-2.b15.el7_3.x86_64/include/linux/jawt_md.h  /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.111-2.b15.el7_3.x86_64/include/jawt_md.h

$ export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.111-2.b15.el7_3.x86_64
$ git clone https://github.com/smecsia/kotlin-standalone.git
$ cd kotlin-standalone/
$ ./gradlew clean standalone
$ ./build/kotlin-standalone
```
```bash
$ ./gradlew clean standalone
```

This will download Avian to `build/avian` directory and then will launch the build of your project into a single
self-contained application file less than 4Mb in size.

## Drawbacks

* There is a `src/util` directory containing the primitive thread pool and blocking queue implementations, that
should be easily found within the java standard library. The reason for this is that Avian does not implement 
many parts of jdk and we have to "reinvent a wheel" for some features.
* Performance is worse than with Oracle JDK. I believe this is because Oracle JDK is highly optimized in many
ways.

## Advanced settings

Avian is a very lightweight JDK implementation. As such, it misses lots of great JDK standard library things.
Fortunately there is a way of building your project with the alternative JDK implementations (e.g. OpenJDK).

### Build with OpenJDK

You can build the project in more advanced way with the full JDK class library support. To do so, you will need to
have the source code and a distribution of OpenJDK. As soon as you get ones, just indicate the path to each of them
in the respective properties within `gradle.properties` file:

```properties
OPEN_JDK_PATH=/absolute/path/to/openjdk/distribution
OPEN_JDK_SRC_PATH=/absolute/path/to/openjdk/source/code
```

## Drawbacks

* The proven compatible version of OpenJDK is openjdk-7u111 (as of November 2016). Not sure if others will work as well.
* The resulting size of a self-contained file will be tens of megabytes instead of a few.
