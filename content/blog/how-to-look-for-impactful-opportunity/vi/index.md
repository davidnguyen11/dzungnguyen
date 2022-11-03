---
title: How to look for impactful opportunity
date: "2022-10-30"
tags: techstory, careerstory
---

<div style="text-align: center;">
  <img src="https://user-images.githubusercontent.com/6290720/199662418-a0bb4882-f43b-43b7-8ee5-49fa051fe318.jpg" width="100%" />
  <i style="font-size: 15px">System design</i>
<div>

Mọi thứ bắt đầu từ năm 2015, khi mình thất nghiệp ba tháng và chẳng còn một đồng dính túi. Lúc đó mình khao khát được đi làm và cống hiến. Cũng may sau ba tháng cũng có việc làm, và mình đã tự hứa là trong mỗi project, mình sẽ làm hết sức mình. Và trong những năm tháng đó nó vô tình tạo cho mình một thói quen mình thấy rất hữu ích là “quan sát" xung quanh xem có ai gặp vấn đề không, sau đó phân tích xem nó có thật sự là vấn đề và suy nghĩ solution để solve. Và đây là câu chuyện làm mình rất tự hào mỗi lần nhắc tới trong sự nghiệp:

<blockquote class="reader-text-block__quote">
  Build the SEO dashboard for ops team which helps companies gain more than 100k unique visits.
  <br />
  (bài viết này là mình ấp ủ từ năm 2017)
</blockquote>

# Overview

Lúc mới vào công ty sau thời gian thất nghiệp, mình chủ yếu phát triển web product. Trong quá trình development mình dành thời gian để hiểu công ty và các team vận hành, đồng thời hiểu thêm về codebase. Mình có để ý có một vấn đề mà mình thấy rất khó chịu cho team engineer và nó làm chậm quá trình development. Cụ thể như sau, team engineer phát triển sản phẩm sẽ làm việc rất chặt chẽ với team SEO để đảm bảo sản phẩm của công ty có content và ranking tốt trên search engine. Team SEO sử dụng JSON file để soạn content, và file size của nó rất lớn. Sau khi họ làm xong họ sẽ đưa cho team engineer để integrate vào product và những task liên quan về SEO là top priority nên engineer phải ngừng việc họ đang làm để integrate JSON file vào các web product. Task nghe có vẻ đơn giản nhưng mình có để ý có một số vấn đề thường xuyên xảy ra như sau:

- Có một số thay đổi cấu trúc trên JSON file từ SEO team buộc engineer phải refactor code để adapt với những thay đổi đó. Vô tình effort engineer bỏ ra cao hơn so với estimation ban đầu.
- JSON file size theo thời gian nó lớn dần, vì nó chứa hết nội dung SEO của tất cả các page. Điều này là rất vô lý, tại sao tôi vào page A mà trang web load tất cả SEO của các trang khác. Và nó vô tình trở thành một trong những yếu tố ảnh hưởng đến trải nghiệm của người dùng vì web performance bị ảnh hưởng.
- Vì team SEO họ không có tech background nên việc nội dung của SEO bị duplicate hay không họ hoàn toàn không biết, nguyên nhân có thể là file JSON quá to nên không thể nào check được. Google liên tục báo về công ty là nội dung SEO bị duplicate khá nhiều và team engineer phải nhảy vào help. Việc này tốn thời gian rất nhiều.
- Do là SEO team thao tác trên JSON file và file size quá to nên việc sai syntax là không tránh khỏi, vấn đề này thường chỉ được phát hiện khi engineer integrate vào product. Mỗi lần như vậy, việc navigate tới cái chỗ bị lỗi là rất khó khăn vì cái thanh scroll của cái JSON file nó nhỏ xíu, kéo một li đi một dặm.

Từ những vấn đề như vậy mình có thể thấy, cả hai team làm việc không hiệu quả và tốn nhiều thời gian vào những cái không cần thiết ảnh hưởng đến performance và làm công ty grow chậm.

