
# Refactoring Tốt và Xấu

Là một lập trình viên với nhiều năm kinh nghiệm, mình đã từng thuê và làm việc với rất nhiều đồng nghiệp khác nhau. Một trong những tình huống phổ biến mình thường gặp là có nhiều người rất tin rằng mã nguồn hiện tại cần phải được refactor lại mạnh mẽ. Tuy nhiên, thực tế cho thấy, hầu hết những mã nguồn đã được refactor đó thường trở nên khó hiểu hơn, khó bảo trì hơn, chạy chậm hơn và thậm chí còn nhiều lỗi hơn.

Đừng hiểu sai, refactoring là một việc cần thiết và quan trọng để duy trì chất lượng mã nguồn. Vấn đề chỉ nảy sinh khi việc refactor được thực hiện sai cách, và rất dễ rơi vào tình huống làm cho mọi thứ tệ hơn khi bạn đang cố gắng cải thiện nó.

Hãy cùng nhau tìm hiểu điều gì tạo nên một lần refactor tốt và cách tránh các cạm bẫy khiến mã nguồn trở nên kém chất lượng.

## Những Yếu Tố Chính Trong Refactoring

### 1. Thay Đổi Phong Cách Lập Trình Quá Mức

Một lỗi thường gặp là khi lập trình viên thay đổi toàn bộ phong cách lập trình trong quá trình refactor. Điều này thường xảy ra khi người lập trình đến từ một nền tảng khác hoặc có quan điểm mạnh về một phong cách lập trình nhất định.

**Ví dụ:**

Trước khi refactor:

```javascript
function processUsers(users) {
  const result = [];
  for (let i = 0; i < users.length; i++) {
    if (users[i].age >= 18) {
      result.push({
        name: users[i].name.toUpperCase(),
        age: users[i].age,
        isAdult: true,
      });
    }
  }
  return result;
}
```

Sau khi refactor không đúng cách:

```javascript
import * as R from 'ramda';

const processUsers = R.pipe(
  R.filter(R.propSatisfies(R.gte(R.__, 18), 'age')),
  R.map(R.applySpec({
    name: R.pipe(R.prop('name'), R.toUpper),
    age: R.prop('age'),
    isAdult: R.always(true),
  }))
);
```

Mặc dù đoạn mã này có thể hấp dẫn đối với những ai thích lập trình hàm, nhưng nó lại làm tăng độ phức tạp không cần thiết, đặc biệt là khi nhóm của bạn không quen thuộc với thư viện mới như Ramda.

Refactor tốt hơn:

```javascript
function processUsers(users) {
  return users
    .filter(user => user.age >= 18)
    .map(user => ({
      name: user.name.toUpperCase(),
      age: user.age,
      isAdult: true,
    }));
}
```

Phiên bản này gọn gàng hơn, dễ đọc hơn và không đòi hỏi sự thay đổi lớn trong cách tiếp cận của nhóm.

### 2. Trừu Tượng Hóa Không Cần Thiết

Thêm abstraction vào mã nguồn có thể làm cho mọi thứ trở nên phức tạp và khó bảo trì, đặc biệt khi nó không thực sự cần thiết. 

**Ví dụ:**

Trước khi refactor:

```javascript
function processUsers(users) {
  const result = [];
  for (let i = 0; i < users.length; i++) {
    if (users[i].age >= 18) {
      result.push({
        name: users[i].name.toUpperCase(),
        age: users[i].age,
        isAdult: true,
      });
    }
  }
  return result;
}
```

Sau khi refactor không cần thiết:

```javascript
class UserProcessor {
  constructor(users) {
    this.users = users;
  }

  process() {
    return this.filterAdults().formatUsers();
  }

  filterAdults() {
    this.users = this.users.filter(user => user.age >= 18);
    return this;
  }

  formatUsers() {
    return this.users.map(user => ({
      name: this.formatName(user.name),
      age: user.age,
      isAdult: true,
    }));
  }

  formatName(name) {
    return name.toUpperCase();
  }
}

const processUsers = (users) => {
  return new UserProcessor(users).process();
};
```

Đoạn mã này tuy có vẻ "hướng đối tượng" hơn, nhưng thực tế lại phức tạp hơn và khó hiểu hơn.

Refactor tốt hơn:

