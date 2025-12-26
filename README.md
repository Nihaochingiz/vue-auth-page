Отлично! Создадим минималистичный дизайн в черно-белой цветовой гамме с акцентами на серых оттенках.

## Обновленный дизайн

### 1. Обновленные компоненты с новым дизайном

**src/components/Login.vue** (обновленный дизайн)
```vue
<template>
  <div class="auth-container">
    <div class="auth-wrapper">
      <div class="auth-card">
        <div class="logo-section">
          <div class="logo">
            <i class="fas fa-shield-alt"></i>
          </div>
          <h1 class="portal-title">SECURE PORTAL</h1>
        </div>
        
        <div class="auth-form-section">
          <h2 class="form-title">АВТОРИЗАЦИЯ</h2>
          <p class="form-subtitle">Войдите в защищенную систему</p>
          
          <form @submit.prevent="handleLogin" class="auth-form">
            <div class="form-group">
              <div class="input-wrapper">
                <i class="fas fa-user input-icon"></i>
                <input 
                  type="email" 
                  v-model="email"
                  placeholder="EMAIL"
                  required
                  class="form-input"
                  :class="{ 'input-error': errors.email }"
                >
              </div>
              <div class="error-message" v-if="errors.email">{{ errors.email }}</div>
            </div>
            
            <div class="form-group">
              <div class="input-wrapper">
                <i class="fas fa-lock input-icon"></i>
                <input 
                  :type="showPassword ? 'text' : 'password'" 
                  v-model="password"
                  placeholder="ПАРОЛЬ"
                  required
                  class="form-input"
                  :class="{ 'input-error': errors.password }"
                >
                <button type="button" class="password-toggle" @click="togglePassword">
                  <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
              <div class="error-message" v-if="errors.password">{{ errors.password }}</div>
            </div>
            
            <div class="form-options">
              <label class="checkbox-wrapper">
                <input type="checkbox" v-model="rememberMe" class="checkbox-input">
                <span class="checkbox-custom"></span>
                <span class="checkbox-label">Запомнить устройство</span>
              </label>
            </div>
            
            <button type="submit" class="submit-button" :disabled="isLoading">
              <span v-if="!isLoading">
                <i class="fas fa-sign-in-alt"></i> ВОЙТИ
              </span>
              <span v-else>
                <i class="fas fa-spinner fa-spin"></i> АВТОРИЗАЦИЯ...
              </span>
            </button>
          </form>
          
          <div class="demo-info">
            <p class="demo-title">ТЕСТОВЫЙ ДОСТУП:</p>
            <div class="demo-credentials">
              <div class="credential">
                <span class="cred-label">Логин:</span>
                <span class="cred-value">admin@portal.ru</span>
              </div>
              <div class="credential">
                <span class="cred-label">Пароль:</span>
                <span class="cred-value">admin123</span>
              </div>
            </div>
          </div>
        </div>
        
        <div class="auth-footer">
          <p class="footer-text">© 2024 SECURE PORTAL SYSTEM • v2.1.0</p>
        </div>
      </div>
    </div>
    
    <div v-if="notification.show" class="notification" :class="notification.type">
      <i :class="notification.icon"></i>
      <span>{{ notification.message }}</span>
      <button class="close-btn" @click="closeNotification">×</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      email: '',
      password: '',
      rememberMe: false,
      showPassword: false,
      isLoading: false,
      errors: {
        email: '',
        password: ''
      },
      notification: {
        show: false,
        message: '',
        type: '',
        icon: ''
      }
    }
  },
  mounted() {
    // Автозаполнение для тестирования
    this.email = 'admin@portal.ru'
    this.password = 'admin123'
  },
  methods: {
    validateForm() {
      this.errors = { email: '', password: '' }
      let isValid = true
      
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      
      if (!this.email) {
        this.errors.email = 'Введите email адрес'
        isValid = false
      } else if (!emailRegex.test(this.email)) {
        this.errors.email = 'Некорректный формат email'
        isValid = false
      }
      
      if (!this.password) {
        this.errors.password = 'Введите пароль'
        isValid = false
      } else if (this.password.length < 6) {
        this.errors.password = 'Пароль должен быть не менее 6 символов'
        isValid = false
      }
      
      return isValid
    },
    
    togglePassword() {
      this.showPassword = !this.showPassword
    },
    
    showNotification(message, type = 'success') {
      this.notification = {
        show: true,
        message,
        type,
        icon: type === 'success' ? 'fas fa-check-circle' : 'fas fa-exclamation-circle'
      }
      
      setTimeout(() => {
        this.closeNotification()
      }, 4000)
    },
    
    closeNotification() {
      this.notification.show = false
    },
    
    async handleLogin() {
      if (!this.validateForm()) {
        return
      }
      
      this.isLoading = true
      
      try {
        // Имитация запроса к API
        await new Promise(resolve => setTimeout(resolve, 1200))
        
        // Проверка учетных данных
        const validUsers = [
          { email: 'admin@portal.ru', password: 'admin123', name: 'Александр Иванов', role: 'Администратор' },
          { email: 'user@portal.ru', password: 'user123', name: 'Мария Петрова', role: 'Пользователь' },
          { email: 'manager@portal.ru', password: 'manager123', name: 'Сергей Сидоров', role: 'Менеджер' }
        ]
        
        const user = validUsers.find(u => u.email === this.email && u.password === this.password)
        
        if (user) {
          // Сохраняем данные пользователя
          const userData = {
            email: user.email,
            name: user.name,
            role: user.role,
            lastLogin: new Date().toISOString(),
            registered: '2023-01-15T10:30:00Z',
            department: user.role === 'Администратор' ? 'Технический отдел' : 
                        user.role === 'Менеджер' ? 'Отдел продаж' : 'Операционный отдел'
          }
          
          localStorage.setItem('userData', JSON.stringify(userData))
          localStorage.setItem('isAuthenticated', 'true')
          
          if (this.rememberMe) {
            localStorage.setItem('rememberMe', 'true')
          }
          
          this.showNotification('Авторизация успешна', 'success')
          
          setTimeout(() => {
            this.$router.push('/portal')
          }, 800)
        } else {
          this.showNotification('Неверные учетные данные', 'error')
        }
      } catch (error) {
        this.showNotification('Ошибка подключения', 'error')
      } finally {
        this.isLoading = false
      }
    }
  }
}
</script>

<style scoped>
.auth-container {
  min-height: 100vh;
  background-color: #000000;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
}

.auth-wrapper {
  width: 100%;
  max-width: 420px;
}

.auth-card {
  background: #0a0a0a;
  border: 1px solid #333333;
  border-radius: 2px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.logo-section {
  background: #000000;
  padding: 40px 30px 30px;
  text-align: center;
  border-bottom: 1px solid #333333;
}

.logo {
  width: 60px;
  height: 60px;
  background: #ffffff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 20px;
}

.logo i {
  font-size: 28px;
  color: #000000;
}

.portal-title {
  color: #ffffff;
  font-size: 20px;
  font-weight: 600;
  letter-spacing: 2px;
  margin: 0;
  text-transform: uppercase;
}

.auth-form-section {
  padding: 40px 30px;
}

.form-title {
  color: #ffffff;
  font-size: 24px;
  font-weight: 500;
  margin-bottom: 8px;
  text-align: center;
}

.form-subtitle {
  color: #888888;
  font-size: 14px;
  text-align: center;
  margin-bottom: 30px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.auth-form {
  margin-bottom: 30px;
}

.form-group {
  margin-bottom: 24px;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icon {
  position: absolute;
  left: 16px;
  color: #888888;
  font-size: 16px;
  z-index: 1;
}

.form-input {
  width: 100%;
  padding: 16px 16px 16px 46px;
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  color: #ffffff;
  font-size: 14px;
  font-family: monospace;
  letter-spacing: 0.5px;
  transition: all 0.2s;
}

.form-input:focus {
  outline: none;
  border-color: #666666;
  box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.05);
}

.form-input.input-error {
  border-color: #ff4444;
}

.form-input::placeholder {
  color: #555555;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.password-toggle {
  position: absolute;
  right: 16px;
  background: none;
  border: none;
  color: #888888;
  cursor: pointer;
  font-size: 16px;
  padding: 4px;
}

.password-toggle:hover {
  color: #cccccc;
}

.error-message {
  color: #ff4444;
  font-size: 12px;
  margin-top: 6px;
  padding-left: 8px;
}

.form-options {
  margin-bottom: 30px;
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  cursor: pointer;
  color: #cccccc;
  font-size: 14px;
}

.checkbox-input {
  display: none;
}

.checkbox-custom {
  width: 18px;
  height: 18px;
  border: 1px solid #333333;
  border-radius: 2px;
  margin-right: 10px;
  position: relative;
  background: #111111;
  transition: all 0.2s;
}

.checkbox-input:checked + .checkbox-custom {
  background: #ffffff;
  border-color: #ffffff;
}

.checkbox-input:checked + .checkbox-custom::after {
  content: "✓";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #000000;
  font-size: 12px;
  font-weight: bold;
}

.checkbox-label {
  user-select: none;
}

.submit-button {
  width: 100%;
  padding: 16px;
  background: #ffffff;
  color: #000000;
  border: none;
  border-radius: 2px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.submit-button:hover:not(:disabled) {
  background: #f0f0f0;
  transform: translateY(-1px);
}

.submit-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.submit-button i {
  font-size: 16px;
}

.demo-info {
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 20px;
  margin-bottom: 30px;
}

.demo-title {
  color: #888888;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 12px;
}

.demo-credentials {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.credential {
  display: flex;
  align-items: center;
  gap: 10px;
}

.cred-label {
  color: #cccccc;
  font-size: 13px;
  min-width: 60px;
}

.cred-value {
  color: #ffffff;
  font-size: 13px;
  font-family: monospace;
  background: #000000;
  padding: 4px 10px;
  border-radius: 2px;
  border: 1px solid #333333;
}

.auth-footer {
  background: #000000;
  padding: 20px 30px;
  border-top: 1px solid #333333;
}

.footer-text {
  color: #555555;
  font-size: 11px;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin: 0;
}

.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 16px 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  max-width: 350px;
  animation: slideIn 0.3s ease-out;
  z-index: 1000;
}

.notification.success {
  border-left: 3px solid #00cc44;
}

.notification.error {
  border-left: 3px solid #ff4444;
}

.notification i {
  font-size: 18px;
}

.notification.success i {
  color: #00cc44;
}

.notification.error i {
  color: #ff4444;
}

.notification span {
  color: #ffffff;
  font-size: 14px;
  flex: 1;
}

.close-btn {
  background: none;
  border: none;
  color: #888888;
  font-size: 20px;
  cursor: pointer;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  color: #ffffff;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@media (max-width: 480px) {
  .auth-card {
    border: none;
    border-radius: 0;
  }
  
  .auth-wrapper {
    max-width: 100%;
  }
  
  .logo-section {
    padding: 30px 20px 25px;
  }
  
  .auth-form-section {
    padding: 30px 20px;
  }
  
  .notification {
    left: 20px;
    right: 20px;
    max-width: none;
  }
}
</style>
```

