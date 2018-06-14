## **理解ES**
1. 全称: ECMAScript
2. js语言的规范
3. 我们用的js是它的实现
4. js的组成
  * ECMAScript(js基础)
  * 扩展-->浏览器端
    * BOM
    * DOM
  * 扩展-->服务器端
    * Node.js
      
## ES5
1. **严格模式**
  * 运行模式: 正常(混杂)模式与严格模式
  * 应用上严格式: 'strict mode';
  * 作用: 
    * 使得Javascript在更严格的条件下运行
    * 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为
    * 消除代码运行的一些不安全之处，保证代码运行的安全
    * 需要记住的几个变化
      * 声明定义变量必须用var
      * 禁止自定义的函数中的this关键字指向全局对象
      * 创建eval作用域, 更安全
      
2. **JSON对象**
  * 作用: 用于在json对象/数组与js对象/数组相互转换
  * JSON.stringify(obj/arr)
      js对象(数组)转换为json对象(数组)
  * JSON.parse(json)
      json对象(数组)转换为js对象(数组)
      
3. Object扩展
	
	```
	<--
    对象本身的两个方法
    * get propertyName(){} 用来得到当前属性值的回调函数
    * set propertyName(){} 用来监视当前属性值变化的回调函数
	-->
	
	var obj = {
        firstName : 'kobe',
        lastName : 'bryant',
        get fullName(){
            return this.firstName + ' ' + this.lastName
        },
        set fullName(data){
            var names = data.split(' ');
            this.firstName = names[0];
            this.lastName = names[1];
        }
    };
    console.log(obj.fullName);
	```
  * Object.create(prototype[, descriptors]) : 创建一个新的对象
    * 以指定对象为原型创建新的对象
    	
    	```
    	var obj = {name : 'curry', age : 29}
    	var obj1 = {};
    	obj1 = Object.create(obj, {
        sex : {
            value : '男',
            writable : true
        	}
    	});
    	```
    * 指定新的属性, 并对属性进行描述
      * value : 指定值
      * writable : 标识当前属性值是否是可修改的, 默认为true
      * **get方法** : 用来得到当前属性值的回调函数
      * **set方法** : 用来监视当前属性值变化的回调函数
  * Object.defineProperties(object, descriptors) : 为指定对象定义扩展多个属性
  	
	  	```
	  var obj2 = {
	    firstName : 'curry',
	    lastName : 'stephen'
		};
		Object.defineProperties(obj2, {
	    fullName : {
	        get : function () {
	            return this.firstName + '-' + this.lastName
	        },
	        set : function (data) {
	            var names = data.split('-');
	            this.firstName = names[0];
	            this.lastName = names[1];
	        }
	    }
		});
	      
	  	```
  
4. Array扩展
  * Array.prototype.indexOf(value) : 得到值在数组中的第一个下标
  * Array.prototype.lastIndexOf(value) : 得到值在数组中的最后一个下标
  * **Array.prototype.forEach(function(item, index){}) : 遍历数组**
  * **Array.prototype.map(function(item, index){}) : 遍历数组返回一个新的数组**
  * **Array.prototype.filter(function(item, index){}) : 遍历过滤出一个子数组**
  
5. **Function扩展**
  * Function.prototype.bind(obj)
      * 将函数内的this绑定为obj, 并将函数返回
  * 面试题: 区别bind()与call()和apply()?
      * fn.bind(obj, args) : 指定函数中的this, 并返回函数
      * fn.call(obj, args) : 指定函数中的this, 并调用函数
      * fn.apply(obj, [args]): 指定函数中的this, 并调用函数
      
6. Date扩展
  * Date.now() : 得到当前时间值

