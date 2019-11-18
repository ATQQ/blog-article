# 抽象工厂模式

## 实体类
```js
class Bird {
    run() {
        console.log('bird');
    }
}

class Dog {
    run() {
        console.log('dog');
    }
}

class Male {
    run() {
        console.log("Male");
    }
}

class Female {
    run() {
        console.log("female");
    }
}

```

## 抽象工厂类与工厂类

```js
class AbstractFactory {
    getPerson() {
        return new Error("子类还未实现此接口");
    }

    getAnimal() {
        return new Error("子类还未实现此接口");
    }
}

class PersonFactory extends AbstractFactory {
    getPerson(person) {
        person = person.toLowerCase();
        switch (person) {
            case "male": return new Male();
            case "female": return new Female();
            default: break;
        }
    }

    getAnimal() {
        return null;
    }
}

class AnimalFactory extends AbstractFactory {
    getPerson() {
        return null;
    }

    getAnimal(animal) {
        animal = animal.toLowerCase();
        switch (animal) {
            case "dog": return new Dog();
            case "bird": return new Bird();
            default: break;
        }
    }
}
```

## 超级工厂

```js
class Factory {
    constructor(choice) {
        choice = choice.toLowerCase();
        switch (choice) {
            case "person": return new PersonFactory();
            case "animal": return new AnimalFactory();
            default: break;
        }
    }
}
```

## demo测试

```js
const personFactory = new Factory("person");
let male = personFactory.getPerson("male"),
    female = personFactory.getPerson("female");

male.run();
female.run();

const animalFactory = new Factory("animal");
let dog=animalFactory.getAnimal("dog"),
bird =animalFactory.getAnimal("bird");

dog.run();
bird.run();
```
---

## demo2

```js
//手机抽象工厂
class MobilePhoneFactory {
    // 提供操作系统接口
    createOs() {
        throw new Error('抽象工厂方法不允许直接调用，你需要将我重写!')
    }

    // 提供硬件接口
    createHardWare() {
        throw new Error('抽象工厂方法不允许直接调用，你需要将我重写!')
    }
}

// 操作系统
class OS {
    controlHardWare() {
        throw new Error('抽象工厂方法不允许直接调用，你需要将我重写!')
    }
}


class AndroidOS extends OS {
    controlHardWare() {
        console.log('我会用安卓的方式去操作硬件')
    }
}

class AppleOS extends OS {
    controlHardWare() {
        console.log('我会用苹果的方式去操作硬件')
    }
}

// 硬件产品
class HardWare {
    operateByOrder() {
        throw new Error('抽象工厂方法不允许直接调用，你需要将我重写!')
    }
}

class QualcommHardWare extends HardWare {
    operateByOrder() {
        console.log("我会用高通的方式去运转");
    }
}

class MiHardWare extends HardWare {
    operateByOrder() {
        console.log('我会用小米的方式去运转');
    }
}

// 山寨安卓手机
class FakeAndroidPhone extends MobilePhoneFactory {
    createHardWare() {
        return new MiHardWare();
    }

    createOs() {
        return new AndroidOS();
    }
}

const myPhone = new FakeAndroidPhone();
//拥有操作系统
const myOs = myPhone.createOs();
//拥有硬件
const myHandWare = myPhone.createHardWare();

// 启动操作系统
myOs.controlHardWare();

// 启动硬件
myHandWare.operateByOrder();
```