# Git-Commit-Message
記錄下Commit Message的一些規範
https://wadehuanglearning.blogspot.com/2019/05/commit-commit-commit-why-what-commit.html

Header: <type>(<scope>): <subject>
 - type: 代表 commit 的類別：feat, fix, docs, style, refactor, test, chore，必要欄位。
 - scope 代表 commit 影響的範圍，例如資料庫、控制層、模板層等等，視專案不同而不同，為可選欄位。
 - subject 代表此 commit 的簡短描述，不要超過 50 個字元，結尾不要加句號，為必要欄位。

Body: 72-character wrapped. This should answer:
 * Body 部份是對本次 Commit 的詳細描述，可以分成多行，每一行不要超過 72 個字元。
 * 說明程式碼變動的項目與原因，還有與先前行為的對比。

Footer: 
 - 填寫任務編號（如果有的話）.
 - BREAKING CHANGE（可忽略），記錄不兼容的變動，
   以 BREAKING CHANGE: 開頭，後面是對變動的描述、以及變動原因和遷移方法。
type: subject 是簡述不要超過 50 個字元

-----
  
type 只允許使用以下類別：
 - feat: 新增/修改功能 (feature)
 - fix: 修補 bug (bug fix)
 - docs: 文件 (documentation)
 - style: 格式 (不影響程式碼運行的變動 white-space, formatting, missing semi colons, etc)
 - refactor: 重構 (既不是新增功能，也不是修補 bug 的程式碼變動)
 - perf: 改善效能 (A code change that improves performance)
 - test: 增加測試 (when adding missing tests)
 - chore: 建構程序或輔助工具的變動 (maintain)
 - revert: 撤銷回覆先前的 commit 例如：revert: type(scope): subject (回覆版本：xxxx)
 
Type 是用來告訴進行 Code Review 的人應該以什麼態度來檢視 Commit 內容。
例如：
看到 Type 為 fix，進行 Code Review 的人就可以用「觀察 Commit 如何解決錯誤」的角度來閱讀程式碼。
若是 refactor，則可以放輕鬆閱讀程式碼如何被重構，因為重構的本質是不會影響既有的功能。
利用不同的 Type 來決定進行 Code Review 檢視的角度，可以提升 Code Review 的速度。因此開發團隊應該要對這些 Type 的使用時機有一致的認同。
  
-----
  
範例 fix：
 
fix: 自訂表單新增/編輯頁面，修正離開頁面提醒邏輯

問題：
1. 原程式碼進入新增頁面後，沒做任何動作之下，離開頁面會跳提醒
2. 原程式碼從新增/編輯頁面回到上一頁後（表單列表頁面），離開頁面會跳提醒

原因：
1. 新增頁面時，頁面自動建立空白題組會調用 sort_item，造成初始化 unload 事件處理器。
2. 回到上一頁後，就不需要監聽 unload 事件，應該把 unload 事件取消。

調整項目：
1. 初始化 unload 事件處理器：排除新增表單時，頁面自動建立空白題組調用 sort_item 的情境
2. 回到上一頁後，復原表單被異動狀態且清除 unload 事件處理器

issue #1335
  
-----
  
範例 feat:
 
feat: message 信件通知功能

因應新需求做調整：
 通知和 message 都要寄發每日信件，
 通知和 message 都用放在同一封信裡面就好，
 不然信件太多可能也不會有人想去看。

調整項目：
1. mail_template.php，新增 message 區塊。
2. Send_today_notify_mail.php，新增 取得每日 Message 邏輯。
3. Message_model_api.php，新增 $where 參數，以便取得每日訊息。
4. Message_api.php、Message_group_user_model_api.php，新增 **取得訊息使用者** 邏輯，以便撈取每日訊息。

issue #863

