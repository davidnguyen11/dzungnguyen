---
title: Hành trình build Spotify extension
date: "2022-06-23"
tags: techstory, tech
---
Trong bài viết này tôi sẽ kể về hành trình tôi xây dựng Spotify extension. Hiện tại sản phẩm đã có hơn 40k+ downloads trên toàn cầu cross-browser (Chrome, Edge và Firefox).

**Check this out** https://www.spotit.page/

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175312055-7209449b-7167-4b26-aa66-04b23ff9f833.jpeg" width="100%" />
<div>

Bài viết sẽ không đề cập tới code mà kể về từ lúc có ý tưởng tới lúc implement và các vấn đề đã gặp, không chỉ về technical mà còn về cả product perspective.

# Ý tưởng
Tôi biết đến Spotify vào năm 2017, từ đó đến nay tôi vẫn luôn sử dụng nó để nghe nhạc mỗi ngày và nó cũng là cái app đầu tiên tôi bỏ tiền ra mua gói premium. Lý do đơn giản là app đơn giản, nhanh và đẹp.

Từ lúc biết tới Spotify tôi vẫn luôn mong có một ngày trở thành engineer làm việc cho họ, nghĩ tới làm việc cho cái app mà mình thích nghe rất là sướng. Sau khi tôi sang Rakuten 2019, tôi vẫn theo đuổi mục tiêu đó. Tôi biết phỏng vấn sẽ rất khó khăn nên tôi chuẩn bị từ rất sớm, mỗi ngày đi làm về, ăn tối xong là tôi ngồi giải leetcode. Cứ vài tháng tôi lại check Spotify career site một lần, và cứ thế tôi apply trong hai năm liên tiếp và cứ bị reject. Năm 2020, sau khi giải hơn 200 bài leetcode trong gần 2 năm, tôi thấy như đã tới ngưỡng, có bài tôi giải cả tuần không ra.

Rồi một hôm, sau khi làm việc ở công ty xong tôi đuối tới mức không nhớ cái icon của Spotify trông như thế nào để tắt nhạc, cái app duy nhất trong ý thức của tôi là Chrome. Tôi ngồi loay hoay hơn 2 phút mới nhớ ra và tắt nhạc. Rồi tôi bất chợt nghĩ “Không biết có ai làm Spotify extension” chưa. Tôi search thì thấy có khá nhiều người làm, khi check từng cái thì không có cái nào vừa ý. Tôi lại nghĩ nếu người ta làm được, chắc mình cũng làm được. Và rồi hành trình bắt đầu.

# Product development
## Requirement
Tôi tự viết requirement cho Spotify extension sẽ bao gồm những features gì. Tôi chỉ cần đơn giản là nó có thể mở được trên browser, giúp tôi điều khiển nhạc như:

- pause
- next
- previous
- right click text to search on Spotify

Tính năng “right click to search” ý tưởng xuất phát từ các thao tác của tôi khi xem youtube. Khi youtube randomly play a song, nếu hay tôi thường hay highlight tên của bài hát và vào Spotify để search. Đôi lúc highlight mà bị thừa hay bị thiếu thì rất khó chịu và các thao tác để tìm một bài nhạc làm tôi tốn quá nhiều bước, vì lý do đó tôi tìm cách để làm sao cho tiện lợi nhất, và ý tưởng này là lấy cảm hứng từ “right click to translate của Chrome”.

Về sau lúc có nhiều người dùng thì tôi nhận được nhiều feature requests hơn như:

- volume control
- love the song
- podcast
- repeat

Và extension của tôi yêu cầu người dùng phải mở desktop app hoặc web app. Tôi chỉ việc sử dụng API để điều khiển cái app làm theo yêu cầu tôi muốn.

## Spotify API

Để phát triển một extension điều khiển Spotify thì yêu cầu đầu tiên là Spotify phải support SDK hay APIs. Tôi lên search ngay với keyword “Spotify public API”. Vì nếu họ không support thì nghỉ khỏe.

May mắn là họ support officially. Tiếp theo tôi tìm các API support các tính năng tôi sẽ build. Sau khi biết là họ support đầy đủ những gì tôi cần thì tôi tiến hành design UI/UX. 

## UI/UX

### Login

Spotify APIs yêu cầu phải có auth token, để có được token thì phải có màn hình login, trên màn hình login sẽ có một đường link, và đường link đó sẽ navigate user qua trang login của Spotify.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175302380-30e1b4e0-94f7-4471-b5a7-daf91297a644.png" width="100%" />
<div>

### Need open the app

