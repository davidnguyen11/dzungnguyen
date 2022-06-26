---
title: Quá trình build resume
date: "2022-04-28"
tags: careerguide
---

Mình muốn chia sẻ kinh nghiệm cá nhân về việc mình đã build personal profile của mình thế nào. Nó tốn mình 2 năm để biết mình nên cần làm gì và 5 năm còn lại để hình thành thói quen và thực hiện. Lưu ý mỗi người sẽ có cách build khác nhau, cách của mình chỉ mang tính chất tham khảo, ko cần phải follow 100% mà filter cái gì best cho mình là được.

# Personal profile là gì?

Mình hiểu nó thế này, personal profile là cái giúp người khác nhìn vào mình và đánh giá mình "ừa thằng này ok để phỏng vấn nó thử". Mình đã từng apply vào các công ty lớn lúc mới ra trường đa phần là bị reject vì profile quá bèo, ít kinh nghiệm và không có gì đặc sắc. Đó cũng là lí do mình quyết tâm phải build cho nó good 1 chút để tăng cơ hội cho việc apply vào các Top Tech.

# Overview

Lúc mới ra trường kinh nghiệm của mình là zero, mình cũng không giỏi giang gì, cũng không tham gia các kỳ thi ACM hay bất kì kì thi gì. Lúc đi tìm việc mình thậm chí ko biết viết resume như thế nào và lúc viết câu hỏi đầu tiên mình nghĩ tới "bỏ cái gì vào mục kinh nghiệm", mình cũng hỏi nhiều bạn bè cùng lớp thì ai cũng bảo là bỏ mấy dự án bài tập vào hoặc dự án làm lúc bảo vệ luận văn ví dụ "quản lý khách sạn", "quản lý nhà hàng", ….nhưng những dự án đó thường nó là ý tưởng từ bài tập và nó khá đại trà không có gì đặc sắc, chắc hẳn nhà tuyển dụng thấy nó chắc cả trăm 😂 nghìn lần . Và cuối cùng mình đã bỏ dự án mình làm lúc bảo vệ luận văn vào resume của mình, đề tài lúc đó của mình là nghiên cứu về `crowdsourcing` cũng có chút chút về nghiên cứu khoa học trong đó.

Năm 2014, lúc phỏng vấn thì anh interviewer cũng hỏi sơ sơ về cái đề tài luận văn này, mình cũng trả lời nhiệt huyết lắm, rồi cũng pass và vào làm thực tập cho công ty outsourcing. Công việc hằng ngày của mình khá nhàm chán, fix bug và đọc document và nhiều khi mình còn không hiểu mình làm cái gì nữa, vì các task nặng đều đưa cho senior và leader làm. Mình cảm thấy không học được gì hết, lúc đó mình mới nghĩ "mình còn nhiều mục tiêu chưa thực hiện được lắm như apply vào FAANG" giờ mà làm mấy cái mà chả có impact gì cho team thì khi nào mới bước ra ngoài thế giới được. Mình stress vô độ, nghi ngờ so sánh bản thân với người khác. Sau đó mình nhớ lại là hồi đại học mình đã luôn ấp ủ là 1 ngày nào đó mình sẽ develop được 1 dự án OSS (Open-source software) cool ngầu và có bài viết đăng quốc tế.

Then the journey begins!!!

# OSS (Open-source software)

## TÌM Ý TƯỞNG

Ban đầu mình sẽ là tạo ra vấn đề và nghĩ rằng người ta sẽ gặp. Sau đó phát triển 1 thư viện nhỏ để solve vấn đề đó. Nhưng bản thân mình làm mà mình còn ko xài thì ai xài. Cứ thế mình phát triển rất nhiều thứ và mình ko xài bất kỳ thứ gì cho tới 1 ngày mình nhận ra cách tiếp cận của mình sai sai, mình cảm thấy mất thời gian nhưng nó cũng giúp mình quen thuộc các toolchain cũng như process phát triển phần mềm, ngẫm lại thì nó vẫn có rất nhiều lợi ích.

