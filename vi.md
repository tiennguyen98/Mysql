
### 4.2.6 Sử dụng tệp tùy chọn

Hầu hết các chương trình MySQL có thể đọc các tùy chọn khởi động từ các tệp tùy chọn (đôi khi được gọi là tệp cấu hình). Các tệp tùy chọn cung cấp một cách thuận tiện để chỉ định các tùy chọn thường được sử dụng để chúng không được nhập vào dòng lệnh mỗi khi bạn chạy một chương trình.

Để xác định xem một chương trình có đọc các tệp tùy chọn hay không, hãy gọi nó bằng tùy chọn `\--help`. (dành cho [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].) Nếu chương trình đọc các tệp tùy chọn, thông báo trợ giúp cho biết tệp nào sẽ tìm và nhóm tùy chọn nào sẽ xác nhận.

Chú thích 

Một chương trình MySQL bắt đầu bằng tùy chọn `\--no-defaults` không đọc các tệp tùy chọn nào khác ngoài `.mylogin.cnf`. 

Nhiều tệp tùy chọn là các tệp văn bản thuần túy, được tạo bằng bất kỳ trình soạn thảo văn bản nào. Ngoại lệ là `.mylogin.cnf` tệp chứa tùy chọn đường dẫn đăng nhập. Đây là tệp được mã hóa do tiện ích [**mysql_config_editor**][4]. See [Section 4.6.6, "**mysql_config_editor** — MySQL Configuration Utility"][4]."Đường dẫn đăng nhập" là nhóm tùy chọn chỉ cho phép một số tùy chọn nhất định: `host`, `user`, `password`, `port` và `socket`.Chương trình phía clientchỉ định đường dẫn đăng nhập để đọc từ `.mylogin.cnf` sử dụng tùy chọn [`\--login-path`][5]. 

Để chỉ định tên tệp đường dẫn đăng nhập thay thế, hãy đặt biến môi trường `MYSQL_TEST_LOGIN_FILE .Biến này được sử dụng bởi tiện ích thử nghiệm  **mysql-test-run.pl** , nhưng cũng được xác nhận bởi [**mysql_config_editor**][4] và bởi MySQL clients như là [**mysql**][6], [**mysqladmin**][7], và vân vân.

MySQL tìm kiếm các tệp tùy chọn theo thứ tự được mô tả trong phần thảo luận sau và đọc bất kỳ tệp nào tồn tại. Nếu một tệp tùy chọn bạn muốn sử dụng không tồn tại, hãy tạo nó bằng cách sử dụng phương thức thích hợp, như vừa thảo luận. 

Trên Windows, các chương trình MySQL đọc các tùy chọn khởi động từ các tệp được hiển thị trong bảng sau, theo thứ tự được chỉ định (các tệp được liệt kê đầu tiên được đọc trước tiên, các tệp được đọc sau được ưu tiên).

**Bảng 4.1 Các tệp tùy chọn Đọc trên các hệ thống Windows**

| File Name                                                                                  | Purpose                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Global options                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Global options                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | Global options                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Global options                                                |  
| `defaults-extra-file`                                                                      | The file specified with [`\--defaults-extra-file`][8], if any |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Login path options (clients only)                             |  

Trong bảng trước, `%PROGRAMDATA%` đại diện cho thư mục hệ thống tệp chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ lưu trữ. Đường dẫn này mặc định là `C:ProgramData`trên Microsoft Windows Vista trở lên và `C:Documents and SettingsAll UsersApplication Data` trên các phiên bản cũ hơn của Microsoft Windows. 

`%WINDIR%` đại diện cho vị trí của thư mục Windows của bạn. Điều này thường `C:WINDOWS`.Sử dụng lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `WINDIR` : 
    
    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thư mục dữ liệu ứng dụng Windows. Sử dụng lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `APPDATA`: 
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục cài đặt cơ sở MySQL. Khi MySQL 5.7 đã được cài đặt bằng cách sử dụng MySQL Installer, điều này thường `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` ở _`PROGRAMDIR`_ đại diện cho thư mục chương trình (thông thường `Program Files` trên các phiên bản tiếng Anh của Windows), Xem [Section 2.3.3, "MySQL Installer for Windows"][9]. 

Trên các hệ thống giống Unix và Unix, các chương trình MySQL đọc các tùy chọn khởi động từ các tệp được hiển thị trong bảng sau, theo thứ tự được chỉ định (các tệp được liệt kê đầu tiên được đọc trước tiên, các tệp được đọc sau được ưu tiên).

Ghi chú 

Trên các nền tảng Unix, MySQL bỏ qua các tệp cấu hình world-writable. Điều này có ý như một thước đo về độ bảo mật. 

**Bảng 4.2 Các tệp tùy chọn được đọc trên các hệ thống giống Unix và Unix **

| File Name               | Purpose                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Global options                                                |  
| `/etc/mysql/my.cnf`     | Global options                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Global options                                                |  
| `$MYSQL_HOME/my.cnf`    | Server-specific options (server only)                         |  
| `defaults-extra-file`   | The file specified with [`\--defaults-extra-file`][8], if any |  
| `~/.my.cnf`             | User-specific options                                         |  
| `~/.mylogin.cnf`        | User-specific login path options (clients only)               |  

Trong bảng trước,, `~` đại diện cho thư mục chính của người dùng hiện tại (giá trị của `$HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được chỉ định bằng [`SYSCONFDIR`][10] tùy chọn để **CMake** khi MySQL được xây dựng. Theo mặc định, đây là `etc` thư mục nằm trong thư mục cài đặt đã biên dịch.

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục trong đó máy chủ cụ thể nằm trong tệp `my.cnf`. Nếu `MYSQL_HOME` không được đặt và bạn khởi động máy chủ bằng cách sử dụng chương trình [**mysqld_safe**][11], [**mysqld_safe**][11] đặt nó thành _`BASEDIR`_, thư mục cài đặt cơ sở MySQL.

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo từng nền tảng hoặc phương pháp cài đặt. Giá trị là vị trí thư mục dữ liệu được xây dựng khi MySQL được biên dịch, chứ không phải vị trí được chỉ định bằng [`\--datadir`][12] tùy chọn khi mà [**mysqld**][1] khởi động.Sử dụng [`\--datadir`][12] tại thời gian chạy không ảnh hưởng đến nơi máy chủ tìm kiếm các tệp tùy chọn mà nó đọc trước khi xử lý bất kỳ tùy chọn nào.

