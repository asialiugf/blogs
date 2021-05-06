### class
``` js

class Car { 
    // 字段 
    public engine:string = "eeeeeee"; 
    public x:string = "rrreee";
    public a:number = 9 ;
 
    // 构造函数 
    constructor(engine:string) { 
        this.engine = engine 
    }  
 
    // 方法 
    disp():void { 
        console.log("发动机为 :   "+this.engine) 
    } 
}

class redcar extends Car {
    color:string = "red";
    haha:number;
    constructor(tt:string,x:number){   /*  构造函数的参数可以 包含父类的参数 */
        super(tt);
        this.haha = x;
        this.engine = tt ;
        //super();
    }
    showcolor():void {
        console.log(this.color);
        console.log(this.engine);
        console.log(this.x);
    }
}

class bluecar extends Car{             /*  可以不写构造函数  */
    showcolor():void {
        console.log("bluecar:");
        console.log(this.x) ;
        console.log(this.engine) ;
    }
}

class greencar extends Car {
    y:number = 5;
    constructor(y:number) {            /*  构造函数的参数可以不包含父类的参数 */
        super("greenengine");          /*  super() 必须首先调用 ！ */
        this.y = y ;
    }
    showcolor():void  {
        console.log(" ");
        console.log("greencar:");
        this.disp();                   /* this.disp() 和 super.disp() 一样 */
        super.disp();
        this.y = this.y + this.a ;
        //this.y = this.y + super.a ;  /*不能使用 super.a ! 只能使用 this.a */
        console.log(this.y);
    }
}

let obj1 = new Car("Engine1");
let obj2 = new Car("Engine2");
obj2.x = "rrr";

console.log(obj1);
console.log(obj2);
console.log(`${obj2.x}`);
obj1.disp();
obj2.disp();

let mycar = new redcar("kkkk",5);
mycar.showcolor();
let hiscar = new bluecar("tttt") ;
hiscar.showcolor();


let hercar = new greencar(100) ;
hercar.showcolor();
hercar.y = hercar.y + 20;
console.log(hercar.y);

```
### result 
```
[Running] ts-node "c:\ts\test01.ts"
Car { engine: 'Engine1', x: 'rrreee', a: 9 }
Car { engine: 'Engine2', x: 'rrr', a: 9 }
rrr
发动机为 :   Engine1
发动机为 :   Engine2
red
kkkk
rrreee
bluecar:
rrreee
tttt
 
greencar:
发动机为 :   greenengine
发动机为 :   greenengine
109
129

[Done] exited with code=0 in 1.146 seconds


```