**src/components/Portal.vue** (обновленный дизайн)
```vue
<template>
  <div class="portal-container">
    <!-- Навигация -->
    <nav class="portal-nav">
      <div class="nav-left">
        <div class="nav-logo">
          <i class="fas fa-terminal"></i>
          <span class="logo-text">SYSTEM PORTAL</span>
        </div>
        <div class="nav-links">
          <a href="#" class="nav-link active">
            <i class="fas fa-home"></i>
            <span>ГЛАВНАЯ</span>
          </a>
          <a href="#" class="nav-link">
            <i class="fas fa-chart-bar"></i>
            <span>АНАЛИТИКА</span>
          </a>
          <a href="#" class="nav-link">
            <i class="fas fa-database"></i>
            <span>ДАННЫЕ</span>
          </a>
          <a href="#" class="nav-link">
            <i class="fas fa-cog"></i>
            <span>НАСТРОЙКИ</span>
          </a>
        </div>
      </div>
      <div class="nav-right">
        <div class="user-menu">
          <div class="user-avatar">
            <i class="fas fa-user-circle"></i>
          </div>
          <div class="user-info">
            <span class="user-name">{{ user.name }}</span>
            <span class="user-role">{{ user.role }}</span>
          </div>
          <button @click="handleLogout" class="logout-btn" title="Выйти из системы">
            <i class="fas fa-sign-out-alt"></i>
          </button>
        </div>
      </div>
    </nav>
    
    <!-- Основной контент -->
    <main class="portal-main">
      <div class="welcome-section">
        <div class="welcome-card">
          <div class="welcome-header">
            <h1 class="welcome-title">
              <i class="fas fa-user-shield"></i>
              ДОБРО ПОЖАЛОВАТЬ, {{ user.name.toUpperCase() }}
            </h1>
            <p class="welcome-subtitle">Сессия активна • Последний вход: {{ formatTime(user.lastLogin) }}</p>
          </div>
          
          <div class="status-indicators">
            <div class="status-item">
              <div class="status-dot active"></div>
              <span class="status-text">СИСТЕМА: АКТИВНА</span>
            </div>
            <div class="status-item">
              <div class="status-dot secure"></div>
              <span class="status-text">СОЕДИНЕНИЕ: ЗАЩИЩЕНО</span>
            </div>
            <div class="status-item">
              <div class="status-dot online"></div>
              <span class="status-text">СТАТУС: ОНЛАЙН</span>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Информация о пользователе -->
      <div class="user-dashboard">
        <div class="dashboard-grid">
          <!-- Профиль пользователя -->
          <div class="dashboard-card profile-card">
            <div class="card-header">
              <h3 class="card-title">
                <i class="fas fa-id-card"></i>
                ПРОФИЛЬ ПОЛЬЗОВАТЕЛЯ
              </h3>
            </div>
            <div class="card-body">
              <div class="profile-info">
                <div class="info-row">
                  <span class="info-label">ИМЯ:</span>
                  <span class="info-value">{{ user.name }}</span>
                </div>
                <div class="info-row">
                  <span class="info-label">EMAIL:</span>
                  <span class="info-value email">{{ user.email }}</span>
                </div>
                <div class="info-row">
                  <span class="info-label">РОЛЬ:</span>
                  <span class="info-value role">{{ user.role }}</span>
                </div>
                <div class="info-row">
                  <span class="info-label">ОТДЕЛ:</span>
                  <span class="info-value">{{ user.department }}</span>
                </div>
                <div class="info-row">
                  <span class="info-label">РЕГИСТРАЦИЯ:</span>
                  <span class="info-value">{{ formatDate(user.registered) }}</span>
                </div>
              </div>
              
              <div class="profile-stats">
                <div class="stat">
                  <div class="stat-number">42</div>
                  <div class="stat-label">СЕССИЙ</div>
                </div>
                <div class="stat">
                  <div class="stat-number">156</div>
                  <div class="stat-label">ОПЕРАЦИЙ</div>
                </div>
                <div class="stat">
                  <div class="stat-number">99.8%</div>
                  <div class="stat-label">АКТИВНОСТЬ</div>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Системная информация -->
          <div class="dashboard-card system-card">
            <div class="card-header">
              <h3 class="card-title">
                <i class="fas fa-server"></i>
                СИСТЕМНЫЙ СТАТУС
              </h3>
            </div>
            <div class="card-body">
              <div class="system-metrics">
                <div class="metric">
                  <div class="metric-header">
                    <span class="metric-title">НАГРУЗКА ЦП</span>
                    <span class="metric-value">34%</span>
                  </div>
                  <div class="metric-bar">
                    <div class="metric-fill" style="width: 34%"></div>
                  </div>
                </div>
                <div class="metric">
                  <div class="metric-header">
                    <span class="metric-title">ПАМЯТЬ</span>
                    <span class="metric-value">68%</span>
                  </div>
                  <div class="metric-bar">
                    <div class="metric-fill" style="width: 68%"></div>
                  </div>
                </div>
                <div class="metric">
                  <div class="metric-header">
                    <span class="metric-title">ХРАНИЛИЩЕ</span>
                    <span class="metric-value">42%</span>
                  </div>
                  <div class="metric-bar">
                    <div class="metric-fill" style="width: 42%"></div>
                  </div>
                </div>
                <div class="metric">
                  <div class="metric-header">
                    <span class="metric-title">СЕТЕВАЯ АКТИВНОСТЬ</span>
                    <span class="metric-value">12%</span>
                  </div>
                  <div class="metric-bar">
                    <div class="metric-fill" style="width: 12%"></div>
                  </div>
                </div>
              </div>
              
              <div class="system-info">
                <div class="info-item">
                  <i class="fas fa-shield-alt"></i>
                  <span>Защита: Активна</span>
                </div>
                <div class="info-item">
                  <i class="fas fa-sync-alt"></i>
                  <span>Обновления: Установлены</span>
                </div>
                <div class="info-item">
                  <i class="fas fa-network-wired"></i>
                  <span>Сеть: Стабильная</span>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Быстрые действия -->
        <div class="quick-actions-section">
          <div class="section-header">
            <h3 class="section-title">
              <i class="fas fa-bolt"></i>
              БЫСТРЫЕ ДЕЙСТВИЯ
            </h3>
          </div>
          <div class="actions-grid">
            <button class="action-btn" @click="showMessage('Создание нового отчета')">
              <div class="action-icon">
                <i class="fas fa-file-alt"></i>
              </div>
              <span class="action-text">НОВЫЙ ОТЧЕТ</span>
            </button>
            <button class="action-btn" @click="showMessage('Поиск в базе данных')">
              <div class="action-icon">
                <i class="fas fa-search"></i>
              </div>
              <span class="action-text">ПОИСК ДАННЫХ</span>
            </button>
            <button class="action-btn" @click="showMessage('Управление пользователями')">
              <div class="action-icon">
                <i class="fas fa-users-cog"></i>
              </div>
              <span class="action-text">ПОЛЬЗОВАТЕЛИ</span>
            </button>
            <button class="action-btn" @click="showMessage('Системные настройки')">
              <div class="action-icon">
                <i class="fas fa-sliders-h"></i>
              </div>
              <span class="action-text">НАСТРОЙКИ</span>
            </button>
          </div>
        </div>
        
        <!-- Последняя активность -->
        <div class="activity-section">
          <div class="section-header">
            <h3 class="section-title">
              <i class="fas fa-history"></i>
              ПОСЛЕДНЯЯ АКТИВНОСТЬ
            </h3>
          </div>
          <div class="activity-list">
            <div class="activity-item" v-for="(activity, index) in activities" :key="index">
              <div class="activity-icon">
                <i :class="activity.icon"></i>
              </div>
              <div class="activity-content">
                <div class="activity-text">{{ activity.text }}</div>
                <div class="activity-time">{{ activity.time }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    
    <!-- Футер -->
    <footer class="portal-footer">
      <div class="footer-content">
        <div class="footer-left">
          <span class="footer-text">SYSTEM PORTAL v2.1.0</span>
          <span class="footer-separator">|</span>
          <span class="footer-text">© 2024 Все права защищены</span>
        </div>
        <div class="footer-right">
          <span class="footer-text">Текущее время: {{ currentTime }}</span>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Portal',
  data() {
    return {
      user: {
        name: '',
        email: '',
        role: '',
        department: '',
        lastLogin: '',
        registered: ''
      },
      currentTime: '',
      activities: [
        { icon: 'fas fa-user-check', text: 'Успешная авторизация в систему', time: 'Только что' },
        { icon: 'fas fa-file-export', text: 'Экспорт данных в CSV формат', time: '15 минут назад' },
        { icon: 'fas fa-database', text: 'Резервное копирование базы данных', time: '1 час назад' },
        { icon: 'fas fa-chart-line', text: 'Генерация аналитического отчета', time: '3 часа назад' },
        { icon: 'fas fa-shield-alt', text: 'Обновление системы безопасности', time: '5 часов назад' }
      ]
    }
  },
  mounted() {
    // Загружаем данные пользователя
    const userData = localStorage.getItem('userData')
    
    if (userData) {
      this.user = JSON.parse(userData)
    } else {
      this.$router.push('/')
    }
    
    // Обновляем текущее время
    this.updateTime()
    this.timer = setInterval(this.updateTime, 1000)
  },
  beforeUnmount() {
    if (this.timer) {
      clearInterval(this.timer)
    }
  },
  methods: {
    updateTime() {
      const now = new Date()
      this.currentTime = now.toLocaleTimeString('ru-RU', {
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit'
      })
    },
    
    formatDate(dateString) {
      if (!dateString) return 'Неизвестно'
      const date = new Date(dateString)
      return date.toLocaleDateString('ru-RU', {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric'
      })
    },
    
    formatTime(dateString) {
      if (!dateString) return 'Неизвестно'
      const date = new Date(dateString)
      return date.toLocaleTimeString('ru-RU', {
        hour: '2-digit',
        minute: '2-digit'
      })
    },
    
    handleLogout() {
      localStorage.removeItem('isAuthenticated')
      localStorage.removeItem('userData')
      this.$router.push('/')
    },
    
    showMessage(text) {
      alert(`Действие: ${text}`)
    }
  }
}
</script>

<style scoped>
.portal-container {
  min-height: 100vh;
  background-color: #000000;
  color: #ffffff;
  display: flex;
  flex-direction: column;
}

/* Навигация */
.portal-nav {
  background: #111111;
  border-bottom: 1px solid #333333;
  padding: 0 30px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-left {
  display: flex;
  align-items: center;
  gap: 40px;
}

.nav-logo {
  display: flex;
  align-items: center;
  gap: 12px;
}

.nav-logo i {
  font-size: 20px;
  color: #ffffff;
}

.logo-text {
  color: #ffffff;
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 20px;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #888888;
  text-decoration: none;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 1px;
  padding: 8px 12px;
  border-radius: 2px;
  transition: all 0.2s;
}

.nav-link:hover {
  color: #ffffff;
  background: #222222;
}

.nav-link.active {
  color: #ffffff;
  background: #333333;
}

.nav-link i {
  font-size: 14px;
}

.nav-right {
  display: flex;
  align-items: center;
}

.user-menu {
  display: flex;
  align-items: center;
  gap: 15px;
}

.user-avatar {
  font-size: 32px;
  color: #ffffff;
}

.user-info {
  display: flex;
  flex-direction: column;
}

.user-name {
  font-size: 14px;
  font-weight: 500;
  color: #ffffff;
}

.user-role {
  font-size: 11px;
  color: #888888;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.logout-btn {
  background: #222222;
  border: 1px solid #333333;
  color: #ffffff;
  width: 36px;
  height: 36px;
  border-radius: 2px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s;
  margin-left: 10px;
}

.logout-btn:hover {
  background: #333333;
  border-color: #444444;
}

.logout-btn i {
  font-size: 16px;
}

/* Основной контент */
.portal-main {
  flex: 1;
  padding: 30px;
}

.welcome-section {
  margin-bottom: 30px;
}

.welcome-card {
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 30px;
}

.welcome-header {
  margin-bottom: 20px;
}

.welcome-title {
  font-size: 24px;
  font-weight: 500;
  color: #ffffff;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 12px;
}

.welcome-title i {
  color: #ffffff;
}

.welcome-subtitle {
  color: #888888;
  font-size: 14px;
  letter-spacing: 0.5px;
}

.status-indicators {
  display: flex;
  gap: 30px;
  flex-wrap: wrap;
}

.status-item {
  display: flex;
  align-items: center;
  gap: 10px;
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.status-dot.active {
  background: #00cc44;
  box-shadow: 0 0 10px rgba(0, 204, 68, 0.3);
}

.status-dot.secure {
  background: #0088ff;
  box-shadow: 0 0 10px rgba(0, 136, 255, 0.3);
}

.status-dot.online {
  background: #ffffff;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
}

.status-text {
  color: #cccccc;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Дашборд */
.user-dashboard {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 30px;
}

.dashboard-card {
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  overflow: hidden;
}

.card-header {
  background: #0a0a0a;
  padding: 20px 30px;
  border-bottom: 1px solid #333333;
}

.card-title {
  font-size: 16px;
  font-weight: 500;
  color: #ffffff;
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.card-title i {
  font-size: 16px;
}

.card-body {
  padding: 30px;
}

.profile-info {
  margin-bottom: 30px;
}

.info-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #222222;
}

.info-row:last-child {
  border-bottom: none;
}

.info-label {
  color: #888888;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.info-value {
  color: #ffffff;
  font-size: 14px;
  font-weight: 500;
}

.info-value.email {
  font-family: monospace;
  font-size: 13px;
}

.info-value.role {
  background: #333333;
  padding: 4px 12px;
  border-radius: 2px;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.profile-stats {
  display: flex;
  justify-content: space-between;
  padding-top: 20px;
  border-top: 1px solid #222222;
}

.stat {
  text-align: center;
}

.stat-number {
  color: #ffffff;
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 4px;
}

.stat-label {
  color: #888888;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.system-metrics {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 30px;
}

.metric {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.metric-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.metric-title {
  color: #cccccc;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.metric-value {
  color: #ffffff;
  font-size: 14px;
  font-weight: 500;
}

.metric-bar {
  height: 6px;
  background: #222222;
  border-radius: 3px;
  overflow: hidden;
}

.metric-fill {
  height: 100%;
  background: #ffffff;
  border-radius: 3px;
}

.system-info {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding-top: 20px;
  border-top: 1px solid #222222;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #cccccc;
  font-size: 14px;
}

.info-item i {
  color: #888888;
  width: 20px;
}

/* Быстрые действия */
.quick-actions-section {
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 30px;
}

.section-header {
  margin-bottom: 25px;
}

.section-title {
  font-size: 16px;
  font-weight: 500;
  color: #ffffff;
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.section-title i {
  font-size: 16px;
}

.actions-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

.action-btn {
  background: #0a0a0a;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 25px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  cursor: pointer;
  transition: all 0.2s;
}

.action-btn:hover {
  background: #222222;
  border-color: #444444;
  transform: translateY(-2px);
}

.action-icon {
  width: 48px;
  height: 48px;
  background: #111111;
  border: 1px solid #333333;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-icon i {
  font-size: 20px;
  color: #ffffff;
}

.action-text {
  color: #ffffff;
  font-size: 13px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Активность */
.activity-section {
  background: #111111;
  border: 1px solid #333333;
  border-radius: 2px;
  padding: 30px;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.activity-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background: #0a0a0a;
  border: 1px solid #222222;
  border-radius: 2px;
  transition: all 0.2s;
}

.activity-item:hover {
  background: #222222;
  border-color: #333333;
}

.activity-icon {
  width: 36px;
  height: 36px;
  background: #111111;
  border: 1px solid #333333;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.activity-icon i {
  font-size: 16px;
  color: #ffffff;
}

.activity-content {
  flex: 1;
}

.activity-text {
  color: #ffffff;
  font-size: 14px;
  margin-bottom: 4px;
}

.activity-time {
  color: #888888;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Футер */
.portal-footer {
  background: #111111;
  border-top: 1px solid #333333;
  padding: 20px 30px;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.footer-left, .footer-right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.footer-text {
  color: #555555;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.footer-separator {
  color: #333333;
}

@media (max-width: 1024px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .actions-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .portal-nav {
    padding: 0 20px;
    height: auto;
    flex-direction: column;
    padding: 15px;
    gap: 15px;
  }
  
  .nav-left {
    flex-direction: column;
    gap: 15px;
    width: 100%;
  }
  
  .nav-links {
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
  }
  
  .nav-right {
    width: 100%;
    justify-content: center;
  }
  
  .portal-main {
    padding: 20px;
  }
  
  .status-indicators {
    flex-direction: column;
    gap: 15px;
  }
  
  .dashboard-grid {
    gap: 20px;
  }
  
  .card-body {
    padding: 20px;
  }
  
  .actions-grid {
    grid-template-columns: 1fr;
  }
  
  .footer-content {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }
}
</style>
```

