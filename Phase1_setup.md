# Job Pipeline Setup Guide - Phase 1

## Overview
This guide provides step-by-step instructions for setting up the development environment for the Job Pipeline project on Mac/Linux systems. This covers Phase 1 prerequisites before beginning pipeline development.

## System Requirements
- macOS 10.15+ or Linux (Ubuntu 18.04+)
- Internet connection for package downloads
- Administrative privileges for system installations

## 1. Development Environment Setup

### 1.1 Install System Prerequisites

**For macOS:**
```bash
# Install Homebrew (package manager)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Python 3.8 (Lambda compatibility requirement)
brew install python@3.8

# Install AWS CLI
brew install awscli

# Install PostgreSQL client tools
brew install postgresql

# Install Git (if not already installed)
brew install git
```

**For Linux (Ubuntu/Debian):**
```bash
# Update package manager
sudo apt-get update

# Install Python 3.8 and venv
sudo apt-get install python3.8 python3.8-venv python3.8-dev

# Install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
rm -rf aws awscliv2.zip

# Install PostgreSQL client
sudo apt-get install postgresql-client

# Install Git
sudo apt-get install git

# Install build tools for Python packages
sudo apt-get install build-essential
```

### 1.2 Verify Installations
```bash
# Verify Python version (should be 3.8.x)
python3.8 --version

# Verify AWS CLI
aws --version

# Verify PostgreSQL client
psql --version

# Verify Git
git --version
```

## 2. Python Environment Setup

### 2.1 Create Virtual Environment
```bash
# Navigate to project directory
cd /path/to/Job_pipeline

# Create isolated Python environment (Lambda compatibility)
python3.8 -m venv wayrocProject

# Activate virtual environment
source wayrocProject/bin/activate

# Verify Python version in environment
python --version  # Should show 3.8.x

# Upgrade pip to latest version
pip install --upgrade pip
```

### 2.2 Install Core Dependencies
```bash
# Make sure virtual environment is activated
source wayrocProject/bin/activate

# Install base AWS and database packages
pip install boto3==1.34.0
pip install psycopg2-binary==2.9.9
pip install mysql-connector-python==8.2.0

# Install OpenAI API client
pip install openai==1.3.0

# Install additional utilities
pip install requests==2.31.0
pip install python-dotenv==1.0.0
```

### 2.3 Lambda-Compatible Package Installation
```bash
# Install packages compatible with AWS Lambda Linux environment
# This is crucial for deployment compatibility
pip install -r requirements.txt \
    --platform manylinux2014_x86_64 \
    --target ./package \
    --implementation cp \
    --only-binary=:all: \
    --python-version 3.8

# Create requirements.txt if it doesn't exist
cat > requirements.txt << EOF
boto3==1.34.0
psycopg2-binary==2.9.9
mysql-connector-python==8.2.0
openai==1.3.0
requests==2.31.0
python-dotenv==1.0.0
EOF
```

## 3. AWS Configuration

