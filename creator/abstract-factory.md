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