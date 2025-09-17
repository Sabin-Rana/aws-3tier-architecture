# AWS 3-Tier Architecture Project

## Architecture Diagram
![AWS 3-Tier Architecture](https://github.com/Sabin-Rana/aws-3tier-architecture/tree/main/Architecture_Diagram)

## Overview
This project demonstrates the implementation of a highly available, scalable, and secure 3-tier architecture on Amazon Web Services (AWS). The architecture consists of a presentation tier (web servers), application tier (app servers), and database tier, all deployed across multiple Availability Zones for high availability and fault tolerance.

## Architecture Components
- **Presentation Tier**: NGINX web servers serving React frontend application
- **Application Tier**: Node.js application servers handling business logic
- **Database Tier**: Amazon RDS Aurora MySQL with Multi-AZ deployment
- **Infrastructure**: VPC with public/private subnets, Load Balancers, Auto Scaling Groups
- **Security**: IAM roles, Security Groups, and Session Manager for secure access

## Features
- **High Availability**: Multi-AZ deployment across two Availability Zones
- **Auto Scaling**: Dynamic scaling based on CPU utilization
- **Load Balancing**: External and Internal Application Load Balancers
- **Security**: No direct SSH access, using AWS Systems Manager Session Manager
- **Monitoring**: CloudWatch integration with SNS notifications
- **VPC Flow Logs**: Network traffic monitoring and logging
- **Infrastructure as Code**: Reproducible architecture setup

## Prerequisites
- AWS Account with appropriate permissions
- Application code (React frontend and Node.js backend)
- Basic understanding of AWS services

## Installation

### Phase 1: S3 Bucket Creation & Application Code Upload

1. **S3 Bucket Creation**
   - Created the primary S3 bucket `s3bucket025` to store the application source code
   - This bucket serves as the central repository for both frontend and backend code

   ![S3 Bucket Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_1.png)

2. **S3 Bucket List View**
   - Viewed the S3 dashboard showing the successfully created buckets
   - Prepared second bucket for VPC Flow Logs

   ![S3 Bucket List](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_2.png)

3. **Application Folder Structure Creation**
   - Created `application-code/` folder to logically separate application code
   - Contains subfolders for each tier of the architecture

   ![Folder Structure](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_3.png)

4. **Code Upload Confirmation**
   - Verified successful upload of application code
   - `app-tier/` contains Node.js backend source
   - `web-tier/` contains React.js frontend and Nginx configuration

   ![Code Upload](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_4.png)

### Phase 2: IAM Role Configuration for Secure Access

5. **IAM Role Service Selection**
   - Initiated IAM role creation with AWS service as trusted entity
   - EC2 as use case for secure access to S3 and Systems Manager

   ![IAM Role Selection](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_5.png)

6. **S3 Read-Only Access Policy**
   - Attached `AmazonS3ReadOnlyAccess` policy
   - Follows principle of least privilege for downloading application code

   ![S3 Access Policy](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_6.png)

7. **SSM Managed Instance Core Policy**
   - Added `AmazonSSMManagedInstanceCore` policy
   - Enables AWS Systems Manager Session Manager connectivity

   ![SSM Policy](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_7.png)

8. **IAM Role Final Configuration**
   - Configured IAM role with name `abcd-ec2-s3-ssm-role-025-9-14`
   - Added descriptive metadata and consistent tagging strategy

   ![IAM Role Configuration](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_8.png)

9. **IAM Role Creation Success**
   - Successfully created IAM role for all EC2 instances
   - Provides secure access without hardcoded credentials

   ![IAM Role Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_9.png)

### Phase 3: VPC Infrastructure Foundation

10. **VPC Creation Configuration**
    - Initiated custom VPC creation with name `MY-VPC`
    - IPv4 CIDR block `10.0.0.0/16` providing 65,536 IP addresses

    ![VPC Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_10.png)

11. **VPC Creation Success**
    - Successfully created custom VPC
    - Serves as isolated network foundation for entire architecture

    ![VPC Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_11.png)

12. **Subnet Creation Initialization**
    - Started subnet creation process within custom VPC
    - Preparing for network segmentation and multi-AZ deployment

    ![Subnet Init](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_12.png)

13. **Web Tier Subnet 1 Creation**
    - Created `WEB-TIER-SUBNET-1` in Availability Zone `us-east-2a`
    - CIDR `10.0.1.0/24` for public web servers

    ![Web Subnet 1](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_13.png)

14. **Application Tier Subnet 1 Creation**
    - Created `APP-TIER-SUBNET-1` in Availability Zone `us-east-2a`
    - CIDR `10.0.2.0/24` for private application servers

    ![App Subnet 1](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_14.png)

15. **Database Tier Subnet 1 Creation**
    - Created `DB-TIER-SUBNET-1` in Availability Zone `us-east-2a`
    - CIDR `10.0.3.0/24` for database instances

    ![DB Subnet 1](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_15.png)

16. **Web Tier Subnet 2 Creation**
    - Created `WEB-TIER-SUBNET-2` in Availability Zone `us-east-2b`
    - CIDR `10.0.4.0/24` for high availability web tier

    ![Web Subnet 2](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_16.png)

17. **Application Tier Subnet 2 Creation**
    - Created `APP-TIER-SUBNET-2` in Availability Zone `us-east-2b`
    - CIDR `10.0.5.0/24` for high availability application servers

    ![App Subnet 2](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_17.png)

18. **Database Tier Subnet 2 Creation**
    - Created `DB-TIER-SUBNET-2` in Availability Zone `us-east-2b`
    - CIDR `10.0.6.0/24` completes database tier high availability

    ![DB Subnet 2](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_18.png)

19. **All Subnets Creation Success**
    - Successfully created all six subnets across two Availability Zones
    - Complete network foundation for multi-AZ, multi-tier deployment

    ![All Subnets](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_19.png)

20. **Web Tier Subnet Settings Access**
    - Accessed subnet settings for `WEB-TIER-SUBNET-1`
    - Configure auto-assignment of public IPv4 addresses

    ![Subnet Settings](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_20.png)

21. **Public IP Auto-Assignment Configuration**
    - Enabled auto-assign public IPv4 addresses for web tier subnets
    - Ensures instances automatically receive public IP addresses

    ![IP Auto-Assign](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3tier_Step_21.png)

22. **Public Subnet Configuration Success**
    - Successfully configured both web tier subnets with public IP auto-assignment
    - Maintained private configuration for application and database tiers

    ![Public Subnet Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_22.png)

### Phase 4: VPC Flow Logs Configuration

23. **VPC Flow Logs S3 Bucket Creation**
    - Created dedicated S3 bucket `myvpcflowlogsbucket025` for VPC Flow Logs
    - Separation ensures operational logs don't interfere with application assets

    ![Flow Logs Bucket](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_23.png)

24. **Dual S3 Bucket Configuration**
    - Verified both S3 buckets successfully created
    - `mys3bucket025` for application code
    - `myvpcflowlogsbucket025` for VPC Flow Logs

    ![Dual Buckets](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_24.png)

25. **VPC Flow Logs Creation Initiation**
    - Started VPC Flow Logs creation for `MY-VPC`
    - Enable comprehensive network traffic monitoring

    ![Flow Logs Init](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_25.png)

26. **S3 Destination Configuration**
    - Configured VPC Flow Logs to send data to dedicated S3 bucket
    - Enables long-term storage and analysis of network traffic

    ![S3 Destination](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_26.png)

27. **Complete Flow Logs Configuration**
    - Finalized VPC Flow Logs configuration
    - Named `My-VPC-Flow-Logs` with S3 bucket destination

    ![Flow Logs Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_27.png)

28. **VPC Flow Logs Creation Success**
    - Successfully created VPC Flow Logs
    - Continuously captures and stores network traffic metadata

    ![Flow Logs Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_28.png)

### Phase 5: Internet Connectivity and Routing

29. **Internet Gateway Creation**
    - Created Internet Gateway `My-Public-IGW`
    - Provides internet connectivity for public subnets

    ![IGW Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_29.png)

30. **Internet Gateway Creation Success**
    - Successfully created Internet Gateway
    - Will be attached to VPC for bidirectional internet communication

    ![IGW Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_30.png)

31. **Internet Gateway VPC Attachment**
    - Attached Internet Gateway to `MY-VPC`
    - Establishes connection for public subnet internet access

    ![IGW Attachment](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_31.png)

32. **Internet Gateway Attachment Success**
    - Successfully attached Internet Gateway to VPC
    - Completes foundation for public subnet internet connectivity

    ![IGW Attach Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_32.png)

33. **Web Tier Route Table Creation**
    - Created `Web-Tier-Route-Table` for public subnets
    - Contains routes directing internet traffic to Internet Gateway

    ![Route Table Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_33.png)

34. **Route Table Routes Configuration**
    - Accessed route editing for Web Tier Route Table
    - Add default route (`0.0.0.0/0`) to Internet Gateway

    ![Route Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_34.png)

35. **Internet Gateway Route Addition**
    - Added default route `0.0.0.0/0` targeting Internet Gateway
    - Enables all traffic from associated subnets to reach internet

    ![IGW Route Add](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_35.png)

36. **Web Tier Route Table Completion**
    - Successfully configured Web Tier Route Table with Internet Gateway route
    - Establishes routing foundation for public subnet internet connectivity

    ![Route Table Complete](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_36.png)

37. **Subnet Association Configuration**
    - Initiated subnet association process
    - Connect web tier subnets with route table

    ![Subnet Association](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_37.png)

38. **Web Tier Subnets Association**
    - Associated both `WEB-TIER-SUBNET-1` and `WEB-TIER-SUBNET-2`
    - Makes them truly public subnets with internet access capability

    ![Web Subnet Assoc](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_38.png)

39. **Public Routing Configuration Success**
    - Successfully completed web tier routing configuration
    - Established internet connectivity for public subnets

    ![Routing Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_39.png)

### Phase 6: NAT Gateway Implementation for Private Subnet Connectivity

40. **NAT Gateway 1 Creation**
    - Created `NAT-GW-1` in `WEB-TIER-SUBNET-1` with Elastic IP
    - Provides outbound internet connectivity for private subnet resources in AZ `us-east-2a`

    ![NAT GW 1](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_40.png)

41. **NAT Gateway 1 Creation Success**
    - Successfully created `NAT-GW-1` with dedicated Elastic IP
    - Establishes secure outbound internet access for private subnets

    ![NAT GW 1 Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_41.png)

42. **NAT Gateway 2 Creation**
    - Created `NAT-GW-2` in `WEB-TIER-SUBNET-2` with separate Elastic IP
    - Provides high availability and redundancy across both AZs

    ![NAT GW 2](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_42.png)

43. **NAT Gateway 2 Creation Success**
    - Successfully created `NAT-GW-2`
    - Completes high availability setup for private subnet connectivity

    ![NAT GW 2 Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_43.png)

44. **App Tier Route Table 1 Creation**
    - Created `APP-Tier-RT-1` route table for private subnet routing in AZ `us-east-2a`
    - Will direct outbound traffic through `NAT-GW-1`

    ![App RT 1](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_44.png)

45. **App Tier Route Table 1 Success**
    - Successfully created `APP-Tier-RT-1`
    - Establishes routing foundation for application tier connectivity

    ![App RT 1 Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_45.png)

46. **App Tier Route Configuration**
    - Configured `APP-Tier-RT-1` routes
    - Add default route for internet-bound traffic through NAT Gateway

    ![App Route Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_46.png)

47. **NAT Gateway 1 Route Addition**
    - Added default route `0.0.0.0/0` targeting `NAT-GW-1`
    - Enables outbound internet access for `APP-TIER-SUBNET-1`

    ![NAT Route Add](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_47.png)

48. **App Tier Route Table 1 Configuration Complete**
    - Successfully configured `APP-Tier-RT-1` with NAT Gateway route
    - Establishes secure outbound internet connectivity

    ![App RT Complete](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_48.png)

49. **App Subnet 1 Association Setup**
    - Initiated subnet association process
    - Connect `APP-TIER-SUBNET-1` with `APP-Tier-RT-1`

    ![App Subnet Assoc](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_49.png)

50. **App Subnet 1 Route Association**
    - Associated `APP-TIER-SUBNET-1` with `APP-Tier-RT-1`
    - Establishes routing relationship for private subnet internet access

    ![App Subnet Route](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_50.png)

51. **App Tier AZ1 Routing Success**
    - Successfully completed routing configuration for `APP-TIER-SUBNET-1`
    - Enables secure outbound internet access for application servers

    ![App Routing Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_51.png)

52. **App Tier Route Table 2 Success**
    - Successfully created `APP-Tier-RT-2` for second Availability Zone
    - Establishes routing foundation for high availability

    ![App RT 2 Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_52.png)

53. **NAT Gateway 2 Route Configuration**
    - Configured `APP-Tier-RT-2` with default route targeting `NAT-GW-2`
    - Provides outbound internet connectivity for second AZ

    ![NAT GW 2 Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_53.png)

54. **App Tier Route Table 2 Complete**
    - Successfully updated `APP-Tier-RT-2` routing configuration
    - Completes NAT Gateway setup for high availability

    ![App RT 2 Complete](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_54.png)

55. **App Subnet 2 Route Association**
    - Associated `APP-TIER-SUBNET-2` with `APP-Tier-RT-2`
    - Enables private subnet internet access through `NAT-GW-2`

    ![App Subnet 2 Route](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_55.png)

56. **Complete Private Routing Success**
    - Successfully completed all private subnet routing configurations
    - Established high availability outbound internet connectivity

    ![Private Routing Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_56.png)

57. **VPC Resource Map Overview**
    - Generated comprehensive VPC resource map
    - Displays complete network architecture across both AZs

    ![VPC Map](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_57.png)

### Phase 7: Security Groups - Layered Security Implementation

58. **Security Groups Creation Initiation**
    - Started security group creation process
    - Implement layered security model with least privilege

    ![SG Init](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_58.png)

59. **External Load Balancer Security Group**
    - Created `1.External-Load-Balancer-SG` allowing HTTP (port 80) from anywhere
    - Internet-facing entry point with public access controls

    ![Ext LB SG](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_59.png)

60. **Web Tier Security Group**
    - Created `2.Web-Tier-SG` allowing HTTP traffic only from `1.External-Load-Balancer-SG`
    - Implements security layer restricting web server access to load balancer only

    ![Web Tier SG](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_60.png)

61. **Internal Load Balancer Security Group**
    - Created `3.Internal-Load-Balancer-SG` allowing HTTP traffic only from `2.Web-Tier-SG`
    - Enables secure communication between web and application tiers

    ![Int LB SG](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_61.png)

62. **Application Tier Security Group**
    - Created `4.App-Tier-SG` allowing custom TCP on port 4000 only from `3.Internal-Load-Balancer-SG`
    - Restricts application server access to internal load balancer only

    ![App Tier SG](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_62.png)

63. **Database Tier Security Group**
    - Created `5.DB-Tier-SG` allowing MySQL/Aurora (port 3306) only from `4.App-Tier-SG`
    - Most restrictive security layer, allowing database access only from application servers

    ![DB Tier SG](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_63.png)

64. **Complete Security Groups Overview**
    - Successfully created all five security groups
    - Implements layered security model with proper tier isolation

    ![All SGs](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_64.png)

### Phase 8: Database Tier Implementation

65. **DB Subnet Group Creation**
    - Initiated DB Subnet Group creation
    - Specify subnets where RDS database instances can be deployed

    ![DB Subnet Group](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_65.png)

66. **DB Subnet Group Configuration**
    - Configured `My-RDS-DB-Subnet-Group` with both Availability Zones
    - Selected DB-tier subnets for high availability database deployment

    ![DB Subnet Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_66.png)

67. **DB Subnet Group Creation Success**
    - Successfully created DB Subnet Group
    - Establishes foundation for Multi-AZ RDS deployment

    ![DB Subnet Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_67.png)

68. **RDS Aurora Database Creation**
    - Initiated Aurora MySQL database creation
    - Selected Aurora for high performance, availability, and MySQL compatibility

    ![RDS Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_68.png)

69. **Database Credentials Configuration**
    - Configured database cluster identifier as `My-RDS`
    - Set username as `admin` with strong password

    ![DB Credentials](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_69.png)

70. **Multi-AZ Database Configuration**
    - Enabled Multi-AZ deployment with Aurora Replica
    - Ensures high availability and automatic failover capability

    ![Multi-AZ Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_70.png)

71. **Database Network Configuration**
    - Configured database with `MY-VPC`
    - Used `My-RDS-DB-Subnet-Group` and `5.DB-Tier-SG` security group

    ![DB Network Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_71.png)

72. **Database Creation Initiation**
    - Finalized all database configuration settings
    - Initiated Aurora MySQL cluster creation process

    ![DB Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_72.png)

73. **Database Creation Success**
    - Successfully created `My-RDS` Aurora MySQL cluster with Multi-AZ
    - Provides highly available and scalable database foundation

    ![DB Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_73.png)

### Phase 9: Application Tier Development and Configuration

74. **Test Application Server Launch**
    - Launched `TEST-APP-SERVER` instance for application configuration
    - Used proper VPC subnet placement and security group configuration

    ![Test App Server](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_74.png)

75. **Application Server Network Configuration**
    - Configured `TEST-APP-SERVER` with `MY-VPC`
    - Used `APP-TIER-SUBNET-1` and `4.App-Tier-SG` security group

    ![App Server Network](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_75.png)

76. **IAM Role Assignment**
    - Assigned `abcd-ec2-s3-ssm-role` IAM instance profile
    - Enables secure access to S3 and Systems Manager

    ![IAM Role Assign](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_76.png)

77. **Session Manager Connection**
    - Connected to `TEST-APP-SERVER` using AWS Systems Manager Session Manager
    - Secure shell access without SSH keys or bastion hosts

    ![Session Manager](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_77.png)

78. **Session Manager Console Access**
    - Successfully established Session Manager connection
    - Confirmed proper IAM role configuration

    ![Session Console](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_78.png)

79. **Administrative Access Setup**
    - Switched to `ec2-user` with administrative privileges
    - Established proper user context for application installation

    ![Admin Access](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_79.png)

80. **MySQL Client Installation**
    - Successfully installed MySQL client on application server
    - Verified installation with `mysql --version` command

    ![MySQL Install](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_80.png)

81. **Database Connection Verification**
    - Successfully tested database connectivity between application server and RDS
    - Used RDS endpoint and admin credentials

    ![DB Connection](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_81.png)

82. **Application Code Deployment**
    - Downloaded application code from S3 bucket `mys3bucket025`
    - Transferred Node.js backend source code to application server

    ![App Code Deploy](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_82.png)

83. **Node.js Environment Setup**
    - Installed Node.js using Node Version Manager (NVM)
    - Configured version 16 and installed PM2 process manager

    ![Node Setup](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_83.png)

84. **Application Dependencies Installation**
    - Successfully installed application dependencies using `npm install`
    - Resolved security vulnerabilities with `npm audit fix`

    ![Dependencies Install](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_84.png)

85. **Application Service Startup**
    - Started Node.js application using PM2 process manager
    - Configured automatic startup on boot
    - Verified application health on port 4000

    ![App Startup](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_85.png)

### Phase 10: Application Tier Infrastructure Automation

86. **AMI Creation Process**
    - Initiated AMI creation from configured `TEST-APP-SERVER`
    - Capture complete application setup for automated deployment

    ![AMI Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_86.png)

87. **App Server AMI Configuration**
    - Created `App-Server-Image` AMI with descriptive metadata and tags
    - Establishes golden image for application server auto scaling

    ![AMI Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_87.png)

88. **AMI Creation Success**
    - Successfully created `App-Server-Image` AMI
    - Available in AMIs console for launch templates and auto scaling groups

    ![AMI Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_88.png)

89. **Target Group Creation**
    - Initiated target group creation for application load balancer
    - Define health check parameters and routing configuration

    ![Target Group](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_89.png)

90. **App Server Target Group Configuration**
    - Created `App-Server-TG` target group with port 4000 configuration
    - Configured health check settings and proper VPC association

    ![Target Group Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_90.png)

91. **Launch Template Creation**
    - Created `App-Server-Launch-Template` using custom AMI
    - Used `t2.micro` instance type, App-Tier security group, and IAM instance profile

    ![Launch Template](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_91.png)

92. **Launch Template Success**
    - Successfully created launch template with all required configurations
    - Enables consistent and automated application server deployment

    ![Launch Template Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_92.png)

93. **Internal Load Balancer Creation**
    - Initiated internal Application Load Balancer creation
    - Distribute traffic between web tier and application tier

    ![Int LB Creation](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_93.png)

94. **Internal ALB Configuration**
    - Configured `2-Internal-Load-Balancer` as internal ALB
    - Proper VPC, both Availability Zones, and appropriate security group

    ![Int ALB Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_94.png)

95. **Internal ALB Target Group Assignment**
    - Associated `App-Server-TG` target group with internal load balancer
    - Configured security group `3.Internal-Load-Balancer-SG`

    ![Int ALB Target](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_95.png)

96. **Test Web Server Launch**
    - Launched `TEST-WEB-SERVER` instance in web tier subnet
    - Proper security group configuration for web tier testing

    ![Test Web Server](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_96.png)

### Phase 11: SNS Notification Configuration

97. **App Server SNS Topic Creation**
    - Created SNS topic `App-Server-Notification`
    - Automated notifications for application server auto scaling events

    ![SNS Topic](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_97.png)

98. **SNS Topic Creation Success**
    - Successfully created `App-Server-Notification` topic
    - Establishes foundation for automated application tier monitoring

    ![SNS Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_98.png)

99. **Email Subscription Configuration**
    - Created email subscription to `App-Server-Notification` topic
    - Used `contactsabinrana@gmail.com` for receiving alerts

    ![Email Sub](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_99.png)

100. **Web Server SNS Topic**
     - Created `Web-Server-Notification` SNS topic
     - Web tier monitoring and alerting

     ![Web SNS](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_100.png)

101. **CloudWatch SNS Topic**
     - Created `CloudWatch-Notification` topic
     - General CloudWatch alarms and monitoring alerts

     ![CW SNS](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_101.png)

102. **Email Notifications Received**
     - Verified SNS subscription confirmation emails received
     - Confirmed proper email delivery configuration

     ![Email Notifications](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_102.png)

103. **SNS Subscription Confirmation**
     - Opened SNS subscription confirmation email
     - Showed proper topic configuration and subscription details

     ![SNS Confirm](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_103.png)

104. **Email Subscription Confirmed**
     - Successfully confirmed SNS email subscription
     - Enabled automated notifications for infrastructure events

     ![Email Confirm](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_104.png)

105. **All Subscriptions Active**
     - Verified all three SNS topic subscriptions confirmed and active
     - Comprehensive monitoring coverage

     ![All Subs Active](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_105.png)

### Phase 12: Auto Scaling Group Implementation

106. **App Server ASG Configuration**
     - Configured `APP-SERVER-ASG` with target tracking scaling policy at 50% CPU utilization
     - Notifications enabled and proper health check configuration

     ![ASG Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_106.png)

107. **Auto Scaling Group Success**
     - Successfully created `APP-SERVER-ASG`
     - Minimum capacity of 2 instances, maximum of 4 instances
     - Proper tagging for resource management

     ![ASG Success](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_107.png)

108. **ASG Instance Launch**
     - Auto Scaling Group automatically launched new application server instances
     - Successful automated deployment based on launch template

     ![ASG Launch](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_108.png)

### Phase 13: Web Tier Implementation and Testing

109. **Web Server Code Deployment**
     - Downloaded web tier code from S3 bucket to `TEST-WEB-SERVER`
     - Transferred React frontend source code and NGINX configuration files

     ![Web Code Deploy](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_109.png)

110. **Web Server File Permissions**
     - Set proper file ownership and permissions using `chown` and `chmod`
     - Ensured secure file access and proper web server operation

     ![File Permissions](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_110.png)

111. **Node.js and React Setup**
     - Installed Node.js environment and React dependencies
     - Prepared environment for React application build and deployment

     ![React Setup](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_111.png)

112. **React Application Build**
     - Successfully executed `npm run build`
     - Created optimized production build of React application

     ![React Build](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_112.png)

113. **NGINX Web Server Configuration**
     - Installed NGINX web server
     - Replaced default configuration with custom configuration from S3
     - Started NGINX service with automatic startup configuration

     ![NGINX Config](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_113.png)

114. **Application Frontend Testing**
     - Accessed the deployed 3-tier application through web browser
     - Confirmed end-to-end functionality with all tiers integrated

     ![Frontend Test](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_114.png)

115. **VPC Flow Logs Verification**
     - Verified VPC Flow Logs successfully captured and stored in S3 bucket
     - Demonstrated comprehensive network traffic monitoring capability

     ![Flow Logs Verify](https://github.com/Sabin-Rana/aws-3tier-architecture/blob/main/Screenshots/AWS_3Tier_Step_115.png)

## Additional Implementation Details

### NGINX Configuration and Internal Load Balancer Integration
After creating the Internal Load Balancer, a critical step was performed to update the NGINX configuration file. The DNS name from `2-Internal-Load-Balancer` was copied and updated in the `nginx.conf` file at line 58. This file was then re-uploaded to the S3 bucket `mys3bucket025`, replacing the old configuration.

# Web Tier Auto Scaling Implementation

For complete web tier automation, the following components were implemented:


## Web Server AMI Creation
* Created AMI from **TEST-WEB-SERVER** after successful configuration  
* Included all NGINX configurations and React build  
* Tagged appropriately for identification  

## Web Tier Launch Template
* Used **Web-Server-Image AMI**  
* Configured with **2.Web-Tier-SG** security group  
* Included IAM instance profile for **S3** and **SSM** access  
* Set proper instance type and configuration  

## External Load Balancer
* Internet-facing **Application Load Balancer**  
* Associated with public subnets (**WEB-TIER-SUBNET-1 and 2**)  
* Used **1.External-Load-Balancer-SG** security group  
* Configured with **Web-Server target group**  

# Security Enhancements
During testing phase, the **2.Web-Tier-SG** security group was temporarily modified to allow HTTP traffic from a specific IP address for testing purposes. This was done to verify frontend application functionality before implementing the complete load balancer setup.

## Security Group Rule Added
* Type: HTTP  
* Port: 80  
* Source: My IP (temporary for testing)  
* Purpose: Direct access testing before load balancer implementation  

**Important Security Note:** This temporary rule was removed once the External Load Balancer was properly configured and testing was complete. The final configuration only allows traffic from the External Load Balancer security group.  

# Troubleshooting Report: Real-World Implementation Challenges

## Challenge 1: IAM Role Not Appearing in EC2 Instance Profile Dropdown
* **Issue:** The IAM role `abcd-ec2-s3-ssm-role-025-9-14` was not appearing in the EC2 instance profile dropdown menu.  
* **Root Cause:** EC2 instances require an instance profile, which serves as a container for the IAM role.  

**Resolution Steps:**
```bash
# Create Instance Profile
aws iam create-instance-profile --instance-profile-name abcd-ec2-s3-ssm-profile

# Attach Role to Instance Profile
aws iam add-role-to-instance-profile \
  --instance-profile-name abcd-ec2-s3-ssm-profile \
  --role-name abcd-ec2-s3-ssm-role-025-9-14

# Verify Configuration
aws iam get-instance-profile --instance-profile-name abcd-ec2-s3-ssm-profile
```
* **Outcome:** ✅ The instance profile appeared in the EC2 dropdown menu, enabling successful IAM role attachment.  

---

## Challenge 2: EC2 Instances Not Registering with Systems Manager
* **Issue:** EC2 instances were not appearing as Online in AWS Systems Manager despite correct IAM role.  
* **Root Cause:** Multi-faceted issue involving SSM Agent configuration, network connectivity, and service registration timing.  

**Resolution Steps:**
* IAM Role Verification: Confirmed required policies were attached  
* SSM Agent Status Check:
```bash
sudo systemctl status amazon-ssm-agent
sudo systemctl start amazon-ssm-agent
sudo systemctl enable amazon-ssm-agent
```
* Network Connectivity Validation: Verified private subnet instances could reach SSM endpoints  
* Instance Profile Verification: Confirmed IAM role attachment from instance metadata  
* SSM Agent Logs Analysis: Examined agent logs for registration issues  

* **Outcome:** ✅ Instances registered successfully with Systems Manager within 5 minutes after network configuration validation and SSM agent restart.  

---

## Challenge 3: Public IP Website Timeout Issues
* **Issue:** Accessing web application via public IP address returned connection timeout errors.  
* **Root Cause:** Network Access Control Lists (NACLs) were configured with default deny rules, blocking HTTP traffic.  

**Resolution:**
* Updated NACL inbound rules to explicitly allow HTTP/HTTPS traffic  

* **Outcome:** ✅ Web application became accessible via public IP after NACL configuration correction.  

# Cost Management and Billing Awareness

## Cost Optimization Strategies Implemented
* **Resource Right-Sizing:** Utilized t2.micro instances appropriate for workload  
* **Storage Optimization:** Configured appropriate EBS volume sizes  
* **Network Efficiency:** Placed resources in same AZ where possible  
* **Resource Tagging:** All resources tagged for cost tracking and management  

## Estimated Monthly Costs
* EC2 Instances (t2.micro): **$51-85/month** (2-4 instances per tier)  
* RDS Aurora MySQL: **~$45-60/month**  
* Load Balancers: **~$44/month** (internal + external)  
* NAT Gateway: **~$45/month per gateway**  
* Data Transfer: Variable based on usage  
* S3 Storage: **<$1/month** for application code  

**Estimated Total:** **$150-250/month** for development environment  

# Usage Instructions

## Accessing the Application
* Navigate to the **External Load Balancer DNS name**  
* Application automatically routes through all tiers:  
  * Frontend served by **NGINX from React build**  
  * API calls routed through **Internal Load Balancer to Node.js**  
  * Database queries handled by **Aurora MySQL cluster**  

## Monitoring and Maintenance
* **CloudWatch Dashboards:** Monitor CPU, memory, and network metrics  
* **SNS Notifications:** Receive alerts for scaling events  
* **VPC Flow Logs:** Analyze network traffic patterns  
* **Session Manager:** Secure access to instances for maintenance  

## Scaling Operations
* **Auto Scaling:** Automatic scaling based on CPU utilization  
* **Manual Scaling:** Adjust ASG capacity as needed  
* **Database Scaling:** Add read replicas for read-heavy workloads  

# Support and Resources
* **AWS Documentation:** [3-Tier Architecture on AWS](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/architecting-for-the-cloud.html)  
* **Web Server Setup:** [AWS EC2 NGINX + Node.js Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html)  
* **Security Guidelines:** [AWS Well-Architected Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)  
* **Monitoring Best Practices:** [Amazon CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)  

# Project Status
* **Architecture Validation:** ✅ Multi-AZ 3-Tier Implementation Verified  
* **Security Review:** ✅ Completed  
* **Documentation:** ✅ Comprehensive Implementation Guide  
* **Production Ready:** ✅  