7. 数值Number扩展
	* 二进制与八进制数值表示法: 二进制用0b, 八进制用0o
	* Number.isFinite(i) : 判断是否是有限大的数
	* Number.isNaN(i) : 判断是否是NaN
	* Number.isInteger(i) : 判断是否是整数
	* Number.parseInt(str) : 将字符串转换为对应的数值
	* Math.trunc(i) : 直接去除小数部分
	
		```
		console.log(0b1010);//10
	    console.log(0o56);//46
	    //Number.isFinite(i) : 判断是否是有限大的数
	    console.log(Number.isFinite(NaN));//false
	    console.log(Number.isFinite(5));//true
	    //Number.isNaN(i) : 判断是否是NaN
	    console.log(Number.isNaN(NaN));//true
	    console.log(Number.isNaN(5));//falsse
	
	    //Number.isInteger(i) : 判断是否是整数
	    console.log(Number.isInteger(5.23));//false
	    console.log(Number.isInteger(5.0));//true
	    console.log(Number.isInteger(5));//true
	
	    //Number.parseInt(str) : 将字符串转换为对应的数值
	    console.log(Number.parseInt('123abc'));//123
	    console.log(Number.parseInt('a123abc'));//NaN
	
	    // Math.trunc(i) : 直接去除小数部分
	    console.log(Math.trunc(13.123));//13
		
		```
	
## ES6
1. **2个新的关键字**
  * let/const
	  * 块作用域
	  * 没有变量提升
	  * 不能重复定义
	  * const值不可变
  
2. **变量的解构赋值**
  * 将包含多个数据的对象(数组)一次赋值给多个变量
  * 数据源: 对象/数组
  * 目标: {a, b}/[a, b]
  
  ```
  let obj = {name : 'kobe', age : 39};
  //对象的解构赋值
  let {age} = obj;
 
  let arr = ['abc', 23, true];
   //数组的解构赋值  不经常用
  let [a, b, c, d] = arr;
  
  function person({name, age}) {
     console.log(name, age);
  }
  person(obj);
  
  ```
  
3. 各种数据类型的扩展
  * 字符串
	    * **模板字符串** 
	      * 作用: 简化字符串的拼接
	      * 模板字符串必须用``
	      * 变化的部分使用${xxx}定义
	      	
	      	```
	     	let data = "aaa";
	     	
	     	console.log(`data: ${data}`)
	      	```
	    * includes(str) : 判断是否包含指定的字符串
	    * startsWith(str) : 判断是否以指定字符串开头
	    * endsWith(str) : 判断是否以指定字符串结尾
	    * repeat(count) : 重复指定次数
	    	
	    	```
	    	let str = 'abcdefg';
	    	console.log(str.includes('a'));//true
	    	console.log(str.startsWith('a'));//true
	    	console.log(str.endsWith('g'));//true
	    	console.log(str.repeat(5));
	       ```  
  * 对象
	    	* **简化的对象写法**
	      
		      ```
		      let name = 'Tom';
		      let age = 12;
		      let person = {
		          name,
		          age,
		          setName (name) {
		              this.name = name;
		          }
		      };
		      ```
	    	* Object.assign(target, source1, source2..) : 将源对象的属性复制到目标对象上
	    	* Object.is(v1, v2) : 判断2个数据是否完全相等
	   	 	* __proto__属性 : 隐式原型属性
	
		    	```
		    	console.log(Object.is('abc', 'abc'));//true
		    	console.log(NaN == NaN);//false
		    	console.log(Object.is(NaN, NaN));//true
		    	console.log(0 == -0);//true
		    	console.log(Object.is(0, -0));//false
		    	//Object.assign(target, source1, source2..)
		    	let obj = {name : 'kobe', age : 39, c: {d: 2}};
		    	let obj1 = {};
		    	Object.assign(obj1, obj);
		    	console.log(obj1, obj1.name);
		    	//直接操作 __proto__ 属性
		    	let obj3 = {name : 'anverson', age : 41};
		    	let obj4 = {};
		    	obj4.__proto__ = obj3;
		    	console.log(obj4, obj4.name, obj4.age);
		    	
		    	```
     
	* 数组
	    * Array.from(v) : 将伪数组对象或可遍历对象转换为真数组
	    * Array.of(v1, v2, v3) : 将一系列值转换成数组
	    * find(function(value, index, arr){return true}) : 找出第一个满足条件返回true的元素
	    * findIndex(function(value, index, arr){return true}) : 找出第一个满足条件返回true的元素下标
	    
	    ```
	    let btns = document.getElementsByTagName('button');
    	Array.from(btns).forEach(function (item, index) {
        console.log(item, index);
    	});
    	
    	let arr = Array.of(1, 'abc', true);
    	console.log(arr);
   
	    let arr1 = [1,3,5,2,6,7,3];
	    let result = arr1.find(function (item, index) {
	        return item >3
	    });
	    console.log(result);//5
	   
	    let result1 = arr1.findIndex(function (item, index) {
	        return item >3
	    });
	    console.log(result1);//2
	    ```
  * 函数
	    * **箭头函数**
	      * 用来定义匿名函数
	      * 基本语法:
	        * 没有参数: () => console.log('xxxx')
	        * 一个参数: i => i+2
	        * 大于一个参数: (i,j) => i+j
	        * 函数体不用大括号: 默认返回结果
	        * 函数体如果有多个语句, 需要用{}包围
	      * 使用场景: 多用来定义回调函数
	      * 箭头函数的特点：箭头函数没有自己的this，箭头函数的this不是调用的时候决定的，而是在定义的时候处在的对象就是它的this. 
	   **箭头函数的this看外层的是否有函数，如果有，外层函数的this就是内部箭头函数的this, 如果没有，则this是window**
   				
	    * **形参的默认值**
	      * 定义形参时指定其默认的值
	      
		      ```
		      function Point(x = 1,y = 2) {
			    	this.x = x;
			    	this.y = y;
			   }
		      ```
	      
	    * **rest(可变)参数**
	      * 通过形参左侧的...来表达, 取代arguments的使用
	      
		      ```
		      function fun(...values) {
			        console.log(arguments);
			        console.log(values);
			        values.forEach(function (item, index) {
			            console.log(item, index);
		      		})
    			}
    			fun(1,2,3);
		      ```
	      
	    * **扩展运算符(...)**
	      * 可以分解出数组或对象中的数据
	      
		      ```
		       let arr = [2,3,4,5,6];
	    		let arr1 = ['abc',...arr, 'fg'];
	    		console.log(arr1);
		      ```

