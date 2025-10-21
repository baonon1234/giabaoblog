---
title: "Kỹ năng nền tảng cho Java và JavaScript Developer"
date: 2025-10-17
draft: false
tags: ["java", "javascript", "git", "linux", "sql", "kỹ-năng", "lập-trình"]
categories: ["kỹ-năng"]
summary: "Chia sẻ về các kỹ năng nền tảng quan trọng cho Java và JavaScript developer, từ Git, Linux đến SQL và deployment."
---

# Kỹ năng nền tảng cho Java và JavaScript Developer

Chào các bạn! Hôm nay mình sẽ chia sẻ về những kỹ năng nền tảng quan trọng mà mọi Java và JavaScript developer cần có. Đây là những kỹ năng không hào nhoáng nhưng quyết định sự thành công trong sự nghiệp lập trình! 😊

## 🎯 Tại sao kỹ năng nền tảng quan trọng?

### Vấn đề thường gặp
- **Code hay nhưng không biết deploy**: Không biết cách đưa code lên server
- **Làm việc nhóm khó khăn**: Không biết Git, conflict liên tục
- **Debug không hiệu quả**: Không biết đọc log, check database
- **Performance kém**: Không biết optimize query, monitor system

### Lợi ích của kỹ năng nền tảng
- **Tự tin hơn**: Biết cách xử lý mọi tình huống
- **Làm việc nhóm tốt**: Hiểu quy trình, không gây conflict
- **Debug nhanh**: Biết cách tìm và fix bug
- **Career growth**: Được đánh giá cao, thăng tiến nhanh

## 🔧 Git - Version Control Mastery

### Git cơ bản cho Java/JavaScript

#### Setup Repository
```bash
# Khởi tạo repository
git init
git config user.name "Gia Bảo"
git config user.email "giabao@example.com"

# Clone project từ GitHub
git clone https://github.com/username/project.git
cd project
```

#### Workflow cơ bản
```bash
# Check status
git status

# Add files
git add .
git add src/main/java/com/example/App.java

# Commit với message rõ ràng
git commit -m "feat: add user authentication system"
git commit -m "fix: resolve null pointer exception in login"
git commit -m "docs: update API documentation"

# Push to remote
git push origin main
```

#### Branch Management
```bash
# Tạo và chuyển branch
git checkout -b feature/user-login
git checkout -b bugfix/login-error

# Merge branch
git checkout main
git merge feature/user-login

# Delete branch
git branch -d feature/user-login
```

#### Advanced Git cho Java/JavaScript

##### Git Hooks cho Java
```bash
# .git/hooks/pre-commit
#!/bin/sh
# Run tests before commit
mvn test
if [ $? -ne 0 ]; then
    echo "Tests failed. Commit aborted."
    exit 1
fi
```

##### Git Hooks cho JavaScript
```bash
# .git/hooks/pre-commit
#!/bin/sh
# Run linting and tests
npm run lint
npm run test
if [ $? -ne 0 ]; then
    echo "Linting or tests failed. Commit aborted."
    exit 1
fi
```

##### Git Flow cho Team
```bash
# Feature development
git checkout develop
git checkout -b feature/new-feature
# ... develop feature ...
git commit -m "feat: implement new feature"
git push origin feature/new-feature
# Create Pull Request

# Hotfix
git checkout main
git checkout -b hotfix/critical-bug
# ... fix bug ...
git commit -m "fix: resolve critical security issue"
git push origin hotfix/critical-bug
```

### Git Best Practices

#### Commit Message Convention
```
type(scope): description

feat(auth): add JWT token validation
fix(api): resolve CORS issue
docs(readme): update installation guide
style(ui): improve button design
refactor(service): extract user service
test(unit): add user service tests
```

#### Branch Naming
```
feature/user-authentication
bugfix/login-validation
hotfix/security-patch
release/v1.2.0
```

## 🐧 Linux - Server Environment

### Linux Commands cho Developer

#### File Operations
```bash
# Navigation
cd /var/www/html
ls -la
pwd

# File operations
cp file.txt backup/
mv old_name.txt new_name.txt
rm -rf temp/
mkdir -p project/src/main/java

# Permissions
chmod 755 script.sh
chown user:group file.txt
```

#### Process Management
```bash
# Check running processes
ps aux | grep java
ps aux | grep node

# Kill process
kill -9 1234
pkill -f "java.*MyApp"

# Check ports
netstat -tulpn | grep :8080
lsof -i :3000
```

#### System Monitoring
```bash
# System resources
top
htop
free -h
df -h

# Check logs
tail -f /var/log/application.log
journalctl -u my-service -f
```

### Linux cho Java Development

#### Java Environment Setup
```bash
# Install Java
sudo apt update
sudo apt install openjdk-17-jdk

# Set JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

# Verify installation
java -version
javac -version
```

#### Maven/Gradle với Linux
```bash
# Maven
mvn clean compile
mvn test
mvn package
mvn spring-boot:run

# Gradle
./gradlew build
./gradlew test
./gradlew bootRun
```

#### Java Application Deployment
```bash
# Build JAR
mvn clean package

# Run application
java -jar target/myapp.jar

# Run with profile
java -jar target/myapp.jar --spring.profiles.active=production

# Run as service
sudo systemctl start myapp
sudo systemctl enable myapp
```

### Linux cho JavaScript Development

#### Node.js Environment
```bash
# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install npm packages
npm install
npm install -g pm2

# Run application
node app.js
npm start
```

