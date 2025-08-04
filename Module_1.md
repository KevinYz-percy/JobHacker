# Phase 1: Environment Setup & Foundation

**Duration:** 1 week  
**Focus:** Development Environment & Core Infrastructure

## Learning Objectives

By the end of this phase, students will:

1. **Master Development Environment Setup** - Configure a professional cloud development environment with proper isolation and compatibility
2. **Understand AWS Fundamentals** - Set up AWS services and understand IAM roles, permissions, and security best practices
3. **Learn Database Architecture** - Set up and configure PostgreSQL database with proper schema design
4. **Practice Infrastructure as Code** - Use command-line tools and scripts for reproducible environment setup
5. **Establish Development Workflows** - Create proper version control, dependency management, and deployment practices

## Course Modules

### Module 1.1: Development Environment Configuration
**Time:** 2 days | **Complexity:** Beginner

#### What You'll Build:
- Isolated Python virtual environment with Lambda-compatible packages
- AWS CLI configuration with proper credentials and regions
- Development toolchain (Git, PostgreSQL client, code editors)

#### Learning Outcomes:
- Understand Python virtual environments and dependency isolation
- Learn why package compatibility matters for serverless deployments
- Master AWS CLI configuration and credential management
- Practice command-line development workflows

#### High-Level Requirements:
- Set up isolated Python development environment
- Configure AWS CLI with proper authentication
- Install Lambda-compatible package dependencies
- Set up PostgreSQL client tools and database connectivity
- Initialize version control with appropriate exclusions

#### Key Technical Concepts:
- **Virtual Environments**: Isolation prevents package conflicts between projects
- **Lambda Compatibility**: Linux packages required for AWS Lambda deployment
- **AWS Regions**: Geographic distribution affects latency and compliance
- **IAM Security**: Principle of least privilege for API access

#### Deliverables:
1. **Environment Setup Script** - Automated setup for team consistency
2. **Configuration Verification** - Tests to validate all components work
3. **Documentation** - Setup guide for future team members

---

### Module 1.2: AWS Infrastructure Foundation
**Time:** 2 days | **Complexity:** Intermediate

#### What You'll Build:
- IAM roles and policies for Lambda functions
- Basic understanding of AWS service integration
- Security best practices for API keys and credentials

#### Learning Outcomes:
- Understand AWS Identity and Access Management (IAM)
- Learn serverless architecture principles
- Practice security best practices for cloud development
- Master AWS service integration concepts

#### High-Level Requirements:
```bash
# AWS Infrastructure Checklist
✅ IAM user created with programmatic access
✅ IAM role 'JobCollection' created for Lambda functions
✅ Policies attached: Lambda, SQS, RDS access
✅ AWS credentials configured in ~/.aws/
✅ Test API calls successful (aws sts get-caller-identity)
```

#### Key Technical Concepts:
- **IAM Roles vs Users**: Service authentication vs human authentication
- **Policy Attachments**: Granular permissions for specific AWS services
- **Credential Management**: Environment variables vs credential files
- **Service Integration**: How AWS services communicate securely

#### Deliverables:
1. **IAM Configuration Script** - Automated role and policy creation
2. **Security Audit Checklist** - Validate security best practices
3. **Access Testing Suite** - Verify all required permissions work

---

### Module 1.3: Database Architecture & Setup
**Time:** 2 days | **Complexity:** Intermediate

#### What You'll Build:
- PostgreSQL database on Neon cloud platform
- Database schema for job data storage
- Connection management and testing utilities

#### Learning Outcomes:
- Understand modern cloud database architectures
- Learn database schema design for scalable applications
- Practice connection pooling and error handling
- Master database security and access control

#### High-Level Requirements:
- Create cloud PostgreSQL database instance
- Design schema for job data storage with appropriate data types
- Implement proper indexing strategy for query performance
- Configure secure database connections and credential management
- Set up connection pooling for serverless environment compatibility

#### Key Technical Concepts:
- **Cloud Databases**: Managed vs self-hosted database trade-offs
- **Connection Pooling**: Managing database connections in serverless environments
- **Schema Design**: Indexing, constraints, and performance considerations
- **Security**: SSL connections, credential management, network access

#### Database Design Requirements:
Your job storage system should include:
- Primary job information (title, company, description, location)
- Metadata fields (posting date, source URL, timestamps)
- Appropriate constraints and data validation
- Indexes for common query patterns
- Support for future AI analysis result storage

#### Deliverables:
1. **Database Schema Script** - Complete table creation and indexing
2. **Connection Manager** - Reusable database connection utilities
3. **Testing Suite** - Database connectivity and performance tests

---

### Module 1.4: Development Workflow & Project Structure
**Time:** 1 day | **Complexity:** Beginner

#### What You'll Build:
- Standardized project directory structure
- Git workflow with proper branching and commit practices
- Environment variable management system
- Code quality tools and pre-commit hooks

#### Learning Outcomes:
- Establish professional development workflows
- Learn version control best practices for team collaboration
- Understand configuration management and secrets handling
- Practice code quality automation

#### High-Level Requirements:
Design and implement a professional project structure that includes:
- Organized directory structure for different component types
- Proper separation of source code, tests, and deployment artifacts
- Environment variable management and security
- Version control configuration with appropriate exclusions
- Documentation and deployment automation scripts

#### Key Technical Concepts:
- **Project Structure**: Organizing code for scalability and maintainability
- **Environment Variables**: Separating configuration from code
- **Git Workflows**: Feature branches, commit messages, and team collaboration
- **Code Quality**: Automated formatting, linting, and testing

#### Deliverables:
1. **Project Template** - Standardized directory structure and files
2. **Git Workflow Guide** - Branching strategy and commit conventions
3. **Development Scripts** - Automation for common development tasks

---

## Assessment Criteria

### Technical Proficiency (70%)
- **Environment Setup** (25%): All tools installed and functional
- **AWS Configuration** (25%): Proper IAM setup and service access
- **Database Setup** (20%): Working PostgreSQL connection and schema

### Professional Practices (30%)
- **Documentation** (15%): Clear setup instructions and troubleshooting guides
- **Code Organization** (15%): Proper project structure and version control

###a
1. **Practical Demonstration**: Students demonstrate working environment
2. **Code Review**: Assess project structure and configuration quality
3. **Troubleshooting Exercise**: Resolve intentionally introduced environment issues

## Common Challenges & Solutions

### Challenge 1: Package Compatibility Issues
**Problem**: Python packages fail in Lambda due to architecture differences
**Solution**: Use `--platform manylinux2014_x86_64` flag for Linux compatibility
**Learning**: Understand deployment environment requirements

### Challenge 2: AWS Credential Configuration
**Problem**: AWS CLI access denied or credential errors
**Solution**: Verify IAM policies and credential file format
**Learning**: Debug cloud authentication and authorization

### Challenge 3: Database Connection Failures
**Problem**: PostgreSQL connection timeouts or SSL errors
**Solution**: Check network settings, SSL requirements, and connection strings
**Learning**: Troubleshoot cloud database connectivity
