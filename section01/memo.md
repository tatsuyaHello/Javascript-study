# 概要
## es6(ECMAScript)(ES2015)
### 用語系
- ECMAScriptは標準規格
- JavaScriptはその実装
- ES6のコードは全てのブラウザで動くわけではない
  - ES6のコードをBabelを使用してES5のコードにトランスパイルする

### 配列の便利メソッド
- 配列用語
  - もともとimportなどをすると使用できたが、需要が高まっていきたので、デフォルトで使用できるように変更した。forループがほとんど不必要になってきた。
  - forループを使うと書くことが多い。シンプルなforループでも情報量が多いので理解がしにくい。

- forEach
  - ロジックが少ない
  - 実行するだけで返り値は無い(返り値が必要な場合は、mapを使用する必要がある)
```
let colors = ['red', 'blue', 'green'];
colors.forEach(function(color) {
	console.log(color);
});
```
- map
  - オブジェクトの配列から興味のある値だけを引き抜く
  - 実行した後に返り値がある(返り値が不必要な場合は、forEachを使用する)
```
let numbers = [1,2,3];
let doubledNumbers = [];
var doubled = numbers.map(function(number){
	return number * 2;
});
```
- filter
```
let products = [
  {name: 'きゅうり', type: '野菜'},
  {name: 'バナナ', type: 'フルーツ'},
  {name: 'セロリ', type: '野菜'},
  {name: 'オレンジ', type: 'フルーツ'},
];
​
let filteredProducts = [];
products.filter(function(product){
  return product.type === 'フルーツ'
})
```
- find
  - 最初に見つかったものだけを返す
```
let users = [
  {name: '太郎'},
  {name: '二郎'},
  {name: '三浪'},
];
users.find(function(user){
	return user.name === '二郎';
});
```
- every
```
let computers = [
  {name: 'Apple', ram: 24},
  {name: 'Compaq', ram: 4},
  {name: 'Acer', ram: 32},
];
computers.every(function(computer){
	return computer.ram >= 16;
})
```
- some
```
computers.some(function(computer){
	return computer.ram >= 16;
})
```
- reduce
  - 初期値が次に引き継がれていくという概念が登場
```
numbers.reduce(function(sum, number){
	return sum += number;
}, 0);
```
```
let primaryColors = [
  {color: 'red'},
  {color: 'yellow'},
  {color: 'blue'},
]

primaryColors.reduce(function(previous, primaryColor){
	previous.push(primaryColor.color);
  return previous;
},[]);
```

### 構文
#### let, const
#### テンプレートリテラル
#### アロー関数
- 読みやすくなる
- コールバック関数は別世界で実行されているというのから脱却
```
const add = function(a, b){
  return a + b;
}
add(1, 2);
```
```
const add = (a, b) => {
  return a + b;
}
```
```
const add = (a, b) => a + b;
```
---
```
const team = {
	members: ['太郎', '花子'],
  teamName: 'スーパーチーム',
  teamSummary: function(){
  	return this.members.map(function(member){
    	return `${member}は${this.teamName}の所属です`
    }.bind(this));
  }
};
team.teamSummary();
```
```
const team = {
	members: ['太郎', '花子'],
  teamName: 'スーパーチーム',
  teamSummary: function(){
    let self = this;
  	return this.members.map(function(member){
    	return `${member}は${self.teamName}の所属です`
    });
  }
};
team.teamSummary();
```
```
const team = {
	members: ['太郎', '花子'],
  teamName: 'スーパーチーム',
  teamSummary: function(){
  	return this.members.map((member) =>{
    	return `${member}は${this.teamName}の所属です`
    });
  }
};
team.teamSummary();
```
#### オブジェクトリテラル
```
//旧
function createBookShop(inventory){
	return{
  	inventory: inventory,
    inventoryValue: function(){
      return this.inventory.reduce((total, book) => total + book.price, 0);
    },
    priceForTitle: function(title){
      return this.inventory.find(book => book.title === title).price;
    }
  };
}
const inventory = [
  {title: 'ハリーポッター', price: 1000},
  {title: 'Javascript book', price: 1500},
];
const bookShop = createBookShop(inventory);
bookShop.inventoryValue();
bookShop.priceForTitle('ハリーポッター');
```
```
//新
function createBookShop(inventory){
	return{
  	inventory,
    inventoryValue(){
      return this.inventory.reduce((total, book) => total + book.price, 0);
    },
    priceForTitle(title){
      return this.inventory.find(book => book.title === title).price;
    }
  };
}

const inventory = [
  {title: 'ハリーポッター', price: 1000},
  {title: 'Javascript book', price: 1500},
];

const bookShop = createBookShop(inventory);

bookShop.inventoryValue();
bookShop.priceForTitle('ハリーポッター');
```

#### 関数のデフォルト引数
#### RestとSpread演算子
```
//rest演算子
function addNumbers(...numbers){
	return numbers.reduce((sum, number) =>{
  	return sum + number;
  }, 0);
}
addNumbers(1,2,3,4,5,6);

//spread演算子
const defaultColors = ['赤', '緑'];
const userFavoriteColors = ['オレンジ', '黄'];
const fallColors = ['茶', '紅葉'];

defaultColors.concat(userFavoriteColors);
[...defaultColors, ...userFavoriteColors];
['青', ...fallColors, ...defaultColors, ...userFavoriteColors];


function validateShoppingList(...items) {
  if(items.indexOf('牛乳') < 0) {
  	return [ '牛乳', ...items];
  }
  return items;
}
validateShoppingList('オレンジ', 'パン');
```

```
//rest
function product(...numbersA) {
  var numbers = [...numbersA];
  return numbers.reduce(function(acc, number) {
    return acc * number;
  }, 1)
}

//Spread
function join(array1, array2) {
  return [...array1, ...array2]
}
```


### その他メモ
```
というオブジェクトがあった際、オブジェクトのプロパティは大括弧を([])を使ってアクセスできる
let person = {name: '竜也'};
person['name']; // 竜也
```
