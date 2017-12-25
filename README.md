## Day 7 - 與 k8s 溝通: Dashboard

### 本日共賞

* Dashboard
* 部署物件

### 希望你知道

* [安裝 k8s](https://ithelp.ithome.com.tw/articles/10192748)
* [運行 minikube](https://ithelp.ithome.com.tw/articles/10193237)

昨天我們利用 API 的方式與 k8s 溝通，今天我們來看一看另外一種 k8s 視覺化的溝通介面 `Dashboard`。

你可以透過兩種方式開啟 `Dashboard`

1. minikube dashboard
2. proxy

**1. minikube dashboard**

```bash
$ minikube dashboard
```

鍵入上面的指令，minikube 會直接開啟瀏覽器執行 `Dashboard`

**2. proxy**

```bash
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

透過 `kubectl proxy` 開啟代理，接著打開瀏覽器輸入 `http://127.0.0.1:8001/ui`

> 如果使用 minikube 這兩個的效果是一樣的，但是如果是使用雲端平台，就要使用 proxy 來開啟 Dashboard

Dashboard 的畫面如下

![https://ithelp.ithome.com.tw/upload/images/20171224/20107062KEliZmak28.png](https://ithelp.ithome.com.tw/upload/images/20171224/20107062KEliZmak28.png)

如上圖，你可以點選左邊的列表選擇想要查看的物件，而內容會顯示在右邊的位置。你可以試著點選操作看看。

> 由於我們還沒有針對 k8s 的物件做說明，所以你可能會不清楚這些物件的意義。別擔心之後我們都會提到的，這裡僅是讓你知道可以使用 Dashboard 操作。


#### 部署物件

![](https://ithelp.ithome.com.tw/upload/images/20171224/20107062w2FjiaisQU.png)

在右上角，你應該可以看到一個 `+ 新建` 的選項，我們可以透過新建來部署物件到 k8s 內，下面我們就透過 Dashboard 部署一個 Deployment 物件。

![](https://ithelp.ithome.com.tw/upload/images/20171224/20107062q781qZEUia.png)

這裡有兩種方式可以部署物件到 k8s 內

1. `填寫應用程式詳細資料`：直接填寫相關設定
2. `上傳 YAML 或 JSON 檔案`：將設定撰寫成 yaml 或 json 檔後在上傳

> k8s 使用 yaml 或 json 檔案來描述需要部署的物件，由於我們還沒有說明 yaml 檔，所以先使用 `填寫應用程式詳細資料` 的方式部署物件。

先撇開進階選項，部署一個 Deployment 物件到 k8s 至少需要填入 `應用程式名稱` 與 `容器映像檔`。以上圖為例，我們指定應用程式名稱為 `from-dashboard` 以及使用 `nginx` 映像檔，都填寫好了以後，點選下面的 `部署` 按鍵。

> `nginx` 映像檔即 Docker 映像檔名稱
>
> 進階選項內容等到更深入了解 k8s 後你就會明白是什麼東西了，這裡可以先忽略

接著點選左方的 Deployment，你會看到

![](https://ithelp.ithome.com.tw/upload/images/20171224/20107062Kv0AQleLWW.png)

1. 點選 Deployment，查看剛剛部署的物件
2. 可以在右邊看到剛剛指定的應用程式名稱 `from-dashboard`
3. `0/1` 表示我們希望建立一個 Pod 但是目前還沒建立成功
4. 可以透過介面查看描述 from-dashboard 的完整 yaml 內容

> 如果是第一次建立，k8s 需要下載 `nginx` 這個映像檔，所以可能會花費比較長的時間

大概幾秒鐘後，刷新頁面你會看到：

![](https://ithelp.ithome.com.tw/upload/images/20171224/20107062QpjfTypFpf.png)

由於部署 Deployment 物件會自動生成指定數量的 Pod，所以你也可以在左邊點選 Pods 查看與 from-dashbash 相關的 Pod。

> 昨天跟今天我們講了一堆不知道是什麼東西的 `Pod` 跟 `Deployment`，如果你是正常人的話應該差不多已經一頭霧水了！不過如果你還能看到這段文字，表示是可造之材的正常人！你可以暫時把 `Pod` 想像是安裝在一台主機上運作的應用程式一樣，例如用 Java 或 PHP 寫成的網頁 (或 Python, Nodejs, WordPress, ... 管他什麼寫的都行)，透過 `Deployment` ，k8s 能知道該怎麼 "操作" `Pod`，例如把 `Pod` 變成兩個、四個、八個, ...，那麼有多個 `Pod` 代表什麼意思呢？表示你的系統能夠接受更多使用者的連線 (Scalability)，表示你不用擔心如果有某些 `Pod` 壞掉怎麼辦 (Availability)。這樣是不是好像有點什麼感覺了呢？

![](https://ithelp.ithome.com.tw/upload/images/20171224/20107062u42cfwPFO6.png)

> 你可以試試看在新建部署的時候指定 Pod 數量，看看這邊有什麼變化

如果你點選某個物件 (例如 `from-dashboard-755cc66964-vcf9t`)，則可以觀看與該物件相關的內容包含狀態、事件等等。

雖然 Dashboard 提供了 UI 介面方便存取 k8s，但是就個人的經驗來說，還是直接使用 `kubectl` 這個工具存取 k8s 方便一些。明天我們就來看看如何使用 `kubectl` 吧！

本文同步發表於 [https://jlptf.github.io/ironman2018-day7/](https://jlptf.github.io/ironman2018-day7/)