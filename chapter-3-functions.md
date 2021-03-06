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

### 只做一件事情

函式應該要維持最小的原則，一次只做一件事，並且把它做好。至於怎麼樣算是只做一件事，作者認為，如果函式只做函式名稱下「同一個抽象概念」的幾個步驟，那麼也算是只做一件事。

如果你能在一個函式中找到能提出成另一個函式的概念，那就是做超過一件事情了。舉例而言，如果一個函式的內容是「測試功能並轉譯為HTML」，很明顯的，這個函式包含了「測試」和「轉譯」兩個概念；相反的，如果「測試」方法包含了檢查「長度」和「型別」，那即便做兩件事情也是同個概念。

#### 函式的段落

如果函式可以明顯的區分出「宣告區」、「初始區」和「過濾區」，這很明顯就是做超過一件事情，因為只做一件事的函式沒有辦法合理的被細分為這些區域。

### 每個函式只有一層抽象概念

一個函式中如果有太多層次的抽象概念，就很容易造成閱讀上的困難。舉例而言，假設有一個產生報表的方法，裡面包含了：

1. 產生報表
2. 取得報表資料
3. 取得報表存取路徑

事實上，這很明顯的就違反了前述「只做一件事情」的原則，並且由上而下包含了太多層次的細節，反而容易使程式碼混淆。

#### 由上而下閱讀程式碼：降層原則

我們閱讀上都習慣一件事情接一件事情的依序閱讀，所以上述方法應該依序拆為以下方法：

1. 取得報表的資料
2. 產生報表的存取路徑
3. 產生報表

將函式維持在單一層級的抽象概念，這個技巧很重要，也是保持程式碼簡短的關鍵。

### Switch敘述

switch語法本質上就是在做一件以上的事情，請看以下的範例code敘述：

```text
public Money calculatePay(Employee e) throws InvalidEmployeeType {
    switch(e.type) {
        case COMMISSIONED:
            return calculateCommissionPay(e);
        case HOURLY:
            return calculateHourlyPay(e);
        case SALARIED:
            return calculateSalariedPay(e);
        default:
            throw new InvalidEmployeeType(e.type);
    }
}
```

這段程式碼有許多問題

1. 太冗長：若加入新的型態則會變得更冗長
2. 做超過一件事情
3. 違反單一職責原則：超過一個以上的理由改變此函式
4. 違反開放閉合原則：新型態一旦加入，整個類別都需要調整

作者認為在使用上我們應該盡可能的利用多型， 最好的利用switch的方法，是將之藏在抽象工廠底下：

```text
public abstract class Employee {
    public abstract boolean isPayday();
    public abstract Money calculatePay();
    public abstract void deliverPay(Money pay);
}
-------------------------------------------------
public interface EmployeeFactory {
    public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}
-------------------------------------------------
public class EmployeeFactoryImpl implements EmployeeFactory {
    public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
        switch(r.type) {
        case COMMISSIONED:
            return new CommissionedEmployee(r);
        case HOURLY:
            return new HourlyEmployee(r);
        case SALARIED:
            return new SalariedEmployee(r);
        default:
            throw new InvalidEmployeeType(r.type);
        }
    }
}
```

這個工廠使用switch敘述來建立實體，其他不同的函式，如isPayday等行為則由多型的方式來指派。

### 使用具體能力的名稱

這點和前一個章節提到的命名相關，而函式的最大特點就是，如果函式本身夠簡短，那就越容易為該函式取一個具體的名稱。其餘的重點作者在命名的章節中都有提到，不要害怕使用長字命名，並且應使用一致的詞彙來替函式命名。

### 函式的參數

作者將函式依照參數數量分為以下幾種：

1. 零個 \(niladic\)：作者認為最為理想的函式狀態。
2. 一個 \(monadic\)：次佳，考量實際運行為最佳的情況。
3. 兩個 \(dyadic\)：再次之。
4. 三個 \(triadic\)：應盡量避免使用。
5. 超過三個 \(polyadic\)：非常糟糕，必須有理由，否則不這麼做。

參數的使用是困難的議題，因為他們會透露函式的概念，讀者看到參數時都得去做更細部的詮釋，等同於強迫了解不這麼重要的細節。更甚者，多個參數將使得創造測試案例變得更加複雜棘手，甚至到難以執行的地步。

#### 單一參數

作者提供兩個使用上極為常見的情境：

* 與這個參數有關的問題，例如：public boolean fileExists\(File myFile\)
* 對這個參數進行某項操作，再將其回傳，例如：public stringToBinary\(String in\)

這兩種的寫法能清楚的掌握函式與參數間的關係。另一種較不普遍但也有用的作法，就是_**事件**_，刻意將函式作為一個事件，用參數來改變系統狀態，在使用上應明確小心以免誤用，例如：public void passwordAttempFailedTimes\(int attemps\)

作者認為，如果不符合上述的三種情境的話，那請考慮使用其他形式的函式寫法，有個大原則是：

_**如果一個函式會轉換輸入參數，那轉換後的產物應出現在回傳值中。**_

_\*\*\*\*_