Spotify extension yêu cầu phải luôn mở desktop app hoặc web app, nếu người dùng không mở thì sao? Vì vậy tôi cần một màn hình để thông báo cho người dùng biết là họ phải mở app. Và để tiện lợi cho user tôi cung cấp thêm cho user một deeplink giúp mở luôn cái app bằng một cú click.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175302950-b6ef6e04-6951-4687-80f7-5ad519f15795.png" width="100%" />
<div>

### Main screen

Màn hình chính sẽ display các thông tin như tên bài hát, tên ca sĩ, các controls như pause, next và previous. Và để làm cho nó bớt boring tôi để tấm ảnh cover của bài nhạc vào màn hình luôn. Cơ bản là thông tin sẽ nằm bên trái và cái hình sẽ nằm bên phải.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175303643-9f3b651a-4157-4108-8f75-1bdd1fe1454f.png" width="100%" />
<div>

Nhưng cho dù tôi có gắn tấm hình vào thế nào thì vẫn không thể làm cho cái extension nhìn hấp dẫn hơn được. Thế là tôi nghĩ ra một cách là dùng màu xuất hiện nhiều nhất của tấm hình để fill vào background của cái extension và dùng màu tương phản với cái background thành màu của tên bài hát và các control.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175303827-e20a1c36-c8db-4671-9396-408bd0cd8b67.png" width="100%" />
<div>

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175303837-3ec2833e-fec4-467e-be71-b51f565cf6b0.png" width="100%" />
<div>

### Right click + search song

Người dùng chỉ cần highlight vào keyword cần search, sau đó Right Click vào chọn Search Spotify for "..." thì extension sẽ navigate qua trang Spotify search với keyword mà user chọn. Tôi chả phải làm tính năng search gì cho phức tạp, xài những gì có sẵn thôi.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175304095-d5ce1de5-5a9a-4696-905d-626e2d902010.png" width="100%" />
<div>

## Tech stack

Tôi sử dụng Typescript được hơn ba năm nên tech stack của tôi chắc chắn phải có Typescript và trong ba năm đó tôi viết unit test rất rất nhiều nên tôi cũng kèm luôn unit test trong dự án này luôn (coverage 100%). Trong version đầu tiên tôi cũng muốn thử cả trình độ của mình với Native DOM API vậy là tôi quyết định không xài bất kỳ một framework nào hết, điều đó cũng đồng nghĩa tôi phải tự manage DOM và render. Tôi sử dụng Webpack để làm build tool

<a href="https://github.com/davidnguyen179/web-extension-boilerplate">Link github</a>

Tổng quan thì tech stack ở giai đoạn đầu như sau:
- Native DOM API
- Typescript
- Jest
- Webpack

Và tôi cũng học cách để build browser extension trong tuần đầu tiên. Build mấy cái extension sample để xem hiểu mọi thứ vận hành thế nào rồi nhảy vào code thôi.

Ở version 2, tôi quyết định viết lại toàn bộ Spotify Extension và lần này tôi sử dụng library.

<a href="https://github.com/davidnguyen179/web-extension-svelte-boilerplate">Link github</a>

- Svelte
- Typescript
- Jest
- Webpack

Chi tiết tôi sẽ nói ở phần sau

## Logo

Vâng tôi cũng nghĩ là tôi thiết kế được cả logo, nên cũng thử một lần cho biết, và đây là kết quả

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175304661-dd09e3a3-60e0-4daa-8287-2ca07300b894.png" width="50%" />
<div>

Tôi đưa cho thằng bạn tôi là designer, nó cười vào mặt tôi 😂. Và cuối cùng tôi nhờ nó làm giùm tôi

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175304671-f09346b1-e5b2-475c-8d66-48a35ec1973d.png" width="50%" />
<div>

## Problems

Trong quá trình phát triển tôi gặp rất nhiều vấn đề, có vấn đề có thể solve được bằng technical, có một số vấn đề phải chấp nhận và tìm cách khác để tiếp cận.

### Lost connection with streaming server

Sau khi extension hoàn tất tôi sử dụng được một thời gian cảm thấy rất ok. Nhưng có một lần tôi pause desktop app lại khoản 5 phút để nói chuyện điện thoại, sau đó tôi mở Spotify extension lên để play music lại thì thấy UI hiển thị màn hình “Need desktop app”

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175302950-b6ef6e04-6951-4687-80f7-5ad519f15795.png" width="100%" />
<div>

