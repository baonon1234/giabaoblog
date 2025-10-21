---
title: "Mini Project: Ứng dụng ghi chú với Java Spring Boot và JavaScript"
date: 2025-10-17
draft: false
tags: ["java", "javascript", "spring-boot", "fetch-api", "project", "crud"]
categories: ["thực-hành"]
summary: "Hướng dẫn xây dựng ứng dụng ghi chú full-stack với Java Spring Boot backend và JavaScript frontend."
---

# Mini Project: Ứng dụng ghi chú với Java Spring Boot và JavaScript

Chào các bạn! Hôm nay mình sẽ hướng dẫn xây dựng một ứng dụng ghi chú hoàn chỉnh sử dụng Java Spring Boot cho backend và JavaScript thuần cho frontend. Đây là project rất tốt để thực hành CRUD operations và API integration! 😊

## 🎯 Mục tiêu dự án

### Chức năng chính
- **Tạo ghi chú mới**: Thêm ghi chú với tiêu đề và nội dung
- **Xem danh sách**: Hiển thị tất cả ghi chú
- **Chỉnh sửa**: Cập nhật nội dung ghi chú
- **Xóa ghi chú**: Xóa ghi chú không cần thiết
- **Tìm kiếm**: Tìm ghi chú theo từ khóa

### Công nghệ sử dụng
- **Backend**: Java Spring Boot, Spring Data JPA, H2 Database
- **Frontend**: HTML, CSS, JavaScript (Vanilla)
- **API**: RESTful API với JSON
- **Build Tool**: Maven

## 🚀 Backend - Java Spring Boot

### 1. Setup Project

#### Tạo Spring Boot Project
```bash
# Sử dụng Spring Initializr
curl https://start.spring.io/starter.zip \
  -d dependencies=web,data-jpa,h2 \
  -d type=maven-project \
  -d language=java \
  -d bootVersion=3.2.0 \
  -d baseDir=notes-app \
  -d groupId=com.example \
  -d artifactId=notes-app \
  -d name=notes-app \
  -d description="Notes Application" \
  -d packageName=com.example.notes \
  -d packaging=jar \
  -d javaVersion=17 \
  -o notes-app.zip
```

#### Maven Dependencies (pom.xml)
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 2. Entity và Model

#### Note Entity
```java
package com.example.notes.entity;

import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "notes")
public class Note {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @Column(columnDefinition = "TEXT")
    private String content;
    
    @Column(name = "created_at")
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    // Constructors
    public Note() {}
    
    public Note(String title, String content) {
        this.title = title;
        this.content = content;
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }
    
    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
    
    public String getContent() { return content; }
    public void setContent(String content) { this.content = content; }
    
    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }
    
    public LocalDateTime getUpdatedAt() { return updatedAt; }
    public void setUpdatedAt(LocalDateTime updatedAt) { this.updatedAt = updatedAt; }
    
    @PreUpdate
    public void preUpdate() {
        this.updatedAt = LocalDateTime.now();
    }
}
```

### 3. Repository Layer

#### Note Repository
```java
package com.example.notes.repository;

import com.example.notes.entity.Note;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface NoteRepository extends JpaRepository<Note, Long> {
    
    // Tìm kiếm theo tiêu đề
    List<Note> findByTitleContainingIgnoreCase(String title);
    
    // Tìm kiếm theo nội dung
    List<Note> findByContentContainingIgnoreCase(String content);
    
    // Tìm kiếm theo cả tiêu đề và nội dung
    @Query("SELECT n FROM Note n WHERE " +
           "LOWER(n.title) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(n.content) LIKE LOWER(CONCAT('%', :keyword, '%'))")
    List<Note> findByKeyword(@Param("keyword") String keyword);
    
    // Sắp xếp theo ngày tạo mới nhất
    List<Note> findAllByOrderByCreatedAtDesc();
}
```

### 4. Service Layer

