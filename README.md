# Ansible RoboShop Deployment

Automated deployment of RoboShop e-commerce application using Ansible. This project contains playbooks to deploy and configure all RoboShop microservices and their dependencies.

## 🏗️ Architecture

RoboShop is a microservices-based e-commerce platform consisting of:

### Application Services
- **Frontend** - Nginx-based web interface
- **Catalogue** - Product catalog service (Node.js)
- **Cart** - Shopping cart service (Node.js)
- **User** - User management service (Node.js)
- **Shipping** - Order shipping service (Java)
- **Payment** - Payment processing service (Python)

### Backend Services
- **MongoDB** - Database for catalogue and user services
- **MySQL** - Database for shipping service
- **Redis** - Session management and caching
- **RabbitMQ** - Message queue for asynchronous communication

## 📋 Prerequisites

- Ansible 2.9 or higher installed on control node
- Python 3.x
- SSH access to target servers
- Target servers running RHEL/CentOS 8 or similar
- Root or sudo access on target servers

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/mepavankumar15/ansible-roboshop.git
cd ansible-roboshop
```

### 2. Configure Inventory
Edit `inventory.ini` to add your server IPs/hostnames:
```ini
[frontend]
frontend.example.com

[catalogue]
catalogue.example.com

[cart]
cart.example.com

[user]
user.example.com

[shipping]
shipping.example.com

[payment]
payment.example.com

[mongodb]
mongodb.example.com

[mysql]
mysql.example.com

[redis]
redis.example.com

[rabbitmq]
rabbitmq.example.com
```

### 3. Run Playbooks

#### Deploy All Services
```bash
ansible-playbook -i inventory.ini roboshop.yaml
```

#### Deploy Individual Services
```bash
# Frontend
ansible-playbook -i inventory.ini frontend.yaml

# Catalogue
ansible-playbook -i inventory.ini catalogue.yaml

# Cart
ansible-playbook -i inventory.ini cart.yaml

# User
ansible-playbook -i inventory.ini user.yaml

# Shipping
ansible-playbook -i inventory.ini shipping.yaml

# Payment
ansible-playbook -i inventory.ini payment.yaml

# Databases and Message Queue
ansible-playbook -i inventory.ini mongodb.yaml
ansible-playbook -i inventory.ini mysql.yaml
ansible-playbook -i inventory.ini redis.yaml
ansible-playbook -i inventory.ini rabbitmq.yaml
```

## 🔧 Configuration

### Service Dependencies
Ensure backend services are deployed before application services:

1. **First**: Deploy databases and message queue
   - MongoDB
   - MySQL
   - Redis
   - RabbitMQ

2. **Then**: Deploy application services
   - Catalogue (depends on MongoDB)
   - User (depends on MongoDB, Redis)
   - Cart (depends on Redis, Catalogue)
   - Shipping (depends on MySQL)
   - Payment (depends on RabbitMQ)

3. **Finally**: Deploy frontend
   - Frontend (depends on all application services)

### Customization
- Modify service files (`.service`) to adjust application configurations
- Update repository files (`.repo`) for different package sources
- Edit `nginx.conf` for frontend customization

## 🔍 Troubleshooting

### Check Service Status
```bash
# On target servers
systemctl status catalogue
systemctl status cart
systemctl status user
systemctl status shipping
systemctl status payment
```

### View Logs
```bash
journalctl -u catalogue -f
journalctl -u cart -f
journalctl -u user -f
journalctl -u shipping -f
journalctl -u payment -f
```

### Common Issues
- **Connection refused**: Ensure firewalls allow required ports
- **Service fails to start**: Check logs with `journalctl`
- **Database connection errors**: Verify backend services are running
- **Permission denied**: Ensure proper SSH keys and sudo access

## 📝 Notes

- Default credentials and configurations should be changed for production use
- Ensure proper network security groups/firewall rules are configured
- Regular backups of MongoDB and MySQL databases are recommended
- Monitor resource usage on all servers

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This project is open source and available under the MIT License.

## 👤 Author

**Pavan Kumar (avyu)**
- GitHub: [@mepavankumar15](https://github.com/mepavankumar15)

## 🙏 Acknowledgments

- RoboShop application architecture
- Ansible community for best practices and modules
