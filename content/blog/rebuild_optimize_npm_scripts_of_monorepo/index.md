---
title: Rebuild & Optimize npm scripts of Monorepo
date: "2022-06-04"
tags: frontend, tech, infrastructure
---

Công ty mình xài Monorepo và có rất nhiều team tham gia vào để phát triển sản phẩm điều đó đồng nghĩa nếu không có rule chung thì code sẽ nhanh chóng thành nồi lẩu thập cẩm. Vì vậy, để đảm bảo Monorepo vận hành tốt, tụi mình force engineer phải follow các điều kiện được đặt ra để đảm bảo quality cho code đồng thời consistent (không ai muốn đọc code xấu cả). Và tụi mình cố gắng để mọi thứ automate để cho engineer chỉ tập trung vào code và ko phải suy nghĩ gì về rules cả. Vì vậy, tụi mình đã phát triển 1 set các npm scripts và integrate vào Githook cũng như CI/CD. Các npm scripts này ban đầu được viết trên Bash. Cụ thể sẽ có các commands sau:

- **Eslint** => check *.ts health
- **Stylelint** => check *.css health
- **Type validity** => check xem mình có xài sai type ko
- **Unit test** => đảm bảo code không bị leak các edge case
- **Visual regression test** => test visual của các component xem có ai làm lệch pixel gì ko hay button ko còn yellow nữa
- **E2E test** => test golden flow của từng trang web
- **Lighthouse** => sử dụng lighthouse APIs collect data để đánh giá web perf sau này

# Context

- Monorepo tụi mình ko xài bất kì 1 framework nào hết (lerna, nx, …) mà tự set up và là main stack của cả một department.
- Có 15k+ *.ts, *.tsx files.
- Có hơn 10 teams làm việc trên đó.

# Problem 1

Mọi thứ diễn ra đúng kế hoạch, nhưng theo thời gian số lượng code ngày càng tăng đồng nghĩa mỗi npm script sẽ phải handle càng nhiều file và thời gian thực thi vì vậy cũng tăng theo, kéo theo độ ức chế của engineer cấp số nhân theo. Hầu như ngày nào mình cũng thấy trên channel mọi người complain "mất quá nhiều thời gian để push code" khoảng > 20 phút. Và chính vì không có 1 action cụ thể nào dẫn đến mọi người sử dụng một option `--no-verify` thần thánh có thể bypass mọi hooks

> git commit -m “blah blah” –no-verify

> git push –no-verify

Well, đúng là commit và push nhanh hơn thật nhưng điều đó cũng đồng nghĩa các rule mà team định nghĩa ra trở nên vô nghĩa nhưng cũng may tụi mình chặn ở tầng CI/CD.

"Lý do tụi mình cho các scripts chạy ở Githook là nhằm giúp mọi người đỡ mất thời gian, fix mọi issues ở local và CI/CD chỉ có pass thôi, nếu bypass thì lỡ CI/CD lỗi thì mình phải xem thử lỗi gì và fix, tốn thêm thời gian để commit và push."

# Problem 2

Đôi khi có có một số PR violate các rule và đáng lý CI/CD sẽ phải fail nhưng scripts của tụi mình lại cho pass lý do đơn giản là tụi mình miss các edge cases trong bash scripts.

**Và bạn biết hậu quả của một cái PR violate các rule được merge vào nhánh chính là gì ko?**

Đó là mọi người từ branch code của họ rebase về nhánh chính và đồng loạt open PRs dẫn đến toàn bộ CI/CD của các nhánh đã rebase fail CI/CD. Đôi khi lỗi hơi nghiêm trọng mọi người toàn bộ bị block ko merge code được vì phải đợi engineer fix logic của các scripts. Tuy rằng fix nhưng không đảm bảo là nó có bug khác trong tương lai.

Và thêm một vấn đề là do các npm scripts được viết bằng bash scripts, mình nhìn vào các script phức tạp mình cũng ko hiểu được ai não có thể căng như vậy khi viết ra một cái logic cực kỳ phức tạp bằng bash. Dẫn tới để fix bug của các script thì phải hiểu biết về bash.

# Solution

Chính hai vấn đề trên dẫn tới tụi mình viết lại toàn bộ npm scripts bằng Typescript.

## PROBLEM 1