Khi debug thì tôi thấy API **“Get Current Playback”** return null, sau đó tôi play music bằng desktop app thì mở lại extension thì API trả về data đầy đủ. Nên giả thuyết của tôi là Spotify không giữ connection với desktop app. Vì Spotify họ sử dụng Streaming server để truyền data liên tục về desktop app, ở desktop hay mobile app sẽ có thuật toán để kết nối tạo ra âm thanh hoàn chỉnh. Việc giữ kết nối rất expensive resource của server và điều đó càng đúng hơn khi Spotify có lượng người dùng rất lớn.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175305186-8e8c709a-67ba-4712-a1ab-e7f8a3a516f1.png" width="100%" />
<div>

**Solution**

May mắn là Spotify API có một endpoint khác là “Get Recently Played Tracks” và solution của tôi là nếu “Get Current Playback” trả về null, tôi sẽ call “Get Recently Played Tracks” lấy ra các bản nhạc gần đây được play và lấy bài đầu tiên để display. Vì tôi thà hiển thị một thông tin nào đó phù hợp cho user còn hơn là hiển thị cái gì đó làm cho user confused. Và tôi có thể nói rằng extension này chỉ work 99% và tôi chấp nhận con số đó.

### Support user to open desktop app via deeplink

Tôi nhận khá nhiều request về

<blockquote class="reader-text-block__quote">
  Có cách nào mở desktop app bằng extension không?
</blockquote>

Tôi nghĩ ngay tới deeplink, cũng may Spotify có hỗ trợ và tôi chỉ cần nhúng deeplink vào. 

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175302950-b6ef6e04-6951-4687-80f7-5ad519f15795.png" width="100%" />
<div>

Nếu click vào link “Click to open Desktop app” thì app desktop app sẽ được mở ra.

### What does the UI look like if you suddenly turn off Spotify and open the extension?

Nếu đột nhiên tôi tắt cái Spotify app và rồi sau đó tôi mở extension thì UI trông như thế nào?

Lúc develop thì tôi không có cách nào có thể detect được là cái app nó đang mở hay đã tắt. Vì vậy thay vì hiển thị cái màn hình “Need open app” tôi hiển thị màn hình chính với thông tin bài nhạc đang được play (tôi lưu vào cache). Rồi khi người dùng click play thì API sẽ throw exception, lúc catch error tôi sẽ dùng deeplink để mở desktop app cho user. Vậy là tôi đã làm cho cái flow trở nên tự nhiên cho user. Tôi tốn rất nhiều thời gian để định nghĩa behavior này.

## Publish

Trước khi tôi publish, tôi ping vài người bạn và nhờ test giùm. Sau đó tôi vì quá háo hức nên tối đó tôi publish. Sau mấy ngày chờ đợi được 10 users 😂. Xong tôi có đăng trên facebook thì một anh bạn của tôi nói là mày publish sai tên “Spotity”, nó phải là “Spotify”. Hậu quả là tôi phải unpublish và publish lại từ đầu.

## Cross-Browser

Sau khi Chrome chạy ổn, tôi mới nghĩ tới việc support Edge và Firefox. Tôi nghiên cứu thì được biết browser extension chỉ có một standard chỉ có một ít khác biệt ở phần config. Vì vậy, tôi chỉ cần customize bộ build của extension để build extension cho từng browser.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175306453-741ac6a5-a3c1-4afd-9dda-791e1b69d925.png" width="100%" />
<div>

Và mỗi lần tôi develop một tính năng mới, tôi sẽ chạy ba lệnh build và release cho ba browsers.

## Documentation

Khi publish extension lên store, browser yêu cầu phải có mô tả sản phẩm chi tiết. Và Edge yêu cầu phải có Privacy Policy trong đó ghi rõ là extension mình có thu thập data của user không. Sau khi xong hai cái này thì nhìn lại cái sản phẩm mình rất chuyên nghiệp.

## Marketing

Mỗi lần tôi release hay đạt được cột mốc số lượng user, tôi đều viết content marketing rất cẩn thận và kèm luôn cả hình gif các tính năng hoạt động thế nào. Tôi nhớ tôi share trên reddit đầu tiên, sau đó twitter, linkedin và sau đó là facebook. 

# Rebuild the product

Tôi nhận thấy cứ mỗi lần tôi add một tính năng vào thì tôi phải làm khá nhiều việc, và đôi khi có một số case buộc tôi phải thay đổi structure của code-base và điều này xảy ra khá thường xuyên, và nó làm cho code-base của tôi không scale được. Tôi muốn tập trung để code app và không phải lo quá nhiều về basic architecture. Sau mấy ngày đấu tranh nội tâm ăn ngủ không ngon. Cuối cùng tôi quyết định rebuild lại extension, nó khá risky vì đến thời điểm này tôi đã có một lượng người dùng kha khá khoảng 10k+ users.

