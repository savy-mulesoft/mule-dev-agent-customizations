# MuleSoft Integration Rules - Streamlined

## Integration Workflow

### CRITICAL: Pre-Setup
- Check if project directory exists, use different name if needed
- **Always run `mkdir -p project-name` before creating files** (macOS)
- Ensure final build succeeds AND XML schema validates (no red errors)

### Step-by-Step Process

1. **Parse Requirements**
   - Source system → Target system
   - Data flow direction (uni/bidirectional, batch/real-time)
   - Transformation needs

2. **Select Connectors** (Follow priority order)
   - Check [Connector Reference](#connector-reference) below
   - If not found → Search Anypoint Exchange
   - If still not found → Ask user: REST/HTTP, JDBC, File, or Custom

3. **Create Project Structure**
   ```
   project-name/
   ├── pom.xml (use template base)
   ├── mule-artifact.json
   └── src/main/
       ├── mule/project-flow.xml
       └── resources/application.properties
   ```

4. **Add Dependencies**
   - Connector dependencies from reference below
   - Required JDBC drivers for databases, if required
   - Keep minimal and up-to-date

5. **Build Flow**
   - Trigger: HTTP Listener or Scheduler
   - Source connector operation
   - Transform (DataWeave)
   - Target connector operation
   - Logging (correlation IDs, entry/exit points)
   - **Error handling is OPTIONAL** - only add if specifically requested

6. **Quality Gates** ✅
   - Maven build: `mvn clean compile` succeeds
   - XML schema: No red errors in IDE
   - Version consistency: POM runtime = mule-artifact.json minMuleVersion
   - **Java 17 Configuration**: Verify both files contain Java 17 settings:
     - POM.xml: `<maven.compiler.source>17</maven.compiler.source>` and `<maven.compiler.target>17</maven.compiler.target>`
     - mule-artifact.json: `"javaSpecificationVersions": ["17"]`
   - **Mule Runtime**: Use latest Mule 4.10.0+ with `<app.runtime>4.10.0</app.runtime>`
   - **Canvas UI Compatibility**: Flows render properly in Anypoint Studio canvas (no red errors)

7. **Git Integration** 🔄
   - Check if GitHub repository exists with project name
   - If repository exists: Create timestamped branch and push
   - If repository doesn't exist: Create new repository and push to main/master
   - Initialize local git, add files, commit, and push

---

## Connector Reference

### SAP Systems
**SAP S/4HANA OData** | Keywords: SAP, S/4HANA, ERP, OData, HANA
```xml
<dependency>
    <groupId>com.mulesoft.connectors</groupId>
    <artifactId>mule-sap-s4hana-cloud-connector</artifactId>
    <version>2.9.3</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

**SAP Ariba** | Keywords: SAP Ariba, Procurement, Purchase Orders
```xml
<dependency>
    <groupId>e5c02810-ef86-427e-8e6b-f3d3abe55974</groupId>
    <artifactId>mule-plugin-sap-ariba-supplier-sandbox</artifactId>
    <version>1.0.0</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

### Databases
**Database Connector** | Keywords: Database, SQL, MSSQL, Oracle, MySQL, PostgreSQL, JDBC
```xml
<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-db-connector</artifactId>
    <version>1.14.8</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

**JDBC Drivers (add as needed):**
- **MSSQL**: `com.microsoft.sqlserver:mssql-jdbc:12.4.2.jre11`
- **Oracle**: `com.oracle.database.jdbc:ojdbc8:21.9.0.0`
- **MySQL**: `mysql:mysql-connector-java:8.0.33`
- **PostgreSQL**: `org.postgresql:postgresql:42.6.0`

### CRM Systems
**Salesforce** | Keywords: Salesforce, CRM, SOQL, Sales Cloud
```xml
<dependency>
    <groupId>com.mulesoft.connectors</groupId>
    <artifactId>mule-salesforce-connector</artifactId>
    <version>10.20.0</version>
    <classifier>mule-plugin</classifier>
</dependency>
```


**Salesforce** | Keywords: slack
```xml
<dependency>
    <groupId>com.mulesoft.connectors</groupId>
    <artifactId>mule4-slack-connector</artifactId>
    <version>2.0.0</version>
    <classifier>mule-plugin</classifier>
</dependency>
```



**Microsoft Dynamics 365** | Keywords: Microsoft Dynamics, CRM, Dynamics 365
```xml
<dependency>
    <groupId>com.mulesoft.connectors</groupId>
    <artifactId>mule-microsoft-dynamics-365-connector</artifactId>
    <version>2.9.0</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

### Messaging
**Kafka** | Keywords: Kafka, Event Streaming, Messaging, Pub/Sub
```xml
<dependency>
    <groupId>com.mulesoft.connectors</groupId>
    <artifactId>mule-kafka-connector</artifactId>
    <version>4.11.1</version>
    <classifier>mule-plugin</classifier>
</dependency>
```


**JMS** | Keywords: JMS, ActiveMQ, IBM MQ, Message Queue
```xml
<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-jms-connector</artifactId>
    <version>1.8.5</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

### Core Connectors (Always Available)
**HTTP/REST** | Keywords: slack
```xml
<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-http-connector</artifactId>
    <version>1.10.5</version>
    <classifier>mule-plugin</classifier>
</dependency>
```




**HTTP/REST** | Keywords: HTTP, REST, API, Webhook
```xml
 <dependency>
            <groupId>com.mulesoft.connectors</groupId>
            <artifactId>mule4-slack-connector</artifactId>
            <version>2.0.0</version>
            <classifier>mule-plugin</classifier>
</dependency>
```

**File** | Keywords: File, FTP, SFTP, File System
```xml
<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-file-connector</artifactId>
    <version>1.5.1</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

**Email** | Keywords: Email, SMTP, IMAP, POP3
```xml
<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-email-connector</artifactId>
    <version>1.7.3</version>
    <classifier>mule-plugin</classifier>
</dependency>
```

---

## Template Essentials

### POM Template Key Elements
```xml
<groupId>e5c02810-ef86-427e-8e6b-f3d3abe55974</groupId>
<app.runtime>4.10.0</app.runtime>
<maven.compiler.source>17</maven.compiler.source>
<maven.compiler.target>17</maven.compiler.target>
<exchange.organization.id>e5c02810-ef86-427e-8e6b-f3d3abe55974</exchange.organization.id>
<mule.maven.plugin.version>4.5.2</mule.maven.plugin.version>
```

### mule-artifact.json Template
```json
{
  "minMuleVersion": "4.10.0",
  "javaSpecificationVersions": [
    "17"
  ]
}
```

### Basic Flow Structure
```xml
<?xml version="1.0" encoding="UTF-8"?>
<mule xmlns="http://www.mulesoft.org/schema/mule/core" 
      xmlns:http="http://www.mulesoft.org/schema/mule/http" 
      xmlns:ee="http://www.mulesoft.org/schema/mule/ee/core"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
      xsi:schemaLocation="...">
    
    <http:listener-config name="HTTP_Listener_config">
        <http:listener-connection host="0.0.0.0" port="8081" />
    </http:listener-config>
    
    <flow name="main-flow">
        <http:listener config-ref="HTTP_Listener_config" path="/api/endpoint"/>
        <logger message="Request: #[payload]" level="INFO"/>
        <ee:transform>
            <ee:message>
                <ee:set-payload><![CDATA[%dw 2.0
output application/json
---
{
    message: "Success",
    data: payload
}]]></ee:set-payload>
            </ee:message>
        </ee:transform>
        <logger message="Response: #[payload]" level="INFO"/>
    </flow>
</mule>
```

---

## Git Integration Workflow

### Repository Management Strategy
1. **Check Repository Existence**
   - Use GitHub MCP server to search for repository with project name
   - If found: Repository exists, proceed with branch creation
   - If not found: Repository doesn't exist, create new one

2. **Branch Strategy**
   - **Existing Repository**: Create timestamped branch `feature/update-YYYYMMDD-HHMMSS`
   - **New Repository**: Push to default branch (main/master)

3. **Git Operations Sequence**
   ```
   Check repo exists → 
   ├── YES: Create timestamped branch → Push to branch
   └── NO: Create new repo → Initialize → Push to main
   ```

### Implementation Steps
1. **Repository Check**: Use `search_repositories` tool with project name
2. **Create Repository** (if needed): Use `create_repository` tool
3. **Initialize Local Git**: `git init` in project directory
4. **Add Files**: `git add .` to stage all project files
5. **Initial Commit**: `git commit -m "Initial MuleSoft integration project"`
6. **Create Branch** (if repo exists): Generate timestamp branch name
7. **Push Code**: Push to appropriate branch (main for new, timestamped for existing)

### Branch Naming Convention
- **New Repository**: `main` or `master` (default)
- **Existing Repository**: `feature/update-YYYYMMDD-HHMMSS`
- **Example**: `feature/update-20241129-200700`

### Git Commands Template
```bash
# Initialize and setup
cd project-name
git init
git add .
git commit -m "Initial MuleSoft integration project"

# For new repository
git branch -M main
git remote add origin https://github.com/username/project-name.git
git push -u origin main

# For existing repository (with timestamp branch)
git checkout -b feature/update-$(date +%Y%m%d-%H%M%S)
git remote add origin https://github.com/username/project-name.git
git push -u origin feature/update-$(date +%Y%m%d-%H%M%S)
```

---

## Quick Decision Tree

```
Integration Request
    ↓
Check Keywords → Find Connector in Reference Above
    ↓
    ├── Found? → Use Maven dependency
    └── Not Found? → Search Exchange OR Ask user for alternatives
    ↓
Create Project → Add Dependencies → Build Flow → Validate → Git Integration
    ↓                                                        ↓
Quality Gates: Build Success + Schema Valid + Version Match → Check GitHub Repo
                                                              ↓
                                                    ├── Exists? → Create timestamped branch → Push
                                                    └── New? → Create repo → Push to main
```

---

## Essential Best Practices

- **Naming**: `source-target-integration` (lowercase-hyphen)
- **Security**: Externalize credentials, use properties
- **Logging**: Include correlation IDs, log entry/exit points
- **Error Handling**: OPTIONAL - only add if specifically requested by user
- **Performance**: Use batch for large data, implement pagination
- **XML Validation**: Always include `xmlns:doc` namespace and proper schema locations
- **Logger Levels**: Use hardcoded values (INFO, ERROR, DEBUG) instead of properties for validation

---

**Org ID**:
**Runtime**: 4.10.0+ | **Java**: 17
