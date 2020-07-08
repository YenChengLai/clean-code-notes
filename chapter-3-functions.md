# Chapter 3 函式

### 簡短

簡短是作者極力主張的要求，比起直接寫一個上千行的函式，作者更加倡導多個長度不大於20行的小函式，函式間透過彼此呼叫進行連動。如此一來，個函式的意圖都會變得明顯。

### 區塊\(Blocks\)和縮排\(Indenting\)

if、else、while等敘述應該都只有一行，而該行通常應是函式的呼叫敘述，舉例而言：

```text
if ( "checked".equals(account.status) &&
  "domestic".equals(account.financialSource))
```

上述程式碼不如改成：

```text
if (account.isChecked() && account.isDomesticSource())
```

這麼做不僅能維持函式的簡短，意圖也因為判斷式獨立為另外的函式而更加清晰。因此，函式不應該大到包含巢狀結構，縮排也應限制在一兩行內。





