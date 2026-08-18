# Lab 04: Install and Configure Apache Tomcat Server

## Objective

Learn how to install, configure, and run Apache Tomcat, a Java application server used to host Java-based web applications.

By the end of this lab, you will be able to:

- Understand what Tomcat is
- Understand where Tomcat fits in web application architecture
- Install Tomcat
- Verify Java installation
- Start and stop Tomcat
- Verify connectivity on port 8080
- Troubleshoot common Tomcat issues
- Understand real-world production deployment scenarios

---

# What is Apache Tomcat?

Apache Tomcat is an open-source:

```text
Web Server
+
Servlet Container
+
Java Application Server
```

used to run Java web applications.

Examples:

```text
JSP Applications
Servlet Applications
Spring Applications
Java Enterprise Applications
```

---

# Why Do We Need Tomcat?

Web applications need a server capable of executing Java code.

Example:

```text
User
  ↓
Browser
  ↓
Tomcat
  ↓
Java Application
  ↓
Database
```

Without Tomcat:

```text
Java Application
      ↓
Cannot Serve Requests
```

---

# Real-World Use Cases

Organizations commonly use Tomcat to host:

- Banking Portals
- Internal Enterprise Applications
- HR Systems
- ERP Tools
- Java APIs
- Spring Boot Applications

Examples:

```text
Employee Portal
Inventory Management
Ticketing Systems
```

---

# Understanding the Web Architecture

```text
Browser
   ↓
Tomcat Server
   ↓
Java Application
   ↓
MariaDB
```

User requests:

```text
http://server:8080
```

Tomcat processes the request and returns the response.

---

# Why Java Is Required

Tomcat runs Java applications.

Therefore Java must be installed first.

Verify:

```bash
java -version
```

Example:

```text
openjdk version "17.x"
```

or

```text
java version "11.x"
```

---

# If Java Is Missing

Example:

```bash
java -version
```

Output:

```text
command not found
```

Install Java first.

Ubuntu:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

RHEL/CentOS:

```bash
sudo yum install java-17-openjdk-devel -y
```

---

# Understanding a Tomcat Installation

Typical structure:

```text
apache-tomcat/
│
├── bin/
├── conf/
├── lib/
├── logs/
├── temp/
├── webapps/
└── work/
```

---

# Important Directories

## bin/

Contains startup and shutdown scripts.

Examples:

```text
startup.sh
shutdown.sh
catalina.sh
```

---

## conf/

Contains configuration files.

Examples:

```text
server.xml
web.xml
tomcat-users.xml
```

---

## logs/

Stores runtime logs.

Examples:

```text
catalina.out
localhost.log
```

---

## webapps/

Stores deployed applications.

Example:

```text
ROOT
manager
host-manager
```

Applications are deployed here.

---

# Extract the Tomcat Archive

Tomcat is often downloaded as:

```text
apache-tomcat-10.x.x.tar.gz
```

Extract:

```bash
tar -xvf apache-tomcat-*.tar.gz
```

---

# Understanding tar

`tar` means:

```text
Tape Archive
```

Common options:

### x

```text
Extract
```

### v

```text
Verbose Output
```

### f

```text
Use File
```

---

# Verify Extraction

```bash
ls
```

Example:

```text
apache-tomcat-10.1.20
```

Move into the directory:

```bash
cd ap*che-tomcat-*
```

---

# Starting Tomcat

Run:

```bash
./bin/startup.sh
```

Expected:

```text
Using CATALINA_BASE:
Using CATALINA_HOME:
Tomcat started.
```

---

# Verify Tomcat Process

Check:

```bash
ps -ef | grep tomcat
```

or

```bash
ps -ef | grep java
```

Example:

```text
tomcat 1234 java ...
```

---

# Verify Listening Port

By default Tomcat listens on:

```text
8080
```

Check:

```bash
ss -tulpn | grep 8080
```

Example:

```text
LISTEN 0 100 *:8080
```

---

# Access Tom*at

Open browser:

```text
*ttp://your-server-ip:8080
```

Exp*cted:

*``text
Apache Tomcat Welcome Page
*``

or*
```text
Apache Tomcat Manager Hom*
```

This confirms Tomcat is oper*tional.

---

# Understanding Port*8080

Default Tomcat port:

```tex*
8080
```

Why*not 80?

Because:

*``text
80 = Standard*HTTP
8080 = Common Application*Port
```

---

# Firewall Consider*tions

If Tomcat is running but in*ccessible:

Check firewall.

Ubunt*:

```bash
sudo ufw allow 8080/tcp*```

RHEL/CentOS:

```bash
sudo fi*ewall-cmd --add-port=8080/tcp --pe*manent
sudo firewall-cmd --reload
*``

