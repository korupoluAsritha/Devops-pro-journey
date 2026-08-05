```mermaid
flowchart TD
A["Login Attempt"]
B["deploy_bot User"]
C["Non-Interactive Shell<br>/sbin/nologin"]
D["Access Denied"]

A --> B
B --> C
C --> D
```