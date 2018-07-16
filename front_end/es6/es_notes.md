## **理解ES**
1. 全称: ECMAScript(European Computer Manufactures Association)
2. js语言的规范
3. 我们用的js是它的实现
4. JS的组成
  * ECMAScript(JS核心基础)
  * 扩展-->浏览器端
    * BOM
    * DOM
  * 扩展-->服务器端
    * Node.js
5. 版本
  1. ECMAScript 1: 与Netscape的JavaScript 1.1基本相同, 只是针对浏览器代码做了小的改动以及要求支持Unicode标准.
  2. ECMAScript 2: 主要是第一版编辑加工的结果, 与ISO/IEC-16262保持严格一致
  3. **ECMAScript 3**: 真正对标准第一次修改, 修改字符串处理、错误定义，增加正则、try-catch等支持. 第三版标志着ECMAScript成为一门真正的变成语言
  4. ECMAScript 4: 全面修订, 完全定义了一门新语言, 增加了强类型变量、真正的类和经典继承等
  5. **ECMAScript 5(09年)**: 基于第三版做微小改动(ES3.1), 增加了原生JSON对象、严格模式等
  6. **ECMAScript 6**: 即ECMAScript 2015, 新版本将按照ECMAScript+年份的形式发布, 每年6月份正式发布一次. 新增let/const、字符串模板等
  7. ECMAScript 7: ECMAScript 2016, 增加幂指运算符等, 变化不大, 草案居多
  8. ECMAScript 8: ECMAScript 2017, 新增Object.values/entries、字符串填充(str.padStart/padEnd)、async/await等
      
## ES5
1. **严格模式**
  * 严格模式: 为JS定义了一种不同的解析与执行模型
  * 应用上严格式: 'strict mode'; 这是一种编译知识, 用于告诉支持的JS引擎切换到严格模式
  * 作用: 
    * 使得Javascript在更严格的条件下运行
    * 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为
    * 消除代码运行的一些不安全之处，保证代码运行的安全
    * 几个重要变化
      * 声明定义变量必须用var
      * 不允许使用with语句
      * 修改arguments数组内容不影响命名参数的值, arguments只读(不允许重写)
      * 不能访问arguments.callee以及函数的caller
      * 未指定环境对象而调用函数, 函数内this值为undefined, 不会转型为window, 除非通过call/apply/bind绑定
      * 创建eval作用域, 更安全, eval外部不能访问eval内创建的变量/函数
      * 不能定义名为eval或arguments的变量/函数
      * 八进制字面量无效(如070), 导致JS引擎抛出错误
      
2. **JSON对象**
  * 作用: 用于在json对象/数组与js对象/数组相互转换
  * JSON.stringify(obj/arr)
      js对象(数组)转换为json对象(数组)
  * JSON.parse(json)
      json对象(数组)转换为js对象(数组)
      
3. Object扩展

  * Object.keys(obj): 返回对象obj上所有可枚举的实例属性(不包含原型链), for in循环则包含
	
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
  * **Object.create(prototype[, descriptors])** : 以指定对象为原型对象创建新的对象
    	
    	```
    	var obj = {name : 'curry', age : 29}
    	var obj1 = {};
    	obj1 = Object.create(obj, {
        sex : {
            value : 'male',
            writable : true
        	}
    	});
    	```
    * 指定新的属性, 并对属性进行描述
      * 数据属性
	      * value: 指定值
	      * writable: 标识当前属性值是否是可修改的, 默认为false
	      * configurable: 标识当前属性是否可以被删除/重新配置 默认为false
	      * enumerable: 标识当前属性是否能用for in 枚举 默认为false
      * 访问器属性
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
  * Object.getPrototypeOf : 返回[[Prototype]]的值, 即返回一个对象实例的原型对象, 等价于__proto__属性(Chrome/Firefox/Safari支持)
  
4. Array扩展
  * Array.prototype.indexOf(value) : 得到值在数组中的第一个下标
  * Array.prototype.lastIndexOf(value) : 得到值在数组中的最后一个下标
  * Array.prototype.every/some/filter/forEach/map/reduce/reduceRight
  * **Array.prototype.forEach(function(item, index){}) : 遍历数组**
  * **Array.prototype.map(function(item, index){}) : 遍历数组返回一个新的数组**
  * **Array.prototype.filter(function(item, index){}) : 遍历过滤出一个子数组**
  * Array.isArray(arr): 判断是否是数组
  
5. **Function扩展**
  * Function.prototype.bind(obj)
      * 将函数内的this绑定为obj, 并将函数返回
  * 区别bind()与call()和apply()?
      * fn.bind(obj, args) : 指定函数中的this, 并返回函数
      * fn.call(obj, args) : 指定函数中的this, 并调用函数
      * fn.apply(obj, [args]): 指定函数中的this, 并调用函数
      