Tiêu chí của tôi là bundle size JS phải nhỏ vì nó là extension không phải là một cái web app to. Tôi loại ReactJS, VueJS ra. Cuối cùng tôi chọn Svelte vì nó hơn bundle size của version cũ chỉ có 10KB (40KB vs 50KB) và syntax của nó xài rất sướng, cũng như nó handle cho tôi vụ re-render.

Tôi làm xuyên suốt hơn một tháng, cuối cùng code của tôi cũng pass hết test cases và đồng thời tôi cũng xài thử một thời gian thì không thấy vấn đề gì. Sau cùng tôi release version mới. Và từ đây việc thêm tính năng mới vào tốn tôi rất ít thời gian.

# Performance

Có một lần tôi đọc feedback của user nói là extension xài ngon nhưng nó quá chậm lúc mở lên. Tôi sử dụng tôi cũng thấy chậm, tôi đặt thời gian trước lúc start lên thì tốn 4s để extension hiển thị. Nhiêu đó thời gian để làm cho user cảm thấy khó chịu và ức chế rồi.

Sau một lúc nghiên cứu thì tôi biết performance đến từ việc tôi call 2 đầu API lúc start extension. Cụ thể là authentication API, để solve vấn đề này tôi lưu thông tin authentication vào cache, mỗi lần user mở extension lên tôi sẽ vào cache lấy auth token ra và so sánh thời gian nếu expire thì tôi gọi API và cập nhật cache, nếu không tôi sẽ lấy thông tin từ cache. Sau đó tôi đo lại thời gian thì từ 4s => 1s.

Problem solved!

# Landing page

Sau khi release được một vài tháng, tôi bắt đầu xây dựng Landing Page để giới thiệu sản phẩm cho mọi người. Lúc đầu mục tiêu của tôi là muốn cho mọi người thấy là có khá nhiều engineering effort đã bỏ vào product này, website ở version đầu tiên thì hơi hướng engineer. Sau đó tôi quyết định build lại Landing Page, và lần này target user của tôi là user thường và ở version thứ 2 (hiện tại) tôi lấy cảm hứng từ trang web của Apple giới thiệu iPhone. Lượng người dùng tăng từ 100 => 500 mỗi ngày.

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/175306961-3787f24b-3270-456f-8031-ace1e5aa057d.png" width="100%" />
<div>

# Feedback from users
Sau khi release sản phẩm tôi nghĩ là xong rồi, đủ để bỏ vào resume apply Spotify tiếp thôi. Nhưng khi sản phẩm của bạn có người dùng thì feedback của người dùng rất là quý giá, nó giúp tôi biết được điểm yếu và hoàn thiện sản phẩm hơn. Tôi tiếp nhận feedback của người dùng bằng một nụ cười, vì “đã có người dùng xài product của tôi”. Tiếp theo đó là 1 năm trời tôi làm việc không ngừng để hoàn thiện product hơn cho user.

Có nhiều feedback tôi đọc mà tôi xúc động luôn, vì không ngờ sản phẩm của mình lại được nhiều người hưởng ứng như vậy.

# Conclusion
Tổng thời gian tôi dành cho Spotify Extension là hơn một năm. Lúc nào cũng bắt gặp tôi với cái laptop, không thì suy tư để tìm giải pháp. Ban ngày làm ở công ty, tối làm side project. Từ lúc release tới giờ tôi đã có hơn 40k+ downloads worldwide và cross browser. Mục đích ban đầu là apply vào Spotify nhưng sau đó tôi nghĩ chẳng còn quan trọng nữa. Sản phẩm cũng giúp tôi tự tin hơn và chứng minh được một điều “khi đã thật sự muốn và tập trung thì chắc chắn sẽ làm được”. Tôi học được rất nhiều từ project này:
- Đôi khi bước lùi lại để nhìn bức tranh tổng quát hơn, solve problem không chỉ technical mà còn là product point of view. 
- Đừng để engineer thiết kế logo hay UI, nát đấy 😂
- Đừng release production khi đang mệt, nát đấy.
- Đọc feedback của người dùng và filter để improve vì người dùng giúp sản phẩm tạo ra impact và làm sản phẩm có giá trị.
- Nếu bạn cảm thấy sản phẩm không ổn thì nó thật sự ko ổn.
- Lúc mệt quá thì đi chơi và suy nghĩ tại sao bắt đầu để có motivation finish what you started.

**Check this out** https://www.spotit.page/
