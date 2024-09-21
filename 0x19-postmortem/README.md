# Postmortem: HealthConnect Patient Booking System Outage

## Issue Summary
- **Outage Duration**:  
  The outage lasted for **2 hours**, starting at **14:00 UTC** and ending at **16:00 UTC** on **September 18, 2024**.
  
- **Impact**:  
  The **HealthConnect Patient Booking System** was **completely inaccessible** during the outage. Approximately **70% of users** experienced issues, including the inability to log in or book appointments. This caused significant disruptions in scheduled consultations and patient-doctor interactions.

- **Root Cause**:  
  A misconfiguration in the **Nginx** server's load balancing rules after a recent update resulted in traffic misrouting, leading to internal server errors (500).

## Timeline
- **14:00 UTC** - Issue detected via monitoring alerts showing a spike in 500 errors from the web server.
- **14:05 UTC** - Initial investigation focused on the database, as past incidents were linked to database overloads.
- **14:15 UTC** - User complaints were received via social media and internal support channels, confirming widespread access issues.
- **14:25 UTC** - The investigation temporarily focused on API rate limits, but no abnormalities were found.
- **14:45 UTC** - The issue was escalated to the backend and DevOps teams, who started reviewing server-side configurations.
- **15:00 UTC** - Nginx load balancer misconfiguration was identified as the root cause.
- **15:15 UTC** - A rollback of the faulty Nginx configuration was performed, restoring proper traffic routing.
- **16:00 UTC** - Service was fully restored, and all systems returned to normal operation.

## Root Cause and Resolution
- **Root Cause**:  
  The issue was caused by an incorrect load balancing rule in the **Nginx** configuration, which led to the misrouting of incoming requests and a spike in 500 errors. This happened after an update to the load balancer without proper testing.

- **Resolution**:  
  The Nginx configuration was rolled back to the previous stable version. The server was then restarted, and incoming traffic was properly routed. Monitoring confirmed that error rates returned to normal.

## Corrective and Preventative Measures
- **Improvements**:  
  - **Enhanced configuration review process** to ensure more thorough checks before applying changes to critical systems like Nginx.
  - **Updated documentation** for server configuration management to help avoid similar mistakes.
  - Improved monitoring systems to detect and alert on misconfigurations in web servers earlier.

- **Actionable Tasks**:
  1. **Patch Nginx server** and test the load balancer for potential edge cases.
  2. Implement **monitoring alerts** for Nginx configuration issues and 500 error spikes.
  3. Set up a **staging environment** to test server configuration changes before they are deployed to production.
  4. Conduct a **training session** for the DevOps team on best practices for server configuration management.
  5. Deploy an **automated rollback system** to quickly revert problematic configuration changes in the future.

---