#### Note Service
```java
package com.example.notes.service;

import com.example.notes.entity.Note;
import com.example.notes.repository.NoteRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class NoteService {
    
    @Autowired
    private NoteRepository noteRepository;
    
    // Lấy tất cả ghi chú
    public List<Note> getAllNotes() {
        return noteRepository.findAllByOrderByCreatedAtDesc();
    }
    
    // Lấy ghi chú theo ID
    public Optional<Note> getNoteById(Long id) {
        return noteRepository.findById(id);
    }
    
    // Tạo ghi chú mới
    public Note createNote(Note note) {
        return noteRepository.save(note);
    }
    
    // Cập nhật ghi chú
    public Note updateNote(Long id, Note noteDetails) {
        Optional<Note> optionalNote = noteRepository.findById(id);
        if (optionalNote.isPresent()) {
            Note note = optionalNote.get();
            note.setTitle(noteDetails.getTitle());
            note.setContent(noteDetails.getContent());
            note.setUpdatedAt(java.time.LocalDateTime.now());
            return noteRepository.save(note);
        }
        return null;
    }
    
    // Xóa ghi chú
    public boolean deleteNote(Long id) {
        if (noteRepository.existsById(id)) {
            noteRepository.deleteById(id);
            return true;
        }
        return false;
    }
    
    // Tìm kiếm ghi chú
    public List<Note> searchNotes(String keyword) {
        return noteRepository.findByKeyword(keyword);
    }
}
```

### 5. Controller Layer

#### Note Controller
```java
package com.example.notes.controller;

import com.example.notes.entity.Note;
import com.example.notes.service.NoteService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/notes")
@CrossOrigin(origins = "*") // Cho phép CORS
public class NoteController {
    
    @Autowired
    private NoteService noteService;
    
    // GET /api/notes - Lấy tất cả ghi chú
    @GetMapping
    public ResponseEntity<List<Note>> getAllNotes() {
        List<Note> notes = noteService.getAllNotes();
        return ResponseEntity.ok(notes);
    }
    
    // GET /api/notes/{id} - Lấy ghi chú theo ID
    @GetMapping("/{id}")
    public ResponseEntity<Note> getNoteById(@PathVariable Long id) {
        Optional<Note> note = noteService.getNoteById(id);
        if (note.isPresent()) {
            return ResponseEntity.ok(note.get());
        }
        return ResponseEntity.notFound().build();
    }
    
    // POST /api/notes - Tạo ghi chú mới
    @PostMapping
    public ResponseEntity<Note> createNote(@RequestBody Note note) {
        Note createdNote = noteService.createNote(note);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdNote);
    }
    
    // PUT /api/notes/{id} - Cập nhật ghi chú
    @PutMapping("/{id}")
    public ResponseEntity<Note> updateNote(@PathVariable Long id, @RequestBody Note noteDetails) {
        Note updatedNote = noteService.updateNote(id, noteDetails);
        if (updatedNote != null) {
            return ResponseEntity.ok(updatedNote);
        }
        return ResponseEntity.notFound().build();
    }
    
    // DELETE /api/notes/{id} - Xóa ghi chú
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteNote(@PathVariable Long id) {
        boolean deleted = noteService.deleteNote(id);
        if (deleted) {
            return ResponseEntity.noContent().build();
        }
        return ResponseEntity.notFound().build();
    }
    
    // GET /api/notes/search?keyword=... - Tìm kiếm ghi chú
    @GetMapping("/search")
    public ResponseEntity<List<Note>> searchNotes(@RequestParam String keyword) {
        List<Note> notes = noteService.searchNotes(keyword);
        return ResponseEntity.ok(notes);
    }
}
```

### 6. Configuration

#### Application Properties
```properties
# application.properties
server.port=8080

# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true

# H2 Console (for development)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

## 🌐 Frontend - JavaScript

### 1. HTML Structure

#### index.html
```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ứng dụng ghi chú</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>📝 Ứng dụng ghi chú</h1>
            <div class="search-container">
                <input type="text" id="searchInput" placeholder="Tìm kiếm ghi chú...">
                <button id="searchBtn">🔍</button>
            </div>
        </header>
        
        <main>
            <div class="note-form">
                <h2>Thêm ghi chú mới</h2>
                <form id="noteForm">
                    <div class="form-group">
                        <label for="noteTitle">Tiêu đề:</label>
                        <input type="text" id="noteTitle" required>
                    </div>
                    <div class="form-group">
                        <label for="noteContent">Nội dung:</label>
                        <textarea id="noteContent" rows="4" required></textarea>
                    </div>
                    <button type="submit" id="submitBtn">Thêm ghi chú</button>
                </form>
            </div>
            
            <div class="notes-section">
                <h2>Danh sách ghi chú</h2>
                <div id="loadingIndicator" class="loading hidden">
                    <div class="spinner"></div>
                    <span>Đang tải...</span>
                </div>
                <div id="notesList" class="notes-list"></div>
                <div id="noNotesMessage" class="no-notes hidden">
                    <p>Chưa có ghi chú nào. Hãy thêm ghi chú đầu tiên!</p>
                </div>
            </div>
        </main>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
