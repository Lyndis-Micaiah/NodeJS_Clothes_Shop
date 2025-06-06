<template>
  <div class="register-container">
    <h1 class="fadeIn">Đăng ký tài khoản</h1>
    <form @submit.prevent="handleRegister" class="form-container">
      <div class="form-group zoomIn">
        <label for="username">Tên đăng nhập</label>
        <input 
          type="text" 
          id="username" 
          v-model="form.username" 
          placeholder="Nhập tên đăng nhập" 
          required 
          class="input-field"
        />
      </div>
      <div class="form-group zoomIn">
        <label for="password">Mật khẩu</label>
        <input 
          type="password" 
          id="password" 
          v-model="form.password" 
          placeholder="Nhập mật khẩu" 
          required 
          class="input-field"
        />
      </div>
      <div class="form-group zoomIn">
        <label for="email">Email</label>
        <input 
          type="email" 
          id="email" 
          v-model="form.email" 
          placeholder="Nhập email" 
          required 
          class="input-field"
        />
      </div>
      <div class="form-group zoomIn">
        <label for="fullName">Họ và tên</label>
        <input 
          type="text" 
          id="fullName" 
          v-model="form.fullName" 
          placeholder="Nhập họ và tên" 
          required 
          class="input-field"
        />
      </div>
      <button type="submit" class="btn-submit">Đăng ký</button>
    </form>
    <p class="login-link">
      Bạn đã có tài khoản? 
      <router-link to="/login" class="highlight">Đăng nhập</router-link>
    </p>
  </div>
</template>

<script>
import axios from 'axios';
import { mapActions } from 'vuex';

export default {
  name: "AppRegister",
  data() {
    return {
      form: {
        username: "",
        password: "",
        email: "",
        fullName: ""
      },
      accounts: []
    };
  },
  created() {
    this.fetchAccounts();
  },
  methods: {
    ...mapActions(['setLoginStatus']),
    async fetchAccounts() {
      try {
        const response = await axios.get('http://localhost:3000/api/accounts');
        this.accounts = response.data;
      } catch (error) {
        console.error('Error fetching accounts:', error);
      }
    },
    async handleRegister() {
      const existingAccount = this.accounts.find(acc => acc.username === this.form.username);
      if (existingAccount) {
        alert("Tên đăng nhập đã được sử dụng.");
        return;
      }

      const newAccount = {
        username: this.form.username,
        password: this.form.password,
        email: this.form.email,
        fullName: this.form.fullName,
        createdAt: new Date().toISOString()
      };

      try {
        await axios.post('http://localhost:3000/api/accounts', newAccount);
        console.log("Đăng ký thành công với tài khoản:", newAccount);
        alert("Đăng ký thành công! Bây giờ bạn có thể đăng nhập.");
        this.$router.push('/login');
      } catch (error) {
        console.error('Error registering account:', error);
        alert('Đăng ký thất bại. Vui lòng thử lại.');
      }
    }
  }
};
</script>

<style scoped>
/* Tổng thể */
.register-container {
  max-width: 450px;
  margin: 80px auto;
  padding: 40px;
  border-radius: 12px;
  background: linear-gradient(to bottom, #00c6ff, #0072ff);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  animation: slideIn 1s ease-out;
  text-align: center;
  font-family: 'Roboto', sans-serif;
}

/* Hiệu ứng cho tiêu đề */
h1 {
  font-size: 32px;
  color: #fff;
  margin-bottom: 20px;
  animation: fadeIn 2s ease-out;
}

/* Các trường nhập liệu */
.form-container {
  margin-top: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group input {
  width: 100%;
  padding: 12px;
  border-radius: 8px;
  border: 2px solid #ddd;
  font-size: 16px;
  transition: all 0.3s ease;
}

/* Hiệu ứng khi focus vào các trường nhập liệu */
.input-field:focus {
  border-color: #00c6ff;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 198, 255, 0.6);
}

/* Nút Đăng ký */
.btn-submit {
  width: 100%;
  padding: 14px;
  border-radius: 8px;
  border: none;
  background-color: #00c6ff;
  color: #fff;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.btn-submit:hover {
  background-color: #0072ff;
  transform: scale(1.05);
}

/* Liên kết đăng nhập */
.login-link {
  margin-top: 15px;
  font-size: 14px;
  color: #fff;
}

.login-link .highlight {
  color: #00c6ff;
  text-decoration: none;
  transition: color 0.3s ease;
}

.login-link .highlight:hover {
  color: #0072ff;
  text-decoration: underline;
}

/* Hiệu ứng cho form nhập liệu */
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

/* Hiệu ứng khi hover vào các ô nhập liệu */
@keyframes zoomIn {
  0% {
    transform: scale(0.9);
    opacity: 0;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

/* Hiệu ứng cho overlay khi click vào form */
@keyframes slideIn {
  0% {
    transform: translateY(100%);
  }
  100% {
    transform: translateY(0);
  }
}
</style>
