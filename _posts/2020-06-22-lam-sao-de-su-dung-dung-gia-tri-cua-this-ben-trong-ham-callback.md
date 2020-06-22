---
layout: post
title: Làm sao để sử dụng đúng giá trị của 'this' bên trong hàm callback?
subtitle:
cover-img: /assets/img/path.jpg
tags: [Javascrip]
---
Đối với những bạn mới học JavaScript thì **this** là một cái gì đó rất huyền bí. Và một trong những lỗi sai phổ biến, đó là sử dụng sai vai trò của **this** bên trong hàm callback.<br>
Ví dụ:
```javascript
class Helper {
  constructor() {
    this.number = 0;
  }

  increase() {
    setTimeout(function() {
      this.number++;
      console.log(this.number); // => NaN
    }, 100);
  }
}

const helper = new Helper();
helper.increase();
```
Trong đoạn code trên, console hiển thị giá trị **NaN** thay vì **1** - như suy nghĩ của hầu hết các newbie. Vậy làm sao để sử dụng đúng giá trị của this bên trong hàm callback?<br>
# Cơ bản về this bên trong function JavaScript
Từ khoá **this** bên trong function JavaScript là một từ khoá đặc biệt. Giá trị của **this** xác định dựa trên cách mà function được gọi, chứ không phải dựa vào vị trí mà nó được định nghĩa.<br>
Ví dụ: <br>
```javascript
function foo() {
  console.log(this);
}

foo(); // 'this' tương ứng với đối tượng Window

const obj = {bar: foo};
obj.bar(); // 'this' tương ứng với đối tượng 'obj'

const a = new foo(); // 'this' tương ứng với đối tượng mới tạo ra 'a'
```
Hoặc một ví dụ khác:
```javascript
const obj = {
  number: 0,
  increase: function() {
    this.number++;
  }
}

console.log(obj.number); // => 0
obj.increase();
console.log(obj.number); // => 1
```
Trong ví dụ trên, *increase* là một function. Vì vậy, giá trị của **this** được xác định dựa trên cách mà function này được gọi. Mà *increase* được gọi bằng cách nào?<br>
Chính là obj.increase(). Suy ra, this ở đây sẽ ứng với obj. Do đó, kết quả thu được là đúng như mong đợi.
# Một số cách sử dụng đúng giá trị của this bên trong hàm callback
## Lưu lại reference của this
Ý tưởng của phương pháp này là mình sẽ dùng một biến bất kỳ để lưu lại giá trị của **this**. Khi đó, mình chả cần quan tâm **this** bị bind với thằng nào.<br>
Khi đó, đoạn code trên đầu bài viết sẽ thay đổi thành:
```javascript
class Helper {
  constructor() {
    this.number = 0;
  }

  increase() {
    let self = this;

    setTimeout(function() {
      self.number++;
      console.log(self.number); // => 1
    }, 100);
  }
}

const helper = new Helper();
helper.increase();
```
Lúc này, giá trị của *self* sẽ ứng với *this* - tương ứng với Helper.
## Sử dụng phương thức bind
Phương thức *bind* sẽ tạo ra một function mới mà khi function này được gọi, giá trị của **this** sẽ tương ứng với giá trị truyền vào.

Áp dụng *bind* vào đoạn code trên, mình sẽ thu được:
```javascript
class Helper {
  constructor() {
    this.number = 0;
  }

  increase() {
    setTimeout((function() {
      this.number++;
      console.log(this.number); // => 1
    }).bind(this), 100);
  }
}

const helper = new Helper();
helper.increase();
```
Kết quả thu được vẫn chính xác như mong đợi.
## Sử dụng arrow function
*Arrow function* khác với function thông thường là nó không có **this**. Vì vậy, từ khoá **this** bên trong *arrow function* sẽ được đối xử như các biến bình thường khác.

