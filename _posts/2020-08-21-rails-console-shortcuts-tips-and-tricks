---
layout: post
title: Rails Console Shortcuts, Tips and Tricks
subtitle: Rails Console
cover-img: /assets/img/path.jpg
tags: [Rails, Tips and Tricks]
---
Là một nhà phát triển Rails, bạn sẽ dành nhiều thời gian trong bảng điều khiển Rails. Nó giống như một cánh cửa sau ứng dụng của bạn. Sử dụng rails console cho phép bạn tương tác với các phần của ứng dụng trực tiếp từ giao diện dòng lệnh mà không cần thông qua trình duyệt. <br>
Để giúp bạn tận dụng tối đa thời gian của mình trong bảng điều khiển, đây là bộ sưu tập các phím tắt, mẹo và thủ thuật.
## Xóa màn hình Console
Bạn có thể sử dụng Ctrl + L để xóa màn hình console, tính năng này hoạt động trên cả Mac và Linux. Nếu bạn đang sử dụng Windows, bạn có thể thử ('cls').
## Tải lại Console
Thông thường, bạn thực hiện thay đổi mã code trong trình soạn thảo yêu thích của mình, sau đó nhấn Tải lại trong trình duyệt của bạn để xem nhanh hiệu quả của những thay đổi đó. Điều này hoạt động vì mã lệnh ứng dụng của bạn được tự động tải lại theo mọi yêu cầu khi bạn đang chạy trong môi trường develop.<br>
Giao diện Console cũng chạy trong môi trường develop theo mặc định, nhưng nó không tự động tải lại mã. Khi một tệp đã được tải xong, tệp đó sẽ được lưu vào bộ nhớ đệm trong suốt thời gian của phiên Console. Bạn có thể làm mới mã bằng cách thoát khỏi Console và bắt đầu một phiên mới, nhưng điều đó thật tẻ nhạt khi thực hiện mỗi khi bạn thay đổi mã code.<br>
Thay vào đó, hãy sử dụng `reload!` lệnh để nhận phiên bản mã mới nhất của bạn:
```
>> reload!
Reloading...
=> true
```
Tuy nhiên có một lưu ý rằng, mọi đối tượng tồn tại ở phiên làm việc trước đó vẫn giữ nguyên trạng thái của chúng. Ví dụ, nếu bạn khởi tạo một đối tượng `movie` trong Rails console, sau đó trong trình soạn thảo code bạn bổ sung 1 phương thức mới vào model Movie thì đối tượng movie ấy sẽ không nhận phương thức đó ngay cả khi bạn gõ lệnh `reload!`. Cách giải quyết là bạn cần khởi tạo lại đối tượng.
## Tìm kiếm câu lệnh đã thực thi
Khi bạn đã chạy một dòng mã trong Console, bạn thường muốn chạy lại cùng một mã sau đó hoặc chỉnh sửa nó để làm điều gì đó hơi khác một chút. Bạn có một số tùy chọn để gọi lại các lệnh.<br>
Bảng điều khiển duy trì lịch sử các lệnh mà bạn có thể điều hướng bằng các phím mũi tên lên và xuống. Để gọi lại dòng trước đó bạn đã nhập, bạn có thể sử dụng mũi tên lên và xuống.<br><br>
Nếu bạn đang sử dụng máy Unix và sử dụng bash shell mặc định, thì bạn có thể tìm kiếm lệnh trước đó bằng cách nhấn Ctrl + R rồi nhập bất kỳ phần nào của lệnh đó. Nó sẽ tìm kiếm trong lịch sử và tự động hoàn thành lệnh đầu tiên khớp với những gì bạn đã nhập. Bạn càng nhập nhiều, nó càng thu hẹp tìm kiếm. Khi bạn đã tìm thấy lệnh mình đang tìm kiếm, hãy nhấn Enter để chạy lệnh đó. Hoặc nhấn phím mũi tên bất kỳ để bắt đầu chỉnh sửa lệnh. Hoặc tiếp tục nhấn Ctrl + R để quay lại liên tiếp qua các lệnh khớp.
## Tự động hoàn thành câu lệnh
Giả sử bạn có một model Movie mà trước đây bạn đã sử dụng trong Console và bây giờ bạn muốn sử dụng nó để chạy một truy vấn. Bắt đầu bằng cách nhập Mov và sau đó nhấn Tab:
```
>> Mov<Tab>
```
Và kết quả như những gì bạn muốn:
```
>> Movie
```
Bây giờ bạn muốn sử dụng phương thức where để truy vấn. Bắt đầu bằng cách nhập `.w` và sau đó nhấn Tab:
```
>> Movie.w<Tab><Tab>

Movie.where                 Movie.with_scope
Movie.with_exclusive_scope  Movie.with_warnings
Movie.with_options
```
Bạn muốn phương thức where, vì vậy hãy nhập ký tự tiếp theo trong tên phương thức (h) và nhấn lại Tab:
```bashrc
>> Movie.wh<Tab>
```
Và kết quả:
```
>> Movie.where
```
Tính năng tự động hoàn thành giúp tiết kiệm rất nhiều thời gian nhập và ghi nhớ.
## Lấy ra giá trị của biểu thức vừa thực hiện
Đây thực sự là một thủ thuật rất hữu ích! Tình huống quen thuộc là, bạn gõ một lệnh vào Rails console, ví dụ như:
```sql
>> Movie.where("total_gross >= ?", 100000000).order(:title)
```
Sau đó, bạn ngay lập tức nhấn Enter và truy vấn chạy. Sau đó, bạn nhớ rằng bạn thực sự muốn làm điều gì đó với kết quả, nhưng không thể gán kết quả cho một biến. Tại thời điểm này, bạn có thể nhấn phím mũi tên lên để nhớ lại dòng mã đó rồi chỉnh sửa, nhưng có một cách dễ dàng hơn.<br><br>
Giá trị trả về của biểu thức cuối cùng được tự động lưu trữ trong biến dấu gạch dưới () đặc biệt. Ví dụ: để gán kết quả truy vấn cho một biến phim, bạn có thể sử dụng:
```
>> movies = _
```
Khi đó chúng ta có thể thao tác với kết quả như bình thường, chẳng hạn bạn thực hiện lệnh:
```
>> movies.size
=> 5
```
Ngoài ra, bạn có thể gọi phương thức kích thước trực tiếp trên biến _:
```
>> _.size
=> 5
```
## Làm việc với các helper
Nếu bạn muốn thử nghiệm một helper trước khi nhúng nó vào view thì sao? Cách làm là sử dụng biến helper, chúng ta có thể xem trực tiếp kết qủa trong Rails console, ví dụ:
```
>> helper.pluralize(3, 'mouse')
=> "3 mice"

>> helper.number_to_currency(1.50)
=> "$1.50"

>> helper.truncate("Iron Man", length: 7)
=> "Iron..."
```
Trên đây là những method mà Rails hỗ trợ sẵn. Ngoài ra bạn có thể gọi những method mà bạn tự định nghĩa, ví dụ với 2 method tự định nghĩa là **title_tag** và **poster_image_for**:
```
>> helper.title_tag(Movie.first)
=> "<title>Flix - Iron Man</title>"

>> helper.poster_image_for(Movie.first)
=> "<img alt=\"Ironman\" src=\"/assets/ironman.png\" />"
```
## Làm việc với route helper
Tương tự, bạn dễ dàng thể thử nghiệm route helper trong Console. Để làm điều đó, hãy gọi các phương thức trên đối tượng **app**:
```
>> app.movies_path
=> "/movies"

>> app.movie_path(Movie.first)
=> "/movies/1"
```
Sử dụng **helper** và **app** cùng nhau, bạn có thể tạo liên kết trong Console:
```
>> helper.link_to("Movies", app.movies_path)
=> "<a href=\"/movies\">Movies</a>"

>> movie = Movie.first
>> helper.link_to(movie.title, app.movie_path(movie))
=> "<a href=\"/movies/1\">Iron Man</a>"
```
## Thiết lập đối tượng mặc định
Tiếp tục là một thủ thuật thú vị nhưng có lẽ bạn sẽ không thường xuyên sử dụng nó. Giả sử bạn đang làm việc với một đối tượng **movie** và gọi đến các phương thức của nó:
```
>> movie = Movie.first

>> movie.title
=> "Iron Man"
>> movie.rating
=> "PG-13"
>> movie.director
=> "Jon Favreau"
```
Nếu bạn dự định sẽ thực hiện nhiều thao tác khác xoay quanh đối tượng này, quả thật chúng ta nên tìm cách nào đó để tránh việc gõ movie mỗi lần gọi đến nó. Để thực hiện điều này, bạn có thể đặt movie làm đối tượng mặc định với lệnh:
```
>> irb movie
```
Bây giờ mỗi khi bạn gọi đến một phương thức, nó sẽ được đặt trong ngữ cảnh của đối tượng **movie**:
```
>> title
=> "Iron Man"
>> rating
=> "PG-13"
>> director
=> "Jon Favreau"
```
Để thoát khỏi phiên làm việc với đối tượng mặc định này và quay lại ngữ cảnh ban đầu, bạn có thể gõ lệnh **exit** hoặc sử dụng tổ hợp phím **Ctrl + D**:
## Chuyển đổi môi trường
Theo mặc định, Console kích hoạt trong môi trường develop như được chỉ ra bởi thông báo hữu ích:
```
$ rails console
Loading development environment
>>
```
Bạn cũng có thể khởi động Console trong một môi trường cụ thể bằng cách nhấn vào tùy chọn -e và chỉ định tên môi trường. Ví dụ: nếu bạn muốn sử dụng Console trong môi trường test, bạn có thể chạy:
```
$ rails console -e test
Loading test environment
>>
```
## Nguồn tham khảo
https://pragmaticstudio.com/tutorials/rails-console-shortcuts-tips-tricks
