### Single responsibility:  

> A class should only have one responsibility
  
Imagine we have a journal class with methods such as "add_entrty" and "save_to_file".If we keep adding functionalities to this class , managing it would become harder and harder and we will create sth called God object.  
To prevent this we break the class to smaller classes that each have only one responsibility. For example we can break the Journal class to JournalEntry and JournalPersistance. This way our code is simpler to read and add functionalities.  
  
  
### Open for extension - Closed for modification  
  
>We should write our code in a way that if we decide to add new functionalities, we would do it  by extension of currently available classes not modification of them.  

For example lets say we want to filter products. by following this rule we create a BaseFilter class with a method filter. Now when ever we want to support a new filter type such as FilterBySize, FilterByColor ..., we just extend the base class and there is no need for modification.  
  
  
### Liskov substitution principle  
  
> If S is subtype of T. Then objects of type S should be valid where the objects of type t are valid.  
  
If we have a class Vehicle and another class VehicleWithDoors that inherits the Vehicle class. Then where ever the Vehicle objects work fine then the VehicleWithDoors should also work fine  
  
  
### Interface Segregation  
  
> states that clients should not be forced to implement interfaces they don't use.  Which could be achieved by creating interfaces that are suited to specific task rather than one general-purpose interface 

For example the below code is not a good practice because its forcing the OldPrinter to implement the fax method while it does not support it.(we should raise not implemented)

```python
class BasePrinter:
    def print():
        pass

    def fax():
        pass

  
class OldPrinter(BasePrinter)
    pass

class ModernPrinter(BasePrinter):
    pass
```

To fix this we make each of the functionalities such as Print and Fax their own class.
```python
class Printer():
    def print(self, document):
        pass

class Fax():
    def fax(self, document):
        pass

class OldPrinter(Printer):
    pass

class ModernPrinter(Printer, Fax):
    pass
```
### Dependency inversion  
  
>high level modules should not depend on low level modules; both should depend on abstractions. Abstractions should not depend on details. Details should depend upon abstractions.  

In the below example the signup class is dependent on the Mongodb class and the details of it. if we change the Mongodb database to a Mysql the signup method in the Login would fail.

```python
class Mongodb:
    def add_with_mongo_expression(self):
        pass

class Login:
    def __init__(self,) -> None:
        dataBase = Mongodb()

    def signup(self):
        self.database.add_with_mongo_expression()
        pass
```
The Login class should be dependent on the abstraction so we can change the database and nothing would fail.

```python
class DatabaseBase:
    def add(self):
        pass

class Mysql(DatabaseBase):
    def add(self):
        pass

class Mongodb(DatabaseBase):
    def add(self):
        pass

class Login:
    def __init__(self,database:DatabaseBase) -> None:
        dataBase = dataBase

    def signup(self):
        self.database.add()
        pass
```


### Builder:

 A design pattern that helps us construct complex objects step by step.
 consists of:
 * Object: The object that we are trying to build
 * Builder : Implements the steps needed to construct the object 
 * Director: Uses the builder methods to construct the object


```python
class User:
    def __init__(self):
        self.email = None
        self.password = None
        self.admin = False

class UserBuilder:
    def set_email(self):
        pass
    def set_password(self):
        pass
    def set_admin(self):
        pass

class AdminUserBuilder(UserBuilder):
    def set_email(self):
        pass
    def set_password(self):
        pass
    def set_admin(self):
        #default to TRUE
        pass

class Director:
    def construct(self, builder):
        builder.set_email()
        builder.set_password()
        builder.set_admin()
        return builder

# Client code
admin_builder = AdminUserBuilder()
director = Director()
admin1 = director.construct(admin_builder)
```

### Factory:

Helps with wholesale creation of objects.
it can be implemented by a single static method inside a the class. but its a better practice to implement a separate class for it.

```python
class Point():
    pass

class PointFactory():
    @staticmethod
    def create_cartesian_point(x,y):
        # returns a Point object
        pass
    @staticmethod
    def create_polar_point(r,thet):
        # returns a Point object
        pass
```


### Prototype:

Lets you to create a copy of an existing objects

```python
class PrototypeMixin():
    def clone():
        # returns a deep copy
        pass
        
class PointShape(PrototypeMixin):
    pass
    
```

### Adapter:

Helps with adapting the interface we have to the interface we need. It can be useful when we don't have access to the original class. For example when we use a third party library.

