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