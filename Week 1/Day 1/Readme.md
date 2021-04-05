# [Asyn / Await in JavaScript](https://viblo.asia/p/giai-thich-ve-asyncawait-javascript-trong-10-phut-1VgZvBn7ZAw)


# [Danh sách từ khóa]
1. [Promise](Week%201/Day%202)


## **Async / Await là?**
+ Một tính năng của JS giúp chúng ta làm việc với các hàm bất đồng bộ theo cách thú vị hơn và dễ hiểu hơn. Nó được xây dựng trên `Promises` và tương thích với tất cả các Promise dựa trên API

## **Async - Khai báo một hàm bất đồng bộ**
+ Tự động biến đổi một hàm thông thường thành một `Promise`.
+ Khi gọi tới hàm `async` nó sẽ xử lý mọi thứ và được trả về kết quả trong hàm của nó
+ `Asyn` cho phép sử dụng `await`

## **Await - Tạm dừng việc thực thi các hàm async**
+ Khi được đặt trước một `Promise`, nó sẽ đợi cho đến khi Promise kết thúc và trả về kết quả
+ `Await` chỉ làm việc với `Promises`, nó không hoạt động với `callbacks()`
+ `Await` chỉ có thể được sử dụng bên trong các function `async`

## **Khi có Async / Await có làm cho promises lỗi thời?**

- Không hoàn toàn như vậy. Khi làm việc với Async / Await, thật ra chúng ta vẫn đang sử dụng ngầm Promises. Vì thế, kể cả khi đang sử dụng Async / Await mà có sự hiểu biết tốt về Promises sẽ rất tốt cho chúng ta.

- Ngoài ra, có những  trường hợp mà Async / Await không sử dụng được mà chúng ta phải sử dụng Promises. Ví dụ như khi chúng ta cần gọi nhiều thao tác bất đồng bộ và chờ cho tất cả chúng thực hiện xong. Nếu thử làm điều này với async và await, điều gì sẽ xảy ra?

```js
async function getABC () {
  let A = await getValueA(); // getValueA take 2 second to finish
  let B = await getValueB(); // getValueB take 4 second to finish
  let C = await getValueC(); // getValueC take 3 second to finish

  return A*B*C;
}
```

- Mỗi lần gọi tới hàm await sẽ đợi cho đến khi hàm await trước đó kết thúc. Vì các wait sẽ đợi và thực hiện từng cái một, toàn bộ chức năng sẽ mất 9 giây để thực hiện xong hàm từ đầu đến cuối (2 + 3 + 4)

- Đây không phải là một giải pháp tối ưu vì A, B và C không phụ thuộc vào nhau, chúng ta có thể lấy chúng cùng một lúc và thời gian chờ sẽ giảm bớt đi

- Trong trường hợp như thế này, sử dụng `Promise` sẽ thích hợp hơn. Để gửi tất cả các yêu cầu cùng lúc, chúng ta sử dụng `Promise.all()`. Việc sử dụng `Promise.all()` sẽ đảm bảo chúng ta có tất cả các kết quả trước khi tiếp tục thực thi code, tuy nhiên việc gọi đến các hàm bất đồng bộ sẽ được chạy song song mà không phải tuần tự từng cái một.

```js
async function getABC () {
  // Promise.all() allow us to send all requests at the same time.
  let result = await Promise.all([ getValueA, getValueB, getValueC ]);
  
  return results.reduce((total,value) => total * value);
}
```
- Bằng cách này, thời gian thực thi sẽ mất ít hơn, hàm `getValueA()` và `getValueC()` sẽ thực hiện xong trước khi `getValueB()` xong. Thay vì phải mất 9 giây để chờ từng hàm trả về thì sẽ chỉ mất 4 giây.


## **Xử lý lỗi trong Async / Await**

- Một điều hay ho khác về `Async` / `Await` là nó cho phép chúng ta bắt các lỗi bằng cách sử dụng `try` / `catch`. Chúng ta chỉ cần để các `await call` vào trong khối `try` / `catch` :
```js
async function doSomethingAsync () {
  try {
    // This async call may fail.
    let result = await someAsyncCall();
  }
  catch (error) {
    // If it does we will catch the error here.
  }
}
```
- Vế `catch` sẽ xử lý các lỗi gây ra bởi các hàm bất đồng bộ hoặc bất kỳ lỗi nào chúng ta có thể gặp phải bên trong khối try.

- Trong một vài tình huống, chúng ta cũng có thể bắt các lỗi khi đang thực hiện `function async`. Vì tất cả các hàm `async` đều trả về `Promises`, chúng ta chỉ cần gọi thêm hàm `.catch()` khi gọi chúng

```js
// Async function without a try / catch block.
async function doSomethingAsync () {
  // This async call may fail.
  let result = await someAsyncCall();
  return result;
}

// We catch the error upon calling the function
doSomethingAsync()
    .then(successHandler)
    .catch(errorHandler);
```

- Dựa vào tình huống cụ thể, chúng ta sẽ sử dụng `try / catch` hoặc `.catch()` để bắt và xử lý lỗi. Tuy nhiên chúng ta không nên sử dụng cả 2 cùng một lúc vì có thể dẫn đến các vấn đề không mong muốn.

## **Hỗ trợ trình duyệt**

- `Asyn` / `Await` có thể sử dụng trong hầu hết các trình duyệt chính, ngoại trừ `IE11` - tất cả các trình duyệt sẽ nhận ra mã `async` / `await` của bạn mà không cần các thư viện bên ngoài.

## **Kết luận**

- Với việc bổ sung Async / Await trong ngôn ngữ JavaScript có một bước nhảy vọt về khả năng dễ đọc và dễ sử dụng cho người mới bắt đầu lập trình JavaScript và người đã có kinh nghiệm

- Link tham khảo : https://viblo.asia/p/giai-thich-ve-asyncawait-javascript-trong-10-phut-1VgZvBn7ZAw


---

### Sử dụng callbacks() để làm việc với bất đồng bộ trong javascript

- Nhược điểm:
   + Các `callbacks()` phải chờ nhau thực hiện -> Thời gian hoàn thành sẽ kéo dài hơn
   + Viết các `callbacks()` lồng nhau khiến cho chương trình rắc rối và khó bảo trì.