```python
class PointLibrary():
    def create_point(self,x,y):
        # return deep copy
        pass
        
# the library helps us build points using the cartesian coordinates
# but we want to use polar coordinates in our program

class PointLibraryAdapter(PointLibrary):
    def create_point(self,r, theta):
        pass
```


### Singleton:

It's a class that can only have one object. It would return a reference to the same object if we try to create new one.

```python
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]
```


### Bridge:

Helps to prevent the cartesian complexity.
for example lets say we want to implement shapes with different colors. one way of doing it is implementing different class for each combonation. for example for shapes of Circle and Square with colors of Blue and Red we would get:
Class BlueSquare
Class RedSquare
Class BlueCircle
Class RedCircle
it's clearly not a good practice since we would get a really complex code by adding more colors and shapes.
what we can do instead is making the shapes to contain the colors.

```python
class ColorAbstract():
    pass
    
class RedColor(ColorAbstract):
    pass
    
class BlueColor(ColorAbstract):
    pass

  
class ShapeAbstract():
    def __init__(self,color) -> None:
        self.color = ColorAbstract()
        
class Square(ShapeAbstract)
    pass
    
class Circle(ShapeAbstract):
    pass

red = RedColor()
red_square = Square(color=red)
```

### Composite:

groups objects and helps to treat them in an uniform manner.
It can also to be used to create an hierarchy of objects which each child object can also have its own children.

```python
class MenuItem:
    def action():
        pass
  
class MenuItemComposite(MenuItem):
    def __init__(self) -> None:
        self.children = []
  
    def add_child()
        pass

    def remove_child():
        pass

    def list_children():
        pass
  

menu_item = MenuItemComposite()
sub_menu = MenuItemComposite()
menu_item.add_child(sub_menu)
```


### Facade:

Helps to create an easy to use **user-interface** for a large body of code.


```python
  
class Journal:
    pass
    
class Task:
    pass
    
class Project:
    pass
    
class Habit:
    pass
  
class JournalFacade
    def add_task()
        pass

    def remove_entry():
        pass

    def list_project():
        pass

    def add_subtask():
        pass
```

### Flyweight:

helps to optimize the space by externally saving the data of the similar objects
in the below example some bullet data can become repetitive between so many objects of bullets in the game and wase a lot of ram. so we put them in the BulletType class and create a flyweight for it so many bullets can reference it.

```python
  
class BulletType:
    pass
    
class Bullet:
    pass
    
class BulletFlyweight:
    def __init__(self) -> None:
        self.types = {}

    def get_or_create(self,type):
        if type in self.types.keys():
            return self.types['type']

```

### Proxy:

controls the access to an object. its especially useful when we don't have direct access to the class. for example when using an third party package


```python
  
class TranslatorLibrary:
    pass
    
class TranslatorProxy(TranslatorLibrary):
    def translate(self,word):
        if word in cached_words_database:
            return cached_word_database['word']
        else:
            return super().translate(self,word)
    


```


### Chain of responsibility:

A request sent to a chain of handlers that process it and pass it to the next one.( Imagine the middlewares in web frameworks)  
  

```python
  
class NewHandler():

    _next_handler: AbstractHandler = None

    def set_next(self, handler: AbstractHandler) -> AbstractHandler:
        self._next_handler = handler
        return handler

    def handle(self, request: Any) -> str:
        if self._next_handler:
            return self._next_handler.handle(request)
        return None
    


```

### Iterator:

An object that facilitates the traversal of a data structure  
in python we use:
iter method: Called to initialize the iterator and returns an iterator object.
next method: It must return the next value in the data stream.

```python
  
class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration
        value = self.data[self.index]
        self.index += 1
        return value
    


```


### Mediator:

Facilitates communication for different components. for example in a smart home, alarm notifies it's 7Am to the mediator and the mediator orders other classes to turn off lights and start coffee makers.
a real world example would be like aircrafts that don't communicate with each other directly but they all communicate with the air traffic controller, who sits in a tall tower.