Mình thấy vấn đề đó vào năm 2016 nhưng có vẻ trong công ty không ai quan tâm trừ một anh Head of Biz. Lúc đó mình cũng đang bận một số dự án nên cũng tạm gác lại để sau.

# Getting started

Đến năm 2017, lúc đó là còn hơn một tháng nữa là Tết và các vấn đề nêu trên nó vẫn cứ thế mà xảy ra thường xuyên hơn. Vì các team đều chạy KPI cho cuối năm, SEO file cứ thế mà update ngày càng nhiều và integration task của engineer ngày càng nhiều. Công ty còn thuê một cậu engineer với hợp đồng ngắn hạn để hỗ trợ các engineer làm integration JSON file vào product. Vô tình cậu ấy bị cuốn vào vòng xoáy refactor codebase. Thêm vào đó, là cậu ấy không hiểu codebase nên lại phải có một anh engineer khác hiểu rõ codebase ngồi pair cùng cậu ấy để làm (double work). Lúc đó mình đã raise vấn đề này lên phía product nhưng mọi người không nghĩ đó là ưu tiên cao nên cũng ignore vì còn nhiều features để develop.

Mình cũng làm liều nói chuyện riêng với Head of Biz về vấn đề trên. Trước khi mình nói chuyện mình cũng đã chuẩn bị sẵn solution. Anh ấy cũng nói đây là vấn đề từ lâu rồi nhưng không ai có time solve cả vì không có đủ người, và ưu tiên hiện tại vẫn là improve sản phẩm. Sau đó, mình có nói với anh ấy là nếu công ty cho mình time thì mình sẽ take project này. Tiếp đó là có meeting nơi các sếp thảo luận xem là nên thế nào, một phần là sếp không muốn involve các bạn engineer khác. Thế là mình nói "Để em làm một mình cũng không sao". Well, the rest is history.

# Development

Sau khi nói xong, tim mình như rớt ra vì cái gì cũng chưa có cả mà dám mạnh miệng. Nhưng sau đó mình tập trung, nhớ lại bài hồi học ở Đại Học về quy trình phát triển phần mềm.

1. Collect requirement: mình sẽ đi nói chuyện với team SEO để xem pain point là gì, sau đó bỏ vào documentation.
2. Phân tích requirement để xem có bao nhiêu feature, sau đó thì tạo JIRA ticket.
3. Rồi dựa vào requirement để thiết kế database. Mình có cơ hội vẽ lại ER diagram (cái mà mình làm rất nhiều hồi đại học), rồi bắt đầu suy ra các table và fields cần thiết.
4. Rồi bắt đầu set up development back end và cả front end

Dự án không có designer, nên mình tự thiết kế UI rồi tiến hành develop, chọn tech stack cho back end và front end. Do mình biết nhiều nhất là JS nên tech stack của mình như sau:

1. Front End: NextJS, React, Material UI
2. Back End: NodeJS, GraphQL
3. Deployment: K8s (collaborate với team DevOps).

Sau 2 tuần code ngày đêm và cả cuối tuần. Cuối cùng cũng hoàn thành được các chức năng cơ bản của cái dashboard. SEO team có thể thêm xóa sửa nội dung trên cái dashboard đó. Nhưng tôi gặp một số vấn đề như sau:

## MIGRATE EXISTING JSON FILE TO DB

Sau khi develop xong thì anh SEO team manager request feature import data có sẵn trên JSON file vào database vì nó có cả nghìn keywords, không thể bắt team ngồi nhập line by line được. Request makes sense, và bắt đầu viết một vài scripts bằng JS để import data database. Sau khi viết xong mình collaborate với team DevOps để access vào server để thực thi các scripts trên để đưa data vào staging và production database. Problem solved!

## TRAINING SEO TEAM HOW TO USE THE DASHBOARD

Sau khi dashboard có data, việc tiếp theo là training cho team để sử dụng. Để đảm bảo mọi thứ chạy smoothly, thì mình chuẩn bị slide, documentation hướng dẫn sử dụng và làm kịch bản training. Sau đó mọi người sử dụng, và mình nhận về một đống bugs và chu trình fix bugs bắt đầu, cụ thể mình dành 1 tuần để fix các major bugs để đảm bảo release xài được.

