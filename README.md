# CloudLaunch - AWS Implementation Project

## 🌟 Project Overview
CloudLaunch is a lightweight platform demonstrating AWS core services including S3 static website hosting, IAM access controls, and VPC network design for AltSchool Cloud Engineering Third Semester.

## 🔗 Live Demo
- **Custom Website URL**: `http://cloudlaunch.gra8sal.xyz`
- **S3 Static Site Link**: `http://cloudlaunch-site-bucket.s3-website-eu-west-1.amazonaws.com`
- **Status**: ✅ Live and functional

## 🏗️ Architecture Implementation

### Task 1: S3 Static Website Hosting + IAM

#### S3 Buckets Created:
1. **`cloudlaunch.gra8sal.xyz`** 
   - Purpose: Public static website hosting
   - Access: Public read-only via bucket policy
   - Features: Static website hosting enabled

2. **`cloudlaunch-private-bucket-salskhal`**
   - Purpose: Private document storage
   - Access: Limited to cloudlaunch-user (GetObject, PutObject only)

3. **`cloudlaunch-visible-only-bucket-salskhal`**
   - Purpose: Demonstration of list-only permissions
   - Access: Visible to cloudlaunch-user but content not accessible

#### IAM Configuration:
**User**: `cloudlaunch-user`

**Custom Policy JSON**:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAllBuckets",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        },
        {
            "Sid": "ListSpecificBuckets",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": [
                "arn:aws:s3:::cloudlaunch.gra8sal.xyz",
                "arn:aws:s3:::cloudlaunch-private-bucket-[YOUR-UNIQUE-ID]",
                "arn:aws:s3:::cloudlaunch-visible-only-bucket-[YOUR-UNIQUE-ID]"
            ]
        },
        {
            "Sid": "GetObjectSiteBucket",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloudlaunch.gra8sal.xyz/*"
        },
        {
            "Sid": "GetPutObjectPrivateBucket",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-private-bucket-[YOUR-UNIQUE-ID]/*"
        },
        {
            "Sid": "VPCReadOnlyAccess",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeVpcs",
                "ec2:DescribeSubnets",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeInternetGateways"
            ],
            "Resource": "*"
        }
    ]
}
```
### Task 2: VPC Network Design

#### VPC Configuration:
- **VPC Name**: `cloudlaunch-vpc`
- **CIDR Block**: `10.0.0.0/16`
- **Region**: eu-west-1

#### Subnets:
1. **Public Subnet**: `cloudlaunch-public-subnet`
   - CIDR: `10.0.1.0/24`
   - Purpose: Load balancers, public-facing services
   - Route: Internet Gateway access

2. **Application Subnet**: `cloudlaunch-app-subnet`
   - CIDR: `10.0.2.0/24` 
   - Purpose: Application servers (private)
   - Route: No internet access

3. **Database Subnet**: `cloudlaunch-db-subnet`
   - CIDR: `10.0.3.0/28`
   - Purpose: Database services (private)
   - Route: No internet access

#### Network Components:
- **Internet Gateway**: `cloudlaunch-igw`
- **Route Tables**: 
  - `cloudlaunch-public-rt` (with 0.0.0.0/0 → IGW)
  - `cloudlaunch-app-rt` (private)
  - `cloudlaunch-db-rt` (private)

#### Security Groups:
1. **`cloudlaunch-app-sg`**
   - Inbound: HTTP (80) from VPC CIDR (10.0.0.0/16)

2. **`cloudlaunch-db-sg`**
   - Inbound: MySQL (3306) from App Subnet (10.0.2.0/24)

## 🔐 AWS Account Access Information

**⚠️ FOR EVALUATOR ONLY**

- **AWS Account ID**: `637304539683`
- **Console URL**: `https://637304539683.signin.aws.amazon.com/console`
- **IAM Username**: `cloudlaunch-user`
- **Temporary Password**: `Password12$`
- **⚠️ Note**: User must change password on first login

## 🛡️ Security Features Implemented

- ✅ **Least Privilege IAM**: User has minimal required permissions only
- ✅ **Network Segmentation**: Private subnets isolated from internet
- ✅ **Bucket Policies**: Controlled public access to website content
- ✅ **Security Groups**: Restricted port access between tiers
- ✅ **Resource Isolation**: Separate access controls per resource type

## 🧪 Testing Performed

### S3 & IAM Testing:
- ✅ Website accessible at cloudlaunch.gra8sal.xyz
- ✅ IAM user can list all three buckets
- ✅ IAM user can upload/download from private bucket
- ✅ IAM user cannot access visible-only bucket contents
- ✅ IAM user cannot delete objects from any bucket

### VPC Testing:
- ✅ VPC and all components visible to IAM user
- ✅ Public subnet has internet gateway route
- ✅ Private subnets have no internet access
- ✅ Security groups properly configured

## 🎯 Project Achievements

### Core Requirements Met:
- [x] S3 static website hosting with custom domain
- [x] IAM user with custom JSON policy
- [x] Three S3 buckets with different access levels
- [x] VPC with proper network segmentation
- [x] Security groups for application tiers
- [x] Comprehensive documentation

### Additional Features:
- [x] Custom domain integration (cloudlaunch.gra8sal.xyz)
- [x] Modern responsive website design
- [x] Proper DNS configuration via Hostinger
- [x] Professional project documentation

## 📊 Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                  cloudlaunch-vpc                        │
│                   10.0.0.0/16                          │
│                                                         │
│  ┌─────────────────┐  ┌─────────────────┐              │
│  │ Public Subnet   │  │ App Subnet      │              │
│  │ 10.0.1.0/24     │  │ 10.0.2.0/24     │              │
│  │ (via IGW)       │  │ (private)       │              │
│  └─────────────────┘  └─────────────────┘              │
│                                                         │
│         ┌─────────────────┐                            │
│         │ DB Subnet       │                            │
│         │ 10.0.3.0/28     │                            │
│         │ (private)       │                            │
│         └─────────────────┘                            │
└─────────────────────────────────────────────────────────┘
```

## 🔧 Technologies Used

- **AWS S3**: Static website hosting, object storage
- **AWS IAM**: User access management, custom policies
- **AWS VPC**: Network isolation and segmentation
- **AWS EC2**: Security groups and network ACLs
- **DNS**: Hostinger domain management
- **Frontend**: HTML5, CSS3, JavaScript (ES6)

## 🏫 Academic Context

**Course**: AltSchool of Engineering - Cloud Engineering
**Semester**: Third Semester
**Assignment**: AWS Fundamentals Implementation
**Focus**: S3, IAM, and VPC core services demonstration

---

- **Submitted by**: Salman-Yusuf Khalid Olaniyi
- **Project Repository**: https://github.com/salskhal/cloudlaunch-aws-project
