
# Top 10 Best Practices trong JavaScript để Viết Code Sạch Sẽ

Viết code JavaScript sạch, dễ bảo trì là điều cực kỳ quan trọng để tạo ra các ứng dụng có khả năng mở rộng và làm việc hiệu quả với các lập trình viên khác. Dưới đây là mười best practices giúp bạn viết code JavaScript sạch và dễ hiểu. 🌟

## 1. Sử Dụng Tên Biến và Hàm Có Ý Nghĩa 📝
Chọn tên biến và hàm có ý nghĩa và mô tả rõ ràng sẽ làm cho code của bạn dễ hiểu và bảo trì hơn. Hãy tránh sử dụng các tên viết tắt hoặc chỉ có một chữ cái, vì chúng có thể gây khó hiểu cho người khác.

**Ví dụ:**

```javascript
// Cách đặt tên chưa tốt
let a = 5;
let b = 10;
function c(x, y) {
  return x + y;
}

// Cách đặt tên tốt
let width = 5;
let height = 10;
function calculateArea(width, height) {
  return width * height;
}
```

## 2. Giữ Các Hàm Nhỏ Gọn và Tập Trung Vào Một Nhiệm Vụ 🎯
Những hàm nhỏ gọn, tập trung vào một nhiệm vụ cụ thể sẽ dễ dàng hơn trong việc kiểm thử, gỡ lỗi, và bảo trì. Hãy viết các hàm thực hiện một công việc và làm thật tốt công việc đó.

**Ví dụ:**

```javascript
// Hàm lớn, không tập trung
function processUserData(user) {
  // Kiểm tra dữ liệu người dùng
  if (!user.name hoặc !user.email) {
    throw new Error('Dữ liệu người dùng không hợp lệ');
  }
  // Lưu người dùng vào cơ sở dữ liệu
  database.save(user);
  // Gửi email chào mừng
  emailService.sendWelcomeEmail(user.email);
}

// Hàm nhỏ, tập trung
function validateUserData(user) {
  if (!user.name hoặc !user.email) {
    throw new Error('Dữ liệu người dùng không hợp lệ');
  }
}

function saveUserToDatabase(user) {
  database.save(user);
}

function sendWelcomeEmail(email) {
  emailService.sendWelcomeEmail(email);
}

function processUserData(user) {
  validateUserData(user);
  saveUserToDatabase(user);
  sendWelcomeEmail(user.email);
}
```

## 3. Sử Dụng Phong Cách Viết Code Đồng Nhất 🌐
Một phong cách viết code đồng nhất giúp code của bạn dễ đọc và dễ bảo trì hơn. Sử dụng các công cụ như ESLint và Prettier để duy trì sự đồng nhất trong toàn bộ mã nguồn.

**Ví dụ (Cấu hình ESLint):**

```json
{
  "extends": "eslint:recommended",
  "env": {
    "browser": true,
    "es6": true
  },
  "rules": {
    "indent": ["error", 2],
    "quotes": ["error", "single"],
    "semi": ["error", "always"]
  }
}
```

## Kết

Cảm ơn anh em đã đọc bài viết. Hi vọng bài viết sẽ giúp ích cho anh em.

Anh em kết nối với mình qua LinkedIn để có nhiều bài viết hay khác nhé: [www.linkedin.com/in/pdthien](www.linkedin.com/in/pdthien)
