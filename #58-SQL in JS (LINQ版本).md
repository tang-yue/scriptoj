### [#58 SQL in JS (LINQ版本)](http://scriptoj.mangojuice.top/problems/58)

----
题目描述：

在 JavaScript 中实现类似 LINQ 风格的数据查询语法。具体来说，就是用：

1、 `from()` 表示数据源，可以有多个不同的数据源，将会进行笛卡尔乘积
2、 `select()` 表示数据投影（选择）
3、`where()` 表示过滤条件，可以组合多个，多个 `where` 之间是 `AND` 连接，`where` 中多个参数是 `OR` 连接的
4、`groupBy()` 表示分组条件（相同结果就会合成一组，可以同时指定好多分组条件，结果将是嵌套分组的）
5、`having()` 表示从分组结果中进行选择（选出特定的分组）类似与 `where`
以上所有函数都需要链式调用风格，但是无需考虑中间断开保存为某个变量的情形，因此放心的用闭包即可。

使用例子：


```
const somenumbers = [1, 2, 3]
query().select().from(somenumbers).execute(); //[1, 2, 3]
query().from(somenumbers).select().execute(); //[1, 2, 3] execute前顺序无关哦

//同样也适用于对象数组
var persons = [
  {name: '彼得', profession: '教师', age: 20, maritalStatus: '已婚'},
  {name: '迈克尔', profession: '教师', age: 50, maritalStatus: '未婚'},
  {name: '彼得', profession: '教师', age: 20, maritalStatus: '已婚'},
  {name: '安娜', profession: '科学家', age: 20, maritalStatus: '已婚'},
  {name: '露丝', profession: '科学家', age: 50, maritalStatus: '已婚'},
  {name: '安娜', profession: '科学家', age: 20, maritalStatus: '未婚'},
  {name: '安娜', profession: '政治家', age: 50, maritalStatus: '已婚'}
];
query().select().from(persons).execute(); // [{name: '彼得',...}, {name: '迈克尔', ...}]

function profession(person) {
  return person.profession;
}

//也可以做一些投影操作
query().select(profession).from(persons).execute(); //["教师", "教师", "教师", "科学家", "科学家", "科学家", "政治家"]

//除了where和having之外，其他的操作不允许重复写
query().select().select().execute(); //Error('Duplicate SELECT');
query().select().from([]).select().execute(); //Error('Duplicate SELECT');
query().select().from([]).from([]).execute(); //Error('Duplicate FROM');
query().select().from([]).where([]).where([]) //这表示AND连接两个条件

//你可以省略execute前的任何一部分
query().select().execute(); //[]
query().from(somenumbers).execute(); // [1, 2, 3]
query().execute(); // []

//需要支持where语法
function isTeacher(person) {
  return person.profession === 'teacher';
}

//SELECT profession FROM persons WHERE profession="teacher" 
query().select(profession).from(persons).where(isTeacher).execute(); //["教师", "教师", "教师"]

//SELECT * FROM persons WHERE profession="teacher" 
query().select().from(persons).where(isTeacher).execute(); //[{person: '彼得', profession: '教师', ...}, ...]

function name(person) {
  return person.name;
}

//SELECT name FROM persons WHERE profession="teacher" 
query().select(name).from(persons).where(isTeacher).execute();//["彼得", "迈克尔", "彼得"]

//还需要支持groupBy语法
//SELECT * FROM persons GROUP BY profession <- Bad in SQL but possible in this kata
query().select().from(persons).groupBy(profession).execute(); 
[
  ["教师",
     [
       {
        name: "彼得",
        profession: "教师"
        ...
      },
      {
        name: "迈克尔",
        profession: "教师"
        ...
      }
    ]
  ],
  ["科学家",
    [
       {
          name: "安娜",
          profession: "科学家"
        },
     ...
   ]
  ]
  ...
]
//where和groupBy可以同时存在
//SELECT * FROM persons WHERE profession='teacher' GROUP BY profession
query().select().from(persons).where(isTeacher).groupBy(profession).execute();

//或者，你可能需要select(下面这个例子演示了如何模拟select unique)
function professionGroup(group) {
  return group[0];
}

//SELECT profession FROM persons GROUP BY profession
query().select(professionGroup).from(persons).groupBy(profession).execute(); //["教师","科学家","政治家"]

//分组不一定要从对象数组里分
function isEven(number) {
  return number % 2 === 0;
}

function parity(number) {
  return isEven(number) ? '偶数' : '奇数';
}

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]; 

//SELECT * FROM numbers
query().select().from(numbers).execute(); //[1, 2, 3, 4, 5, 6, 7, 8, 9]

//SELECT * FROM numbers GROUP BY parity
query().select().from(numbers).groupBy(parity).execute(); //[["奇数",[1,3,5,7,9]],["偶数",[2,4,6,8]]]

//你甚至可以嵌套多个分组
function isPrime(number) {
  if (number < 2) {
    return false;
  }
  var divisor = 2;
  for(; number % divisor !== 0; divisor++);
  return divisor === number;
}

function prime(number) {
  return isPrime(number) ? '素数' : '合数';
}

//SELECT * FROM numbers GROUP BY parity, isPrime
query().select().from(numbers).groupBy(parity, prime).execute(); // [["奇数",[["合数",[1,9]],["素数",[3,5,7]]]],["偶数",[["素数",[2]],["合数",[4,6,8]]]]]

//都有groupby了，怎么能少了having
function odd(group) {
  return group[0] === 'odd';
}

query().select().from(numbers).groupBy(parity).having(odd).execute(); //[["奇数",[1,3,5,7,9]]]

//排序也是很重要滴

function descendentCompare(number1, number2) {
  return number2 - number1;
}

//SELECT * FROM numbers ORDER BY value DESC 
query().select().from(numbers).orderBy(descendentCompare).execute(); //[9,8,7,6,5,4,3,2,1]
 
//from需要支持多个不同来源
var teachers = [
  {
    teacherId: '1',
    teacherName: '彼得'
  },
  {
    teacherId: '2',
    teacherName: '安娜'
  }
];


var students = [
  {
    studentName: '迈克尔',
    tutor: '1'
  },
  {
    studentName: '露丝',
    tutor: '2'
  }
];

function teacherJoin(join) {
  return join[0].teacherId === join[1].tutor;
}

function student(join) {
  return {studentName: join[1].studentName, teacherName: join[0].teacherName};
}

//SELECT studentName, teacherName FROM teachers, students WHERE teachers.teacherId = students.tutor
query().select(student).from(teachers, students).where(teacherJoin).execute(); //[{"studentName":"迈克尔","teacherName":"彼得"},{"studentName":"露丝","teacherName":"安娜"}]

//where和having可以支持多个参数，表示OR连接
function tutor1(join) {
  return join[1].tutor === "1";
}

//SELECT studentName, teacherName FROM teachers, students WHERE teachers.teacherId = students.tutor AND tutor = 1 
query().select(student).from(teachers, students).where(teacherJoin).where(tutor1).execute(); //[{"studentName":"迈克尔","teacherName":"彼得"}] <- AND 连接

function lessThan3(number) {
  return number < 3;
}

function greaterThan4(number) {
  return number > 4;
}

//SELECT * FROM number WHERE number < 3 OR number > 4
query().select().from(numbers).where(lessThan3, greaterThan4).execute(); //[1, 2, 5, 7] <- OR连接

function greatThan1(group) {
  return group[1].length > 1;
}

function isPair(group) {
  return group[0] % 2 === 0;
}

function id(value) {
  return value;
}

function frequency(group) {
  return { value: group[0], frequency: group[1].length };      
}

//SELECT number, count(number) FROM numbers GROUP BY number HAVING count(number) > 1 AND isPair(number)
query().select(frequency).from(numbers).groupBy(id).having(greatThan1).having(isPair).execute(); // [{"value":2,"frequency":2},{"value":6,"frequency":2}])
```