## 2. package.json

```json
{
  "name": "vue-auth-portal",
  "version": "1.0.0",
  "description": "Минималистичный Vue.js портал с авторизацией",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.3.4",
    "vue-router": "^4.2.4"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.3.1",
    "vite": "^4.4.5"
  },
  "keywords": ["vue", "auth", "portal", "black-white", "minimalist"],
  "author": "",
  "license": "MIT"
}
```

## 3. vite.config.js

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  server: {
    port: 3000,
    open: true,
    host: true
  },
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'terser'
  }
})
```

## 4. README.md

```markdown
# Vue Auth Portal

Минималистичное Vue.js приложение с системой авторизации и порталом в черно-белой цветовой гамме.

![Demo Screenshot](https://via.placeholder.com/800x450/000000/FFFFFF?text=Vue+Auth+Portal)

## 🚀 Особенности

- **Минималистичный дизайн** - Черно-белая цветовая гамма с акцентами серого
- **Полная авторизация** - Система входа с валидацией
- **Защищенный портал** - Личный кабинет после успешной авторизации
- **Адаптивный дизайн** - Корректное отображение на всех устройствах
- **Vue 3 Composition API** - Современный подход к разработке
- **Vue Router** - Навигация между страницами
- **Локальное хранение** - Сохранение сессии в localStorage

## 📁 Структура проекта

```
vue-auth-portal/
├── public/
│   └── index.html
├── src/
│   ├── components/
│   │   ├── Login.vue    # Компонент авторизации
│   │   └── Portal.vue   # Компонент портала
│   ├── App.vue          # Корневой компонент
│   └── main.js          # Точка входа
├── package.json         # Зависимости и скрипты
├── vite.config.js       # Конфигурация Vite
└── README.md           # Документация
```

## 🛠 Установка и запуск

### Предварительные требования
- Node.js 16 или выше
- npm 7 или выше

### Установка

1. Клонируйте репозиторий:
```bash
git clone <repository-url>
cd vue-auth-portal
```

2. Установите зависимости:
```bash
npm install
```

### Запуск в режиме разработки
```bash
npm run dev
```
Приложение будет доступно по адресу: [http://localhost:3000](http://localhost:3000)

### Сборка для production
```bash
npm run build
```

### Просмотр собранного приложения
```bash
npm run preview
```

## 👤 Тестовые учетные записи

Для тестирования используйте следующие учетные данные:

| Email | Пароль | Роль |
|-------|--------|------|
| admin@portal.ru | admin123 | Администратор |
| user@portal.ru | user123 | Пользователь |
| manager@portal.ru | manager123 | Менеджер |


## 🔧 Технологии

- **Vue.js 3** - Прогрессивный JavaScript фреймворк
- **Vue Router 4** - Официальный маршрутизатор для Vue.js
- **Vite** - Современный инструмент сборки
- **Font Awesome** - Иконки
- **CSS3** - Современные стили и анимации

## 📱 Адаптивность

Приложение полностью адаптивно и корректно отображается на:
- Десктопах (от 1200px)
- Планшетах (768px - 1199px)
- Мобильных устройствах (до 767px)

## 🔒 Безопасность

- Валидация форм на стороне клиента
- Защищенные маршруты с проверкой авторизации
- Хранение данных сессии в localStorage
- Защита от XSS атак


**Примечание**: Это учебное приложение, предназначенное для демонстрации возможностей Vue.js. В реальных проектах требуется бэкенд для обработки аутентификации и хранения данных.
```

## 5. Инструкция по запуску

```bash
# 1. Клонируйте/создайте проект
mkdir vue-auth-portal
cd vue-auth-portal

# 2. Инициализируйте npm проект
npm init -y

# 3. Установите зависимости
npm install vue vue-router
npm install --save-dev vite @vitejs/plugin-vue

# 4. Создайте структуру папок и файлы как указано выше

# 5. Запустите приложение
npm run dev
```

Приложение запускается командой `npm run dev` и открывается на `http://localhost:3000`.