Nếu nhiều phiên bản của một tùy chọn đã cho được tìm thấy, cá thể cuối cùng được ưu tiên, với một ngoại lệ: dành cho **[mysqld**][1], cá thể đầu tiên của tùy chọn `[\--user`][13] được sử dụng như một biện pháp đề phòng bảo mật, để ngăn người dùng được chỉ định trong tệp tùy chọn bị ghi đè trên dòng lệnh

Mô tả sau đây về cú pháp tệp tùy chọn áp dụng cho các tệp bạn chỉnh sửa theo cách thủ công. Điều này không bao gồm `.mylogin.cnf`, được tạo bằng  **[mysql_config_editor**][4] và được mã hóa.

Bất kỳ tùy chọn dài có thể được đưa ra trên dòng lệnh khi chạy một chương trình MySQL có thể được đưa ra trong một tập tin tùy chọn là tốt. Để có được danh sách các tùy chọn có sẵn cho một chương trình, hãy chạy nó với tùy chọn `\--help`. (Dành cho **[mysqld**][1], Sử dụng `[\--verbose`][2] và [\--help`][3].) 

Cú pháp để chỉ định các tùy chọn trong một tệp tùy chọn tương tự như cú pháp dòng lệnh (Xem [Section 4.2.4, "Using Options on the Command Line"][14]).Tuy nhiên, trong một tệp tùy chọn, bạn bỏ qua hai dấu gạch ngang hàng đầu từ tên tùy chọn và bạn chỉ định một tùy chọn trên mỗi dòng. Ví dụ, `\--quick` và `\--host=localhost`trên dòng lệnh nên được chỉ định là `quick` và `host=localhost`trên các dòng riêng biệt trong một tệp tùy chọn. Để chỉ định một tùy chọn của biểu mẫu `\--loose-_`opt_name`_` trong một tệp tùy chọn, viết nó thành `loose-_`opt_name`_`. 

Các dòng trống trong các tệp tùy chọn bị bỏ qua. Các dòng không trống có thể có bất kỳ dạng nào sau đây:

* `#_`comment`_`, `;_`comment`_`

Dòng chú thích bắt đầu bằng `#` hoặc  `;`. Một chú thích  `#` cũng có thể bắt đầu ở giữa một dòng. 

* `[_`group`_]`

_`group`_ là tên của chương trình hoặc nhóm mà bạn muốn đặt tùy chọn. Sau một dòng nhóm, mọi dòng thiết lập tùy chọn áp dụng cho nhóm được đặt tên cho đến khi kết thúc tệp tùy chọn hoặc một nhóm nhóm khác được cung cấp. Tên nhóm tùy chọn không phân biệt chữ hoa chữ thường.

* `_`opt_name`_`

Tương đương với `\--_`opt_name`_` trên dòng lệnh. 

* `_`opt_name`_=_`value`_`

tương đương với `\--_`opt_name`_=_`value`_` trên dòng lệnh. Trong tệp tùy chọn, bạn có thể có khoảng trống xung quanh ký tự `=` , cái gì đó không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu ngoặc kép, hữu ích nếu giá trị chứa một ký tự `#.. 

khoảng cách đầu và cuối được tự động xóa khỏi tên và giá trị tùy chọn.

Bạn có thể sử dụng trình tự thoát `b`, `t`, `n`, `r`, `\`, và `s` trong các giá trị tùy chọn để đại diện cho backspace, tab, dòng mới, dấu xuống dòng, dấu gạch chéo ngược và ký tự khoảng trắng. Trong các tệp tùy chọn, các quy tắc thoát này sẽ áp dụng:

* Dấu gạch chéo ngược theo sau là một ký tự chuỗi thoát hợp lệ được chuyển đổi thành ký tự được trình bày theo trình tự. Ví dụ, s được chuyển đổi thành một khoảng trắng.
* Dấu gạch chéo ngược không được theo sau bởi một ký tự chuỗi thoát hợp lệ vẫn không thay đổi. Ví dụ, `S` được giữ nguyên. 

Các quy tắc trước có nghĩa là dấu gạch chéo ngược có thể được cung cấp dưới dạng `\`,hoặc như ``nếu nó không được theo sau bởi một ký tự chuỗi thoát hợp lệ.

Các quy tắc cho các chuỗi thoát trong các tệp tùy chọn khác nhau một chút so với các quy tắc cho các chuỗi thoát trong các chuỗi ký tự trong các câu lệnh SQL. Trong bối cảnh sau, nếu "_`x`_" không phải là ký tự chuỗi thoát hợp lệ, `_`x`_` trở thành "_`x`_" thay vì `_`x`_`. Xem [Section 9.1.1, "String Literals"][15]. 

Các quy tắc thoát cho các giá trị tệp tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, sử dụng ``làm dấu tách tên đường dẫn. Dấu phân tách trong tên đường dẫn Windows phải được viết là `\`nếu nó được theo sau bởi một ký tự chuỗi thoát. Nó có thể được viết như `\` hoặc ``nếu không phải vậy. Cách khác, `/`có thể được sử dụng trong tên đường dẫn Windows và sẽ được coi là ``. Giả sử bạn muốn chỉ định thư mục cơ sở của `C:Program FilesMySQLMySQL Server 5.7`trong một tệp tùy chọn. Điều này có thể được thực hiện theo nhiều cách. Vài ví dụ:
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên nhóm tùy chọn giống với tên chương trình, các tùy chọn trong nhóm sẽ áp dụng riêng cho chương trình đó. Ví dụ: the `[mysqld]` và `[mysql]` nhóm áp dụng cho **[mysqld**][1] mày chủ và **[mysql**][6] chương trình máy khách, tương ứng. 

Nhóm tùy chọn `[client]` được đọc bởi tất cả các chương trình máy khách được cung cấp trong bản phân phối MySQL (nhưng không phải bởi **[mysqld**][1]). Để hiểu cách các chương trình khách hàng của bên thứ ba sử dụng API C có thể sử dụng các tệp tùy chọn, hãy xem tài liệu C API tại [Section 27.8.7.50, "mysql_options()"][16]. 

Nhóm `[client]`cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các máy khách. Ví dụ, `[client]` là nhóm thích hợp để sử dụng để chỉ định mật khẩu để kết nối với máy chủ. (Nhưng hãy đảm bảo rằng bạn chỉ có thể truy cập vào tệp tùy chọn để người khác không thể tìm thấy mật khẩu của bạn.) Hãy chắc chắn không đặt tùy chọn trong` [client]`nhóm trừ khi nó được mọi chương trình khách hàng sử dụng. Các chương trình không hiểu tùy chọn thoát sau khi hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng.

Liệt kê các nhóm tùy chọn chung hơn trước tiên và các nhóm cụ thể hơn sau này. Ví dụ, một nhóm `[client]` gtổng quát hơn vì nó được đọc bởi tất cả các chương trình khách, trong khi `[mysqldump]` nhóm chỉ đọc bởi **[mysqldump**][17].Các tùy chọn ghi đè tùy chọn được chỉ định sau đó được chỉ định trước đó, vì vậy hãy đặt các nhóm tùy chọn theo thứ tự  `[client]`, `[mysqldump]` cho phép **[mysqldump**][17]-tùy chọn cụ thể để ghi đè các tùy chọn `[client]`. 

Đây là một tệp tùy chọn toàn cục điển hình:
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là một tệp tùy chọn người dùng điển hình:
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo các nhóm tùy chọn chỉ đọc bởi máy chủ **[mysqld**][1] từ loạt phát hành MySQL cụ thể, sử dụng các nhóm có tên `[mysqld-5.6]`, `[mysqld-5.7]`, và vân vân. Nhóm sau chỉ ra rằng cài đặt `[sql_mode`][18] chỉ nên được sử dụng bởi các máy chủ MySQL có số phiên bản 5.7.x:
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng `!include` chỉ thị trong các tệp tùy chọn để bao gồm các tệp tùy chọn khác và `!includedirđể tìm kiếm các thư mục cụ thể cho các tệp tùy chọn. Ví dụ: để bao gồm tệp `/home/mydir/myopt.cnf`sử dụng chỉ thị sau:
    
    
    !include /home/mydir/myopt.cnf

Để tìm kiếm thư mục `/home/mydir`và đọc các tệp tùy chọn được tìm thấy ở đó, sử dụng chỉ thị này:
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự các tập tin tùy chọn trong thư mục sẽ được đọc.

chú thích

Bất kỳ tập tin được tìm thấy và bao gồm bằng cách sử dụng chỉ thụ `!includedir`trên hệ điều hành Unix phải có tên tệp `.cnf`.Trên Windows, chỉ thị này sẽ kiểm tra các tệp bằng đuôi `.ini` hoặc `.cnf `

Viết nội dung của tệp tùy chọn được bao gồm giống như bất kỳ tệp tùy chọn nào khác. Tức là, nó phải chứa các nhóm tùy chọn, mỗi nhóm đứng trước  dòng `[_`group`_]` cho biết chương trình áp dụng các tùy chọn nào.

Trong khi tệp được bao gồm đang được xử lý, chỉ những tùy chọn trong nhóm mà chương trình hiện tại đang tìm kiếm được sử dụng. Các nhóm khác bị bỏ qua. Giả sử rằng một tệp  `my.cnf` chứa dòng sau:
    
    
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/home/mydir/myopt.cnf` trông như sau:
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi **[mysqld**][1], chỉ sử dụng nhóm `[mysqld]` trong `/home/mydir/myopt.cnf`. Nếu tệp được xử lý bởi **[mysqladmin**][7], Chỉ sử dụng nhóm `[mysqladmin]`.Nếu tệp được xử lý bởi bất kỳ chương trình nào khác, không có tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng. 

Chỉ thị `!includeir` được xử lý tương tự ngoại trừ tất cả các tệp tùy chọn trong thư mục có tên được đọc.

Nếu một tệp tùy chọn chứa chỉ thị `!include` hoặc `!includedir` các tệp được đặt tên bởi các chỉ thị đó được xử lý bất cứ khi nào tệp tùy chọn được xử lý, bất kể chúng xuất hiện ở đâu trong tệp.

[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

  