----
参考答案：


```
const query = (()=>{
    const m={'from':{'once':true,'index':0,'mReduce':(dataArr,arr)=>arr.concat(
            dataArr.length<2?[].concat(...dataArr):
            dataArr.reduce(
                (data,arr)=>[].concat(...data.map(v=>arr.map(a=>v.concat([a])))),
            [[]]))},
        'where':{'once':false,'index':1,'mReduce':(fArr,arr)=>arr.filter(v=>fArr.some(f=>f(v)))},
        'groupBy':{'once':true,'index':2,'mReduce':(fArr,arr)=>
            arr.reduce((a,v)=>(fArr.reduce((items,f)=>(
                (items.find(item=>item[0]==f(v)) || (items[items.length]=[f(v),[]]))[1]
            ),a).push(v),a),[])},
        'having':{'once':false,'index':3,'mReduce':(fArr,arr)=>arr.filter(v=>fArr.some(f=>f(v)))},
        'orderBy':{'once':true,'index':4,'mReduce':(fArr,arr)=>fArr.reduceRight((a,f)=>a.sort(f),arr)},
        'select':{'once':true,'index':5,'mReduce':(fArr,arr)=>fArr.reduce((a,f)=>a.map(f),arr)}};
    return () => 
        new(function(){
            var doArr=[];
            for(let method in m)
                this[method]=(...dataArr)=>{
                    if(m[method].once && doArr.some(x=>x[0]==method))
                        throw new Error('Duplicate '+method.toUpperCase());
                    doArr.push([method,dataArr]);
                    return this;
                }
            this.execute=()=>
                doArr.sort((a,b)=>m[a[0]].index-m[b[0]].index)
                    .reduce((d,f)=>f[1].length?m[f[0]].mReduce(f[1],d):d,[])
        })()
})()
```


