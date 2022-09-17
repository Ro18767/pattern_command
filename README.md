# pattern_command

## Цель патерна
### Проблема
Перед нами стоит задача объеденять одни группы класов вызываюшие похожее поведение у других класов
### Решение
Паттерн команда вместо хранения его методов поведения внутри всех классов
решает проблемы тем что соберет дуплирущее поведение внотри одного класса команда.


## Плюсы и минусы
### Плюсы
+ бирает прямую зависимость между объектами, вызывающими операции, и объектами, которые их непосредственно выполняют.
+ Позволяет реализовать простую отмену и повтор операций.
+ Позволяет реализовать отложенный запуск операций.
+ Позволяет собирать сложные команды из простых.
+ Реализует принцип открытости/закрытости.
 ### Минусы
+ Усложняет код программы из-за введения множества дополнительных классов.

## Аналогия
есть пульт кнопки на телике и телик
кнопки на телике и на пульте выполняют теже веши если бы мы писаль код то для отсуцтвия дублирования мы бы использовали команды

![мем](https://user-images.githubusercontent.com/108687865/190861350-301b90fa-4883-41ac-98ca-ac31f106df4c.jpg)

## Пример
```cpp
#include <iostream>

using namespace std;

class TV {
public:
  void turn_on() {
    cout << "turn_on" << endl;
  }
  void turn_off() {
    cout << "turn_off" << endl;
  }
  void make_louder() {
    cout << "make_louder" << endl;
  }
  void make_quieter() {
    cout << "make_quieter" << endl;
  }
};


class Comand {
public:
  virtual void execute() = 0;
};
class TV_Comand : public Comand {
protected:
  TV* tv;
public:
  TV_Comand(TV* _tv) :tv{ _tv } {
    if (tv == nullptr)
      throw "tv == nullptr";
  }
};
class Turn_on_TV_Comand : public TV_Comand {
public:
  Turn_on_TV_Comand(TV* _tv) : TV_Comand(_tv) {}
  void execute() override {
    tv->turn_on();
  }
};
class Turn_off_TV_Comand : public TV_Comand {
public:
  Turn_off_TV_Comand(TV* _tv) : TV_Comand(_tv) {}
  void execute() override {
    tv->turn_off();
  }
};
class Make_louder_TV_Comand : public TV_Comand {
public:
  Make_louder_TV_Comand(TV* _tv) : TV_Comand(_tv) {}
  void execute() override {
    tv->make_louder();
  }
};
class Make_quieter_TV_Comand : public TV_Comand {
public:
  Make_quieter_TV_Comand(TV* _tv) : TV_Comand(_tv) {}
  void execute() override {
    tv->make_quieter();
  }
};
class Remote {
  TV* tv;
public:
  Remote(TV* _tv) :tv{ _tv } {
    if (tv == nullptr)
      throw "tv == nullptr";
  }
  void button1() {
    Turn_on_TV_Comand comand(tv);
    comand.execute();
  }
  void button2() {
    Turn_off_TV_Comand comand(tv);
    comand.execute();
  }
  void button3() {
    Make_louder_TV_Comand comand(tv);
    comand.execute();
  }
  void button4() {
    Make_quieter_TV_Comand comand(tv);
    comand.execute();
  }
};
int main() {
  TV* tv = new TV();
  Remote* remote = new Remote(tv);

  remote->button1();
  remote->button2();
  remote->button3();
  remote->button4();
}
```
### Выводит в консоль
```
turn_on
turn_off
make_louder
make_quieter
```


![ClassDiagram2](https://user-images.githubusercontent.com/108687865/190861485-110f3e04-afd6-4255-8c90-8570c6d75ec0.jpg)


## Список типов
+ Базовые типы Comand и TV_Comand - выступает как абстракции для реалезованых команд
+ наследники Turn_on_TV_Comand, Turn_off_TV_Comand, Make_louder_TV_Comand и Make_quieter_TV_Comand - выступает как реалезованые команды
+ Remote - создатель команд
+ TV - выступает как клас к которому обращяются команды чтоб выполнится




