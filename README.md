# Lambda.java (By using new feature lambda expression in java 8 implemented arrays , lists, bifunctional and collectors)

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.function.BiFunction;
import java.util.stream.Collectors;

public class Lambda {

    @FunctionalInterface
    public interface FooConsumer<T, U, S> {

        void accept(T t, U u, S s);
    }

    public class Person {

        private String name;
        private List<String> addresses;
        private int age;

        public Person(String name, List<String> addresses, int age) {
            this.name = name;
            this.addresses = addresses;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public List<String> getAddresses() {
            return addresses;
        }

        public void setAddresses(List<String> addresses) {
            this.addresses = addresses;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        @Override
        public String toString() {
            return name + " (" + age + ")";
        }

    }

    public static void main(String[] args) {
        Lambda app = new Lambda();

        app.someMethod();
        app.someOtherMethod();
        app.someLambdaMethodCaller();
        app.SomeLambdaGeneratorCaller();
        app.optionalExample();

        app.mapExample();
        app.flatMapExample();
        app.filterExample();
        app.skipExample();
        app.limitExample();
        app.distinctExample();
        app.sortExample();
        app.reduceExample();
    }

    public void someMethod() {
        FooConsumer<String, String, String> foo = (a, b, c) -> {
            System.out.println(a + ", " + b + " and " + c);
        };

      
        foo.accept("One", "Two", "Three");

        
        foo.accept("Six", "Nine", "Ten");
    }

    public void someOtherMethod() {
        BiFunction<Integer, Integer, String> foo = (a, b) -> {
            return "The product of " + a + " and " + b + " is " + (a * b);
        };

        String foo1 = foo.apply(5, 10);
        String foo2 = foo.apply(3, 5);


        System.out.println(foo1);

      
        System.out.println(foo2);
    }

    public void someLambdaMethodExecutor(BiFunction<Integer, Integer, Integer> fn) {
        System.out.println("Result of fn(1, 2) is " + fn.apply(1, 2));
    }

    public void someLambdaMethodCaller() {
        BiFunction<Integer, Integer, Integer> add = (a, b) -> {
            return a + b;
        };
        
        BiFunction<Integer, Integer, Integer> multiply = (a, b) -> a * b;

        someLambdaMethodExecutor(add);

  
        someLambdaMethodExecutor(multiply);
    }

    public BiFunction<Integer, Integer, Integer> SomeLambdaGenerator(String which) {
        if ("add".equals(which))
            return (a, b) -> {
                return a + b;
            };
        else
            return (a, b) -> {
                return a * b;
            };
    }

    public void SomeLambdaGeneratorCaller() {
        BiFunction<Integer, Integer, Integer> add = SomeLambdaGenerator("add");
        BiFunction<Integer, Integer, Integer> multiply = SomeLambdaGenerator("multiply");

        
        System.out.println("1 + 2 = " + add.apply(1, 2));

        System.out.println("1 * 2 = " + multiply.apply(1, 2));
    }

    public void optionalExample() {
        Optional<String> someString = Optional.ofNullable("I'm here!");

    
        Optional<String> someAbsentString = Optional.ofNullable(null);

        
        someString.ifPresent(str -> System.out.println("Some String: " + str));

        
        someAbsentString.ifPresent(str -> System.out.println("Some Absent String: " + str));

        
        System.out.println("someAbsentString present? " + (someAbsentString.isPresent() ? "Yes" : "No"));

    }
    
    public void mapExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        List<Integer> mapped = numbers.stream().map(number -> number * 5).collect(Collectors.toList());

        System.out.println(mapped);
    }

    public void flatMapExample() {
        Person john = new Person("John", Arrays.asList("John's Home", "John's Office"), 26);
        Person mary = new Person("Mary", Arrays.asList("Mary's Home"), 25);

        List<Person> people = new ArrayList<Person>(Arrays.asList(john, mary));

        
        List<List<String>> incorrect = people.stream().map(person -> person.getAddresses()).collect(Collectors.toList());

      
        List<String> allAddresses = people.stream().flatMap(person -> person.getAddresses().stream()).collect(Collectors.toList());

        
        System.out.println(incorrect);

        
        System.out.println(allAddresses);
    }

    public void filterExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        List<Integer> odds = numbers.stream().filter(number -> number % 2 == 1).collect(Collectors.toList());

      
        System.out.println(odds);
    }

    public void skipExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        List<Integer> remainder = numbers.stream().skip(3).collect(Collectors.toList());

       
        System.out.println(remainder);
    }

    public void limitExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        List<Integer> taken = numbers.stream().limit(3).collect(Collectors.toList());

    
        System.out.println(taken);
    }

    public void distinctExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 1, 2, 3, 5));
        List<Integer> distinct = numbers.stream().distinct().collect(Collectors.toList());

        System.out.println(distinct);
    }

    public void sortExample() {
        Person john = new Person("John", Arrays.asList("John's Home", "John's Office"), 26);
        Person mary = new Person("Mary", Arrays.asList("Mary's Home"), 25);
        Person sean = new Person("Sean", Arrays.asList("Sean's Home"), 33);
        List<Person> people = new ArrayList<Person>(Arrays.asList(john, mary, sean));

      
        List<Person> sorted = people.stream().sorted((p1, p2) -> p2.getAge() - p1.getAge()).collect(Collectors.toList());

       
        System.out.println(sorted);
    }

    public void reduceExample() {
        List<Integer> numbers = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        int sum = numbers.stream().reduce(50, (a, b) -> a + b);

        
        List<Integer> asList = numbers.stream().reduce(new ArrayList<Integer>(), (list, number) -> {
            list.add(number);
            return list;
        }, (list1, list2) -> {
            list1.addAll(list2);
            return list1;
        });

        
        System.out.println(sum);

       
        System.out.println(asList);
    }
}