Bây giờ, đoạn code trên sẽ thay đổi thành:
```javascript
class Helper {
  constructor() {
    this.number = 0;
  }

  increase() {
    setTimeout(() => {
      this.number++;
      console.log(this.number); // => 1
    }, 100);
  }
}

const helper = new Helper();
helper.increase();
```
## Trực tiếp set giá trị của this
Một số phương thức cho phép trực tiếp set giá trị của biến **this**. Ví dụ các phương thức ứng với Array như: *map(), forEach(), filter(),...*<br>
Ví dụ:
```javascript
class Helper {
  constructor() {
    this.number = [0, 1];
    this.amount = 1000;
  }

  increase() {
    this.number.forEach(function(item) {
      item += this.amount; // Uncaught TypeError: Cannot read property 'amount' of undefined
      console.log(item);
    });
  }
}

const helper = new Helper();
helper.increase();
```
Bên trong *function*, **this** có giá trị là **undefined**. Vì vậy, kết quả thu được là lỗi **Uncaught TypeError: Cannot read property 'amount' of undefined.**<br>
Bây giờ, mình sẽ truyền giá trị của **this** vào phương thức **forEach()** bên trên để xem kết quả thế nào?
```javascript
class Helper {
  constructor() {
    this.number = [0, 1];
    this.amount = 1000;
  }

  increase() {
    this.number.forEach(function(item) {
      item += this.amount;
      console.log(item);
    }, this);
  }
}

const helper = new Helper();
helper.increase();

// 1000
// 1001
```
Awesome! Kết quả thu được là hoàn toàn chính xác.
# Lời kết
Trên đây là một số cách để sử dụng đúng giá trị của **this** bên trong hàm callback mà mình đã tham khảo dựa trên một câu hỏi trên Stackoverflow, bạn có thể tham khảo tại đây [How to access the correct 'this' inside a callback?](https://stackoverflow.com/questions/20279484/how-to-access-the-correct-this-inside-a-callback)<br>
Xin chào và hẹn gặp lại !
`.` = bất kỳ ký tự nào ngoại trừ xuống dòng<br>
`\d` = khớp với một ký tự số, tương đương [0-9]<br>
`\D` = khớp với một ký tự không phải số<br>
`\s` = khớp với một ký tứ khoảng trắng<br>
`\S` = khớp với một ký tự không phải khoảng trắng<br>
`\n` = khớp với ký tự xuống dòng<br>
![](https://images.viblo.asia/0a6f8a51-2771-44fa-9183-7ec4bde2a63a.png)<br>
## **6/10**<br>
Chỉ khớp với một số lượng nhất định của các ký tự hoặc nhóm được khớp:<br>
`*` = 0 lần hoặc nhiều lần<br>
`+` = 1 hoặc nhiều lần<br>
`?` = 0 hoặc 1<br> 
`{3}` = chính xác 3 lần <br> 
`{2,4}` = 2,3,4 lần<br>
`{2,}` = 2 hoặc nhiều hơn:<br>
![](https://images.viblo.asia/1a3fda23-dc25-495c-ba95-7bae7a9d2d23.png)<br>
## **7/10**<br>
Sử dụng cặp ngoặc `()` để khớp 1 nhóm. Nếu không sử dụng cờ  `g`,  `match` sẽ trả về toàn bộ nhóm khớp với mẫu đã cho <br> Có thể sử dụng `|` trong `()` với ý nghĩa `|` = hoặc<br>![](https://images.viblo.asia/d6ec18a8-d590-4139-8118-5b926c13daca.png)<br>
## **8/10**<br>
Để tìm kiếm ký tự đặc biệt, sử dụng `\`<br>
Các ký tự đặc biệt trong JS regex: `^ $ \ . * + ? ( ) [ ] { } | ![]`
![](https://images.viblo.asia/02c5ac44-d5e8-462b-8b77-f7f89b5902ed.png)<br>
## **9/10**<br>
Để khớp bất kỳ thứ gì nhưng ngoại trừ một ký tự nào đó, sử dụng `^` bên trong dấu ngoặc vuông `[]`<br>
Điều này dẫn đến `^` có 2 nghĩa, có thể gây nhầm lẫn. Nó có nghĩa là "bắt đầu chuỗi" khi nó ở phía trước biểu thức chính quy và "không phải ký tự này" khi được sử dụng bên trong dấu ngoặc vuông<br>
![](https://images.viblo.asia/fb167242-c3f8-4eee-942e-39ae87edba2b.png)<br>
## **10/10**<br>
Regex có thể tìm và khớp tất cả mọi thứ, từ url đến tên tệp.<br>
Tuy nhiên! hãy cẩn thận với tác vụ phức tạp, chẳng hạn như phân tích địa chỉ email hoặc HTML (không phải là ngôn ngữ thông thường và do đó không thể được phân tích cú pháp đầy đủ bằng một biểu thức thông thường)<br><br>
Tất nhiên, còn nhiều hơn nữa để regex như lazy, greedy, lookahead, nhưng hầu hết những gì các lập trình viên web muốn làm với các biểu thức thông thường chỉ cần sử dụng các khối xây dựng cơ sở này.<br>
Bài viết được dịch và tham khảo tại [đây](https://dev.to/chrisachard/intro-to-regex-for-web-developers-2fj4?fbclid=IwAR03y8jIenOjRtE_rcJMvoqfvvD2gdQc6YKyj_pEdrK3h6RnbdLEF0cmji0)