Còn việc thiết kế npm scripts, mình đặt ra cho npm scripts như sau:
- Chỉ chạy các npm scripts trên các file được thay đổi ở current branch (selectively)
- Cover unit test 100%
- Chỉ chạy selectively trên PR và specific branch
- Nếu code mà được merge về “master” thì ở nhánh master sẽ thực thi npm scripts trên all files thay vì selectively
- Nếu PR có thay đổi các file quan trọng có thể ảnh hưởng tới development ví dụ "package.json" thì thực thi npm scripts trên all files
- Thiết kế lại các output trên terminal để cung cấp thông tin hữu ích cho engineers đồng thời thêm emoji và màu sắc cho text success hay error, mục đích giúp engineers họ nhanh chóng phát hiện được lỗi và navigate đến đúng file.
- CLI là sản phẩm cho engineers vì vậy mình đầu tư rất nhiều thời gian để thiết kế các output trên terminal sao cho phù hợp cho mấy bạn engineers giúp các bạn tăng productivity
- Set up được 1 architecture chung cho mấy bạn engineer sau vào có thể develop các scripts khác consistently

Bằng cách thực thi npm scripts selectively mình solve được problem 1 là giảm thời gian thực thi (all files vs changed files) theo lý thuyết. Dĩ nhiên là mình sẽ đo thời gian thực thi trước và sau improvement.

## PROBLEM 2

Việc viết bằng Typescript giải quyết vấn đề Bash script nêu ở trên. Vì các engineer sử dụng Typescript hằng ngày nên họ có thể jump và diagnose nhanh chóng.

# Implementation

Mình sẽ không viết ra từng dòng code mà trong phần này mình chỉ nêu các bước và vấn đề mình gặp trong quá trình develop. Nhiều khi mấy bạn đọc xong thì thấy idea sao mà đơn giản vậy.

Trong quá trình develop có một số task mình buộc phải thực thi các command để có được output cần thiết. Để thực thi các command programmatically, mình sử dụng NodeJS child process.

Mình quy ước behavior của script như sau
- success => exit 0
- fail => exit 1

Lý do là vì CI/CD environment sẽ chạy trên môi trường Linux và theo mình nhớ OS nó quy ước vậy.

## HOW TO DETECT CHANGED FILES IN THE CURRENT BRANCH?

Để thực thi các command selectively thì câu hỏi đầu tiên là "Làm cách nào để lấy ra các changed files ở current branch?"

Mình dùng NodeJS <a href="https://nodejs.org/api/child_process.html" target="_blank">child process</a> thực thi 2 commands của git để lấy ra danh sách các changed files:

> git merge-base <branch> <branch>

> git diff <merge-page-hash-string> --name-only

## LEARN TO USE API OF ESLINT, PRETTIER, STYLELINT

Để implement các eslint, stylelint và prettier scripts, mình dựa vào các APIs mà các tool này support rồi sử dụng theo nhu cầu trong code.

- eslint https://eslint.org/docs/developer-guide/nodejs-api
- stylelint https://stylelint.io/user-guide/usage/node-api/
- prettier https://prettier.io/docs/en/api.html

Cụ thể khi mình gõ command chạy eslint, stylelint hay prettier thì script sẽ thực hiện các nhiệm vụ sau:

1.  Lấy changed files
2. Filter ra các file theo extension (ví dụ chạy eslint thì mình chỉ cần quan tâm *.ts, *.tsx)
3. Gọi các APIs mà các tool nó support
4. Nếu pass
> Thông báo ra terminal các file check thành công và text "Success" với color "xanh lá" (nên suy nghĩ về engineers họ cần gì để design cho phù hợp)

> exit 0

5. Nếu fail
> Thông báo ra terminal các file check failed và text "Fail" với color "đỏ"

> Display các files với các path file và line rõ ràng để engineers họ có thể navigate tới file đó nhanh chóng

> exit 1

## KEEP TYPE CHECK COMMAND RUN ALL FILES

Để check valid type của cả 1 repo, mình sử dụng Typescript CLI với option `--noEmit`. Mình đã cố tìm cách cho nó chạy selectively nhưng đào sâu vào trong source code của Typescript thì nó quá nhiều knowledge trong đó và tốn quá nhiều thời gian để tìm hiểu. Mình đã thử cách khác là đo thời gian thực thi của option `--noEmit` cho toàn bộ 15k+ files thì thời gian thực thi khoản 30s hoặc 40s và nó có thể chấp nhận được. Thế là mình chỉ việc dùng child process chạy Typescript CLI với option `--noEmit` cho toàn bộ files.

## UNIT TEST

Đây là cái script mà mình tốn rất nhiều thời gian, nó hoàn toàn không khó để implement nhưng cái chính là phải hiểu jest hoạt động và debug (it’s a pain in the ass).

**Graph dependencies**

Unit test không giống như linter, vì mỗi file mình thay đổi sẽ ảnh hưởng đến các file khác, cái này được gọi là graph dependencies.

_Ví dụ:_

- Module A được sử dụng bởi module B và module C
- Khi engineer họ thay đổi module A thì khi chạy unit test thì phải chạy trên module A, module B và module C

**Jest options**