Verify:

```bash
sudo firewall*cmd --list-ports
```

---

# Stopp*ng Tomcat

Run:

```bash
./bin/shu*down.sh
```

Expected:

```text
To*cat stopped.
```

---

# Restartin* Tomcat

```bash
./bin/shutdown.sh*./bin/startup.sh
```

---

# Tomca* Deployment Process

Real-world wo*kflow:

```text
Developer
     ↓
C*eates WAR File
     ↓
Tomcat webap*s/
     ↓
Deployment
     ↓
Users *ccess Application
```

Example:

`*`text
employee-portal.war
```

Cop*ed to:

```text
webapps/
```

Tomc*t automatically deploys it.

---

* Common Troubleshooting Scenarios
*---

# Scenario 1: Tomcat Won't St*rt

Check:

```bash
./bin/startup.*h
```

Output:

```text
Tomcat sta*ted
```

but application is unavai*able.

---

## Check Logs

```bash*tail -f logs/catalina.out
```

Thi* is the primary Tomcat log.

---

* Scenario 2: Java Missing

Error:
*```text
Neither the JAVA_HOME nor *he JRE_HOME environment variable i* defined
```

Verification:

```ba*h
java -version
```

---

## Fix

*nstall Java.

Set:

```bash
export*JAVA_HOME=/usr/lib/jvm/java-17-ope*jdk
```

Verify:

```bash
echo $JA*A_HOME
```

---

# Scenario 3: Por* Already in Use

Error:

```text
A*dress already in use
```

Check:

*``bash
ss -tulpn | grep 8080
```

*ossible result:

```text
Another p*ocess using 8080
```

---

## Fix
*Stop conflicting process.

Or modi*y:

```text
conf/server.xml
```

C*ange:

```xml
<Connector port="808*"... />
```

To:

```xml
<Connecto* port="9090"... />
```

Restart To*cat.

---

# Scenario 4: Browser C*nnot Connect

Verify process:

```*ash
ps -ef | grep java
```

---

V*rify port:

```bash
ss -tulpn | gr*p 8080
```

---

Verify firewall:
*```bash
firewall-cmd --list-ports
*``

or

```*ash*ufw status
```

---

# Scenario 5:*Application Returns 404

Verify de*loyed application:

```bash
ls web*pps/
```

Expected:

```text
myapp*myapp.war
```

Missing application*

```text
Deployment problem
```

*--

# Tomcat Troubleshooting Flow
*```mermaid
flowchart TD
    A["App*ication Inaccessible"]
    B["Chec* Tomcat Process"]
    C["Check Por* 8080"]
    D["Check Logs"]
    E[*Check JAVA_HOME"]
    F["Check Fir*wall"]
    G["Verify Application D*ployment"]
    H["Issue Resolved"]*
    A --> B
    B --> C
    C -->*D
    D --> E
    E --> F
    F --* G
    G --> H
```

---

# Install*tion Workflow

```mermaid
flowchar* TD
    A["Install Java"]
    B["E*tract Tomcat"]
    C["Start Tomcat*]
    D["Verify Process"]
    E["V*rify Port 8080"]
    F["Access Bro*ser"]

    A --> B
    B --> C
   *C --> D
    D --> E
    E --> F
``*

---

# Command Summary

Verify J*va:

```bash
java -version
```

Ex*ract Tomcat:

```bash
tar -xvf apa*he-tomcat-*.tar.gz
```

Start Tomc*t:

```bash
./bin/startup.sh
```

*top Tomcat:

```bash
./bin/shutdow*.sh
```

Check process:

```bash
p* -ef | grep java
```

Check port:
*```bash
ss -tulpn | grep 8080
```
*View logs:

```bash
tail -f logs/c*talina.out
```

---

# Key Learnin*s

- Apache Tomcat is a Java appli*ation server.
- Tomcat hosts Java *eb applications.
- Java must be in*talled before Tomcat.
- Tomcat arc*ives are extracted using `tar`.
- *omcat is controlled with startup a*d shutdown scripts.
- Default list*ning port is 8080.
- `catalina.out* is the primary troubleshooting lo*.
- Common failures involve Java, *orts, firewalls, and deployments.
* Tomcat is widely used in enterpri*e applications.

---

# Interview *uestions

## What is Apache Tomcat*

Apache Tomcat is an open-source *