6. Date扩展
  * Date.now() : 得到当前时间毫秒值

	
## ES6
1. **2个新的关键字**
  * let/const
	  * 块级作用域(ES5只有全局和函数作用域)
	  * 不会预处理, 没有变量提升
	  * 不能重复定义
	  * 暂时性死区(防止变量声明前就使用这个变量)
	  * const值不可变
  
2. **变量的解构赋值**
  * 将包含多个数据的对象(数组)一次赋值给多个变量
  * 数据源: 对象/数组
  * 对象: 变量必须与属性同名, 才能取到正确的值
  * 数组: 数组的元素是按次序排列的, 变量的取值由它的位置决定
  * 目标: {a, b}/[a, b]
  
  ```
  let obj = {name : 'joy', age : 30};
  //对象的解构赋值
  
 
  //name是匹配的模式, n才是变量
  let { name: n, age: a } = obj;
  n // 'joy'
  a // 30
  //简写
  let {age} = obj;
  //指定默认值
  let {x, y = 5} = {x: 1};
  x // 1
  y // 5
  
 
  let arr = ['abc', 23, true];
   let [a, b, c, d] = arr;
  //指定默认值, 数组的解构赋值(ES6内部使用严格相等运算符(===)判断一个位置是否有值. 只有当一个数组成员严格等于undefined, 默认值才会生效)
  let [x, y = 'b'] = ['a'];    // x='a', y='b'
  let [x = 1, y = x] = [];     // x=1; y=1
  let [x = 1, y = x] = [2];    // x=2; y=2
  let [x = 1, y = x] = [1, 2]; // x=1; y=2
  
  function person({name, age}) {
     console.log(name, age);
  }
  person(obj);
  
  ```
  