### 3.1 AWS Account Setup
1. Create AWS account at [aws.amazon.com](https://aws.amazon.com)
2. Create IAM user with programmatic access
3. Attach policies: `AWSLambdaFullAccess`, `AmazonSQSFullAccess`, `AmazonRDSFullAccess`

### 3.2 Configure AWS CLI
```bash
# Configure AWS credentials
aws configure

# Enter when prompted:
# AWS Access Key ID: [Your access key]
# AWS Secret Access Key: [Your secret key]
# Default region name: us-east-1
# Default output format: json

# Verify configuration
aws sts get-caller-identity
```

### 3.3 Create IAM Role for Lambda
```bash
# Create trust policy for Lambda
cat > trust-policy.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF

# Create IAM role
aws iam create-role \
    --role-name JobCollection \
    --assume-role-policy-document file://trust-policy.json

# Attach basic Lambda execution policy
aws iam attach-role-policy \
    --role-name JobCollection \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Clean up
rm trust-policy.json
```

## 4. Database Setup

### 4.1 Neon PostgreSQL Setup
1. Sign up at [neon.tech](https://neon.tech)
2. Create new project: "job-pipeline"
3. Create database: "job_data"
4. Note connection details:
   - Host
   - Database name
   - Username
   - Password
   - Port (usually 5432)

### 4.2 Database Connection Testing
```bash
# Test connection using psql (replace with your credentials)
psql "postgresql://username:password@host:5432/job_data?sslmode=require"

# Or create a test script
cat > test_connection.py << EOF
import psycopg2
import os

# Replace with your Neon credentials
conn_params = {
    'host': 'your-host.neon.tech',
    'database': 'job_data',
    'user': 'your-username',
    'password': 'your-password',
    'port': 5432,
    'sslmode': 'require'
}

try:
    conn = psycopg2.connect(**conn_params)
    print("✅ Database connection successful!")
    conn.close()
except Exception as e:
    print(f"❌ Database connection failed: {e}")
EOF

python test_connection.py
```

## 5. External API Keys Setup

### 5.1 Apify API Key
1. Sign up at [apify.com](https://apify.com)
2. Go to Settings → Integrations → API tokens
3. Generate new token
4. Note token for environment variables

### 5.2 OpenAI API Key
1. Sign up at [platform.openai.com](https://platform.openai.com)
2. Go to API Keys section
3. Create new secret key
4. Note key for environment variables

### 5.3 Environment Variables Setup
```bash
# Create .env file for local development
cat > .env << EOF
# Database Configuration
NEON_HOST=your-host.neon.tech
NEON_DATABASE=job_data
NEON_USER=your-username
NEON_PASSWORD=your-password
NEON_PORT=5432

# API Keys
APIFY_API_KEY=your-apify-key
OPENAI_API_KEY=your-openai-key

# AWS Configuration
AWS_REGION=us-east-1
EOF

# Add .env to .gitignore
echo ".env" >> .gitignore
```

## 6. Project Structure Setup

### 6.1 Create Directory Structure
```bash
# Create necessary directories
mkdir -p {logs,package,tests,scripts}

# Verify structure
tree -L 2
```

### 6.2 Git Repository Setup
```bash
# Initialize git repository (if not already done)
git init

# Initial commit
git add .
git commit -m "Initial project setup"
```

## 7. Development Tools (Optional)

### 7.1 Code Quality Tools
```bash
# Install development tools
pip install black==23.12.0     # Code formatter
pip install flake8==6.1.0      # Linter
pip install pytest==7.4.3      # Testing framework
```

### 7.2 VS Code Extensions (Recommended)
- Python
- AWS Toolkit
- PostgreSQL
- GitLens

## 8. Verification Checklist

### System Setup
- [ ] Python 3.8+ installed
- [ ] AWS CLI installed and configured
- [ ] PostgreSQL client installed
- [ ] Git installed

### Python Environment
- [ ] Virtual environment created (`wayrocProject`)
- [ ] Virtual environment activated
- [ ] Core dependencies installed
- [ ] Lambda-compatible packages installed

### AWS Configuration
- [ ] AWS credentials configured
- [ ] AWS CLI access verified
- [ ] IAM role created for Lambda functions

### Database Setup
- [ ] Neon PostgreSQL account created
- [ ] Database created and accessible
- [ ] Connection tested successfully

### API Access
- [ ] Apify API key obtained
- [ ] OpenAI API key obtained
- [ ] Environment variables configured

### Project Structure
- [ ] Directory structure created
- [ ] Git repository initialized
- [ ] .gitignore configured
- [ ] Environment file created

## Troubleshooting

### Common Issues

**Python Version Conflicts:**
```bash
# If system has multiple Python versions
which python3.8
/usr/local/bin/python3.8 -m venv wayrocProject
```

**AWS CLI Configuration Issues:**
```bash
# Check AWS configuration
aws configure list
cat ~/.aws/credentials
cat ~/.aws/config
```

**Database Connection Issues:**
```bash
# Test with verbose logging
psql "postgresql://user:pass@host:5432/db?sslmode=require" -c "SELECT version();"
```

**Package Installation Issues:**
```bash
# Clear pip cache
pip cache purge

# Reinstall with verbose output
pip install -v boto3
```

## Next Steps

After completing this setup:

1. Verify all components with the checklist above
2. Run test scripts to ensure connectivity
3. Proceed to Phase 2: Data Collection Pipeline development
4. Begin implementing `job_collector.py` Lambda function

## Support

For issues during setup:
1. Check AWS documentation for service-specific problems
2. Refer to Neon documentation for database issues  
3. Consult Python packaging guides for dependency problems
4. Review project CLAUDE.md for additional context
