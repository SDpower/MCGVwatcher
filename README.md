# MCGVwatcher

## 構想
說明一下這用途用MC專業版都知道有GlobalVariable.dll可以在程式碼會傳值。
那這個程式目的就是從外部取ＭＣ某個直，如果你有ＭＣ呼叫 GVSetNamedString("目標名稱",“字串內容”);
那這程式監控該值變化幫你發送到某網址，目前是設定給ifttt line通知用。
有何好處？一般來說寫檔案就佔用IO 程式多個開啟就會搶IO 那MC寫檔案你又去開檔案讀取很容易打架並且當機。
那這個是直接呼叫GlobalVariable.dll 所以也是跟他要資料速度極快沒有IO搶佔問題。
那你會問我為何不寫Dll呼叫即可？因為寫過遇到很多問題且這樣並沒有辦法簡化callback速度與相依性還是在還是有可能造成ＭＣ檔機。
程式很小22K c# .net framework 4寫的win10以直接執行。
我事業外做這個的寫過太多程式以前是從一開始群益開放API到後續百家綻放。
目前待業中所以就全力開發。

## 目前程式優點
* 程式沒有依賴性ＭＣ獲此程式當機都沒影任何干擾。
* 多線程速度極快各有各的thread。
    * 輪詢資料
    * 更新log
    * 發送的URL 
* C#速度夠快開發也不會太難。
* 所有畫面設定值都是異動後存擋ini。


## 檔案清單

下載位置： https://drive.google.com/file/d/1xl-y5jR-2kK2GdOWm_LjvV1voIZzdfjz/view?usp=sharing
![](https://i.imgur.com/47MTMD0.png)

* MCGVwatcher.exe 主程式
    * 檔案檢核碼 SHA256:844c2e774721c5b91c6ae378e9948873ce8611de142a515d23a2a966d85c3ed9
* MCGVwatcher.ini 設定擋自動產生
* Newtonsoft.Json.dll 微軟套件json不知道為何這樣大有其他建議歡迎提供
    * 檔案檢核碼 SHA256:b624949df8b0e3a6153fdfb730a7c6f4990b6592ee0d922e1788433d276610f3

### 使用方式

主畫面
![](https://i.imgur.com/85ad0qb.png)

設定監控的字串名稱 啟動後不可修改
![](https://i.imgur.com/8F4C7mj.png)
設定要發送的網址，目前只會發送POST的JSON格式。
~~~
{"Value1":"內容"}
~~~
點選啟用收到資料後會直接發送。（建議先不要勾選也要避免大量發送才不會被Band掉
![](https://i.imgur.com/6oXHH5T.png)

收到異動資料區塊。
![](https://i.imgur.com/VmWyhwS.png)

發送的紀錄區塊。
![](https://i.imgur.com/q9k30kK.png)

設定輪詢的間隔。
![](https://i.imgur.com/kFzbeKe.png)

### MC 範例

![](https://i.imgur.com/BfZFUjz.png)

~~~
Inputs:
	name("name"),
	event("GVwatcher"),
	BuyName("Buy"),
	SellName ("Sell"),
	Empty("Empty");

variables:Payload("");

once begin
    ClearPrintLog;
    Payload=DoubleQuote + "Test" + DoubleQuote+":" + BuyName +":" + SellName  +":" + Empty +":" + DateTimeToString_Ms(DateTime) +":" + DateTimeToString_Ms(ComputerDateTime);
	//print(Payload);
	GVSetNamedString(event,Payload);
end;
~~~

使用重點：
GVSetNamedString 這個就是設定一個字串，並附於他字串值。你可以看範例一樣附加時間
DateTimeToString_Ms(DateTime) 這樣可以更明確知道MC發出的時間。

依照我這下圖就是ＭＣ發出09:26:24.503 監控程式收到 09:26:24.513 總共只耗費10ms. 
![](https://i.imgur.com/XaQ9AMB.png)

你有多組策略就多開視窗設定不同監看值：
自己自由發揮
* PJ0001
* PJ0002

### IFTTT Line

這裡選用IFTTT是因誤蕤果自行串接Line通知還要一直更新token 實在很麻煩。
這裡就不多做說明請上網搜尋：https://www.google.com/search?client=firefox-b-d&q=line+ifttt
最後設定可以改成這樣
![](https://i.imgur.com/xS6Gclg.png)