Sau đó mình dừng 1 thời gian để tìm việc khác, lúc sang công ty mới, mình có nhiều cơ hội solve nhiều problem hay, trong quá trình làm mình có quan sát và thấy có 1 số nơi trong code có thể publish thành OSS được. Sau đó, mình mới chuẩn bị tài liệu design solution cũng như là lợi ích của việc phát triển OSS và trình bày với sếp lớn. Sau đó mình được approve để đưa cái package đó ra OSS.

Ngoài ra, mình còn quan sát bản thân xem có gặp vấn đề gì trong cuộc sống hằng ngày ko và các vấn đề đó có thể develop software để solve problem của mình không. Nếu được thì mình sẽ tiến hành lên plan và phát triển software để solve problem cho mình.

__Ví dụ:__

- Khi mình phát triển Spotify Extension thì vấn đề mình gặp phải là: Lúc mình làm việc xong mình hơi mệt và mình muốn tắt nhạc mà mình quên luôn cái icon của Spotify, cái duy nhất mình thấy là Chrome, mình lúc đó tự hỏi "Ủa, có ai develop Spotify Extension chưa?", mình search thì thấy có nhưng đa phần rất xấu. Câu hỏi kế tiếp "Tại sao mình không làm?". Và phần còn lại là lịch sử. Trong quá trình làm mình có publish luôn cả 2 cái boilerplate cho việc phát triển Cross-Browser extension thêm phần dễ dàng.
- Có giai đoạn mình giải leetcode rất nhiều và hôm đó mình tình cờ gặp 1 bài nó dùng "Priority Queue" và ngôn ngữ JS không có cái đó. Mình dành thời gian đọc hiểu concept và quyết định build cái "Priority Queue" OSS cho JS. Mình để ý là có rất nhiều bài trên leetcode có tiềm năng đưa ra thành OSS.

Trong 1 quá trình dài phát triển OSS thì mình để ý thấy là mình không nhất thiết phải build quá nhiều, tập trung vào chất lượng và giải quyết đúng bài toán. Focus on QUALITY instead of QUANTITY

## CÁC YẾU TỐ GIÚP SOFTWARE CÓ ĐỘ TIN CẬY

**TOOLS**

Lúc mình phát triển OSS, mình có tham khảo 1 số thư viện nổi tiếng xem nó có cái gì trong source code của nó, khi mình bắt gặp 1 từ khoá nào lạ lạ, thì mình sẽ Google ngay xem nó là cái tool gì. Ví dụ, CI/CD, githook, linter, codecov, …

Nếu tool giúp mình solve 1 số problem về development thì mình sẽ integrate vào dự án của mình còn không thì có thể gắng vào trang trí nhìn cho cool ngầu hơn.

**README**
Không chỉ việc develop, mà hướng dẫn sử dụng cũng cực kỳ quan trọng, cứ tưởng tượng mình là người xài thư viện thì mình sẽ expect cái gì trong README, mình muốn nó chi tiết và rõ ràng, có code ví dụ và nêu rõ vấn đề mình cố solve là gì và thường là mình dùng các từ tiếng Anh đơn giản để tiếp cận được nhiều engineer hơn. Đồng thời mình cũng thêm luôn cả các badge để show số lượng download, version của package… Mình có 1 thói quen là mình hay viết blog mô tả lại quá trình phát triển OSS, và dĩ nhiên là mình nhúng luôn bài cái link bài viết vào README để tăng traffic cho bài viết của mình.

**UNIT TEST**

Lúc mình phát triển OSS, mình cũng mong muốn nó được nhiều người sử dụng và đối tượng người dùng của mình là engineer. Thường thì các kĩ sư kinh nghiệm sẽ nhìn vào codebase xem có được đầu tư không và unit test là 1 trong những thành phần quan trọng.

Có thể hình dung đơn giản thế này, mình đi mua chiếc xe, mình sẽ tìm hiểu xe này nó là brand gì và công ty đứng đằng sau nó là ai, chế độ bảo hành sao. Bạn sẽ không muốn 1 chiếc xe từ 1 brand ko uy tín, lỡ hôm nay mua xong mai hư mà nó lại không bảo hành thì sẽ như thế nào.

**CUSTOMER SUPPORT**

