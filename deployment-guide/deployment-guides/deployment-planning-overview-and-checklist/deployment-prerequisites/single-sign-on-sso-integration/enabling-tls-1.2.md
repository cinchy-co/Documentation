---
description: This page details how to enable TLS 1.2 on Cinchy v5.
---

# Enable TLS 1.2

1. Navigate to the **CinchySSO Folder > appsettings.json** file.
2. Find the following line:

```json
add key="TlsVersion" value=""
```

3. Replace the above line with the following:

```json
add key="TlsVersion" value="1.2"
```

4. Navigate to the **Cinchy Folder > web.config** file.

5. Find the following line:

```json
add key="TlsVersion" value=""
```

6. Replace the above line with the following:

```json
add key="TlsVersion" value="1.2" 
```

7. Restart the application pools in IIS for the changes to take effect.
