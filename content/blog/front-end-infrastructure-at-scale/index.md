---
title: Front End infrastructure at scale
date: "2022-05-27"
tags: frontend, tech
---

Front End mấy năm gần đây không giống như Front End của 20 năm trước. Nếu là trước đây khi nói về FE thì mọi người sẽ nghĩ ngay tới HTML, CSS và JS. Các web app ngày càng có nhiều interactive và ngày càng có rất nhiều tính năng để giúp các công ty thu hút người dùng và tăng lợi nhuận. Ở các công ty to, nguồn lực của họ rất nhiều, họ cho mỗi team take care 1 tính năng hay một trang web. Việc chia như vậy được gọi là Micro Front End. Hiểu nôm na là mỗi team ôm mỗi trang hay 1 tính năng cụ thể và lúc ráp các trang đó lại ra 1 product hoàn chỉnh. Lợi ích của việc chia ra như vậy là giúp các team hoạt động độc lập và deliver feature nhanh hơn, giúp công ty chạy business và kiếm tiền nhiều hơn. Chính vì nhu cầu này dẫn đến việc phát triển FE được đầu tư khá nhiều từ phía JS community, kể từ khi NodeJS và React ra đời, kéo theo mấy năm sau đó hàng loạt các thư viện cũng như framework based trên JS ra đời càng nhiều, làm cho hệ sinh thái FE rất phát triển đa dạng và phong phú, có thể hỏi 1 bạn beginner bắt đầu FE và họ sẽ kể cho bạn nghe họ cảm thấy choáng ngợp thế nào.
Mình làm Front End cũng được kha khá năm, cũng trải qua và thấy nhiều thứ, sau đây mình xin chia sẻ bức tranh tổng quan về Front End development at scale, hi vọng sẽ có ích cho mấy bạn Front End engineer và cũng welcome mọi người thảo luận các case study.

**Lưu ý:** bài này mình sẽ không đề cập về ReactJS vì giờ nó thành norm của Front End development. Khi mình nói về component trong bài viết thì nó là React Component

# Typescript

Ai làm front end cũng biết JS là ngôn ngữ không có type, lúc source code to lên thì đôi khi “variable” type xài lúc này có thể không còn đúng ở lúc sau nữa, nhiều lúc làm mình tốn rất nhiều não và thời gian chỉ để biết được cái type của cái variable đó là cái gì.

Đôi lúc cái props A không được phép `undefined`, mà vô tình hay cố ý nó lại `undefined` dẫn tới cái web app nó crash vào ngày thứ 7 và chủ nhật, kỹ sư phải mở máy lên để mà investigate và cho thêm 1 vài đoạn “if” thần thánh để fix. Và trong code base chắc có cả đống chỗ như vậy.

Interface chặt chẽ, ai làm React rồi chắc cũng biết mình có thể pass whatever props vào bên trong cái component. Có nhiều trường hợp cái “props A” đang được xài ở component (1) này, sau 1 thời gian refactor họ remove cái “props A” rồi, nhưng cái component xài component (1) vẫn còn pass cái props A vào sẽ làm cho engineer rất bối rối, ko biết có magic code nào ở đây không. Hoặc có 1 số trường hợp engineer họ xoá cái props A đi vì không thấy trong code, nhưng có 1 chỗ bí mật nào đó sử dụng, thế là cái app crash. Typescript solve problem này bằng cách tạo ra interface rất chặt chẽ cho component của mình, nếu mình xài thiếu hay dư props thì compiler sẽ báo lỗi và buộc mình phải sửa.

Typescript do Microsoft phát triển, xài kết hợp với Visual Studio Code thì sẽ có thêm rất nhiều tính năng giúp tăng tốc việc navigate files khi codebase lớn. Mình nghĩ “cái gì mà tool nó làm được thì để nó làm để dành sức làm việc khác”

# Mono repo & Poly repo

Mình sẽ không so sánh vì có rất nhiều bài viết nói về vấn đề này rồi. Mình chỉ nói cảm nhận sau khi trải qua 2 cách tiếp cận này.

## POLY REPO

