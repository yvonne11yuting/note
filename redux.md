# redux閱讀筆記
這邊參考Valentino Gagliardi的[React Redux Tutorial for Beginners: The Definitive Guide](https://www.valentinog.com/blog/react-redux-tutorial-beginners/)<br>
這篇從觀念開始帶入，讓學習者可以反思學習redux的價值和意義而不是盲目跟風。

## 什麼是state(狀態)?
[沒有redux介入的react state]
每個React的class component都有自己的state來保存data<br>
可以透過setState來更新local component<br>

但其實......
```javascript
var state = {
  buttonClicked: 'no',
  modalOpen: 'no'
}
```
我們也可以建一個object然後設定初始化state
當我們click某個按鈕時可以變更state為
```javascript
var state = {
  buttonClicked: 'yes',
  modalOpen: 'yes'
}
```
我們要怎麼追蹤這些state呢？<br>
有什麼library可以幫助我們達到這個目的嗎？

## redux解決了什麼？
一個典型的javascript應用充滿了state，比如說以下幾項都是
* user看到什麼？(data)
* 我們要取什麼daya?
* 我們要給user看到什麼URL?
* 哪些項目可以在頁面中被選取？
* 有任何的error在應用程式內嗎？

所以javascript到處都是state<br>
但你可以想要一個React application有多少個state嗎？<br>
是的，我們可以保存state在父層component內(只要這個application維持**小小的**)<br>
然後事情就慢慢變得棘手了 QAQ<br>
如果我們開始上下傳遞state，即使是一個簡單的to-do-list都能搞到無法管理<br>
誰想要一個胖子react component?<br>
所以有什麼替代方案來管理React component的state呢？<br>
**Redux**就是其中一個解套方法<br>
一開始看Redux可能還看不太出它的效果，但他幫助每個React component有確切的狀態。<br>
Redux在某處容納了state，並且取得與管理state的邏輯可以活存在React之外。

## 應該學Redux嗎？
很多初學者感到害怕。<br>
Redux不難，但不要因為看到Redux就急於學習，<br>
如果你有動力和熱情，就是時候開始學Redux了。<br>

Redux與框架無關，畢竟是狀態管理嘛。學一次，到處用(Vue, Angluar)也需要狀態管理的。<br>
Redux可能消失嗎？或許會<br>
但它的pattern是永存的！<br>
我們應該學習管理UI的pattern因為這些對我們職涯上是無價的。

## 我應該用Redux嗎？
要用Redux、Flux或Mobx都隨便你。<br>
用這些library也是有成本的，他們增加了一層抽象層。<br>
但Valentino Gagliardi把它當成投資而不是成本。<br>
要怎麼知道什麼時候可以開始用redux呢？<br>
當Valentino Gagliardi發現他應該考慮redux時的情況為...<br>
* 有好多React components 需要傳遞相同的state，但他們不是親子關係(parent/child relationship)時
* 當你開始對將state透過props傳遞給多個component感到困窘時

如果你覺得這對你無感時不要擔心，Valentino Gagliardi也有同感 <br>
（但我覺得很有感啊...一直傳props傳到起笑，每隔一段時間還要想一下自己現在props傳到哪了，真的崩潰）<br>
> Dan Abramov曾說過，Flux library就像眼鏡，當你需要它的時候你就知道了。






