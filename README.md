# HW3 Report

## Generate images with GANPaint
![](https://i.imgur.com/6YaPzi8.gif)
### analysis
GANPaint並不是隨意生成物件在圖片上的，而是會根據之前學習到的特徵來進行生成。像是它會學習到門都是貼在地上的，不會有浮在牆壁上的門，因此在生成門時它會讓門貼著地板。在上面的GIF圖，我們一開始想在空曠處畫上一棵樹，結果失敗了，接著我們卻成功的在圖片邊框附近畫上了樹，我們推測這是因為它的dataset裡有樹木的圖片裡面的樹都是在邊框附近的，所以直接在空曠的圖片中間畫上樹對它來說是不合理的。
## Dissect GAN model
### GAN Dissection
Introduction

自從GAN出現以後，越來越多人開始研究GAN，也出現了各種應用。我們明白GAN的原理也會使用GAN，但是，我們對它內部的運作還有實際上影響它生成圖片的機制其實並不了解，而這篇paper的作者察覺到了這些事，所以他們設計了這個方法來研究GAN裡面的每個unit分別都影響著圖片生成的哪些部分。這個方法不只使我們更了解GAN的運作與組成，也對改善與設計GAN有了更好的方式。

我們dissect了github上的範例model (three layers of the LSUN living room progressive GAN trained by Karras)。雖然他的GAN editing server我們無法順利啟動，但是可以從生成的.json檔可以看到，每個unit代表著甚麼，哪個物件又代表著那些unit。

#### Example
![](https://i.imgur.com/AV8QW2n.jpg)

layer7的unit61裡面的資訊，可以看到它是屬於sofa這個label的

![](https://i.imgur.com/FlSy559.jpg)

在layer7中sofa這個物件對應到的units

## Compare with other methods
### Inpainting
Introduction

Globally and Locally Consistent Image Completion

這篇paper用了gan的概念來達到image completion,其中比較特別的部分是他用了global和local的generator來負責不同的部分，global負責檢查這塊區域與在整張的一致性，而local負責檢查他與鄰居是否夠一致，而global generator與local generator所產生出來的會被discriminator檢驗。這樣的用生成的方法與傳統用patch-base的方法比起來，會更加的自然，因為它可以自己生產出合理的區塊

以下為其架構圖：
![](https://i.imgur.com/RskBbXY.png)

inference

在inference之前，必須先將要移除的object先mask起來。
並且提供原圖以及mask圖。
以下mask圖為我們跑R-CNN所得到的。

圖1：

![](https://i.imgur.com/BNiYfKW.png)

mask:

![](https://i.imgur.com/Agl2Rho.png)

result:

![](https://i.imgur.com/Dtuieih.png)

圖2：

![](https://i.imgur.com/7Tk520X.png)

mask:

![](https://i.imgur.com/pbQ48xS.png)

result:

![](https://i.imgur.com/CDH3unH.png)

可以看得出來圖一右邊的長頸鹿幾乎快消失了，但是左邊的兩隻馬就有點明顯。推測很有可能是被global的generator影響，所以沒辦法整張都是綠色的草原，一定會有點馬的顏色在裡面，所以才產生這樣的結果。
而圖二的結果則是相當差勁，合理推測是因為原圖的背景以及顏色均較複雜且多元，所以整個network在生成時，被附近複雜的背景所影響，而導致整體結果變差。

#### comparison
移除物件效果
+ GANDissection
對像是雲或是草地這種大範圍紋理類的物件移除效果非常好。在移除物品類的物件有時會顯得不自然，例如樹木在移除時有可能會產生在空中有一小搓樹枝的不自然圖片。

![](https://i.imgur.com/7hngZDt.png) ![](https://i.imgur.com/KYE6otP.jpg)


+ Inpainting
容易有移除不乾淨的狀況產生，處理過的圖片會留有殘影。

![](https://i.imgur.com/D7CsNtR.png) ![](https://i.imgur.com/oPEmZpJ.png)








