[Source](https://www.sitepoint.com/10-tips-git-next-level/ "Permalink to 10 Tips to Push Your Git Skills to the Next Level — SitePoint")

# 10 mẹo để nâng cao kỹ năng sử dụng GIT của bạn nên một level mới.

Gần đây, chúng tôi đã xuất bản một vài hướng dẫn để giúp bạn làm quen với nền tảng Git và sử dụng Git trong môi trường làm việc nhóm. Các lệnh mà chúng ta đã thảo luận đủ để giúp một nhà phát triển tồn tại trong thế giới Git. Trong bài đăng này, chúng tôi sẽ cố gắng khám phá cách quản lý thời gian một cách hiệu quả và tận dụng các tính năng mà Git cung cấp.

_Lưu ý: Một số lệnh trong bài viết này bao gồm cả một phần của lệnh trong dấu ngoặc vuông (ví dụ: git add -p [file_name]). Trong những ví dụ này, bạn sẽ chèn số, định danh cần thiết mà không có dấu ngoặc vuông_

## 1\. Tự động hoàn thành câu lệnh Git

Nếu bạn chạy lệnh Git thông qua dòng lệnh, đó là một công việc nhàm chán khi phải gõ từng câu lệnh bằng tay. Để giúp đỡ việc này, bạn có thể kích hoạt tự động hoàn thành lệnh Git chỉ trong vòng vài phút.

Để có được kịch bản, hãy chạy lệnh sau trong một hệ thống Unix

    
    cd ~
    curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash

Tiếp theo, thêm các dòng sau vào tập tin `~/.bash_profile`:
    
    
    if [ -f ~/.git-completion.bash ]; then
        . ~/.git-completion.bash
    fi

Mặc dù tôi đã đề cập đến điều này trước đó, tôi không thể nhấn mạnh đủ: Nếu bạn muốn sử dụng các tính năng của Git đầy đủ, bạn chắc chắn nên chuyển sang giao diện dòng lệnh!

## 2\. Bỏ qua các tệp trong Git

Bạn có mệt mỏi với các tập tin được biên dịch (như `.pyc`) xuất hiện trong kho Git của bạn? Hay bạn không muốn thêm chúng vào Git? Nhìn xa hơn, có một cách để bạn có thể ra lệnh cho Git bỏ qua một số tập tin và thư mục. Đơn giản chỉ cần tạo một tệp với tên `.gitignore` và liệt kê các tệp tin và thư mục mà bạn không muốn Git theo dõi. Bạn có thể thực hiện ngoại lệ bằng cách sử dụng dấu chấm than (!).


    
    *.pyc
    *.exe
    my_db_config/
    
    !main.pyc

## 3\. Ai đã làm sai code của tôi?

Đó là bản năng tự nhiên của con người để đổ lỗi cho người khác khi có điều gì đó không ổn. Nếu máy chủ của bạn bị hỏng, rất dễ để tìm ra thủ phạm - chỉ cần thực hiện lệnh `git blame`. Lệnh này cho bạn thấy tác giả của mỗi dòng trong một tệp tin, commit trong lần thay đổi cuối cùng của dòng đó và thời gian của commit.

    
    git blame [file_name]

![git blame demonstration][3]

Và trong ảnh chụp màn hình bên dưới, bạn có thể thấy lệnh này sẽ trông như thế nào trên một repository lớn hơn:

![git blame on the ATutor repository][4]

## 4\. Xem qua lịch sử của Repository

Chúng tôi đã xem xét việc sử dụng `git log` trong một hướng dẫn trước đó, tuy nhiên, có ba lựa chọn mà bạn cần biết.


* `\--oneline` – Nén thông tin hiển thị bên cạnh mỗi commit với một commit tối giản và thông báo commit, tất cả được hiển thị trong một dòng đơn.
* `\--graph` – Tùy chọn này rút ra một biểu đồ dựa trên văn bản của lịch sử ở phía bên tay trái của đầu ra. Không sử dụng lệnh này nếu bạn đang xem lịch sử cho nhánh đơn lẻ
* `\--all` – Hiển thị lịch sử của tất cả các nhánh.

Dưới đây là những gì kết hợp các tùy chọn lại như sau:

![Use of git log with all, graph and oneline][5]

## 5\. Không bao giờ mất dấu một commit
Hãy nói rằng bạn đã commit một điều gì đó mà bạn không muốn và dẫn tới cần thực hiện hard reset để trở lại trạng thái trước của bạn. Sau đó, bạn nhận ra rằng bạn đã mất một số thông tin khác trong quá trình và muốn khôi phục lại hoặc ít nhất là xem nó. Lệnh `git reflog` sẽ giúp bạn.

Một lệnh `git log` cơ bản cho bạn thấy commit mới nhất, commit cha của nó, commit cấp 3 của nó, vân vân. Tuy nhiên, `git reflog` là một danh sách các commit head trỏ đến. Hãy nhớ rằng nó chỉ trên hệ thống local của bạn; chứ không phải là một phần của repository và không bao gồm push hoặc merge.

Nếu tôi chạy `git log`, tôi nhận được các commit là một phần trong repository của tôi:


![Project history][6]

Tuy nhiên, lệnh `git reflog` hiển thị một commit (`b1b0ee9 - HEAD @ (4)`) mà đã bị mất khi tôi thực hiện hard reset:

![Git reflog][7]

## 6\. Phân loại các phần của một tệp đã thay đổi cho một commit

Nói chung, thực tế để thực hiện các commit dựa trên tính năng, nghĩa là mỗi commit phải đại diện cho một tính năng hoặc một lỗi được sửa. Hãy xem xét điều gì sẽ xảy ra nếu bạn đã sửa hai lỗi hoặc thêm nhiều tính năng mà không thực hiện commit thay đổi. Trong trường hợp như vậy, bạn có thể đặt những thay đổi trong một commit duy nhất. Nhưng có một cách tốt hơn: stage các tập tin riêng lẻ và commit chúng một cách riêng biệt.

Giả sử bạn đã thực hiện nhiều thay đổi cho một tệp và muốn chúng xuất hiện trong các commit riêng biệt. Trong trường hợp đó, chúng ta thêm các tập tin bằng tiền tố `-p` trogn lệnh add của chúng ta.

    
    
    git add -p [file_name]

Chúng ta hãy cùng nhau chứng minh điều đó. Tôi đã thêm ba dòng mới vào `file_name` và tôi chỉ muốn các dòng đầu tiên và thứ ba xuất hiện trong commit của tôi. Hãy xem những gì một lệnh `git diff` cho chúng ta thấy.


![Changes in repo][8]

Và chúng ta hãy xem những gì xảy ra khi chúng tôi thêm tiền tố `-p` trong lệnh `add`.


![Running add with -p][9]

Có vẻ như Git cho rằng tất cả những thay đổi đều là một phần của cùng một ý tưởng, từ đó nhóm nó thành một khối lớn. Bạn có các lựa chọn sau:

* Nhập y để stage một phần
* Nhập n để không stage phần đó
* Nhập e để tự chỉnh sửa phần lớn
* Nhập d để thoát hoặc đi tới tệp tiếp theo.
* Nhập s để chia tách phần lớn ra.


Trong trường hợp của chúng tôi, chúng tôi chắc chắn muốn chia nhỏ nó thành các phần nhỏ hơn để bổ sung có chọn lọc và bỏ qua phần còn lại.


![Adding all hunks][10]

Như bạn thấy, chúng tôi đã thêm vào dòng đầu tiên và thứ ba và bỏ qua thứ hai. Sau đó bạn có thể xem trạng thái của kho dữ liệu và thực hiện một commit

![Repository after selectively adding a file][11]

## 7\. Squash Nhiều Commit

Khi bạn submit code của mình để review và tạo ra một pull request (thường gặp trong các dự án mã nguồn mở), bạn có thể được yêu cầu thay đổi mã của mình trước khi nó được chấp nhận. Bạn thực hiện thay đổi, chỉ để được yêu cầu thay đổi lại nó trong lần xem xét tiếp theo. Trước khi bạn biết nó, bạn có một vài commit thêm. Bạn có thể ghi đè chúng bằng cách sử dụng lệnh `rebase`.

    
    git rebase -i HEAD~[number_of_commits]

Nếu bạn muốn ghi đè 2 commit cuối cùng, lệnh bạn chạy là như sau.
    
    
    git rebase -i HEAD~2

Khi chạy lệnh này, bạn sẽ được đưa đến một giao diện tương tác liệt kê các commit và hỏi bạn cho những commit được khi đè. Tốt nhất là bạn `pick` những commit mới nhất và `squash` những cái cũ.


![Git squash interactive][12]

Sau đó bạn được yêu cầu cung cấp thông báo commit cho commit mới. Quá trình này bản chất là viết lại lịch sử commit của bạn.

![Adding a commit message][13]

## 8\. Giấu những thay đổi

Giả sử bạn đang làm việc trên một lỗi nhất định hoặc một tính năng, và bạn đột nhiên được yêu cầu để demo công việc của bạn. Công việc hiện tại của bạn chưa đủ để commit, và bạn không thể đưa ra demo trong trường hợp này (mà không quay lại những thay đổi). Trong tình huống như vậy, `git stash` có thể giúp. Stash cơ bản lấy tất cả các thay đổi của bạn và lưu trữ chúng để sử dụng về sau. Để giấu các thay đổi của bạn, bạn chỉ cần chạy lệnh sau

    
    git stash

Để kiểm tra danh sách các stashes, bạn có thể chạy sau đây
    
    git stash list

![Stash list][14]

Nếu bạn muốn un-stash và khôi phục lại những thay đổi chưa được commit, bạn áp dụng các stash
    
    git stash apply

Trong ảnh chụp màn hình mới nhất, bạn có thể thấy mỗi stash có một định danh, một số duy nhất (mặc dù chúng ta chỉ có một stash trong trường hợp này). Trong trường hợp bạn muốn áp dụng chỉ stashes cụ thể, bạn thêm định danh cụ thể vào lệnh apply:

    
    git stash apply stash@{2}

![After un-stashing changes][15]

## 9\. Kiểm tra các commit đã mất

Mặc dù `reflog` là một cách để kiểm tra các lần commit bị mất, nhưng nó không khả thi trong các kho lưu trữ lớn. Đó là khi lệnh `fsck` (kiểm tra hệ thống tập tin) xuất hiện.

    
    git fsck --lost-found

![Git fsck results][16]

Ở đây bạn có thể thấy một commit bị mất. Bạn có thể kiểm tra các thay đổi trong commir bằng cách chạy lệnh `git show [commit_hash]` hoặc khôi phục nó bằng cách chạy lệnh `git merge [commit_hash]`.

Here you can see a lost commit. You can check the changes in the commit by running `git show [commit_hash]` or recover it by running `git merge [commit_hash]`.

`git fsck` có lợi thế hơn `reflog`. Hãy nói rằng bạn đã xóa một nhánh phía remote và sau đó clone kho dữ liệu. Với `fsck` bạn có thể tìm kiếm và phục hồi các chi nhánh từ xa đã xóa


## 10\. Cherry Pick

Tôi để giành lệnh Git thanh lịch nhất trong phần cuối cùng. Lệnh `cherry-pick` là lệnh Git yêu thích của tôi, bởi vì nó có nghĩa đen cũng như lợi ích của nó!

Theo cách đơn giản nhất, `cherry-pick` đang lấy một commit duy nhất từ ​​một nhánh khác và merge nó với nhánh hiện tại của bạn. Nếu bạn đang làm việc theo kiểu song song trên hai hoặc nhiều nhánh, bạn có thể nhận thấy một lỗi xảy ra ở tất cả các chi nhánh. Nếu bạn đã giải quyết nó, bạn có thể *cherry pick* commit vào các nhánh khác, mà không ảnh hưởng tới các tập tin hay commit khác.


Hãy xem xét một kịch bản mà chúng ta có thể áp dụng. Tôi có hai nhánh và tôi muốn `cherry-pick` commit `b20fd14: Cleaned junk` vào một cái khác.


![Before cherry pick][17]

Tôi chuyển sang nhánh mà tôi muốn để `cherry-pick` commit và chạy như sau:
    
    
    git cherry-pick [commit_hash]

![After cherry pick][18]

Mặc dù lần này chúng tôi đã có một `cherry-pick` rỡ ràng, bạn nên biết rằng lệnh này có thể dẫn đến xung đột, do đó hãy cẩn thận khi sử dụng.

## Tóm tắt
Với điều này, chúng ta sẽ đến phần cuối của danh sách các mẹo mà tôi nghĩ rằng có thể giúp bạn nâng cao kỹ năng Git của mình. Git là tốt nhất hiện có và nó có thể hoàn thành bất cứ điều gì bạn có thể tưởng tượng. Vì vậy, luôn luôn cố gắng để thử thách chính mình với Git. Rất có thể, bạn sẽ học thêm được thứ gì đó mới mẻ.


[ ![Shaumik Daityari][19]][20]

[1]: http://www.sitepoint.com/git-for-beginners/
[2]: http://www.sitepoint.com/getting-started-git-team-environment/
[3]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png
[4]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png
[5]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png
[6]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png
[7]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png
[8]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png
[9]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png
[10]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png
[11]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png
[12]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png
[13]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png
[14]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png
[15]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png
[16]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png
[17]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png
[18]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png
[19]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/01/feb5b18a7ac559ec6c8e8afcf96418ac-96x96.jpeg
[20]: https://www.sitepoint.com/author/sdaityari/
