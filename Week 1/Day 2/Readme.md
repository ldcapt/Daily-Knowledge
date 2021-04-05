# [**Promise in JavaScript**](https://ehkoo.com/bai-viet/tat-tan-tat-ve-promise-va-async-await#header)

---

# Danh sách từ khóa
1. [AJAX](#)
2. [WebSocket](#)
3. [Worker](#)


---

## **Promise là gì?**

> `Promise` là một đối tượng sẽ trả về một giá trị trong tương lai

- Promise là một **cơ chế** trong JavaScript giúp bạn thực thi các tác vụ bất đồng bộ mà không bị rơi vào `callback hell` hay là tình trạng các hàm `callback()` lồng vào nhau ở quá nhiều tầng. Các tác vụ bất đồng bộ có thể là gửi AJAX request, gọi hàm bên trong `setTimeout`, `setInterval` hoặc `requestAnimationFrame`, hay thao tác với WebSocket hoặc Worker ... Dưới đây là một `callback hell` điển hình.

```js
api.getUser('pikaklog', function(err, user) {
  if (err) throw err
  api.getPostsOfUser(user, function(err, posts) {
    if (err) throw err
    api.getCommentsOfPosts(posts, function(err, comments) {
      // vân vân và mây mây ...
    })
  })
})
```

Ví dụ trên được viết lại bằng `Promise` sẽ là

```js
api
  .getUser('pikalong')
  .then(user => api.getPostsOfUser(user))
  .then(posts => api.getCommentsOfPosts(posts))
  .catch(err => {
    throw err
  })
```

Để tạo một `Promise` object thì dùng class Promise có sẵn trong trình duyệt như sau

```js
const p = new Promise(
  /* executor */ function(resolve, reject) {
    // Thực thi các tác vụ bất đồng bộ ở đây, và gọi `resolve(result)` 
    // khi tác vụ hoàn thành. Nếu xảy ra lỗi, gọi đến `reject(error)`
  }
)
```

Trong đó `executor` là một hàm có 2 tham số:
- `resolve` là hàm sẽ được gọi sau khi `Promise` hoàn thành
- `reject` là hàm sẽ được gọi khi có lỗi xảy ra


Ví dụ:
```js
api.getUser = function(username) {
  // Hàm api.getUser() trả về một promise object
  http.get(`/users/${username}`, (err, result) = > {
    // Nếu có lỗi bên trong callback, gọi đến hàm `reject()`
    if (err) return reject(err);
    
    // Ngược lại, dùng `resolve()` để trả về dữ liệu cho `.then()`
    resolve(result);
  })
}
```
Như vậy `api.getUser()` sẽ trả về một **promise object**. Chúng ta có thể truy xuất đến kết quả trả về bằng phương thức `.then()` như sau
```js
function onSuccess(user) {
  console.log(user);
}

function onError(err) {
  console.error(err)
}

api.getUser('pikalong').then(onSuccess, onError)
```

Phương thức `.then(onSuccess, onError)` nhận vào 2 hàm: `onSuccess` được gọi khi **promise** hoàn thành và `onError` được gọi khi có lỗi xảy ra. Bên trong tham số `onSuccess` bạn có thể trả về một giá trị đồng bộ. Ví dụ như giá trị số, chuỗi, `null`, `undefined`, array, object hoặc một **promise object** khác. Các giá trị bất đồng bộ sẽ được bọc bên trong một Promise, cho phép bạn kết nối (chaining) nhiều **promise** lại với nhau.

```js
promise() 
  .then(() => {
    return 'foo';
  })
  .then(result1 => {
    console.log(result1); // 'foo'
    return anotherPromise();
  })
  .then(result2 => console.log(result2)) // `result2` là kết quả của anotherPromise()
  .catch(err => {
    console.log(err);
  })
```