```python
  
from __future__ import annotations
from abc import ABC

class Mediator(ABC):

    def notify(self, sender: object, event: str) -> None:
        pass

class ConcreteMediator(Mediator):
    def __init__(self, component1: Component1, component2: Component2) -> None:
        self._component1 = component1
        self._component1.mediator = self
        self._component2 = component2
        self._component2.mediator = self

    def notify(self, sender: object, event: str) -> None:
        if event == "A":
            print("do something")
        elif event == "D":
            print("do something else)
            

class BaseComponent:

    def __init__(self, mediator: Mediator = None) -> None:
        self._mediator = mediator

    @property
    def mediator(self) -> Mediator:
        return self._mediator

    @mediator.setter
    def mediator(self, mediator: Mediator) -> None:
        self._mediator = mediator


class Component1(BaseComponent):
    def do_a(self) -> None:
        print("Component 1 does A.")
        self.mediator.notify(self, "A")

    def do_b(self) -> None:
        print("Component 1 does B.")
        self.mediator.notify(self, "B")

class Component2(BaseComponent):
    def do_c(self) -> None:
        print("Component 2 does C.")
        self.mediator.notify(self, "C")

    def do_d(self) -> None:
        print("Component 2 does D.")
        self.mediator.notify(self, "D")

if __name__ == "__main__":
    # The client code.
    c1 = Component1()
    c2 = Component2()
    mediator = ConcreteMediator(c1, c2)

    print("Client triggers operation A.")
    c1.do_a()

    print("\n", end="")

    print("Client triggers operation D.")
    c2.do_d()

```

### Memento:

The Memento design pattern is a behavioral pattern in software design that allows you to capture and externalize an object’s internal state so that it can be restored to that state later. in the below example it's also a good practice to create a third class to handle and save mementos(singe responsibility)

```python
  
class NoteMemento:

    def __init__(self,text) -> None:
        self.text = text

class Note:

    def __init__(self) -> None:
        self.text = ''
        self.mementos = []

    def add_text(self,new_text):
        self.text += new_text
        self.save(self.text)

    def get_text(self):
        return self.text

    def save(self,text):
        self.mementos.append(NoteMemento(text=text))

    def restore(self,memento:NoteMemento):
        self.text = memento.text
  
notebook = Note()
notebook.add_text("hi")
notebook.add_text(" my name is Amin.")
notebook.get_text() #returns hi my name is Amin
notebook.restore(notebook.mementos[0])
notebook.get_text() #returns hi

```


### Observer:

Subscription mechanism to notify objects about an event. 
The event happening could be our game character getting damage, which when happening we want to notify other classes that care about this event.

```python
# lets notify different elements about our character getting danage
class CharacterHealthSubject:
    subscribers = {}

    def subscribe(self,observer):
        pass

    def unsubscribe(self,observer):
        pass

    def notify(self,event):
        for sub in self.subscribers[event]:
            sub.notify()

class HealthBar:

    def notify():
        pass # reduce health

class Graphics:

    def notify():
        pass # show red graphics

class Animation:

    def notify():
        pass # show falling animation
```


### State:

Helps us to change the behaviour of an object based on the state it's in. 

```python
  
class GameCharacterState:
    def punch(self):
        pass

class NormalState(GameCharacterState):

    def punch(self,character):
        print("10 damages to enemy")

class BoostedState(GameCharacterState):

    def punch(self,character):
        print("30 damages to enemy")
        character.state = NormalState()

class Character:

    def __init__(self):
        # Set the initial state
        self.state = NormalState()

    def punch(self):
        self.state.punch(self) 
        #we also pass the self to punch to change state inside it
```

### Strategy:

Enables the exact behavior of the system to be selected at the runtime meaning that you can switch the algorithm or strategy based upon the situation..

```python
  
class AbstractSortingStrategy:
    def sort():
        pass

class BubbleSortStrategy(AbstractSortingStrategy):
    def sort():
        pass
  
class QuickSortStrategy(AbstractSortingStrategy):
    def sort():
        pass
        
class Context:
    def __init__(self,strategy:AbstractSortingStrategy) -> None:
        self.strategy = strategy

    def set_strategy(self, strategy:AbstractSortingStrategy):
        self.strategy = strategy
  
    def execute(self,items:list[int]):
        return self.strategy.sort(items)
        
```


### Template:

Allows to define a template of an algorithm, with concrete implementations in subclasses which helps us to change the details about how the algorithm works and redefine certain steps of an algorithm without changing the algorithm's structure.

```python
  
class ScoreQuizTemplate():

    def get_answers(self):
        pass #get user answers

    def get_correct_answers(self):
        pass #get correct answers from database

    def score_answers(self):
        pass #score based on user answers and correct answers

    def mark(self):
        pass #return A,B,C,F based on score

    def score(self):
        self.get_answers()
        self.get_correct_answers()
        self.score_answers()
        self.mark()

class ScoreIranianQuiz():
  
    def mark(self):
        pass #return a value in range 0-20

class ScoreTestQuiz():

    def score_answers(self):
        pass #we also have negative marks for wrong answers
        
```
