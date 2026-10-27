# Run ImageJ with Maven on Windows

This guide takes you from installing the tools to opening the ImageJ desktop application. Use Windows PowerShell for the commands below.

You need **JDK 8**, **JDK 11**, and **Apache Maven 3.9.x**.

To check your current JDK version and maven,

For Java, check in "C:\Program Files\Java" or run _"java -version"_ in command prompt.


For Maven, run _"mvn -version"_ in command prompt.

## 1. Create an Oracle account

1. Open the [Oracle Java downloads page](https://www.oracle.com/java/technologies/downloads/).
2. When a download asks you to sign in, select **Create Account** if you do not already have an Oracle account.
3. You can use your **UM email address** to register or your personal email address. Complete the registration form and any email verification.
4. Sign in with your Oracle account to download the JDKs, and review the download's license terms. (no payment is needed)

## 2. Install JDK 8 and JDK 11

On the [Oracle Java downloads page](https://www.oracle.com/java/technologies/downloads/), find the **Java 8** and **Java 11** sections. Download the **Windows x64 JDK installer** for each version and run both installers. (These instructions assume a Windows x64 computer)

Copy the file path for 

```text
C:\Program Files\Java\jdk1.8.0_511
C:\Program Files\Java\jdk-11.0.xx
```
## 3. Install Maven

If Maven has been installed, skip this step. Otherwise:

1. Open the [Apache Maven download page](https://maven.apache.org/download.cgi).
2. Download an **Apache Maven 3.9.x binary ZIP archive**, not the source archive.
3. Extract it to a folder you can keep, such as `C:\Tools\apache-maven-3.9.x`. Replace `3.9.x` with the actual version throughout this guide.

You will add its `bin` folder to this terminal's `Path` in step 5. You do not need to change Windows Environment Variables permanently.

## 4. Tell Maven where JDK 8 is installed

Go to the following Windows user folder:

```text
C:\Users\YOUR_USERNAME\.m2\
```

For example, if your username is `Ezra`, the path is `C:\Users\Ezra\.m2\`. It does **not** go inside the ImageJ project.

If the file is new, paste the following content. Update `jdkHome` to your actual **JDK 8** file path:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<toolchains>
  <toolchain>
    <type>jdk</type>
    <provides>
      <version>1.8</version>
    </provides>
    <configuration>
      <jdkHome>C:\Program Files\Java\jdk1.8.0_511</jdkHome>
    </configuration>
  </toolchain>
</toolchains>
```

Save it as `toolchains.xml`.

If the file already contains toolchains for other projects, keep them and add only the new `<toolchain>...</toolchain>` entry inside the existing `<toolchains>` element. Do not create a second root element or duplicate an existing matching JDK 8 entry.

**Note: This file persists and is available to all Maven projects under your Windows account.** Creating it does not change `JAVA_HOME` or the Java executable on your `Path`. Other Maven builds can select its entries if they request matching toolchains.

## 5. Select JDK 11 temporarily in PowerShell

Open a PowerShell window in the ImageJ directory. Run the following, replacing the JDK path with your actual **JDK 11** file path:

```powershell
$env:JAVA_HOME = "C:\Program Files\Java\jdk-11.0.xx"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

If Maven was not already available, also add your extracted Maven installation:

```powershell
$env:Path = "C:\Tools\apache-maven-3.9.x\bin;$env:Path"
```

**These environment changes are temporary.** They apply to this PowerShell session and programs launched from it. They are not saved to Windows Environment Variables and disappear when you close this terminal.

Verify the selected tools:

```powershell
java -version
javac -version
mvn -version
```

All three should identify Java **11** as the active Java version. `mvn -version` should also show the Maven version and a Java home pointing to your JDK 11 installation. JDK 8 remains available through `toolchains.xml`.

## 6. Open the project folder and run ImageJ

In a code editor, open ImageJ, then build and launch the application using the following command in the terminal:

```powershell
mvn -Pexec
```

The first run needs internet access to download Maven plugins and dependencies. Wait for compilation to finish; the ImageJ desktop window should open. The terminal may remain occupied while the application is running.

## 7. Run it again later

Keep the JDK and Maven installations and your `toolchains.xml`. In each new PowerShell session, repeat the temporary environment commands from step 5, move to the project folder, and run `mvn -Pexec`.