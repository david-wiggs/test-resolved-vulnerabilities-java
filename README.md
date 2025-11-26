# ☕ Test Resolved Vulnerabilities - Java

This repository demonstrates the **resolved vulnerabilities feedback** feature in dependency-review-action using Java/Maven dependencies.

## Overview

This repository contains a Spring Boot web application with intentionally vulnerable Java dependencies to test the positive feedback when security issues are resolved.

## Vulnerable Dependencies

The `pom.xml` includes Maven dependencies with known vulnerabilities:

- **Spring Boot 2.6.0** - Multiple security vulnerabilities in older versions
- **Jackson Databind 2.12.0** - Deserialization vulnerabilities (CVE-2020-36518, etc.)
- **Log4j 2.14.1** - Log4Shell and other RCE vulnerabilities (CVE-2021-44228, etc.)
- **Commons Text 1.9** - String interpolation RCE (CVE-2022-42889)
- **Commons Collections 3.2.1** - Deserialization RCE (CVE-2015-6420)
- **H2 Database 1.4.200** - Multiple security issues
- **SnakeYAML 1.27** - Deserialization vulnerabilities

## Demo Application

The repository includes a Spring Boot web application that:
- Uses multiple vulnerable dependencies in working code
- Provides REST endpoints to demonstrate functionality
- Shows information about current dependency versions
- Exposes which vulnerabilities exist

### API Endpoints

- `GET /` - Main info page with vulnerability details
- `GET /test-jackson?json={}` - Test Jackson JSON parsing
- `GET /test-commons?text=hello` - Test Commons Text and Collections
- `GET /test-yaml?yamlContent=test: value` - Test SnakeYAML parsing
- `GET /health` - Application health check

## Testing the Feature

### 🚀 Quick Test

1. **View existing PR** (if available) or create a new one
2. **Upgrade vulnerable dependencies** in `pom.xml`:
   ```xml
   <spring-boot.version>3.2.0</spring-boot.version>
   <jackson.version>2.15.0</jackson.version>
   <log4j.version>2.21.0</log4j.version>
   ```
   And update individual dependencies:
   ```xml
   <dependency>
       <groupId>org.apache.commons</groupId>
       <artifactId>commons-text</artifactId>
       <version>1.10.0</version>
   </dependency>
   ```
3. **Create PR** and watch the resolved vulnerabilities feedback!

### 📋 Expected Results

**In PR Comments:**
```
🎉 **10+ vulnerabilities resolved** by upgrading Java dependencies:

| Package | Old Version | Severity | Advisory |
|---------|-------------|----------|----------|
| log4j-core | 2.14.1 | Critical | Log4Shell RCE vulnerability |
| jackson-databind | 2.12.0 | High | Deserialization vulnerabilities |
| commons-text | 1.9 | High | String interpolation RCE |
...
```

**In Action Logs:**
- Detailed Java vulnerability information
- JSON output for automation
- Maven-specific dependency details

## Testing Scenarios

### Scenario 1: Upgrade All Dependencies
```bash
git checkout -b upgrade-all-java-deps
# Update pom.xml with latest secure versions
git commit -m "feat: upgrade all Java dependencies to resolve vulnerabilities"
```

### Scenario 2: Remove Unnecessary Dependencies
```bash
git checkout -b remove-unused-java-deps
# Remove commons-collections, commons-text, and other unused deps
git commit -m "feat: remove unused Java dependencies with vulnerabilities"
```

### Scenario 3: Focus on Critical Issues
```bash
git checkout -b fix-critical-java-vulns
# Only upgrade Log4j and Jackson (critical issues)
git commit -m "security: fix critical Java vulnerabilities (Log4Shell, Jackson)"
```

## Workflow Features

The GitHub Actions workflow (`.github/workflows/test-resolved-vulnerabilities.yml`):
- ✅ Tests with JDK 17
- ✅ Uses Maven for dependency management
- ✅ Builds and tests the application
- ✅ Uses `david-wiggs/dependency-review-action@main`
- ✅ Shows Java-specific resolved vulnerability output
- ✅ Includes application testing and startup validation

## Running Locally

```bash
# Compile the application
mvn clean compile

# Run tests
mvn test

# Start the application
mvn spring-boot:run

# Visit http://localhost:8080 to see vulnerability info
# Test endpoints:
# http://localhost:8080/test-jackson
# http://localhost:8080/test-commons
# http://localhost:8080/test-yaml
```

## Security Notes

⚠️ **Important**: This repository contains intentionally vulnerable dependencies for testing purposes. Do not use in production!

✅ **After testing**: Upgrade to secure versions of all dependencies.

## Java-Specific Vulnerability Types

- **Log4Shell (CVE-2021-44228)**: Remote code execution via Log4j JNDI lookup
- **Jackson Deserialization**: Object deserialization vulnerabilities
- **Commons Collections**: Unsafe deserialization leading to RCE
- **Commons Text**: String interpolation leading to RCE
- **Spring Boot**: Various security misconfigurations and bypasses
- **SnakeYAML**: Unsafe YAML deserialization
- **H2 Database**: SQL injection and other database vulnerabilities

## Integration with CI/CD

The resolved vulnerabilities feature provides:
- **JSON output** for Maven-based automation scripts
- **Positive feedback** to encourage security improvements
- **Detailed logging** for security auditing
- **Multi-ecosystem support** across Java, Python, Node.js, etc.

---

**Ready to test?** Create a PR that upgrades the vulnerable Java dependencies and see the resolved vulnerabilities feature in action! 🚀