Mình bắt đầu hình thành thói quen là đọc Issue ở github để xem có ai gặp vấn đề gì ko. Nếu có thì mình sẽ quyết định như thế nào fix hay không fix. Mình cũng nhận được feature request và cũng học được cách quyết định là nên thêm hay ko thêm. Cơ bản là mình học được lúc nào nên say "NO" vì cơ bản mình ko thể chiều ý hết tất cả mọi người.

Có lúc có người dùng gửi mail riêng cho mình để hỏi là làm cách nào xài sản phẩm của mình, lúc đó mình vui muốn khóc, vì có người xài hàng của mình. Dĩ nhiên mình đã support họ tới nơi.

**RELEASE NOTE**

Mình nghĩ cái này rất quan trọng, ví dụ mình ra version "1.0.0" và có 1 lượng người dùng khá lớn, và sau đó vài tháng mình lên version "2.0.0", nếu mình cứ âm thầm lặng lẽ publish và ko thông báo cho người dùng biết thì khi họ nâng cấp và app của họ bị crash thì sẽ ảnh hưởng đến uy tín của cái lib của bạn. Vì vậy, mình lúc nào cũng viết release note 1 cách tường minh và nếu bạn là người nổi tiếng thì có thể share trên Twitter, … để người dùng người ta biết.

**KẾT QUẢ**

- https://spotit.page/ hơn 40k+ downloads
- https://github.com/davidnguyen179/web-extension-boilerplate
- https://github.com/davidnguyen179/web-extension-svelte-boilerplate
- https://github.com/davidnguyen179/react-paginating 9k+ downloads monthly
- https://github.com/vutoan266/react-images-uploading 86k+ downloads monthly as a collaborator

# Blog

## QUÁ TRÌNH TÌM STRUCTURE ĐỂ VIẾT

Mình bắt đầu tập viết từ những năm tháng đại học, chủ đề của mình viết thường là tutorial. Lúc mới đi làm mình phải tạm gác lại để có thời gian ôn luyện đổi công ty. Sau khi chuyển công ty thì mình mới bắt đầu nghĩ tới việc viết lại và trong 1 buổi chiều thời tiết dễ chịu, mình quyết định visit cái site mình đã từng muốn đăng bài viết ở đó và mình đã quyết định gửi mail cho họ là ngỏ ý muốn viết bài cho họ. Vâng họ gửi mail `OK` và cho mình làm writer và chủ đề mình chọn là viết 1 tính năng nhỏ trong cái tool `SASS`, sau đó họ cho mình 1 anh editor và collaborate để ra được cái outline và mình đã viết trong 1 tháng với đủ cung bậc cảm xúc, cái ngày mà nó publish mình đã thở phào nhẹ nhõm. Giờ thì nó sẽ được đọc bởi đọc giả toàn cầu. Nhưng cái mình vui nhất là cái process mình đã học được khi viết bài:

1. Xác định mình muốn viết gì
2. Xác định xem target audience là ai để lựa chọn cách diễn tả cho phù hợp tech or non-tech
3. Lên outline cho bài viết của mình. Mỗi một point cần phải cho đọc giả output và đặt mình là vị trí của đọc giả. Nếu mình là đọc giả mình muốn có được gì sau khi đọc phần này.
4. Demo nếu có (ví dụ mình muốn demo cho mn xem cái tính năng này work thế nào) e.g, code sandbox or whatever tool...

Sau khi mình viết xong, mình thường sẽ nhờ bạn bè giỏi tiếng Anh đọc và cho comments

Nguyên tắc của mình là nhờ 1 người đừng nhờ quá 3 lần, mất tình bạn như chơi đó 😂.

## TÌM Ý TƯỞNG ĐỂ VIẾT

Chủ đề để viết thì rất bao la, nhưng khi chọn nên thu hẹp phạm vi lại để mình có thể dễ tìm ý tưởng để viết, tránh nghĩ quá to rồi nản ko viết. Có thể chọn 1 tính năng nào đó trong cái framework mình dùng để viết hoặc cách setup cái framework đó (tutorial). Lúc đầu mới viết mình thường chọn những chủ đề như vậy.

