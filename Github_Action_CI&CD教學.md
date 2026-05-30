## 先建立 Azure 資源

1. 先建立 Azure 資源

- 建立 App Service（API）與 Static Web App（前端），並確認 API 可用網址與前端可用網址。

2. 設定 GitHub Actions 所需的 Secrets 與 Variables

- API 與 Web workflow 主要使用這兩份檔案：
003-deploy-api.yml
002-deploy-web.yml

3. API 部署前必要設定
AZURE_CREDENTIALS 要放在 GitHub Secrets，且內容需為完整 JSON（clientId、clientSecret、subscriptionId、tenantId）。

{
  "clientId": "完整的應用程式(用戶端)識別碼 GUID，位置:Entra ID->應用程式註冊->應用程式 (用戶端) 識別碼",
  "clientSecret": "完整的 client secret 值"，位置:Entra ID->應用程式註冊-> 所有應用程式 中的項目->憑證及祕密,
  "subscriptionId": "完整的訂用帳戶識別碼 GUID，位置:訂用帳戶->訂用帳戶 ID",
  "tenantId": "完整的租用戶識別碼 GUID，位置:Entra ID->租用戶識別碼"
}

4. Web 部署前必要設定
AZURE_STATIC_WEB_APPS_API_TOKEN 要用 Static Web App 的 deployment token，不可用 App Registration 的 secret 代替。
VITE_API_URL 要填 API 網址（不加 /api）。

5. 設定 CORS
在 API App Service 的 Application settings 設定 AllowedOrigins 陣列來源，程式讀取位置在：
 - AllowedOrigins__0 = https://delightful-grass-08102f900.7.azurestaticapps.net
 - AllowedOrigins__1 = http://localhost:5173


6.重新啟動與重部署
App Service 設定更新後要重啟 API。
VITE_API_URL 是 build-time 參數，改完後要重跑 Web workflow。

### 使用方式

1. API CI/CD
用 GitHub Actions 部署到 Azure App Service，觸發來自 main push 或手動選環境。
參考：003-deploy-api.yml

2. Web CI/CD
用 GitHub Actions build React，再用 Azure/static-web-apps-deploy 上傳到 SWA。
參考：002-deploy-web.yml




