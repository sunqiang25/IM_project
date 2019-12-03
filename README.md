全球人壽客服排班  
輸入輸出格式說明文件(CSR版)
=======================================

* (指定選項擇一)表示需輸入一個指定的群組名稱，目前可選用的選項如下：  
 日子集合： all, Mon, Tue, Wed, Thr, Fri  
 班別集合： all, morning, noon, night, other,  
 (如果需要增加或修改，班別的部分請從Classes.csv修改；  
 日期則是自動計算，必須修改程式才能修改日期組）  
* 套件需求  
 需先使用pip安裝：numpy、pandas  
 商業套件： gurobipy （開發時使用的是免費學術版。商業使用有30天免費試用)  
 python3已經包含（不須另外安裝）的套件：datetime、calendar  

# 輸入資料格式說明
### CSR員工資料表
欄位: 英文名字、中文名字、員工id、員工年資、員工職位、員工技能、員工上月斷頭周晚班次數、員工上月末日週五是否晚班、員工一週能排的晚班次數  
可上晚班的次數  
![](IM_project/img/Picture1.png)

### 班別時間表
列: 班別代號/行: 涵蓋時段  

### 班別集合表
第一項是班別集合名稱，隨後的項目是所涵蓋的班別代號  

### 進線人力需求表
列: 上班時段/行: 日期  

### 上個月排班結果表

# 可調整參數說明
### 程式執行時間

### 目標式係數
P1~4   皆為≧0的數字。 0為無視此條件，100為和主要條件（缺工數）有同樣影響一
（這裡程式需要多加個常數，讓每個可變動項的影響力相同）  

|數學變數名稱|變數意涵|輸入格式|
|---|---|---
|P1|冗員多寡的重要程度|數字
|P2|每位CSR排晚班次數公平性的重要程度|數字
|P3|每位CSR每週內每天午休時間一致性的重要程度|數字
|P4|擁有特定技能CSR優先排特定班別的重要程度|數字

### 指定班別
透過獨立的表格來指定，表格中每一橫列左到右依序是：  
1. 員工編號（英文名稱）  
2. 日期（正整數，不含月份）  
3. 班別英文代號（A3、CD...之類的）  


|數學變數名稱|變數意涵|輸入格式|
|---|---|---
||CSR員工 i 指定第 j 天須排班別 k|獨立的表格，橫列左到右為：CSR員工ID，日期(不含月)，班別英文代號

### 指定日子與班別之一般CSR員工或指定職位人數下限
透過獨立的表格來指定，表格中每一橫列左到右依序是：  
1. 職位中文名稱（不限請填「任意」）  
2. 日子（正整數: 1至該月最後一天的日期）  
3. 班別（指定選項擇一，例如all,night)  
4. 最少多少人（正整數）  

|數學變數名稱|變數意涵|輸入格式|
|---|---|---
||限定上班人數下限的特定職稱的CSR員工集合|Position:職位中文名稱(不限職位請填「任意」)
||限制上班人數下限的特定日子集合|Dates:正整數: 1至該月最後一天的日期
||限定特定職稱的CSR員工上班人數下限的班別集合|Classes:班別集合代號（指定選項中擇一)
||日子 j 特定班別所設定的上班人數下限|Need:正整數

### 指定日子與班別之特定CSR員工上班次數上限
透過獨立的表格來指定，表格中每一橫列左到右依序是：  
1. 日子（指定選項擇一，例如all,Mon)  
2. 班別（指定選項擇一，例如all,night)  
3. 上限次數（正整數）  

|數學變數名稱|變數意涵|輸入格式|
|---|---|---
||限制每位CSR員工排某些班別有次數上限的特定日子集合|Dates:日子集合代號（指定選項中擇一)
||限定每位CSR員工排某些班別有次數上限的班別集合|Classes:班別集合代號（指定選項中擇一)
||每位CSR員工在特定日子排特定班別的次數上限|Limit:正整數

### 擁有特定技能CSR員工優先排特定班別限制
透過獨立的表格來指定，表格中每一橫列不一樣長，以下面的順序排列：  
1. 技能名稱（字串，例如 chat）  
2. 此技能要優先的班別（可以有多項，分在不同儲存格。每一項都是英文代號)  

|數學變數名稱|變數意涵|輸入格式|
|---|---|---
||擁有特定技能的CSR員工集合|從CSR資料表直接讀取
||非“擁有特定技能CSR員工所優先排之特定班別”的班別集合|獨立的表格：第一項是技能名稱，隨後的項目是要優先的班別

### 指定日子與班別之CSR員工年資占比限制
表格左到右依序：天(set)、班別(set)、下限比例(int,0~1)、(指定的年資數)  
1. 日子（指定選項擇一，例如all,Mon)  
2. 班別（指定選項擇一，例如all,night)  
3. 最少占多少比例（0~1之間的數字，包含0和1）  
4. 指定的年資（正整數）  

|數學變數名稱|變數意涵|輸入格式|
|---|---|---
||特定年資CSR員工集合|Senior:年資寫在表格中的最後一個直行
||限制特定年資CSR員工占總排班人數特定比例以上的特定日子集合|Dates:日子集合代號（指定選項中擇一)
||限制特定年資CSR員工占總排班人數特定比例以上的特定班別集合|Classes:班別集合代號（指定選項中擇一)
||在特定日子中數個指定班別，針對特定群組之CSR員工，必須佔總排班人數的特定比例|Ratio:0~1的數字，包含0和1