Khi viết quen được 1 chút, thì mình thường tìm các chủ đề trong công việc hằng ngày ví dụ cách mà mình solve problem hoặc develop 1 cái gì cool ngầu mang lại giá trị cho công ty hay giải quyết vấn đề của bản thân. Việc viết blog mang lại 1 số lợi ích như sau:

- Quảng cáo brand cho công ty
- Quảng cáo brand cho bản thân
- Và sau này đi phỏng vấn không nhớ mở blog ra xem lại (vì vậy nên viết cực kì chi tiết)
- Và giúp ít cho công việc sau này, đặt biệt là vào các công ty to thường làm cái gì nó cũng bắt mình document, nên kĩ năng viết rất quan trọng.
- Và trong quá trình viết nó cũng giúp mình cô đọng và hiểu rõ vấn đề hơn nữa.

Hôm trước mình đọc bài viết của bạn mình nói về <a href="https://www.facebook.com/groups/1177470863076165/permalink/1194701151353136" target="_blank">The Importance of Storytelling in your career</a>, anh ấy đề cập việc mỗi người nên phát triển kĩ năng “Story telling", vâng mình thấy nó hoàn toàn chính xác, mỗi người đều có 1 câu chuyện, hãy làm câu chuyện của bạn thật hấp dẫn và chân thật bằng cách trải nghiệm thật nhiều.

## KẾT QUẢ

- https://www.sitepoint.com/the-benefits-of-inheritance-via-extend-in-sass/
- https://medium.com/@dzungnguyen179 Có bài viết mình đã được retweet từ CEO của Vercel

# Marketing

Sau khi đã biết liên kết được OSS và blog, bước kế tiếp mình muốn người khác biết về cái mình làm. Có 1 cái mình thấy rất may mắn là lúc đi làm mình cũng hay quan sát và tò mò cách mà các bộ phận marketing họ làm content thế nào và chiến lược share social như thế nào và khi nào nên share. Thế là mình cũng bắt chước làm theo, cụ thể như sau:
- Soạn nội dung ngắn gọn cho cái mình muốn chia sẻ ví dụ dự án nào là cái gì, vấn đề nó muốn giải quyết là gì và nó hơn gì những cái có sẵn trên thị trường.
- Mình đưa cái link bài viết blog của mình vào luôn để tăng traffic cho bài viết
- Xem thử giờ nào mọi người online nhiều nhất và bắt đầu chia sẻ
- Đây là danh sách social network mà mình chia sẻ Reddit, Facebook, Twitter, LinkedIn, Story và mình luôn để hashtag để bài đăng của mình nó đi xa và rộng hơn chút.

Cứ ra 1 version hay mình đạt được 1 milestone nào là mình sẽ soạn nội dung mà đăng như vậy. Lợi ích là nó sẽ boost users, ví dụ 10 người vào xem mà có 1 người install thì mình cũng thấy là thành công.

**Note:** Tất cả những gì mà mình làm, mình đều tìm cách để đo đạc số liệu xem cái mình làm có hiệu quả hay không.

## Kết

Những gì mình nêu trên là cả 1 quá trình, lúc đầu mình thử thì nó hơi nhọc nhằn do tiếng Anh khá lởm, nhưng mình không từ bỏ vì mình biết đó là cách duy nhất để tăng cơ hội của mình khi apply vào Top Tech và nó không hề dễ 1 chút nào, để có profile tốt bạn phải trải nghiệm và làm nhiều để có nhiều kinh nghiệm. Và làm gì thì cũng nên đặt toàn tâm toàn ý vào cái mình làm, nó sẽ mang lại nhiều kết quả không ngờ tới.

> Gửi cho các bạn đang trên hành trình xây dựng sự nghiệp, mình hi vọng qua bài viết này các bạn sẽ tìm được 1 điểm khởi đầu để xây dựng những thứ cool ngầu và hãy làm câu chuyện của bạn hấp dẫn hơn.

Đừng nản, ai rồi cũng sẽ trải qua mấy cảm giác này, nếu bạn đứng dậy được thì bạn sẽ mạnh hơn.

<blockquote class="reader-text-block__quote">
  What doesn't kill you makes you stronger 🤜
</blockquote>