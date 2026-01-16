《CI/CD ：本機 → GitHub → Vercel》學習筆記

本機 → 版本控制 → GitHub → 自動部署 → Vercel → 回滾 → 再發布


對應到 CI/CD：

- **CI** = 自動從 GitHub 拉程式  
- **CD** = 自動部署到 Vercel

---

## 🧾 1. 帳號（Services Account Setup）

需要 2 個服務帳號：

| 服務 | 用途 |
|---|---|
| GitHub | 版本控制 + CI Source |
| Vercel | 自動部署 + CDN + HTTPS |

📌 **避雷：**

- GitHub 用個人帳號即可
- Vercel 建議用 **GitHub 授權登入**（自動串 OAuth）

---

## 🖥 2. Windows 本機環境準備

### 📍 2.1 安裝 Git

PowerShell：

```ps1
winget install --id Git.Git -e
```


📌 避雷：

裝完要 重開 PowerShell 讓 PATH 生效

Git Bash 會一起裝（可忽略或使用）



### 📍 2.2 設定 Git Identity
PowerShell：
```ps1
git config --global user.name "你的名字"
git config --global user.email "你的GitHub email"
```

📌 避雷：

email 用 GitHub 的，不然 commit 不會綁帳號



### 📍 2.3 建本機資料夾 + 初版 v1
PowerShell：
```ps1
mkdir vercel-cicd-demo
cd vercel-cicd-demo
```


建立 index.html：
PowerShell：
```html
<h1>CI/CD Demo v1</h1>
```  



## 🔧 3. 本機 → Git（版本控制本體）

git初始化：

PowerShell：
```ps1
git init
git add .
git commit -m "feat: initial version v1"
```

📌 避雷：

沒 commit = 沒版本

沒 commit = 不能 push


## 🗂 4. GitHub Repository

GitHub 操作：

New Repo → Public → 名稱：vercel-cicd-demo


不要勾：

[ ] README

[ ] .gitignore

[ ] License


📌 避雷：

本機已有 git，不要二次初始化  



## ☁ 5. 本機 → GitHub（Push）

設定 remote：

PowerShell：
```ps1
git remote add origin https://github.com/<你的帳號>/vercel-cicd-demo.git
git branch -M main
git push -u origin main
```

推上後 GitHub 出現：

v1 commit

index.html

📌 避雷：

Repository not found → repo 路徑錯 or 沒建立

Browser Authentication → 正常（OAuth）  


## 📦 6. GitHub → Vercel（CI/CD 設定）

Vercel：

New Project → Import Git Repository → vercel-cicd-demo


授權：

Install GitHub App → Allow Selected Repositories


📌 避雷：

不選 repo = Vercel 看不到

private repo 要授權才可 deploy  



## 🚀 7. Vercel 首次部署（CD 上線）

按：

Deploy


Vercel 自動：

✔ 拉 GitHub
✔ Build（靜態超快）
✔ CDN 部署
✔ HTTPS
✔ Preview

結果會給一個 URL：

https://vercel-cicd-demo-xxxx.vercel.app


📌 避雷：

靜態不需 Framework 設定

不改 Output Folder

Vercel 自動當 entry point  

## 🔁 8. CI/CD 自動更新（v2測試）

回本機更新index.html：

```html
<h1>CI/CD Demo v2</h1>
```

提交：

PowerShell：
```ps1
git add .
git commit -m "chore: update to v2"
git push
```

流程：

GitHub commit → webhook → Vercel redeploy → Production 更新


📌 避雷：

不 push = 不部署

push main 才更新 production  

## 🕹 9. 回滾（Rollback）＋ Promote

Vercel → Deployments：

v2 (Production)
v1 (History)


切換：

Promote v1 → Production 回到 v1
Promote v2 → Production 回到 v2


📌 避雷：

Rollback 不刪 commit

Rollback 不改 GitHub

只改 production 指向（pointer）

##🗃 10 GitHub 看版本

v1 & v2 對應：

v1 = commit #1
v2 = commit #2


查看：

GitHub → Commits


📌 避雷：

GitHub 不會自動叫 v1/v2

要命名 → 用 Tag or Release

🧩 1️⃣1️⃣ 本次練習獲得能力樹

你已通關：

Local Dev
↓
Git Versioning
↓
GitHub Hosting
↓
CI (webhook)
↓
CD (Deploy)
↓
Rollback
↓
Re-promote


## 🟦 10 windows刪除git

如果是照教程的話，請用以下步驟

先查：

winget list Git


若看到：

Git.Git


就可以卸：

winget uninstall --id Git.Git