```

### 2. CSS Styling

#### styles.css
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    border-radius: 15px;
    box-shadow: 0 20px 40px rgba(0,0,0,0.1);
    overflow: hidden;
}

header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 30px;
    text-align: center;
}

header h1 {
    font-size: 2.5rem;
    margin-bottom: 20px;
}

.search-container {
    display: flex;
    max-width: 400px;
    margin: 0 auto;
    gap: 10px;
}

#searchInput {
    flex: 1;
    padding: 12px;
    border: none;
    border-radius: 25px;
    font-size: 16px;
    outline: none;
}

#searchBtn {
    padding: 12px 20px;
    background: rgba(255,255,255,0.2);
    border: none;
    border-radius: 25px;
    color: white;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

#searchBtn:hover {
    background: rgba(255,255,255,0.3);
}

main {
    padding: 30px;
}

.note-form {
    background: #f8f9fa;
    padding: 25px;
    border-radius: 10px;
    margin-bottom: 30px;
}

.note-form h2 {
    color: #333;
    margin-bottom: 20px;
    font-size: 1.5rem;
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
    color: #555;
}

.form-group input,
.form-group textarea {
    width: 100%;
    padding: 12px;
    border: 2px solid #e1e5e9;
    border-radius: 8px;
    font-size: 16px;
    transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
    outline: none;
    border-color: #667eea;
}

#submitBtn {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 12px 30px;
    border: none;
    border-radius: 25px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.3s;
}

#submitBtn:hover {
    transform: translateY(-2px);
}

.notes-section h2 {
    color: #333;
    margin-bottom: 20px;
    font-size: 1.5rem;
}

.notes-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
}

.note-card {
    background: white;
    border: 2px solid #e1e5e9;
    border-radius: 10px;
    padding: 20px;
    transition: all 0.3s;
    position: relative;
}

.note-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0,0,0,0.1);
    border-color: #667eea;
}

.note-card h3 {
    color: #333;
    margin-bottom: 10px;
    font-size: 1.2rem;
}

.note-card p {
    color: #666;
    line-height: 1.6;
    margin-bottom: 15px;
}

.note-meta {
    font-size: 0.9rem;
    color: #999;
    margin-bottom: 15px;
}

.note-actions {
    display: flex;
    gap: 10px;
}

.btn {
    padding: 8px 16px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 500;
    transition: all 0.3s;
}

.btn-edit {
    background: #28a745;
    color: white;
}

.btn-edit:hover {
    background: #218838;
}

.btn-delete {
    background: #dc3545;
    color: white;
}

.btn-delete:hover {
    background: #c82333;
}

.loading {
    text-align: center;
    padding: 40px;
}

.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #667eea;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 10px;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.no-notes {
    text-align: center;
    padding: 40px;
    color: #666;
}

.hidden {
    display: none;
}

/* Responsive */
@media (max-width: 768px) {
    .container {
        margin: 10px;
        border-radius: 10px;
    }
    
    header {
        padding: 20px;
    }
    
    header h1 {
        font-size: 2rem;
    }
    
    main {
        padding: 20px;
    }
    
    .notes-list {
        grid-template-columns: 1fr;
    }
}
```

### 3. JavaScript Logic

