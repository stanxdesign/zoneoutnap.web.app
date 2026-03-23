# 享發呆官方網站

享發呆是台中南區的紓筋、采耳與頭療服務品牌，本專案為其對外公開的官方網站原始碼。網站以單頁靜態頁面為主，重點在於快速呈現品牌形象、服務項目、營業資訊、交通方式與預約入口，讓使用者可直接透過 LINE 官方帳號完成聯繫與預約。

## 專案背景

本網站的定位不是大型內容平台，而是品牌門面與轉單入口，因此設計上採用低維護成本、載入快速、可直接部署的靜態架構。現階段內容主要聚焦在以下幾類資訊：

1. 品牌主視覺與介紹文案
2. 服務方案與活動資訊
3. 店址、營業時間與聯絡方式
4. 導流至 LINE 官方帳號與 Instagram

這樣的架構適合小型商家或個人品牌，能在不引入複雜後端系統的前提下，維持穩定公開頁面與基本行銷追蹤能力。

## 技術概要

本專案目前是純靜態網站，沒有前端框架、打包流程或後端服務。

### 核心技術

1. HTML5：頁面結構與內容編排
2. CSS：內嵌於首頁的樣式設定與版面控制
3. Firebase Hosting：負責網站靜態內容部署與對外提供
4. Google Analytics 4：透過 gtag.js 進行流量追蹤
5. Google Tag Manager：集中管理追蹤碼
6. JSON-LD 結構化資料：提供 LocalBusiness 基本資訊給搜尋引擎
7. Web App Manifest：提供網站圖示與基礎 PWA 顯示資訊

### 架構特性

1. 單頁式網站，主要內容集中於 public/index.html
2. 所有畫面樣式直接寫在首頁的 style 區塊中
3. 圖片資源混合使用本地檔案與外部 CDN 連結
4. 以外部連結完成預約、導航與社群導流
5. 沒有資料庫、後端 API、會員系統或 CMS

## 專案結構

```text
.
├── firebase.json
├── README.md
└── public
	 ├── 404.html
	 ├── index.html
	 ├── robots.txt
	 ├── sitemap.xml
	 ├── site.webmanifest
	 ├── logo.png
	 ├── favicon.ico
	 ├── favicon-16x16.png
	 ├── favicon-32x32.png
	 ├── apple-touch-icon.png
	 ├── android-chrome-192x192.png
	 └── android-chrome-512x512.png
```

### 重要檔案說明

1. public/index.html
	網站首頁，也是目前主要內容與樣式的唯一核心檔案。

2. public/404.html
	Firebase Hosting 找不到頁面時顯示的錯誤頁。

3. public/site.webmanifest
	定義網站在行動裝置加入主畫面時使用的圖示與顯示模式。

4. public/robots.txt
	指定爬蟲允許存取範圍，並附上 sitemap 位置給搜尋引擎使用。

5. public/sitemap.xml
	列出站點所有公開 URL，讓搜尋引擎更有效率地掌握索引範圍。

6. firebase.json
	Firebase Hosting 設定檔，指定公開目錄為 public，並設定忽略檔案規則。

## 首頁內容組成

首頁目前包含以下模組：

1. 品牌標誌與主標題
2. 品牌介紹文案
3. 營業時間與聯絡電話
4. 服務方案橫向捲動展示區
5. 店址與交通方式說明
6. Google Maps 導航連結
7. 快閃活動與社群追蹤引導
8. 合作夥伴資訊
9. 底部固定導覽列，提供 Instagram 與 LINE 預約入口

## 第三方服務與外部依賴

目前首頁整合了多個外部服務，維護時應一併注意：

1. Google Analytics
	使用 GA4 追蹤碼 G-DX8VYT6MJC。

2. Google Tag Manager
	使用容器 ID GTM-KTRS5654。

3. justfont
	用於載入 tearsfont 字型與英文字型設定。

4. LINE 官方帳號
	作為主要預約與時段查詢入口。

5. Instagram
	作為品牌活動與社群更新導流。

6. Google Maps
	作為店址導航入口。

7. 外部圖片來源
	部分服務圖來自 LINE CDN，若來源失效，首頁顯示也會受影響。

## 部署方式

網站目前設計為 Firebase Hosting 靜態部署。

### Firebase Hosting 設定

firebase.json 內容如下：

```json
{
  "hosting": {
	 "public": "public",
	 "ignore": [
		"firebase.json",
		"**/.*",
		"**/node_modules/**"
	 ]
  }
}
```

這表示實際對外提供的內容只會來自 public 目錄。

### 一般更新流程

1. 修改 public/index.html 或 public 內相關靜態資源
2. 在本機瀏覽器檢查畫面、連結與文案是否正確
3. 使用 Firebase Hosting 部署最新內容

若本機已安裝 Firebase CLI，常見指令如下：

```bash
firebase deploy
```

若只部署 Hosting：

```bash
firebase deploy --only hosting
```

## 本機維護建議

由於目前沒有建置工具，維護上建議遵守以下原則：

1. 內容修改以 public/index.html 為主，避免散落多處造成版本不一致
2. 若要更換圖片，優先改用專案內部靜態檔案，降低外部 CDN 失效風險
3. 若要新增追蹤工具，先確認是否可由 GTM 管理，避免首頁重複嵌入過多腳本
4. 若頁面內容持續增加，建議逐步拆分 CSS 與結構，降低單檔維護成本
5. 若未來有頻繁更新需求，可考慮導入 CMS 或較完整的前端架構

## SEO 與可見性設計

目前網站已具備基本搜尋引擎與社群分享資訊：

1. canonical URL
2. meta description（已含品牌、服務與預約關鍵詞）
3. robots meta（index/follow）
4. Open Graph title、description、url、image、site_name、locale
5. Twitter Card
6. HealthAndBeautyBusiness 結構化資料（@graph）
7. FAQPage 結構化資料
8. OfferCatalog 服務清單結構化資料
9. 行動裝置 viewport、theme-color 設定
10. 品牌 favicon 與完整 web manifest
11. robots.txt（允許爬蟲 + sitemap 位置）
12. sitemap.xml

若未來要進一步強化，可補充：

1. 專用社群分享圖（建議 1200 x 630 PNG，放於站內避免外部 CDN 失效）
2. 商家座標（geo.latitude / geo.longitude）加入結構化資料
3. 每次服務項目或價格調整時同步維護 OfferCatalog 與 FAQ 內容

## 已知維運特性

1. 網站為純前端靜態頁，更新速度快，但所有內容都需手動修改 HTML
2. 首頁 CSS 與 HTML 耦合度高，小改動通常很直接，但大改版成本較高
3. 部分內容直接依賴外部平台連結，需定期檢查是否有效
4. 404 頁與首頁風格目前並未完全一致，可視需求後續調整

## 適合的後續擴充方向

1. 將 CSS 從 index.html 拆出為獨立樣式檔
2. 將文案、服務項目與聯絡資訊抽離成可維護資料區塊
3. 新增更完整的部署與版本管理流程
4. 補上內容更新紀錄，方便交接與追蹤

## 維護結論

這個專案的核心價值在於簡單、穩定與易部署。只要持續維護 public/index.html 的內容正確性、外部連結可用性，以及 Firebase Hosting 的部署流程穩定，便能支撐目前品牌官網需求。