4. set/Map容器结构
  * 容器: 能保存多个数据的对象, 同时必须具备操作内部数据的方法
  * 任意对象都可以作为容器使用, 但有的对象不太适合作为容器使用(如函数)
  * **Set的特点**: 保存多个value, value是不重复 ====>数组元素去重
  * **Map的特点**: 保存多个key--value, key是不重复, value是可以重复的
  * API
    	* Set()/Set(arr) //arr是一维数组
		    * add(value)
		    * delete(value)
		    * clear();
		    * has(value)
		    * size
		    	
		    	```
		    	let set = new Set([1,2,3,4,3,2,1,6]);
		    	
			    set.add('abc');
			    
			    set.delete(2);
			    
			    console.log(set.has(2));//false
			    console.log(set.has(1));//true
			    
			    set.clear();
		    	```
    	* Map()/Map(arr) //arr是二维数组
		    * set(key, value)
		    * delete(key)
		    * clear()
		    * has(key)
		    * size
		    	
		    	```
		    	let map = new Map([['abc', 12],[25, 'age']]);
		    	
			    map.set('男', '性别');
			    console.log(map.get(25));//age
			    
			    map.delete('男');
			    
			    console.log(map.has('男'));//false
			    console.log(map.has('abc'));//true
			    
			    map.clear();
		    	```
    
5. **for--of循环**
  * 可以遍历任何容器
  * 数组
  * 对象
  * 伪/类对象
  * 字符串
  * 可迭代的对象
  	
	  	```
	  	let arr = [1,2,3,4,5];
	    for(let num of arr){
	        console.log(num);
	    }
	    
	    let set = new Set([1,2,3,4,5]);
	    for(let num of set){
	        console.log(num);
	    }
	    
	    let str = 'abcdefg';
	    for(let num of str){
	        console.log(num);
	    }
	    
	    let btns = document.getElementsByTagName('button');
	    for(let btn of btns){
	        console.log(btn.innerHTML);
	    }
  	```
  
6. **Promise**
  * 解决`回调地狱`(回调函数的层层嵌套, 编码是不断向右扩展, 阅读性很差)
  * 能以同步编码的方式实现异步调用
  * 在es6之前原生的js中是没这种实现的, 一些第三方框架(jQuery)实现了promise
  * ES6中定义实现API: 
    
	    ```
	    // 1. 创建promise对象
	    var promise = new Promise(function(resolve, reject){ 
	      // 做异步的操作 
	      if(成功) { // 调用成功的回调
	        resolve(result); 
	      } else { // 调用失败的回调
	        reject(errorMsg); 
	      } 
	    }) 
	    // 2. 调用promise对象的then()
	    promise.then(function(
	      result => console.log(result), 
	      errorMsg => alert(errorMsg)
	    ))
	    ```