- Nó giúp team manage code mà team đang vận hành, thường là khá nhỏ dẫn tới việc build hay deploy này nọ nó cũng nhanh.
- Mỗi team tự manage issue không ảnh hưởng gì tới team khác
- Tự quyết định tech stack

Nhưng có 1 vấn đề hồi đó mình gặp đó là công ty phát triển một thư viện xài chung, và có rất nhiều projects (> 20) được owned bởi các team xài thư viện của công ty. Sau khi thư viện release version mới thì việc thông báo cho các team khác để up version là rất khó khăn nếu không có một process cụ thể thì sẽ xảy ra trường hợp có team đã up version mới nhất, có team thì lib bị out date.

Nếu có quá nhiều projects thì cách quản lý, tìm kiếm cũng sẽ khó khăn và tốn rất nhiều thời gian. Cứ ngồi control + tab để xem project này và project kia khoản 10-15p là loạn luôn, nhiều khi quên mất luôn mình đang định làm gì.

## MONO REPO

Tất cả projects của một domain sẽ được gói gọn trong một repo siêu to khổng lồ, hồi đó team mình chơi hơi mạnh setup từ đầu mono repo và không xài bất kì 1 framework nào (lerna, nx, ...), nên cũng có khá nhiều technical challenge và có hơn > 10 teams tham gia vào monorepo để rebuild lại product.

<blockquote class="reader-text-block__quote">
Thế tại sao tụi mình lại quyết định chuyển qua mono repo?
</blockquote>

