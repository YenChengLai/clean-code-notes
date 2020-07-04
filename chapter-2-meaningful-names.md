# Chapter 2 有意義的命名

### 讓名稱代表意圖－使之名符其實

初學者最常犯的錯誤之一，就是忽略命名的重要而隨意命名，花時間給予好的命名就像投資，會在未來為你省下更多的時間。有幾個原則我們要特別注意：

1. 好的命名不需要額外再提供註解

```text
int d;  // elapsed time in days
```

上述註解完全就是多餘的，我們只要對命名做一些調整，含義就十分清楚了：

```text
int elapsedTimeInDays;
```

2. 好的命名可以大大降低程式的隱含性\(Implicity\)，以下面的程式碼為例：

```text
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<>();
    for (int[] x： theList)
        if (x[0] == 4)
            list1.add(x);
    return list1;
}
```

上述的程式碼無法讓人明瞭其意義：

* theList的內容是什麼？
* theList的第一項內容物件\(索引數0\)代表什麼意義？
* 4是什麼意思？
* 回傳的list1又代表什麼？

假設這是一個踩地雷遊戲，而該方法是要回傳標誌旗子的方格。如果改成以下程式碼：

```text
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<>();
    for (Cell cell: gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
```

程式碼的複雜度其實沒有任何的改變，但因為降低了隱含性，可讀性就大大的提升了。



