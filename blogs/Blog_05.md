
# Viết code ES6 tối giản

Trong hệ sinh thái JavaScript thay đổi liên tục, việc thiết kế code đơn giản và hiệu quả ngày càng quan trọng. Sự ra đời của ES6 đã giới thiệu một loạt các tính năng mới giúp đơn giản hóa và tối ưu hóa cách viết JavaScript. Bài viết này thảo luận về các phương pháp ES6 mới có thể thay thế các mẫu cũ, phức tạp, giúp code trở nên gọn gàng và dễ đọc hơn. Từ việc ép kiểu Boolean đến toán tử spread mạnh mẽ, những chiến lược này giúp tăng khả năng đọc, hiệu suất, và tính bảo trì của code. Hãy cùng xem qua một số kỹ thuật thiết yếu mà bất kỳ lập trình viên JavaScript nào cũng nên theo để viết code ES6 tối giản.

## 1. Ép kiểu Boolean
Phương pháp được đề xuất hiện nay theo hướng dẫn của Airbnb:
```javascript
const age = Boolean(input.value) // cũ
const age = !!input.value // mới
```

## 2. Nullish Coalescing
Trả về giá trị bên phải khi toán hạng bên trái là null hoặc undefined.
```javascript
// cũ
const addId = (user, id) => {
     user.id = 
          id !== null && id !== undefined
               ? id
               : "Unknown"
     return user
}

// mới
const addId = (user, id) => {
     user.id = id ?? "Unknown"
     return user
}
```

## 3. Default Parameters
Mô tả: Tham số hàm mặc định là undefined, vì vậy việc đặt giá trị mặc định là rất hữu ích.
```javascript
// cũ
const createUser = (name, email) => {
     const user = {
          email,
          name: name ?? "Unknown",
     }
     // tạo user
}

// mới
const createUser = (
     name = "Unknown",
     email
) => {
     const user = { email, name }
     // tạo user
}
```

## 4. Optional Chaining
Mô tả: Giúp đọc giá trị của thuộc tính lồng sâu mà không cần kiểm tra từng bước.
```javascript
// cũ
const hasValidPostcode = u => 
     u &&
     u.address &&
     u.address.postcode &&
     u.address.postcode.valid

// mới
const hasValidPostcode = u => u?.address?.postcode?.valid
```

## 5. Destructuring Objects
Mô tả: Giảm số lượng code bằng cách giải nén thuộc tính từ object thành các biến riêng biệt.
```javascript
// cũ
const save = params => {
     saveData(
          params.name,
          params.email,
          params.dob
     )
}

// mới
const save = ({name, email, dob}) => {
     saveData(name, email, dob)
}
```

## 6. Destructuring Arrays
Mô tả: Giảm số lượng code bằng cách giải nén giá trị từ mảng thành các biến riêng biệt.
```javascript
// cũ
const data = [
     ["axios", "recharts"],
     ["flocked", "flick"]
]

const plugins = data[0], apps = data[1]

// mới
const data = [
     ["axios", "recharts"],
     ["flocked", "flick"]
]

const [plugins, apps] = data
```

## 7. Spread Operator
Mô tả: Kết hợp hai object thành một bằng cú pháp rất gọn gàng khi làm việc với object.
```javascript
// cũ
const details = {name: "Man Utd"}
const stats = {games: 7, points: 21}

const team = Object.assign(
     {},
     details,
     stats
)

// mới
const details = {name: "Man Utd"}
const stats = {games: 7, points: 21}

const team = {
     ...details,
     ...stats
}
```

## 8. For(of)
Mô tả: Lượng code có thể tương đương, nhưng for(of) được biết là nhanh hơn forEach khoảng 24%.
```javascript
// cũ
const array = []
const fillArray = items => {
     items.forEach(i => array.push(i))
}

// mới
const array = []
const fillArray = items => {
     for (let i of items) {
          array.push(i)
     }
}
```

## Kết luận

Tóm lại, việc sử dụng các tính năng ES6 có thể đơn giản hóa đáng kể code JavaScript, làm cho nó ngắn gọn, dễ đọc và hiệu quả hơn. Việc tích hợp các phương pháp hiện đại này sẽ mang lại code gọn gàng và dễ bảo trì hơn, cải thiện cả quá trình phát triển và hiệu suất. Hãy áp dụng những cách tiếp cận này để nâng cao tiêu chuẩn code và đơn giản hóa công việc của mình.

Chúc mọi người code vui vẻ 👨‍💻