Product của tụi mình có rất nhiều team phát triển, có 1 số team họ chỉ có 1 tính năng rất nhỏ và muốn embed vào trang web chính, thế là họ phát triển và export ra 1 cái bundle file bao gồm jQuery, lodash và các thể loại thư viện hạng nặng (you name it). Hãy tưởng tượng có 10 teams như vậy cho ra 10 cái bundle files siêu to khổng lồ nhúng vào trang web chính làm cho trang web nó rất nặng nề và conflict version lẫn nhau. Chưa kể khi có bug, thì việc mà set up development để reproduce trở nên rất khó khăn, tốn rất nhiều thời gian để communicate (trust me it's a pain in the ass 😂). Thêm vào đó là UI ko consistent, ví dụ designer họ đưa mã màu chuẩn, nhưng team khác họ lấy màu lệch 1 chút nhưng nhìn vẫn giống. Tổng hợp lại nhìn cái product ko khác gì cái nồi lẩu. Đó là lý do tụi mình migrate qua mono repo để solve problem trên. Tuy nhiên nó cũng đi kèm với khá nhiều technical challenge.

- Webpack config rất phức tạp, và trở thành cái khó upgrade nhất. Vì để thay đổi thì đòi hỏi phải hiểu rất sâu về webpack config.
- Nâng cấp version các thư viện rất khó khăn, đòi hỏi phải collaborate với các team để họ test và gather benchmark. Mình còn nhớ việc upgrade React version diễn ra 4 tháng và NodeJS (mình làm) mất 1 năm thăng trầm.
- Vì có quá nhiều team tham gia vào mono repo nên codebase cũng vì thế mà to lên rất nhiều (>15k *.ts, *.tsx files), kéo theo các scripts chạy để validate (linter, unit test, …) trở nên rất chậm theo (mỗi lần push code mất > 20 min), đòi hỏi phải bỏ ra 1 effort rất lớn để maintain, tụi mình có hẳn 1 team để phát triển các improvement đó. Mình sẽ viết 1 bài khác về cách mình đã optimize như thế nào để nó down từ 20 min xuống còn at most 5 min (stay tuned)
- Learning curve khá to vì mono repo tụi mình có quá nhiều thứ để cover, vì vậy khi 1 team nào mới join thì phải có 1 engineer bên core team advocate vào team kia để support 1 thời gian.

# Design language

Design language là ngôn ngữ được sử dụng chung giữa designer và front end engineer nghĩa là designer họ sẽ định nghĩa các chuẩn màu, size text, space, … khi engineer develop họ sẽ sử dụng các chuẩn đó để phát triển tính năng, design language sẽ được sử dụng cho tất cả các team nhằm đảm bảo tính thống nhất (consistency) của sản phẩm.

**Ví dụ:**

- Designer họ định nghĩa mỗi element có margin 5px. Nhưng engineers họ sử dụng 4px => vi phạm tính consistency của sản phẩm.
- Hoặc designer họ định nghĩa mã màu đỏ (1), engineers họ dùng 1 mã màu khác nhìn cũng giống màu đỏ (1) => vi phạm tính consistency

Để sử dụng Design language một cách hiệu quả thì cần có sự đồng thuận của các team và sự đồng ý của sếp (phụ thuộc vào cách thuyết phục và lợi ích nó mang lại cho product). Mình còn nhớ team mình đi giải thích cho team designer hiểu được concept của design language mất mấy con trăng.

Tụi mình thì dùng Storybook để implement design language và cái storybook đó tụi mình đưa lên 1 domain internal để mọi người dễ coi. Engineers chỉ việc sử dụng các component đã được build mà phát triển feature. Tuy nhiên, designer họ vẫn có thể sử dụng sai các standard và không aware được là đã có bao nhiêu component có sẵn nên cứ mỗi lần họ design spec thì lại sinh ra 1 tá component khác nhìn y chang và chỉ khác nhau color 1 chút và spacing. Để mitigate cái pain này, tụi mình đã phát triển 1 plugin cho storybook integrate với figma có nghĩa là designer họ sẽ vào storybook, tìm component họ cần và click cái nút figma để đưa cái component đó từ storybook sang figma của họ.

Sau khi engineers họ open PR thì reviewer cũng phải strictly review. Họ có nhiệm vụ là xem các component của engineers xài trong code và suggest cho engineers sử dụng các component có sẵn thay vì tạo ra cái mới.

# Testing

Là một trong những thành phần quan trọng nhất đối với team mình, tụi mình đầu tư rất nhiều thoughts vào đây.

Ứng dụng web của bên mình chia ra làm 3 level:

- Page: sử dụng n containers
- Container: sử dụng n visual components
- Visual component: xử lý CSS và logic thể hiện của component đó

## UNIT TEST

Tụi mình set test coverage là 100%, tuy nhiên bug là vẫn bug. Và chỗ nào có function thì chỗ đó có unit test. Tụi mình cũng sử dụng 1 số logic tương tự như snapshot để test container nhưng đây không là cách hay (vẫn đang tìm giải pháp thay thế).

## VISUAL REGRESSION TESTING

Team mình dùng cách này để test các visual component, đa phần mọi người sẽ nghĩ là để test component thì dùng snapshot testing nhưng file snapshot nó thay đổi rất thường xuyên và chưa kể component có thể được chỉnh sửa bởi rất nhiều engineers, đôi khi vô tình nó làm component lệch 1 pixel hoặc text ko nằm đúng vị trí, với snapshot testing mình sẽ không tài nào biết được mấy cái thay đổi visual đó. Visual regression testing generate ra 1 cái hình của cái component đó, và lần sau nếu mình có thay đổi gì thì nó sẽ generate ra 1 tấm hình khác và so sánh với baseline pixel by pixel, nếu có thay đổi nó sẽ sinh ra 1 tấm ảnh khác với các highlight changes.

## E2E TESTING

Tụi mình sử dụng để test golden flow của product và chỉ chạy ở phía CI/CD. Ví dụ test hành trình của user từ lúc vào trang chủ, vào trang chi tiết và mua hàng và thường nó được apply cho “page” level. Để tránh test phụ thuộc vào real API, bên mình có support mock data bằng typescript hay json file, rồi lúc test chỉ cần chuyển cái mock data file name lên query string.

**Ví dụ:** blahblah.com?mock=success

# CI/CD & githook

Để đảm bảo code follow convention, thì tụi mình dùng githook và CI/CD để check code health. Tụi mình tạo ra các npm scripts để dùng chung giữa githhook và CI/CD.

## Githook

**pre-commit**
- Run prettier

**pre-push**

- Run typescript compiler để validate type
- Run eslint để check *.ts, *.tsx code có follow convention rule ko
- Run stylelint để check CSS có follow convention rule ko
- Run unit test

## CI/CD
- Chạy tất cả các command ở pre-push
- visual regression testing cho tất cả các components
- run e2e testing

Mỗi stage của CI/CD tụi mình đo lường thời gian thực thi, để sau này có thể dựa vào đó mà biết nhanh hay chậm để optimize.

# Make sure to upgrade libs in some periods

Upgrade không phải là một công việc đơn giản nhưng cần thiết, mình lấy ví dụ Typescript version 4 có rất nhiều tính năng hay và rất tiện lợi, nhưng vì dự án mình lúc đó NodeJS chưa nâng cấp mới thì ko thể upgrade lên Typescript 4 được. Năm 2020, mình giúp cả 1 department upgrade NodeJS từ 10 lên 14, lý do node 10 sẽ ko còn support nữa và nó cũng là blocker làm mình ko upgrade mấy thư viện khác lên được. Lúc đầu mình nghĩ khá đơn giản là “Well, it’s just upgrade stuff”, trong quá trình upgrade, có 1 số package được build ra dưới dạng binary (node gyp) nhưng vì file binary của nó không support node 14 trên môi trường linux, mình phải dựng 1 con máy ảo chỉ để build cái binary từ cái package đó cho node 14 trên linux rồi publish package đó lên internal npm server. Sau khi làm xong tụi mình chạy load test kết quả cho ra chậm gần gấp đôi (150ms => 250ms), mình nghĩ ra đủ thể loại hypothesis, collect data rồi học cả stat để đánh giá kết quả, làm rất nhiều profiling, và cả tá report để discuss với team. Có 1 số hypothesis mình phải remove legacy code (không phải trong scope) nhưng kết quả cũng chỉ nhanh hơn dc 3 hay 4ms. Nhưng cuối cùng sao 1 loạt debate thì mình cũng thuyết phục đc họ là merge.

Sau cùng mình rút ra kinh nghiệm là đảm bảo upgrade các lib trong một khoảng thời gian quy định, các version mới sẽ mang lại nhiều tính năng cho engineer sử dụng và bảo mật hơn, đừng để khi khoảng cách giữa version hiện tại và version cần upgrade quá to lúc đó sẽ sinh rất nhiều pain.

# Collect web vitals metrics

Google họ định nghĩa các metrics (<a href="https://web.dev/vitals/" target="_blank">web vitals</a>) để đánh giá performance của 1 website. Trước khi đánh giá một website nhanh hay chậm tụi mình sẽ dựa vào các metrics đó tổng hợp thành 1 report và đánh giá dựa trên các con số, để có được các metrics đó thì chỉ có cách duy nhất là collect hoặc dựa vào các dịch vụ như Cloudflare để tổng hợp cho mình. Sau khi có các con số rồi thì mình có thể nghĩ tới hypothesis để optimize. Sau khi optimize mình có thể monitor các metrics đó xem có cải thiện để chứng minh cho hypothesis của mình hay không

# Mock data

Thường thì phát triển product bây giờ họ cố gắng sử dụng một API endpoint duy nhất (Dĩ nhiên đầu API này kết nối rất nhiều APIs enpoint khác) để tổng hợp data cho cái web để nó render. Có thể hình dung đơn giản là gateway hoặc là graphql. Thì công ty mình để bớt phụ thuộc vào Back End team, tụi mình cho phép truyền 1 param cụ thể là “?mock=<mock-data-filename>” trong mỗi cái file mock tụi mình simulate data từ Back End theo từng case. Làm như vậy giúp cho việc develop nhanh hơn và bớt phụ thuộc vào team khác. Nhưng khi đã có nhiều mock data file thì sẽ dẫn tới việc khó quản lý.

# Kết luận

Những gì mình chia sẻ phía trên là những gì thực tế mình làm mỗi ngày, nó có thể phù hợp hoặc không phù hợp tuỳ vào mức độ scale của team. Hy vọng qua bài viết mọi người có một cái nhìn khác về Front End.

<blockquote class="reader-text-block__quote">
  It’s not just making the button yellow, it’s just something else
</blockquote>
