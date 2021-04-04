# [Asyn / Await in JavaScript](https://viblo.asia/p/giai-thich-ve-asyncawait-javascript-trong-10-phut-1VgZvBn7ZAw)


- **Async / Await là?**
   + Một tính năng của JS giúp chúng ta làm việc với các hàm bất đồng bộ theo cách thú vị hơn và dễ hiểu hơn. Nó được xây dựng trên `Promises` và tương thích với tất cả các Promise dựa trên API

- **Async - Khai báo một hàm bất đồng bộ**
   + Tự động biến đổi một hàm thông thường thành một `Promise`.
   + Khi gọi tới hàm `async` nó sẽ xử lý mọi thứ và được trả về kết quả trong hàm của nó
   + `Asyn` cho phép sử dụng `await`

- **Await - Tạm dừng việc thực thi các hàm async**
   + Khi được đặt trước một `Promise`, nó sẽ đợi cho đến khi Promise kết thúc và trả về kết quả
   + `Await` chỉ làm việc với `Promises`, nó không hoạt động với `callbacks()`
   + `Await` chỉ có thể được sử dụng bên trong các function `async`


### Sử dụng callbacks() để làm việc với bất đồng bộ trong javascript

- Nhược điểm:
   + Các `callbacks()` phải chờ nhau thực hiện -> Thời gian hoàn thành sẽ kéo dài hơn
   + Viết các `callbacks()` lồng nhau khiến cho chương trình rắc rối và khó bảo trì.