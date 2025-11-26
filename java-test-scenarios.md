# Java Testing Scenarios

## Scenario Files for Different Test Cases

### pom-fixed.xml Properties - All Dependencies Upgraded
```xml
<properties>
    <java.version>17</java.version>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    
    <!-- Secure versions -->
    <spring-boot.version>3.2.0</spring-boot.version>
    <jackson.version>2.15.4</jackson.version>
    <log4j.version>2.21.1</log4j.version>
    <commons-text.version>1.11.0</commons-text.version>
</properties>
```

### Dependencies - Upgraded Versions
```xml
<!-- Replace vulnerable versions with these -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-text</artifactId>
    <version>1.11.0</version>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.2.224</version>
    <scope>runtime</scope>
</dependency>

<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>2.0</version>
</dependency>

<!-- Remove commons-collections entirely -->
<!-- <dependency>
    <groupId>commons-collections</groupId>
    <artifactId>commons-collections</artifactId>
    <version>3.2.1</version>
</dependency> -->
```

## How to Test Each Scenario

### Test 1: Complete Security Upgrade
```bash
git checkout -b java-security-upgrade
# Update pom.xml properties to use secure versions
# Update individual dependency versions
git add pom.xml
git commit -m "security: upgrade all Java dependencies to resolve vulnerabilities"
git push origin java-security-upgrade
# Create PR
```

### Test 2: Remove Dangerous Dependencies
```bash
git checkout -b java-remove-dangerous-deps
# Remove commons-collections, old commons-text
# Keep only essential dependencies with secure versions
git add pom.xml
# Update Java code to remove imports of removed dependencies
git add src/
git commit -m "refactor: remove dangerous Java dependencies with known vulnerabilities"
git push origin java-remove-dangerous-deps
# Create PR
```

### Test 3: Critical-Only Fixes
```bash
git checkout -b java-critical-fixes
# Only upgrade Log4j and Jackson (most critical)
# Leave other vulnerabilities for demonstration
git add pom.xml
git commit -m "security: fix critical Java vulnerabilities (Log4Shell, Jackson RCE)"
git push origin java-critical-fixes
# Create PR
```

### Test 4: Spring Boot Upgrade
```bash
git checkout -b java-spring-boot-upgrade
# Upgrade Spring Boot to 3.x (major version jump)
# This will resolve many transitive dependency vulnerabilities
git add pom.xml
# May need to update Java code for Spring Boot 3.x compatibility
git add src/
git commit -m "feat: upgrade Spring Boot to 3.x to resolve security vulnerabilities"
git push origin java-spring-boot-upgrade
# Create PR
```

## Expected Vulnerability Counts

- **Original**: 15+ vulnerabilities across all Maven dependencies
- **Complete Upgrade**: 0-2 remaining vulnerabilities
- **Remove Dangerous**: 10+ resolved vulnerabilities
- **Critical-Only**: 5+ resolved critical/high vulnerabilities
- **Spring Boot Upgrade**: 12+ resolved vulnerabilities (many transitive)

## Java-Specific Vulnerability Details

### Critical Vulnerabilities
- **CVE-2021-44228** (Log4Shell): CVSS 10.0 - Remote Code Execution
- **CVE-2022-42889** (Commons Text): CVSS 9.8 - Remote Code Execution
- **CVE-2015-6420** (Commons Collections): CVSS 9.8 - Remote Code Execution

### High Severity
- **Jackson Databind**: Multiple CVEs for deserialization
- **SnakeYAML**: Unsafe deserialization vulnerabilities
- **H2 Database**: SQL injection and path traversal

### Maven-Specific Testing

```bash
# Check for vulnerabilities in current project
mvn org.sonatype.ossindex.maven:ossindex-maven-plugin:audit

# Update to latest versions
mvn versions:display-dependency-updates

# Security-focused dependency analysis
mvn dependency:analyze-report
```

## Automation Examples

```java
// Example of processing resolved vulnerabilities in Java
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

public class VulnerabilityProcessor {
    public void processResolvedVulns(String resolvedVulnsJson) {
        ObjectMapper mapper = new ObjectMapper();
        try {
            JsonNode vulns = mapper.readTree(resolvedVulnsJson);
            
            long javaVulns = StreamSupport.stream(vulns.spliterator(), false)
                .filter(v -> "maven".equals(v.get("ecosystem").asText()))
                .count();
                
            long criticalVulns = StreamSupport.stream(vulns.spliterator(), false)
                .filter(v -> "critical".equals(v.get("severity").asText()))
                .count();
                
            System.out.println("Resolved " + javaVulns + " Java vulnerabilities");
            System.out.println("Including " + criticalVulns + " critical issues");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Spring Boot Specific Notes

When upgrading Spring Boot from 2.x to 3.x:
- Java 17+ required
- Jakarta EE instead of Java EE
- Many dependency versions automatically updated
- Significant security improvements in newer versions

This provides an excellent test case for seeing many resolved vulnerabilities at once!