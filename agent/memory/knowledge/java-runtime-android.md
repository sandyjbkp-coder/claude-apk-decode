# Java Runtime Android

**Java Environment**  
- **Java Home:** `/data/user/0/com.androidharness.app.kernelsu/files/linux/lib/jvm/java-17-openjdk`  
- **Java Version:** openjdk version "17.0.21.1" 2026-11-17  
- **Java Home Path:** `/data/user/0/com.androidharness.app.kernelsu/files/linux/lib/jvm/java-17-openjdk`  

**Verification**  
- `java -version`: openjdk version "17.0.21.1" 2026-11-17  
  OpenJDK Runtime Environment (build 17.0.21.1+0)  
  OpenJDK 64-Bit Server VM (build 17.0.21.1+0, mixed mode)  
- `javac -version`: javac 17.0.21.1  

**PATH Configuration**  
- `export JAVA_HOME=/data/user/0/com.androidharness.app.kernelsu/files/linux/lib/jvm/java-17-openjdk`  
- `export PATH="$JAVA_HOME/bin:$PATH"`  

**Gradle Status**  
- Java is now functional with correct JAVA_HOME and PATH  
- `gradle` command not found on default PATH (gradle binary not installed in this environment)  
- Android build capability depends on Gradle being available via project's `gradlew` wrapper  

**Important Notes**  
- Java was already present in the Linux environment at `/data/user/0/com.androidharness.app.kernelsu/files/linux/lib/jvm/java-17-openjdk`  
- No Termux package manager was available (Termux not installed)  
- openjdk-17 was NOT installed via pkg (Termux unavailable); Java was pre-existing  
- `/data/local/tmp/androidharness-scratch/jdk17/` path is NOT accessible — it's a different scratch directory  
- The JDK at `/data/local/tmp/androidharness-scratch/jdk17/` was NOT used — it's incompatible with Android bionic libc (as warned)  
- Gradle is ready for use via Android project's `gradlew` wrapper; no system-wide gradle install needed  

**Status:** READY — Java 17 and javac are both functional. Export JAVA_HOME and PATH as shown above before running Gradle/Android builds.
