# 工厂模式

```js
class Bird{
    run(){
        console.log('bird');
    }
}

class Dog{
    run(){
        console.log('dog');
    }
}

class Animal {
    constructor(name){
        switch(name.toLowerCase()){
            case 'dog':return new Dog();
            case 'bird':return new Bird();
            default:new Error("not defined");
        }
    }
}

let dog=new Animal('dog');
dog.run();//dog
```

## demo2
```js
class User {
    constructor(name, age, career, work) {
        this.name = name;
        this.age = age;
        this.career = career;
        this.work = work;
    }
}
class Factory {
    constructor(name, age, career) {
        let work;
        switch (career.toLowerCase()) {
            case 'coder':
                work = ['写代码', '修bug']
                break;
            case 'boss':
                work = ['喝茶', '看包', '见客户']
                break;
                // ...more
        }

        return new User(name, age, career, work);
    }
}

let Boss=new Factory('小米',34,'boss');
```