#### PM2 Process Management
```bash
# Start application
pm2 start app.js --name "myapp"

# Monitor
pm2 monit
pm2 logs

# Restart
pm2 restart myapp
pm2 stop myapp
```

#### Nginx Configuration
```nginx
server {
    listen 80;
    server_name myapp.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## 🗄️ SQL - Database Mastery

### SQL cơ bản cho Java/JavaScript

#### Database Setup
```sql
-- MySQL
CREATE DATABASE myapp;
USE myapp;

-- PostgreSQL
CREATE DATABASE myapp;
\c myapp;
```

#### Table Creation
```sql
-- Users table
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Posts table
CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

#### Advanced Queries

##### JOIN Operations
```sql
-- Inner Join
SELECT u.username, p.title, p.created_at
FROM users u
INNER JOIN posts p ON u.id = p.user_id
WHERE p.published = TRUE;

-- Left Join
SELECT u.username, COUNT(p.id) as post_count
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
GROUP BY u.id, u.username;
```

##### Aggregation Functions
```sql
-- Count posts by user
SELECT u.username, COUNT(p.id) as post_count
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
GROUP BY u.id, u.username
HAVING post_count > 0
ORDER BY post_count DESC;

-- Average posts per user
SELECT AVG(post_count) as avg_posts
FROM (
    SELECT COUNT(p.id) as post_count
    FROM users u
    LEFT JOIN posts p ON u.id = p.user_id
    GROUP BY u.id
) as user_posts;
```

##### Performance Optimization
```sql
-- Create indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_published ON posts(published);

-- Explain query plan
EXPLAIN SELECT u.username, p.title
FROM users u
INNER JOIN posts p ON u.id = p.user_id
WHERE p.published = TRUE;
```

### Database Integration

#### Java với JPA/Hibernate
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Post> posts;
    
    // Getters and setters
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    
    @Query("SELECT u FROM User u WHERE u.username LIKE %:username%")
    List<User> findByUsernameContaining(@Param("username") String username);
}
```

#### JavaScript với Sequelize
```javascript
// User model
const User = sequelize.define('User', {
    username: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true
    },
    email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true
    },
    passwordHash: {
        type: DataTypes.STRING,
        allowNull: false
    }
});

// Post model
const Post = sequelize.define('Post', {
    title: {
        type: DataTypes.STRING,
        allowNull: false
    },
    content: {
        type: DataTypes.TEXT
    },
    published: {
        type: DataTypes.BOOLEAN,
        defaultValue: false
    }
});

// Associations
User.hasMany(Post);
Post.belongsTo(User);

// Query with include
const usersWithPosts = await User.findAll({
    include: [{
        model: Post,
        where: { published: true }
    }]
});
```

## 🚀 Deployment và DevOps

### Docker cho Java
```dockerfile
# Dockerfile
FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/myapp.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=production
    depends_on:
      - database
  
  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: myapp
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

### Docker cho JavaScript
```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@database:5432/myapp
    depends_on:
      - database
  
  database:
    image: postgres:15
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## 📊 Monitoring và Debugging

### Application Monitoring
```bash
# Check application logs
tail -f /var/log/myapp.log

# Monitor system resources
htop
iostat -x 1

# Check database connections
mysql -u root -p -e "SHOW PROCESSLIST;"
```

### Performance Tuning
```sql
-- MySQL slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;

-- Check slow queries
SELECT * FROM mysql.slow_log ORDER BY start_time DESC LIMIT 10;
```

## 🎯 Best Practices

### 1. Git Workflow
- **Commit thường xuyên**: Mỗi feature nhỏ một commit
- **Message rõ ràng**: Mô tả chính xác thay đổi
- **Branch strategy**: Feature branch, hotfix branch
- **Code review**: Luôn review code trước khi merge

### 2. Linux Administration
- **Log rotation**: Tránh log file quá lớn
- **Backup strategy**: Backup database và code
- **Security**: Update system, firewall, SSH keys
- **Monitoring**: Set up alerts cho critical issues

### 3. Database Management
- **Indexing**: Tạo index cho các query thường dùng
- **Normalization**: Thiết kế database chuẩn
- **Backup**: Regular backup và test restore
- **Performance**: Monitor và optimize slow queries

## 📚 Learning Resources

### Git
- [Pro Git Book](https://git-scm.com/book)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)
- [GitHub Learning Lab](https://lab.github.com/)

### Linux
- [Linux Command Line Basics](https://www.udemy.com/course/linux-command-line-basics/)
- [Linux Academy](https://linuxacademy.com/)
- [Red Hat System Administration](https://www.redhat.com/en/services/training)

### SQL
- [SQLBolt](https://sqlbolt.com/)
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)
- [MySQL Documentation](https://dev.mysql.com/doc/)

## 🎯 Kết luận

Kỹ năng nền tảng là nền móng vững chắc cho sự nghiệp lập trình:

1. **Git**: Quản lý code, làm việc nhóm hiệu quả
2. **Linux**: Tự tin khi làm việc với server
3. **SQL**: Hiểu và tối ưu database

**Lời khuyên:**
- Học từng kỹ năng một cách có hệ thống
- Thực hành thường xuyên với project thực tế
- Tham gia cộng đồng để học hỏi
- Không ngừng cập nhật kiến thức mới

Chúc các bạn thành công trong sự nghiệp lập trình! Nếu có câu hỏi gì, hãy comment bên dưới nhé! 😊

---

*Bài viết được viết dựa trên kinh nghiệm thực tế của tác giả. Các kỹ năng này cần thời gian để thành thạo, hãy kiên trì luyện tập!*