Ở công ty mình xài `jest` để chạy unit test. `Jest` đã support một số option ở CLI giúp thực thi test selectively

- https://jestjs.io/docs/cli#--onlychanged
- https://jestjs.io/docs/cli#--changedsince

Khi tìm được 2 options này mình thử integrate vào code-base và chạy ngon lành. Tuy nhiên, cách tụi mình design unit test hơi khác. Unit test của tụi mình support testing container, function. Và tụi mình abstract nó thành 1 module khác để test container, engineer chỉ cần định nghĩa config (có thể *.ts hoặc *.json) thì cái module abstract sẽ tự động import dynamically các file config đó và initialize unit test để run. Điều đó đồng nghĩa config và container độc lập.

_Ví dụ:_

- Container A có config A để chạy unit test
- Container A không import config A => abstract layer sẽ tự động tạo connection giữa container A và config A
- Mình thay đổi config A ở current branch
- Jest chỉ collect được config A và nó bỏ qua container A
- Thế là nó không chạy unit test

Mình investigate vào source code của jest thì cách nó collect graph deps bằng cách check file có `import` module nào khác không. Và nó là điều đó bằng cách Regular Expression.

Câu hỏi kế tiếp `Không biết jest có support customize graph deps?` => 100% Yes

**Customize graph deps**

Jest có hỗ trợ mình customize graph deps bằng config đặc biệt gọi là <a href="https://jestjs.io/docs/configuration#dependencyextractor-string" target="_blank">dependencyExtractor</a>

Documentation của config này rất sơ sài, mình đã phải đục vào source code của nó xem, nhưng mình sẽ không hiểu gì nếu không biết tổng quan architecture nó thế nào. May mắn thay, jest team có làm 1 cái video giải thích cụ thể về mọi thứ bên trong jest hoạt động thế nào và nó rất hay

- https://jestjs.io/docs/architecture

**Debug**

Lúc làm mình muốn debug, mình đặt rất nhiều `console.log(...)` trong cái method `extract(...)`, lúc chạy test nó ko “print” cái gì ra terminal luôn, mình tưởng mình làm sai.

Lúc sau mình đặt `console.log(...)` bên trong source code của jest trong node_modules thì thấy nó rõ ràng là có gọi cái method đó.

Rồi mình thử dùng mấy hàm ghi file (<a href="https://nodejs.org/api/fs.html" target="_blank">fs</a>) của NodeJS để trong method `extract(...)` thì thấy là nó có ghi hết mấy cái graph deps vào cái file.

**Define rule cho graph deps**

Sau khi đã biết nó hoạt động thế nào, giờ thì mình định nghĩa các rules, lúc graph deps của jest chạy nó sẽ chạy qua các customize rules của mình rồi tiến hành check nếu match rule thì nó sẽ collect deps.

Mình định nghĩa bằng Regular Expression đến các file (*.ts, *.json) nằm trong thư mục `config/`.

<blockquote class="reader-text-block__quote">
  Well, it works like magic!!!
</blockquote>

# Make sure npm script run correctly

Như mình đề cập ở trên “Problem 2”, để tránh xảy ra các edge cases xảy ra, tụi mình viết unit test cho npm scripts và review strictly. Code cover 100%, trong quá trình viết unit test mình cũng gặp khá nhiều rắc rối trong việc mock các built-in lib của NodeJS và suy nghĩ để tìm approach khác để mock rất thú vị.

# Result

Cuối cùng sau 6 tháng làm mình đã merge 4 npm scripts đã được rebuild vào nhánh master. Và tụi mình bắt đầu monitor và thời gian thực thi của CI/CD giảm significantly từ > 20 min down xuống còn nhiều nhất là 5 min. Có một số branch chỉ thay đổi README.md thì nó pass ngay và luôn.

Điều làm mình vui nhất là mình được mọi người trong department khen, tán dương và mình được present nói về quá trình làm rebuild cái đó trước > 50 engineers của department. Và mình cũng build được cái frame để các engineer khác lúc code script mới chỉ việc follow cái frame đó.

# Conclusion

Yeah, đó là một hành trình 6 tháng khá thú vị đủ cung bậc cảm xúc đặc biệt là lúc nghiên cứu `jest` (kiểu như không biết bắt đầu từ đâu). Thành quả mang lại là ra cái gì đó có ích cho engineer.

Qua đây mình cũng rút ra một điều mình cho là khá insight trong quá trình rebuild npm scripts là suy nghĩ các thông tin cần thiết cho engineer sao cho họ dễ dàng debug hay navigate file và improve UX sao cho họ tốn ít effort để đạt được cái họ cần trong quá trình debug. Và sau đó có một số bạn engineer khác volunteer contribute vào rebuild các scripts khác.
