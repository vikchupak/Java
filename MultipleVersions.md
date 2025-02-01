# Install and manage multiple java versions

## Using update-alternatives (Manual, system-wide)

Install Multiple Java Versions
```bash
sudo apt install openjdk-8-jdk openjdk-17-jdk
```

Switch Between Java Versions
```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

## Using SDKMAN! (Recommended, user-specific)

Install SDKMAN!
```bash
curl -s "https://get.sdkman.io" | bash
```

Install Multiple Java Versions
```bash
sdk install java 8.0.382-amzn
sdk install java 17.0.8-tem
```

Switch Between Versions
```bash
# For a session
sdk use java 8.0.382-amzn
# Permanently
sdk default java 8.0.382-amzn
```

# SDKMAN! vs update-alternatives

***If I already installed multiple java versions using apt, will SDKMAN manage them?***

No, **SDKMAN!** will not directly manage Java versions installed via `apt`. SDKMAN! manages its own set of Java installations (and other tools like Gradle, Kotlin, etc.) independently from the system-wide installations done with `apt`.

### Here's how they interact:

1. **`apt` installations**:
   - Java versions installed via `apt` are system-wide and managed by `update-alternatives`.
   - You typically install JDK versions via `sudo apt install openjdk-8-jdk`, `openjdk-17-jdk`, etc.
   - These versions can be switched using `sudo update-alternatives --config java`.

2. **`SDKMAN!` installations**:
   - SDKMAN! installs Java versions into its own directory (`~/.sdkman/`) and does not interfere with the system-wide `apt` installations.
   - You can install, switch, and manage Java versions with `sdk install java 8.0.382-amzn`, `sdk install java 17.0.8-tem`, etc.
   - You switch versions using `sdk use` or set defaults using `sdk default`.

### **How to Use Both Together?**

You **can use both** methods, but they operate independently:

- **Switch between `apt` Java versions** with `update-alternatives`.
- **Switch between SDKMAN! Java versions** using `sdk use`.

If you have both **`apt`-installed Java versions** and **SDKMAN! Java versions**, just be mindful that:

- `update-alternatives` will manage the **system-wide Java version**.
- SDKMAN! will manage the **user-specific version**.

---

### **Example Scenario:**
1. You have **Java 8** and **Java 17** installed via `apt`:
   ```bash
   sudo apt install openjdk-8-jdk openjdk-17-jdk
   sudo update-alternatives --config java  # Switch system-wide Java
   ```

2. You want to install a **different JDK** via SDKMAN! (e.g., **GraalVM** or **Temurin**):
   ```bash
   sdk install java 8.0.382-amzn  # Install via SDKMAN!
   sdk install java 17.0.8-tem  # Install via SDKMAN!
   ```

3. To switch between the **SDKMAN! versions** (without touching the system-wide ones):
   ```bash
   sdk use java 17.0.8-tem
   ```

---

### **TL;DR**
SDKMAN! will **not manage** Java versions installed via `apt`—you'll need to manage them separately using `update-alternatives` for system-wide installations and SDKMAN! for user-specific installations.

## When I run java command which java will be used, SDKMAN or system-wide?

The **`java`** command will use whichever Java version is currently set as the default, depending on how you've configured your environment. Here's how to determine which one will be used:

### 1. **If You Are Using SDKMAN!**
- If you have switched to a Java version using SDKMAN! with the `sdk use` or `sdk default` command, then SDKMAN! will manage the Java version for **that session** (or permanently, if set as default).
  
  - **Example:**  
    ```bash
    sdk use java 17.0.8-tem  # Switch to Java 17 from SDKMAN!
    java -version  # Shows Java 17 from SDKMAN!
    ```

- **SDKMAN! Setup**: SDKMAN! modifies the `PATH` for the `java` command so that the SDKMAN! installation takes priority.

---

### 2. **If You Are Using `update-alternatives` (System-wide)**
- If you're using **`update-alternatives`** (installed via `apt`), the Java version set as the default system-wide version via `update-alternatives` will be used.
  
  - **Example:**  
    ```bash
    sudo update-alternatives --config java  # Set default Java using apt
    java -version  # Shows the system-wide Java version
    ```

---

### **Which Version Will Be Used?**
1. **SDKMAN! Overrules**: If you’ve switched to a Java version using SDKMAN! (using `sdk use` or `sdk default`), **SDKMAN’s version will take priority** for the `java` command.
2. **System-wide (via `apt`)**: If you haven't switched with SDKMAN!, the **system-wide Java** (managed by `update-alternatives`) will be used.

### **Check Which Java Is Being Used**
You can check which Java version is active by running:
```bash
which java
```
- If SDKMAN! is in use, it will show something like:  
  `/home/your-user/.sdkman/candidates/java/current/bin/java`
  
- If `update-alternatives` is in use, it will show something like:  
  `/usr/lib/jvm/java-17-openjdk-amd64/bin/java`

---

### **To Ensure SDKMAN! Takes Priority**
Ensure that the `sdkman` path is included **before** the system paths in your `PATH` variable:
```bash
echo $PATH
```
SDKMAN! should modify the `PATH` to prioritize its directory. If it's working correctly, SDKMAN! will manage the Java versions, overriding the system ones.

---

### **TL;DR**  
- If **SDKMAN! is active**, it will override the system-wide version for the `java` command.
- If you're using **`update-alternatives`**, the system-wide Java version will be used.
