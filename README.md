# Zabbix AWS Monitoring Infrastructure - Centralized Cloud Supervision

## 📋 Project Overview
Deployment of a centralized cloud monitoring infrastructure on AWS using containerized Zabbix for hybrid environment monitoring (Linux & Windows systems).

## 🏗️ Architecture

### AWS Infrastructure
- **VPC**: Single VPC with public subnet for simplified access
- **Security Groups**: Configured for Zabbix web interface (80/443), agents (10050/10051), and management (SSH/RDP)

### EC2 Instances
| Instance | Type | OS | Purpose | Specifications |
|----------|------|----|---------|----------------|
| Zabbix Server | t3.large | Ubuntu 22.04 | Monitoring server with Docker | 2 vCPU, 8GB RAM |
| Linux Client | t3.medium | Ubuntu 22.04 | Monitored Linux host | 2 vCPU, 4GB RAM |
| Windows Client | t3.large | Windows Server | Monitored Windows host | 2 vCPU, 8GB RAM |

### Container Stack
- **Zabbix Server**: MySQL-based Zabbix server container
- **Zabbix Web**: Nginx + PHP web interface
- **MySQL Database**: MySQL 8.0 for Zabbix data storage
- **Zabbix Agent**: Containerized agent for server self-monitoring

## 🚀 Quick Start

### Access Zabbix Interface
**URL**: `http://[PUBLIC_IP]/`  
**Default Credentials**: 
- Username: `Admin`
- Password: `zabbix`

### Current Deployment Status
- **Public IP**: `3.83.89.153`
- **Access URL**: `http://3.83.89.153/`
- **Container Status**: Running (3 containers)

## 🔧 Installation & Configuration

### 1. AWS Infrastructure Setup
```bash
# VPC Creation with public subnet
# Security Groups configuration for ports 80, 443, 10051, 22, 3389
# EC2 instances deployment with appropriate IAM roles
```

### 2. Zabbix Server Deployment
```bash
# On Zabbix Server EC2 instance
git clone https://github.com/zabbix/zabbix-docker
cd zabbix-docker

# Deploy with Docker Compose
docker-compose up -d
```

### 3. Agent Installation

#### Linux Client:
```bash
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo apt update
sudo apt install zabbix-agent2
```

#### Windows Client:
- Download Zabbix Agent 2 MSI installer
- Install with Server=`[ZABBIX_SERVER_IP]`
- Configure firewall for port 10050

## 📊 Monitoring Configuration

### Host Registration
1. Log into Zabbix web interface (`http://[IP]/`)
2. Navigate to **Configuration → Hosts**
3. Add new hosts with:
   - Host name: Client hostname
   - Visible name: Descriptive name
   - Groups: Linux servers / Windows servers
   - Agent interfaces: Client IP on port 10050

### Templates Applied
- **Linux**: Template OS Linux by Zabbix agent
- **Windows**: Template OS Windows by Zabbix agent 2
- **Network Devices**: ICMP Ping for availability monitoring

## 🛠️ Technical Details

### Docker Compose Configuration
```yaml
version: '3.8'
services:
  zabbix-server:
    image: zabbix/zabbix-server-mysql:ubuntu-6.4-latest
    ports: ["10051:10051"]
    environment: [DB settings...]
    
  zabbix-web:
    image: zabbix/zabbix-web-nginx-mysql:ubuntu-6.4-latest
    ports: ["80:8080", "443:8443"]
    
  zabbix-db:
    image: mysql:8.0
    volumes: [mysql_data:/var/lib/mysql]
```

### Security Configuration
- **AWS Security Groups**: Restricted access to required ports only
- **Zabbix Authentication**: Strong passwords, user role management
- **Database**: Password-protected MySQL with encrypted connections

## 📈 Features Implemented

### ✅ Completed
- [x] AWS VPC and network configuration
- [x] Dockerized Zabbix server deployment
- [x] Linux client monitoring setup
- [x] Basic dashboard creation
- [x] Real-time metrics collection
- [x] Alert configuration for critical services

### 🔄 In Progress
- [ ] Windows client integration
- [ ] Advanced dashboard customization
- [ ] Email/SMS notification setup
- [ ] Automated reporting

## 🚨 Troubleshooting

### Common Issues & Solutions

1. **Zabbix Web Interface Not Accessible**
   ```bash
   # Check container status
   docker ps
   docker logs zabbix-web
   
   # Verify security groups
   # Check AWS Security Group for port 80 access
   ```

2. **Agents Not Reporting**
   ```bash
   # Check agent connectivity
   telnet [ZABBIX_SERVER_IP] 10051
   
   # Verify agent configuration
   cat /etc/zabbix/zabbix_agent2.conf
   ```

3. **High Resource Usage**
   - Adjust Housekeeper settings in Zabbix
   - Review history and trend storage periods
   - Consider database optimization

## 📁 Project Structure
```
zabbix-aws-monitoring/
├── docker-compose.yml          # Main deployment file
├── README.md                   # This file
├── config/
│   ├── zabbix_server.conf      # Server configuration
│   └── zabbix_web.conf         # Web interface config
├── scripts/
│   ├── deploy.sh               # Automated deployment
│   └── backup.sh               # Backup procedures
└── docs/
    ├── architecture.png        # Infrastructure diagram
    └── setup_guide.md          # Detailed setup instructions
```

## 🔗 Useful Links

### Zabbix Resources
- [Official Zabbix Documentation](https://www.zabbix.com/documentation)
- [Zabbix Docker Repository](https://github.com/zabbix/zabbix-docker)
- [Zabbix Templates Library](https://share.zabbix.com/)

### AWS Resources
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS VPC Guide](https://docs.aws.amazon.com/vpc/)
- [AWS Security Best Practices](https://docs.aws.amazon.com/security/)

## 👥 Team & Contact
- **Student**: [Your Name]
- **Supervisor**: Prof. Azeddine KHIAT
- **Academic Year**: 2025/2026
- **Program**: [Your Program Name]

## 📝 License & Acknowledgments
This project is developed for educational purposes as part of the academic curriculum. All trademarks and logos are the property of their respective owners.

---

*Last Updated: January 2026*  
*Project Status: Active Deployment*# zabbix-aws-monitoring
