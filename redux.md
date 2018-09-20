# redux閱讀筆記
這邊參考Valentino Gagliardi的[React Redux Tutorial for Beginners: The Definitive Guide](https://www.valentinog.com/blog/react-redux-tutorial-beginners/)<br>
這篇從觀念開始帶入，讓學習者可以反思學習redux的價值和意義而不是盲目跟風。

## 一個最小的React開發環境
Valentino Gagliardi提供了一個建置好了github repo
```bash
git clone https://github.com/valentinogagliardi/webpack-4-quickstart.git
```
clone下來後就可以先移除./src/App.js

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

## 開始了解Redux store
Actions. Reducers.<br>
好像有點懂，好像不太懂，話說，他們是怎麼黏在一起的？<br>

**store編排了所有在Redux的moving parts**<br>

store是Redux的根本，整個應用程式的state都存活在store內。<br>
所以要開始使用Redux，我們應該建立一個用於包裝state的store。<br>

回到開發環境，我們開始安裝redux
```bash
yarn add redux -D
```
建立store資料夾
```bash
mkdir -p src/js/store
```
在src/js/store下建立一個新檔案index.js初始化store
```javascript
// src/js/store/index.js
import { createStore } from "redux";
import rootReducer from "../reducers/index";
const store = createStore(rootReducer);
export default store;
```
* **createStore**是一個function來建立Redux store
* createStore將 **reducer** 作為第一個參數，在我們的case內即 **rootReducer**

你還可以將初始狀態(initial state)傳遞給createStore。但大多數時候你不必這樣做。<br>
傳遞初始化狀態對 **伺服器渲染(server side rendering, SSR)** 很有用。<br>
無論如何，**state來自reducer**

筆記：[Reducer returned undefined during initialization](https://stackoverflow.com/questions/36619093/why-do-i-get-reducer-returned-undefined-during-initialization-despite-pr)

現在重要的是了解reducer的作用。<br>
在Redux reducer產生state，這state不是我們手做產物。<br>
好了，有了這些知識我們可以繼續我們的第一個Redux reducer了。

## 開始了解Redux reducers
雖然initial state對server side rendering(SSR)很有用，在Redux，state必須完全從reducer return

好啦，到底什麼是reducer?

**reducer就是個javascript的function**<br>

一個reducer function有兩個參數：**current state** 和 **action**（之後我們再來跟action好好深交）<br>

Redux的第三個準則是，state是一成不變的，而且不能隨意改變。<br>

在簡單的React中，我們可以透過setState改變local state，但在Redux我們做不到。<br>

在這個範例裡，我們建立一個簡單的reducer有個initial state作為第一個參數。第二個參數我們置入action，到目前為止，我們的reducer不做任何事只返回initial state。

建立一個資料夾來放root reducer
```bash
mkdir -p src/js/reducers
```

然後在reducer內建立index.js檔案
```bash
touch src/js/reducers/index.js
```
indexjs內的程式碼如下
```javascript
// src/js/reducers/index.js
const initialState = {
  articles: []
};
const rootReducer = (state = initialState, action) => state;
export default rootReducer;
```

因為作者說他盡量把它簡單化，所以我們的第一個reducer看起來很蠢，他只return了initial state然後沒做任何事。<br>
下一段要加入action了，應該會開始變有趣囉！

## 開始了解Redux actions
Redux reducer毫無疑問的是Redux最重要的概念，<br>
Reducer產生application的state。<br>

但reducer要怎麼知道什麼時候要產生下個state呢？<br>
改變state的唯一方法是向store發送信號，那個信號就是 **action**。<br>
"Dispatching an action"(調度action)就是發送信號的過程。<br>

好囉～所以我們要怎麼改變那個不變的state？<br>
結果是產生的state其實是一個複製的current state加上新的data。<br>

感人的是Redux操作的只是javascript的object。<br>
所以我們的action看起來像
```javascript
{
  type: 'ADD_ARTICLE',
  payload: { name: 'React Redux Tutorial', id: 1 }
}
```

* type: 每個action都會有type屬性來描述state該怎麼改變。
* payload: 在上面的例子裡，payload是一篇新文章，reducer將會在之後把這篇文章加到current state。

最棒的實踐方法就是將每個action都包裝在一個個的function，像這樣的function就是action creator<br>

讓我們透過建立一個簡單的Redux action將所有內容放在一起吧。<br>

建立action資料夾
```bash
mkdir -p src/js/actions
```
在actions內建立index.js，內容程式碼如下：
```bash
touch src/js/actions/index.js
```
```javascript
// src/js/actions/index.js
export const addArticle = article => ({ type: "ADD_ARTICLE", payload: article });
```
對，type屬性就是一串string。<br>
reduxer將使用該string來決定怎麼計算下一個state。<br>
由於string容易因為手殘眼花而打錯，所以最好把他先宣告成常數(constant)，<br>
這方法有助於避免麻煩的debug。<br>

所以我們來建個常數資料夾吧XD
```bash
mkdir -p src/js/constants
```
接這在constants資料夾下建立action-types.js
```bash
touch src/js/constants/action-types.js
```
內容程式碼如下：
```javascript
// src/js/constants/action-types.js
export const ADD_ARTICLE = "ADD_ARTICLE";
```

接著再次打開actions下的index.js(src/js/actions/index.js)，並且更新它
```javascript
// src/js/actions/index.js
import { ADD_ARTICLE } from "../constants/action-types";
export const addArticle = article => ({ type: ADD_ARTICLE, payload: article });
```
我們離一個可用的Redux applicaiton只差一步啦，接下來來重構我們的reducer囉

## 重構reducer
先來概括一下Redux的主要概念

* Redux store就像大腦，他負責協調Redux的所有moving parts
* 應用程式的state單獨存在，在store內是個不可變的物件(object)
* 當store接收到action時就會觸發reducer
* 由reducer來回傳下個新state
* reducer是一個javascript function，內含兩個參數
(state和action)
* reducer function通常有switch statement(雖然有點笨，但也可以用if/else)
* reducer依據action type來計算下個state。此外，當沒有取到對應的action type時，至少應該要回傳initial state

當action type取到對應的case時，reducer會計算下個state並回傳一個new object，範例如下：
```javascript
// ...
  switch (action.type) {
    case ADD_ARTICLE:
      return { ...state, articles: [...state.articles, action.payload] };
    default:
      return state;
  }
// ...
```

我們之前建立的reducer不會回傳initial state，我們來修正這個問題。<br>
打開src/js/reducers/index.js並更新程式碼如下：
```javascript
import { ADD_ARTICLE } from "../constants/action-types";
const initialState = {
  articles: []
};
const rootReducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_ARTICLE:
      state.articles.push(action.payload);
      return state;
    default:
      return state;
  }
};
export default rootReducer;
```
在這邊我們發現一個問題，上面的code雖然有效，但reducer打破了Redux的規則 **不變性**<br>

[Array.prototype.push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)會改變原來的array。ಠ_ಠ<br>
好囉，push不能用啦，我們改用[Array.prototype.concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)，用concat會回傳一個新array，我們就可以不用擔心打破規則囉。＼(^o^)／

再修正後的程式碼如下
```javascript
// ...
switch (action.type) {
  case ADD_ARTICLE:
    return { ...state, articles: state.articles.concat(action.payload) };
  default:
    return state;
}
// ...
```
等等，我們還沒完成。使用spread運算子可以讓我們的reducer更好
```javascript
// ...
switch (action.type) {
  case ADD_ARTICLE:
    return { ...state, articles: [...state.articles, action.payload] };
  default:
    return state;
}
// ...

```

在上面的例子裡，initial state完全不受影響，<br>
初始的articles資料不會被隨意改變，初始的state也不會被更改，<br>
產生的state也是initial state的副本，一切符合期待。<br>

以下兩個key point來避免Redux突變
* array：[Using concat(), slice(), and …spread](https://egghead.io/lessons/react-redux-avoiding-array-mutations-with-concat-slice-and-spread)
* object：[Using Object.assign() and …spread](https://egghead.io/lessons/react-redux-avoiding-object-mutations-with-object-assign-and-spread)


注意：object spread運算子仍在stage 3，為了避免舊瀏覽器不支援，我們必須安裝[Object rest spread transform](https://babeljs.io/docs/en/babel-plugin-transform-object-rest-spread/)

```bash
yarn add babel-plugin-transform-object-rest-spread -D
```

打開.babelrc更新設定
```json
{
    "presets": ["env", "react"],
    "plugins": ["transform-object-rest-spread"]
}
```

> 專家建議：<br>
> 隨著我們的app變大，reducer也會跟著一起長大。
> 我們可以將一個大型reducer拆成多個function然後使用[combineReducers](https://redux.js.org/api/combinereducers)讓他們黏在一起

下一節我們要在console中使用Redux，hold住！

