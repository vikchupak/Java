# JRE vs JDK

| Feature | **JRE (Java Runtime Environment)** | **JDK (Java Development Kit)** |
|---------|-----------------------------------|--------------------------------|
| **Purpose** | Runs Java applications | Develops and runs Java applications |
| **Includes Compiler?** | ❌ No | ✅ Yes (javac) |
| **Includes JVM?** | ✅ Yes | ✅ Yes |
| **Includes Development Tools?** | ❌ No | ✅ Yes (debugger, profiler, etc.) |
| **Includes JRE?** | ✅ Yes (built-in) | ✅ Yes |
| **Used By** | End-users running Java apps | Developers writing Java apps |

---

### **1. What is JRE? (Java Runtime Environment)**
- **JRE** is used to **run** Java applications but does **not** include development tools.  
- It contains:
  - **JVM (Java Virtual Machine)** – Executes Java bytecode.  
  - **Java Class Libraries** – Core Java API for runtime execution.  
  - **Java Plugin & Java Web Start** (for older Java applets, now deprecated).  
- **Use Case**: If you just need to run Java programs, install **JRE**.  

Install
```bash
sudo apt install -y openjdk-11-jre
```

---

### **2. What is JDK? (Java Development Kit)**
- **JDK** includes everything in **JRE** plus **developer tools**.  
- It contains:
  - **JVM + JRE** (built-in).  
  - **Java Compiler (`javac`)** – Converts Java code into bytecode.  
  - **Development Tools** – Debuggers, profilers, and documentation tools.  
  - **JAR Utility (`jar`)** – Packages Java classes into `.jar` files.  
- **Use Case**: If you want to **develop** Java applications, install **JDK**.  

Install
```bash
sudo apt install -y openjdk-11-jdk
```

---

### **3. JDK vs JRE Example**
#### **Running a Java Program (JRE)**
If you have a compiled `.class` file, you can run it with **JRE**:  
```sh
java MyProgram
```
  
#### **Compiling & Running a Java Program (JDK)**
If you have a `.java` file, you need **JDK** to compile and run it:  
```sh
javac MyProgram.java  # Compiles to MyProgram.class
java MyProgram        # Runs the compiled class
```

---

### **4. Which One Do You Need?**
- **Only running Java applications?** → Install **JRE**  
- **Developing Java applications?** → Install **JDK**  

**Note:**  
- From **Java 11 onward**, **JRE is no longer provided separately**—the **JDK includes everything** you need to run and develop Java apps.  
- Popular JDK distributions: **Oracle JDK, OpenJDK, AdoptOpenJDK, Amazon Corretto, Azul Zulu**.  

# JRE vs JRE-Headless  

| Feature                | **openjdk-11-jre**       | **openjdk-11-jre-headless**  |
|------------------------|-------------------------|------------------------------|
| **Includes GUI Support?** | ✅ Yes (Swing, JavaFX, AWT) | ❌ No (No GUI libraries)      |
| **Uses Less Resources?**  | ❌ No (Requires more dependencies) | ✅ Yes (Smaller size, no GUI) |
| **Recommended for Servers?** | ❌ No | ✅ Yes (Ideal for headless environments) |
| **Installation Size**  | Larger | Smaller |

---

### **1️⃣ What is `jre-headless`?**
- `openjdk-11-jre-headless` is a **lightweight version** of JRE.  
- **Does not** include **GUI-related libraries** like:
  - **Swing (`javax.swing`), JavaFX, AWT (`java.awt`)**  
  - Fonts, graphics rendering, and window-related components.  
- Ideal for **server-side applications**, microservices, and cloud environments.  

---

### **2️⃣ When to Use Which?**
- **Use `openjdk-11-jre`** if:
  - You need **GUI-based Java applications** (Swing, JavaFX, AWT, etc.).
  - Example: Running **Eclipse IDE, GUI-based tools, or desktop applications**.

- **Use `openjdk-11-jre-headless`** if:
  - You’re running **Java applications on a server** (without GUI).
  - Example: Running **Tomcat, Spring Boot, Microservices, Jenkins, Kafka**.

---

### **3️⃣ Installation Commands**
- Install **JRE (with GUI support)**:  
  ```sh
  sudo apt install -y openjdk-11-jre
  ```
- Install **JRE Headless (no GUI, lightweight)**:  
  ```sh
  sudo apt install -y openjdk-11-jre-headless
  ```

# There is no jdk-headless

| Package | Includes GUI? | Includes Compiler (`javac`)? | Use Case |
|---------|-------------|----------------------|----------|
| `openjdk-11-jdk` | ✅ Yes (GUI support) | ✅ Yes | For Java **development** |
| `openjdk-11-jre` | ✅ Yes (GUI support) | ❌ No | Running GUI-based Java apps |
| `openjdk-11-jre-headless` | ❌ No (No GUI) | ❌ No | Running Java apps on servers (without GUI) |

---

### **Installation Commands**
- **For development (JDK)** → `sudo apt install -y openjdk-11-jdk`  
- **For GUI-based Java apps (JRE)** → `sudo apt install -y openjdk-11-jre`  
- **For server-side Java apps (Headless JRE)** → `sudo apt install -y openjdk-11-jre-headless`  

# From Java 11 onward, JRE is no longer provided separately—the JDK includes everything you need to run and develop Java apps.

***If so, why does the command `sudo apt install -y openjdk-11-jre exist`?***

The reason the `openjdk-11-jre` package still exists in **Ubuntu's package manager** is mainly for **compatibility and modularity**.  

### **Why Does `openjdk-11-jre` Still Exist?**
1. **Ubuntu Packaging System**  
   - Even though Oracle/OpenJDK **does not distribute a separate JRE** starting from **Java 11**, Ubuntu (and Debian-based systems) maintain a separate **JRE package** for users who only need to run Java applications.
   - This is done to **avoid installing unnecessary development tools** (`javac`, debugger, etc.) for users who don't need them.

2. **Modular JDK Design (JEP 220 - Java 9+)**  
   - Since **Java 9**, the JDK is **modular**, meaning you can run Java programs with only a minimal subset of required modules.
   - The **Ubuntu/Debian maintainers split the JDK into JDK and JRE packages** to keep the system lightweight and optimized.

3. **Smaller Installation Size**  
   - **JDK** includes **development tools** like:
     - `javac` (compiler)
     - `javadoc`
     - `jdb` (debugger)
   - **JRE** only contains:
     - `java` (runtime)
     - Core Java libraries  
   - Installing `openjdk-11-jre` saves space compared to installing the full `openjdk-11-jdk`.

4. **Dependency Management**  
   - Some applications in Linux repositories declare a dependency on **JRE**, not JDK.
   - For example, if you install a Java-based application via `apt`, it might **automatically pull `openjdk-11-jre`** instead of `openjdk-11-jdk`.

---

### **What Happens When You Install JDK (`openjdk-11-jdk`)?**
- **The JDK includes the JRE**, so running Java applications still works.  
- If you install `openjdk-11-jdk`, you do **not** need to install `openjdk-11-jre` separately.

---

### **What Should You Install?**
| Use Case | Recommended Package |
|----------|---------------------|
| Running Java applications only | `sudo apt install -y openjdk-11-jre` |
| Developing Java applications | `sudo apt install -y openjdk-11-jdk` |

So, while **Java 11+ does not officially separate JRE**, Linux distributions **still provide `openjdk-11-jre` for user convenience and compatibility**. 🚀
