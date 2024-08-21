# Database Scaling Strategy

## Overview

Our database scaling strategy encompasses three main stages: Single Container, Multiple Containers, and Full SQL Server Transition. The Single Container approach is expected to meet our needs for the foreseeable future. Subsequent stages are designed as contingency plans for significant growth. Throughout all stages, we maintain a Reporting Component as a Windows service on a separate VM, exposing APIs for report generation.

## Common Architecture Elements

- ASP.NET Core web application
- SQL Server databases (Express initially, with potential transition to full SQL Server)
- Reporting Component (Windows service on VM)
  - Exposes public APIs
  - Receives connection strings as API parameters
  - Generates reports based on provided connection strings

## 1. Single Container Approach (Primary Solution)

Architecture:
```
[VM]
|-- Reporting Service (Windows Service)
    |-- Public APIs

[Docker Container]
|-- ASP.NET Core App
|-- SQL Server Express
    |-- Common DB (stores connection strings)
    |-- Customer DB 1, 2, ...
```

Key Points:
- Expected to meet our needs for the foreseeable future
- SQL Server Express limitations: 10GB per DB, 1 socket or 4 cores, 1GB buffer memory
- Multiple connection strings stored in Common DB
- ASP.NET Core app retrieves connection strings and passes to Reporting Service as needed

## 2. Multiple SQL Server Express Containers (Scaling Contingency)

Architecture:
```
[VM]
|-- Reporting Service (Windows Service)
    |-- Public APIs

[Container 1]               [Container 2, 3, ...]
|-- ASP.NET Core App        |-- SQL Server Express
|-- SQL Server Express          |-- Customer DB X, Y, ...
    |-- Common DB
    |-- Customer DB 1, 2, ...
```

Key Points:
- Implemented if we hit the limits of the single container approach
- Scales beyond single SQL Server Express limitations
- Connection strings for all databases stored in Common DB
- ASP.NET Core app manages connections across containers
- Reporting Service remains unchanged, receiving connection strings via API calls

## 3. Transition to Full SQL Server (Unlikely Scenario for Huge Surge)

Architecture:
```
[VM]
|-- Reporting Service (Windows Service)
    |-- Public APIs

[Container]
|-- ASP.NET Core App

[Full SQL Server Instance(s)]
|-- Common DB
|-- Customer DBs
```

Key Points:
- Considered only in case of an unlikely huge surge in demand
- Options: Self-hosted, Managed services, or Containerized SQL Server
- Potential for hybrid setup (mix of Express and Full SQL Server)
- Reporting Service adapts seamlessly through provided connection strings

## Reporting Component Integration

1. ASP.NET Core app retrieves connection strings from Common DB
2. App calls Reporting Service API with required connection strings as parameters
3. Reporting Service accesses databases using provided connection strings
4. Results returned via API response

Advantages:
- Enhanced security (no stored connection strings in Reporting Service)
- Flexibility to work with any accessible database
- Scalability aligned with database scaling strategy

## Code Impact and Deployment Considerations

- Minimal code changes required for database scaling
- ASP.NET Core app needs to implement Reporting Service API calls
- Reporting Service designed to accept and use connection string parameters
- Deployment updates:
  - Container orchestration configurations
  - Connection string management in Common DB
  - Network configurations for cross-component communication

## Conclusion

This strategy provides a flexible, scalable approach to database management and reporting. The Single Container approach with SQL Server Express is expected to serve our needs well into the future. However, we have clear, actionable plans for scaling if the need arises. The API-based reporting component enhances security and flexibility, integrating seamlessly with all stages of our database architecture. This design balances immediate cost-effectiveness with long-term scalability, positioning the system to handle our current needs efficiently while being prepared for potential future growth.