## PUBLIC API & UNIT TEST

Một vấn đề đau đầu khác là, mình không thể sử dụng cùng một repo backend dùng cho dashboard để phát triển public API cho product chính vì mình phải lo về vấn đề Authentication và Authorization vốn đã rất phức tạp. Lúc đó mình nghĩ ra một cách đơn giản và hiệu quả là tạo một repo khác để develop public API, deploy hoàn toàn ở một service khác.

May mắn thay public API nó không cần nhiều endpoints (mình nhớ đâu đó tầm 3 hay 4 endpoints). Sau khi develop xong, mình viết cả unit test và set up CI/CD để đảm bảo quality cho API.

## INTEGRATE SEO API VỚI PRODUCT

Vấn đề cuối cùng là integrate Public API vào sản phẩm web chính. Vì mọi người ai cũng bận, nên mình làm luôn công đoạn integration API vào product chính. Sau đó mình deploy lên `staging` để team SEO và QA phụ mình test.

## GET THAT SHIT OUT

Sau năm tuần (trước Tết vài ngày), thì cuối cùng mình cũng release. Plan là sẽ test product sử dụng SEO API trong dịp Tết vì lượng traffic ở khoảng thời gian này là thấp, nếu có chuyện gì xảy ra thì cũng rollback được.

# Kết quả

Mình ăn Tết mà tâm lo lắng về kết quả. Sau Tết được vài ngày thì mình trace bên team Biz xem metrics thế nào. Tôi đã rất kinh ngạc với kết quả, lượng traffic của công ty tăng 100k. Nhưng vẫn keep calm, mình muốn confirm với team marketing xem họ có đang chạy campaign để attract traffic không. Sau khi mình confirm với họ xong thì lúc này mình mới thật sự vui. Dự án này mang lại một số benefits:

- Nó giúp cho product stable trên search ranking vị trí 1 và 2 trên Google Search Engine với các target keyword.
- Team SEO không còn phải phụ thuộc vào team Engineer, tự sáng tạo trong các nội dung và làm việc, hiệu quả hơn.
- Team engineer có thời gian tập trung vào phát triển feature giúp công ty grow nhanh hơn.

# Extra benefit

Sau khi chạy dashboard một thời gian, thì mình vẫn thường xuyên fix bugs và lúc này mình gặp 2 vấn đề khá to ảnh hưởng đến dashboard:

1. NextJS ở thời điểm bấy giờ chỉ support 2 environment chính là development (localhost) và production (chắc tới giờ vẫn vậy). Nếu công ty mà có thêm môi trường staging thì nó sẽ chạy với môi trường development và cực kỳ chậm. Mình đã solve nó ở [bài này](https://medium.com/@dzungnguyen179/nextjs-multiple-environment-builds-e8b2ccb11c04).

2. Mỗi lần mình release dashboard version mới là mình phải nói chuyện với DevOps clear cache cho mình đâu đó trên tầng infra (cực kì annoying). Mình nghĩ ra một cách là đẩy hết các static resources theo version lên amazon s3 khi chạy CI/CD và ở bộ dashboard chỉ cần sử dụng link từ s3. Sau đó mình proposed solution này để làm flow deploy chính cho các team khác.

# Kết luận

Mình học được rất nhiều từ project này từ technical lẫn cả việc deal with people. Và ở những project sau mình cứ thế mà chủ động, quan sát và curious, nếu có ý tưởng mình thường brainstorm với đồng nghiệp để xem nó có thật sự là vấn đề và đáng để solve không.

Mình nghĩ là không phải product nhiều features là có thể giúp công ty grow được, nó còn bao gồm team operation đằng sau và các department khác. Nên nếu mình có thể tackle được problem và giúp operation side hay các department khác tăng năng suất thì sẽ đồng thời đẩy công ty đi lên.

Nếu bây giờ bắt mình develop cái đó chắc mình sẽ có giải pháp khác tốt hơn, xài sẵn mấy cái web framework nào build sẵn CRUD để có thể test được MVP nhanh.