#### script.js
```javascript
class NotesApp {
    constructor() {
        this.apiUrl = 'http://localhost:8080/api/notes';
        this.notes = [];
        this.currentEditingId = null;
        
        this.init();
    }
    
    init() {
        this.bindEvents();
        this.loadNotes();
    }
    
    bindEvents() {
        // Form submission
        document.getElementById('noteForm').addEventListener('submit', (e) => {
            e.preventDefault();
            this.handleSubmit();
        });
        
        // Search functionality
        document.getElementById('searchBtn').addEventListener('click', () => {
            this.handleSearch();
        });
        
        document.getElementById('searchInput').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                this.handleSearch();
            }
        });
    }
    
    // API Helper Methods
    async makeRequest(url, options = {}) {
        try {
            const response = await fetch(url, {
                headers: {
                    'Content-Type': 'application/json',
                    ...options.headers
                },
                ...options
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error('API Error:', error);
            this.showError('Có lỗi xảy ra khi kết nối với server');
            throw error;
        }
    }
    
    // CRUD Operations
    async loadNotes() {
        this.showLoading(true);
        try {
            this.notes = await this.makeRequest(this.apiUrl);
            this.renderNotes();
        } catch (error) {
            this.showError('Không thể tải danh sách ghi chú');
        } finally {
            this.showLoading(false);
        }
    }
    
    async createNote(noteData) {
        try {
            const newNote = await this.makeRequest(this.apiUrl, {
                method: 'POST',
                body: JSON.stringify(noteData)
            });
            this.notes.unshift(newNote);
            this.renderNotes();
            this.clearForm();
            this.showSuccess('Thêm ghi chú thành công!');
        } catch (error) {
            this.showError('Không thể thêm ghi chú');
        }
    }
    
    async updateNote(id, noteData) {
        try {
            const updatedNote = await this.makeRequest(`${this.apiUrl}/${id}`, {
                method: 'PUT',
                body: JSON.stringify(noteData)
            });
            const index = this.notes.findIndex(note => note.id === id);
            if (index !== -1) {
                this.notes[index] = updatedNote;
            }
            this.renderNotes();
            this.clearForm();
            this.showSuccess('Cập nhật ghi chú thành công!');
        } catch (error) {
            this.showError('Không thể cập nhật ghi chú');
        }
    }
    
    async deleteNote(id) {
        if (!confirm('Bạn có chắc chắn muốn xóa ghi chú này?')) {
            return;
        }
        
        try {
            await this.makeRequest(`${this.apiUrl}/${id}`, {
                method: 'DELETE'
            });
            this.notes = this.notes.filter(note => note.id !== id);
            this.renderNotes();
            this.showSuccess('Xóa ghi chú thành công!');
        } catch (error) {
            this.showError('Không thể xóa ghi chú');
        }
    }
    
    async searchNotes(keyword) {
        if (!keyword.trim()) {
            this.loadNotes();
            return;
        }
        
        this.showLoading(true);
        try {
            this.notes = await this.makeRequest(`${this.apiUrl}/search?keyword=${encodeURIComponent(keyword)}`);
            this.renderNotes();
        } catch (error) {
            this.showError('Không thể tìm kiếm ghi chú');
        } finally {
            this.showLoading(false);
        }
    }
    
    // Event Handlers
    handleSubmit() {
        const title = document.getElementById('noteTitle').value.trim();
        const content = document.getElementById('noteContent').value.trim();
        
        if (!title || !content) {
            this.showError('Vui lòng điền đầy đủ thông tin');
            return;
        }
        
        const noteData = { title, content };
        
        if (this.currentEditingId) {
            this.updateNote(this.currentEditingId, noteData);
            this.currentEditingId = null;
        } else {
            this.createNote(noteData);
        }
    }
    
    handleSearch() {
        const keyword = document.getElementById('searchInput').value.trim();
        this.searchNotes(keyword);
    }
    
    editNote(note) {
        document.getElementById('noteTitle').value = note.title;
        document.getElementById('noteContent').value = note.content;
        this.currentEditingId = note.id;
        
        // Scroll to form
        document.querySelector('.note-form').scrollIntoView({ behavior: 'smooth' });
    }
    
    // UI Methods
    renderNotes() {
        const notesList = document.getElementById('notesList');
        const noNotesMessage = document.getElementById('noNotesMessage');
        
        if (this.notes.length === 0) {
            notesList.innerHTML = '';
            noNotesMessage.classList.remove('hidden');
            return;
        }
        
        noNotesMessage.classList.add('hidden');
        notesList.innerHTML = this.notes.map(note => this.createNoteCard(note)).join('');
    }
    
    createNoteCard(note) {
        const createdAt = new Date(note.createdAt).toLocaleDateString('vi-VN');
        const updatedAt = note.updatedAt ? new Date(note.updatedAt).toLocaleDateString('vi-VN') : '';
        
        return `
            <div class="note-card">
                <h3>${this.escapeHtml(note.title)}</h3>
                <p>${this.escapeHtml(note.content)}</p>
                <div class="note-meta">
                    <small>Tạo: ${createdAt}</small>
                    ${updatedAt ? `<br><small>Cập nhật: ${updatedAt}</small>` : ''}
                </div>
                <div class="note-actions">
                    <button class="btn btn-edit" onclick="app.editNote(${JSON.stringify(note).replace(/"/g, '&quot;')})">
                        ✏️ Chỉnh sửa
                    </button>
                    <button class="btn btn-delete" onclick="app.deleteNote(${note.id})">
                        🗑️ Xóa
                    </button>
                </div>
            </div>
        `;
    }
    
    clearForm() {
        document.getElementById('noteForm').reset();
        this.currentEditingId = null;
    }
    
    showLoading(show) {
        const loadingIndicator = document.getElementById('loadingIndicator');
        if (show) {
            loadingIndicator.classList.remove('hidden');
        } else {
            loadingIndicator.classList.add('hidden');
        }
    }
    
    showSuccess(message) {
        this.showNotification(message, 'success');
    }
    
    showError(message) {
        this.showNotification(message, 'error');
    }
    
    showNotification(message, type) {
        // Remove existing notifications
        const existingNotifications = document.querySelectorAll('.notification');
        existingNotifications.forEach(notification => notification.remove());
        
        // Create new notification
        const notification = document.createElement('div');
        notification.className = `notification ${type}`;
        notification.textContent = message;
        notification.style.cssText = `
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 5px;
            color: white;
            font-weight: 500;
            z-index: 1000;
            animation: slideIn 0.3s ease;
            background: ${type === 'success' ? '#28a745' : '#dc3545'};
        `;
        
        document.body.appendChild(notification);
        
        // Auto remove after 3 seconds
        setTimeout(() => {
            notification.remove();
        }, 3000);
    }
    
    escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }
}

// Initialize app when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    window.app = new NotesApp();
});
```

