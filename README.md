
class Animal {

    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println(name + " is barking.");
    }
}

public class Main {

    public static void main(String[] args) {

        // Create a Dog object
        Dog myDog = new Dog();

        // Assign a name to the dog
        myDog.name = "Buddy";

        // Call the inherited eat() method
        myDog.eat();

        // Call the Dog class bark() method
        myDog.bark();
    }
}
