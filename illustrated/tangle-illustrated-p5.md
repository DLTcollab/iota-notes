# The Tangle: 圖解介紹
### _第五章：共識、確認可信度與協調器_

上週我們提到了雙重支付的問題，像是 Alice 重複使用她的錢的例子。本文將會是此系列最後一篇文章，我們將展示 Tangle 如何解決這樣的問題，以及我們如何決定哪筆歷史紀錄才是有效的。

要解釋這樣的問題，我們得先了解一下雙重支付的情境：

![](https://cdn-images-1.medium.com/0*hu_bY4iSNNPWBpS1.)

如你所見 Alice 只有 5i 但卻同時付給了 Bob 和 Charlie。這樣的問題很明顯：我們沒辦法同時將兩筆交易都視為有效。用 Tangle 的術語來說明的話就是我們沒辦法讓未來的交易能夠同時確認這兩筆交易，因為這樣一來會使 Alice 的餘額變成負的。

現在我們已經知道加權隨機漫步演算法，也就是最終其中一個分支會比另一條的權重還重，因此共識會產生在交易為有效的分支上。但從 Bob 或 Charlie 的角度來看的話會產生問題：他們該如何才能知道真的有從 Alice 獲得錢呢？

想像 Bob 和 Charlie 是賣恐龍的，然後 Alice 都向他們各買一隻暴龍當作玩具。如果他們都在交易一出現在 Tangle 時就馬上給她暴龍的話，最後一定會有其中一人發現自己甚麼錢沒拿到，他們該如何知道何時才能安全把暴龍交出去呢？

這是一件很重要的議題，事實上比特幣在 2009 年當時是第一個成功處理這項問題的技術。為了展示 Tangle 如何處理，我們先介紹一個概念叫做確認可信度，這個可信度用來衡量交易受到 Tangle 確認的層級。

確認可信度大致上會做以下的事情：

1. 運行 tip 選擇演算法 100 遍
2. 算出這 100 個 tips 有多少確認我們的交易，以下稱 A
3. 我們交易的確認可信度就會是「百分之 A」

換句話說，交易的可信度就是有多少 tip 確認它的比例，不是所有 tips 都是相等的，越新的 tips 越可能受到採納。為了方便解釋我們一樣在[視覺模擬](https://public-krwdbaytsx.now.sh)中加上了確認可信度。擁有 95% 以上可信度的交易會用粗框來表示。

![](https://cdn-images-1.medium.com/0*uhiAGN6uJ6a_pB1F.)

在上面的 Tangle 中，交易 9 總共被 4 個 tips 中的其中 2 個確認。如果我們用均勻隨機 tip 選擇的畫，他的可信度會是 50%。但是會去驗證它的 tips 比較可能是新的 tips，所以會稍微增加其可信度。

現在我們很確定能告訴 Bob 和 Charlie 何時能安全寄送他們的暴龍給 Alice。一旦 Alice 的交易達到非常高的可信門檻像是 95% 的話，它就不太可能在被共識排除在外了。我們這邊說的是不太可能而不是絕對不可能，如果 Alice 想要詐欺的話，她可以靠足夠大量的算力來達成雙重支付。

首先她可以先產生一筆交易給 Chrlie 而不是 Bob，她需要先確認兩筆舊的交易，不過對 Charlie 的交易還沒受到驗證。接下來她會產生極盡她所能的交易出來，想辦法增加這筆交易分支的權重。如果她有足夠算力的話，她能夠讓整個 IOTA 網路相信她新的分支，因此重寫整個歷史紀錄然後成功進行了雙重支付。如果我們再看一次 Bob 可信度的話，可以發現從 95% 降到零。

這樣的攻擊用下圖的 Tangle 來顯示，時間是往下進行：

![](https://cdn-images-1.medium.com/1*zf8LxIx88dZTj2YIHl9T0w.png)

這樣的情景是建構在如果 Alice 能夠比其他人產生的交易加起來還要多或是相近的狀況下才有風險，在成熟健全的網路環境下是無法達成的。不過這算是目前 IOTA 遇到最大的問題，系統中還沒有辦法存在足夠的交易數量來安全建立共識並抵擋雙重支付。

由於 IOTA 必須建在具有擴展性的條件下，我們設置了自主暫時性的共識機制來作為安全防護：協調器。每過兩分鐘會由 IOTA 基金會產生一筆交易作為里程碑（milestone），而確認這些交易的交易則會直接被視為有 100% 可信度。使用協調器的話 Alice 的第二筆交易永遠不會受到確認。這樣的防護機制是為了讓 IOTA 網路能夠在安全的環境下逐漸成長，讓更多實際應用採用網路藉此更加成長，最後讓 Tangle 分散式共識演算法能實際套用。到那個時候 IOTA 基金會將會關閉協調器，讓整體 Tangle 自行去演化。這會是一個漸進的程序，當網路足夠建全能夠撤下協調器時，網路本身也會因為龐大的交易數量讓網路變得更有效率。

我要感謝所有跟隨此系列文的大家，能夠撰寫並回答你們的留言與問題是一件很有趣的事。我也很樂意接收意見詢問下一個要討論的議題為何，目前可能的有 Tangle 上的攻擊向量、解釋 α 的定義為何、對於 tip 選擇演算法的實際優化以及其他你們想了解的事情都可以。

歡迎到 Discord 親自留言詢問我 @alongal#3938。