3. 各种数据类型的扩展
  * 字符串
	    * **模板字符串** 
	      * 作用: 简化字符串的拼接, 保留字符格式(所有的空格和缩进都会被保留在输出之中)
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
 
  * 数值Number扩展

	    * 二进制与八进制数值表示法: 二进制用0b, 八进制用0o
	    * Number.isFinite(i): 判断是否是有限大的数
	    * Number.isNaN(i): 判断是否是NaN
	    * Number.isInteger(i): 判断是否是整数
	    * Number.parseInt(str): 将字符串转换为对应的数值
	    * Number.parseFloat(str): 
	    * Math.trunc(i): 直接去除小数部分

  * 对象
      *  **简化的对象写法**
	      
		      ```
		      let name = 'Tom';
		      let age = 12;
		      
		      //普通写法
		      let obj = {
        			name: name,
        			age: age,
        			setName: function (name) {
            			this.name = name;
       	 		}
    			};
		      //简化写法
		      let person = {
		          name,
		          age,
		          setName(name) {
		              this.name = name;
		          }
		      };
		      
		      //如果是Generator函数, 前面需要加上*
		      
		   
		      ```
		      
	  * 属性名表达式: 将表达式放在方括号内
	  			
	  			```
	  			let propKey = 'foo';

				let obj = {
				  [propKey]: true,
				  ['a' + 'bc']: 123
				};
	  			
	  			```
	  			
	  * Object.assign(target, source1, source2...): 将源对象的所有可枚举自身属性(不包含继承属性)复制到目标对象上(注: 浅拷贝, 即拷贝属性值的引用)
	  * Object.is(v1, v2): 判断2个数据是否完全相等[参考](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is), 与严格比较运算符(===)的行为基本一致
	  * __proto__属性: 隐式原型属性, ES标准规定只有浏览器必须部署这个属性，其他运行环境不一定需要部署, 最好不用, 使用Object.setPrototypeOf写操作、Object.getPrototypeOf读操作、Object.create生成操作 代替
	
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
	    * Array.of(v1, v2, v3) : 将一系列值转换成数组(弥补数组构造函数Array()的不足)
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
	    
	    * **扩展运算符(...)**
	      * 可以分解出数组或对象中的数据
	      
		      ```
		       let arr = [2, 3, 4, 5, 6];
	    		let arr1 = ['abc', ...arr, 'fg'];
	    		console.log(arr1);
	    		
	    		let arr1 = [0, 1, 2];
				let arr2 = [3, 4, 5];
				arr1.push(...arr2);
	    		
		      ```

  * 函数
	    * **箭头函数**
	      * 用来定义匿名函数
	      * 基本语法:
		        * 没有参数: () => console.log('xxxx')
		        * 一个参数: i => i+2
		        * 大于一个参数: (i, j) => i + j
		        * 函数体不用大括号: 默认返回结果
		        * 函数体如果有多个语句, 需要用{}包围
	      * 使用场景: 多用来定义回调函数
	      * 箭头函数的特点: 箭头函数没有自己的this，箭头函数的this不是调用的时候决定的，而是在**定义**的时候处在的对象就是它的this.
	      * **箭头函数的this看外层的是否有函数, 如果有, 外层函数的this就是内部箭头函数的this, 如果没有, 则this是window**
	      * 如果箭头函数在其他函数的内部, 它也将共享该函数的arguments变量. 不可以使用arguments对象, 该对象在函数体内不存在. 如果要用, 可以用 rest 参数代替
	      * 不可以使用yield语句, 即不能用作Generator函数
	      
	      ```
	      function square() {
  				let example = () => {
    			let numbers = [];
			    for (let number of arguments) {
			      numbers.push(number);
    			}

    			return numbers;
  			};

		  	return example();
			}

			square(2, 4, 7.5, 8); // returns: [2, 4, 7.5, 8]
	      
	      ```
   				
	    * **形参的默认值**
	      * 定义形参时指定其默认的值
	      
		      ```
		      function Point(x = 1,y = 2) {
			    	this.x = x;
			    	this.y = y;
			   }
		      ```
	      
	    * **rest(可变)参数**
	      * 通过形参左侧的...来表达, 取代arguments的使用(arguments是伪数组, rest是真正的数组), 只能是最后部分形参参数
	      
		      ```
		      function fun(...values) {
			        console.log(arguments);
			        console.log(values);
			        //arguments伪数组, 没有forEach方法
			        values.forEach(function (item, index) {
			            console.log(item, index);
		      		})
    			}
    			fun(1,2,3);
		      ```
	      
	   * 严格模式
	      * 只要函数参数使用了默认值、解构赋值、或者扩展运算符, 那么函数内部就不能显式设定为严格模式, 否则会报错
	      
		      ```
		     // 报错
				function doSomething(a, b = a) {
				  'use strict';
				  // code
				}
				
				// 报错
				const doSomething = function ({a, b}) {
				  'use strict';
				  // code
				};
				
				// 报错
				const doSomething = (...a) => {
				  'use strict';
				  // code
				};
				
				const obj = {
				  // 报错
				  doSomething({a, b}) {
				    'use strict';
				    // code
				  }
			};
			
				//reason: use strict同时适用于函数参数和函数体
				// 报错
				function doSomething(value = 070) {
				  'use strict';
				  return value;
				}
			
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
  * 可以便利Map容器、Set容器、数组、字符串、arguments等可迭代的对象(具有iterator接口: 调用next()方法返回{value: xxx, done: bool})
  	
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
  * promise对象的3个状态
  		* pending: 初始化状态
 	 	* fullfilled: 成功状态
 	 	* rejected: 失败状态
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
	    promise.then(
	      result => console.log(result), 
	      errorMsg => alert(errorMsg)
	    )
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
	      return "result"
	    }
	    
	    // 生成遍历器对象
	    let Gt = generatorTest();
	    // 执行函数，遇到yield后即暂停
	    console.log(Gt); // 遍历器对象generatorTest
	    let result = Gt.next("aaa"); // 函数执行,遇到yield暂停
	    console.log(result); // {value: "hello", done: false}
	    result = Gt.next("bbb"); // 函数再次启动
	    console.log(result); // {value: 'generator', done: false}
	    result = Gt.next();
	    console.log(result); // {value: "result", done: true}表示函数内部状态已经遍历完毕
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
  		* 不需要像Generator去调用next方法, 遇到await等待, 当前的异步操作完成就往下执行
    	* 返回的总是Promise对象, 可以用then方法进行下一步操作
       * async取代Generator函数的星号*, await取代Generator的yield
       * 语意上更为明确, 使用简单
    
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
  * 用 class 定义类/实现类的继承
  * 用 constructor() 定义构造方法(相当于构造函数)
  * 一般方法: xxx () {}
  * 用extends来定义子类
  * 用super()来父类的构造方法
  * 子类方法自定义: 将从父类中继承来的方法重新实现一遍
  * js中没有方法重载(方法名相同, 但参数不同)的语法
  
	  	```
	  	class Person {
	        //调用类的构造方法
	        constructor(name, age) {
	            this.name = name;
	            this.age = age;
	        }
	        //定义一般的方法
	        showName() {
	            console.log(this.name, this.age);
	        }
	    }
	    let person = new Person('kobe', 39);
	    person.showName();
	
	    //定义一个子类
	    class StrPerson extends Person {
	        constructor(name, age, salary) {
	            super(name, age);//调用父类的构造方法
	            this.salary = salary;
	        }
	        showName() {//在子类自身定义方法
	            console.log(this.name, this.age, this.salary);
	        }
	    }
	    let str = new StrPerson('weide', 38, 1000000000);
	    console.log(str);
	    str.showName();
	  	```
  
8. **模块化**

## ES7
* 指数运算符: **
* Array.prototype.includes(value, start) : 判断数组中是否包含指定value

	```
	console.log(3 ** 3);//27
	let b = 4;
	b **= 3; //等同于 b = b * b * b;
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