```javascript
const isAdult = user => user.age >= 18;
const formatUser = user => ({
  name: user.name.toUpperCase(),
  age: user.age,
  isAdult: true,
});

function processUsers(users) {
  return users.filter(isAdult).map(formatUser);
}
```

Đây là một cách tiếp cận gọn gàng, dễ hiểu và không làm tăng độ phức tạp không cần thiết.

### 3. Tạo Ra Sự Không Nhất Quán

Đôi khi, lập trình viên cập nhật một phần của mã để nó hoạt động khác hoàn toàn với phần còn lại. Điều này có thể gây bối rối và làm giảm hiệu suất làm việc của cả đội.

**Ví dụ:**

Nếu toàn bộ ứng dụng React sử dụng React Query để lấy dữ liệu:

```javascript
import { useQuery } from 'react-query';

function UserProfile({ userId }) {
  const { data: user, isLoading } = useQuery(['user', userId], fetchUser);

  if (isLoading) return <div>Loading...</div>;
  return <div>{user.name}</div>;
}
```

Thế nhưng, một thành viên lại quyết định sử dụng Redux Toolkit cho một thành phần duy nhất:

```javascript
import { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchPosts } from './postsSlice';

function PostList() {
  const dispatch = useDispatch();
  const { posts, status } = useSelector(state => state.posts);

  useEffect(() => {
    dispatch(fetchPosts());
  }, [dispatch]);

  if (status === 'loading') return <div>Loading...</div>;
  return <div>{posts.map(post => <div key={post.id}>{post.title}</div>)}</div>;
}
```

Điều này làm cho mã nguồn không nhất quán và khó duy trì.

Refactor tốt hơn:

```javascript
import { useQuery } from 'react-query';

function PostList() {
  const { data: posts, isLoading } = useQuery('posts', fetchPosts);

  if (isLoading) return <div>Loading...</div>;
  return <div>{posts.map(post => <div key={post.id}>{post.title}</div>)}</div>;
}
```

Giữ mã nhất quán là điều rất quan trọng để giúp cả đội dễ dàng làm việc và bảo trì mã nguồn.

### 4. Không Hiểu Rõ Mã Trước Khi Refactor

Refactor một đoạn mã mà bạn chưa hiểu rõ có thể dẫn đến việc tạo ra lỗi mới hoặc làm giảm hiệu suất. Điều quan trọng là bạn phải nắm rõ mã nguồn trước khi thực hiện bất kỳ thay đổi lớn nào.

**Ví dụ:**

Trước khi refactor:

```javascript
function fetchUserData(userId) {
  const cachedData = localStorage.getItem(`user_${userId}`);
  if (cachedData) {
    return JSON.parse(cachedData);
  }

  return api.fetchUser(userId).then(userData => {
    localStorage.setItem(`user_${userId}`, JSON.stringify(userData));
    return userData;
  });
}
```

Refactor không đúng cách:

```javascript
function fetchUserData(userId) {
  return api.fetchUser(userId);
}
```

Phiên bản này loại bỏ cơ chế caching, làm tăng số lượng gọi API và có thể làm giảm hiệu suất của ứng dụng.

Refactor đúng cách:

```javascript
async function fetchUserData(userId) {
  const cachedData = await cacheManager.get(`user_${userId}`);
  if (cachedData) {
    return cachedData;
  }

  const userData = await api.fetchUser(userId);
  await cacheManager.set(`user_${userId}`, userData, { expiresIn: '1h' });
  return userData;
}
```

Phiên bản này giữ lại cơ chế caching và có thể cải thiện bằng cách sử dụng một hệ thống cache phức tạp hơn.

## Kết Luận

Refactoring là một phần không thể thiếu trong phát triển phần mềm, nhưng nó cần được thực hiện một cách thận trọng và phải hiểu rõ ngữ cảnh của mã nguồn. Mục tiêu là cải thiện mã mà không làm thay đổi hành vi bên ngoài của nó.

Lần tới khi bạn cảm thấy muốn thực hiện thay đổi lớn cho một đoạn mã, hãy dừng lại và suy nghĩ cẩn thận. Hiểu rõ mã trước khi thay đổi, giữ mã nhất quán và thực hiện những cải tiến nhỏ mà có giá trị lớn. Đồng đội của bạn sẽ cảm ơn bạn vì điều đó!
