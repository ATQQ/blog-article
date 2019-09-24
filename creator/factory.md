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