## 🚀 Chạy ứng dụng

### 1. Backend (Spring Boot)
```bash
# Navigate to project directory
cd notes-app

# Run Spring Boot application
mvn spring-boot:run

# Or build and run JAR
mvn clean package
java -jar target/notes-app-0.0.1-SNAPSHOT.jar
```

### 2. Frontend
```bash
# Open index.html in browser
# Or use a simple HTTP server
python -m http.server 8000
# Then open http://localhost:8000
```

### 3. Test API
```bash
# Test endpoints
curl http://localhost:8080/api/notes
curl -X POST http://localhost:8080/api/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Note","content":"This is a test note"}'
```

## 🎯 Tính năng nâng cao

### 1. Thêm validation
```javascript
// Client-side validation
validateNote(title, content) {
    if (!title.trim()) {
        throw new Error('Tiêu đề không được để trống');
    }
    if (!content.trim()) {
        throw new Error('Nội dung không được để trống');
    }
    if (title.length > 100) {
        throw new Error('Tiêu đề không được quá 100 ký tự');
    }
    if (content.length > 1000) {
        throw new Error('Nội dung không được quá 1000 ký tự');
    }
}
```

### 2. Thêm pagination
```java
// Backend - NoteController
@GetMapping
public ResponseEntity<Page<Note>> getAllNotes(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "10") int size) {
    Page<Note> notes = noteService.getAllNotes(page, size);
    return ResponseEntity.ok(notes);
}
```

### 3. Thêm authentication
```java
// Spring Security configuration
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/api/notes/**").authenticated()
            .anyRequest().permitAll()
            .and()
            .httpBasic();
        return http.build();
    }
}
```

## 📚 Tài liệu tham khảo

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Fetch API MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [JavaScript ES6+](https://javascript.info/)

## 🎯 Kết luận

Dự án này giúp bạn:

1. **Hiểu CRUD operations**: Create, Read, Update, Delete
2. **Làm việc với API**: RESTful API design
3. **Frontend-Backend integration**: JavaScript với Java
4. **Database operations**: JPA, Hibernate
5. **Error handling**: Xử lý lỗi đúng cách

**Bước tiếp theo:**
- Thêm authentication
- Implement pagination
- Add real database (MySQL, PostgreSQL)
- Deploy lên cloud (AWS, Heroku)

Chúc các bạn code vui vẻ! Nếu có câu hỏi gì, hãy comment bên dưới nhé! 😊

---

*Bài viết được viết dựa trên kinh nghiệm thực tế của tác giả. Code có thể cần điều chỉnh tùy theo phiên bản Spring Boot.*