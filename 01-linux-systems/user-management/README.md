```mermaid
flowchart TD
A[Login Attempt]
B[deploy_bot User]
C[/sbin/nologin]
D[Access Denied]


A --> B
B --> C
C --> D
```