7. **generator函数**
	* ES6提供的解决异步编程的方案之一
  	* Generator函数是一个状态机, 内部封装了不同状态的数据
  	* 可暂停函数(惰性求值), yield可暂停，next方法可启动。每次返回的是yield后的表达式结果
  	* 调用next方法函数内部逻辑开始执行, 遇到yield表达式停止, 返回{value: yield后的表达式结果/undefined, done: false/true}
  	* yield语句返回结果通常为undefined, 当调用next方法时传参内容会作为启动时yield语句的返回值
    
	    ```
	    function* generatorTest() {
	      console.log('函数开始执行');
	      let argument = yield 'hello';
	      console.log(`argument: ${argument}`);//argument: bbb
	      console.log('函数暂停后再次启动');
	      yield 'generator';
	      console.log("遍历完毕");
	    }
	    
	    // 生成遍历器对象
	    let Gt = generatorTest();
	    // 执行函数，遇到yield后即暂停
	    console.log(Gt); // 遍历器对象
	    let result = Gt.next("aaa"); // 函数执行,遇到yield暂停
	    console.log(result); // {value: "hello", done: false}
	    result = Gt.next("bbb"); // 函数再次启动
	    console.log(result); // {value: 'generator', done: false}
	    result = Gt.next();
	    console.log(result); // {value: undefined, done: true}表示函数内部状态已经遍历完毕
	    ```

8. **async函数**
	* 概念: 真正意义上去解决异步回调的问题，同步流程表达异步操作
  	* 本质: Generator的语法糖
  	* 语法: 
      
      ```
      async function foo(){
        await 异步操作;
        await 异步操作；
      }
      ```
  	* 特点:
  		* 不需要像Generator去调用next方法，遇到await等待，当前的异步操作完成就往下执行
    	* 返回的总是Promise对象，可以用then方法进行下一步操作
       * async取代Generator函数的星号*，await取代Generator的yield
       * 语意上更为明确，使用简单
    
	    ```
	    async function awaitTest() {
	      let result = await Promise.resolve('执行成功');
	      console.log(result);
	      let result2 = await Promise.reject('执行失败');
	      console.log(result2);
	      let result3 = await Promise.resolve('还想执行一次');// 执行不了
	      console.log(result3);
	    }
	    awaitTest();
	    ```

7. **class类**
  * 用 class 定义一类
  * 用 constructor() 定义构造方法(相当于构造函数)
  * 一般方法: xxx () {}
  * 用extends来定义子类
  * 用super()来父类的构造方法
  * 子类方法自定义: 将从父类中继承来的方法重新实现一遍
  * js中没有方法重载(方法名相同, 但参数不同)的语法
  
	  	```
	  	class Person {
	        //调用类的构造方法
	        constructor(name, age){
	            this.name = name;
	            this.age = age;
	
	        }
	        //定义一般的方法
	        showName(){
	            console.log(this.name, this.age);
	        }
	    }
	    let person = new Person('kobe', 39);
	    person.showName();
	
	    //定义一个子类
	    class StrPerson extends Person{
	        constructor(name, age, salary){
	            super(name, age);//调用父类的构造方法
	            this.salary = salary;
	        }
	        showName(){//在子类自身定义方法
	            console.log(this.name, this.age, this.salary);
	        }
	    }
	    let str = new StrPerson('weide', 38, 1000000000);
	    console.log(str);
	    str.showName();
	  	```
  
8. **模块化(后面讲)**

## ES7
* 指数运算符: **
* Array.prototype.includes(value) : 判断数组中是否包含指定value

	```
	console.log(3 ** 3);//27
	let arr = [1, 2, 3, 4, 'abc'];
	console.log(arr.includes(2));//true
	console.log(arr.includes(5));//false
	```
  
  
* **区别方法的2种称谓**
  * 静态(工具)方法
    * Fun.xxx = function(){}
  * 实例方法
    * 所有实例对象 : Fun.prototype.xxx = function(){} //xxx针对Fun的所有实例对象
    * 某个实例对象 : fun.xxx = function(){} //xxx只是针对fun对象