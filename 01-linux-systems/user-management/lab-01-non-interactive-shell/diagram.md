```mermaid
flowchart TD
A["Login Attempt"]
B["deploy_bot User"]
C["Non-Interactive Shell<br>/sbin/nologin"]
D["This account is currently not available"]
E["Access Denied"]

A --> B
B --> C
C --> D
D --> E
```

