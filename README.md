# AWS Elastic Beanstalk Custom Configuration
This reporsitory contains configuration files and custom resources for deploying an application to AWS Beanstalk with enhanced monitoring and scaling features.

All custom configuration files are placed under the ".ebextensions/" directiory, which is a special directory supported by AWS Elastic Beanstalk where you can define custom configuration files "*.config" in YAML format to customise your EC2 instance during environment provisioning.


# PROBLEM STATEMENT
By default, AWS Elastic Beanstalk provides only **basic EC2 instance-level metrics**, such as:

  - CPU Utilization, Network In/Out, Disk I/O (limited)

Additionally,

  - Elastic Beanstalk **does not collect memory or disk space utilization metrics**.
  - Auto-scaling in Beanstalk is **limited to basic metrics like CPU utilization, unless custom configurations are applied.
  - Without detailed visibility into memory and disk usage, it's difficult to diagnose performance issues or apply intelligent auto scaling policies based on real usage patterns.\

This lack of observability can lead to:

  - Over-provisioning of resources to avoid memory issues
  - Application crashes due to memory exhaustion without clear visibility
  - Inflexible or reactive auto scaling behavior

This custom configuration package enhances a standard Elastic Beanstalk environment by:

  - Installing and configuring the **CloudWatch Agent** to collect **additional system-level metrics**, such as:
    - `mem_used_percent` (Memory utilization) & `disk_used_percent` (Disk usage)
  - Creating **CloudWatch Alarms tied to Auto Scaling policies**, e.g., scaling out when memory utilization exceeds 80%
  - Enabling **custom Auto Scaling** based on memory usage (instead of just CPU)
  - Improving **observability and resource efficiency**, enabling better performance tuning and cost control

With this setup, your environment gains deeper insights and smarter scaling capabilities, moving beyond the limitations of default Elastic Beanstalk monitoring.

# STEP BY STEP SETUP

### Option 1: Enable Only Custom Metrics (CloudWatch Agent)

If you only need additional system-level metrics (e.g. memory and disk usage), follow these steps:

1. **Under your application root directory**, create a folder named `.ebextensions`:

   ```bash
   mkdir .ebextensions

2. Add the `01-cloudwatch.config` file into the .ebextensions/ directory and deploy your application.

### Option 2: Enable Custom Metrics + Memory-Based Auto Scaling

1. **Under your application root directory**, create a folder named `.ebextensions`:

   ```bash
   mkdir .ebextensions

2. Add both `01-cloudwatch.config` & `02-autoscaling.config` file into the .ebextensions/ directory and deploy your application.


# Note
You can control the **minimum and maximum number of EC2 instances** for auto scaling directly from the **Elastic Beanstalk environment configuration**. These settings determine the scaling boundaries, while the custom `.config` files define **when** to scale based on memory usage.
