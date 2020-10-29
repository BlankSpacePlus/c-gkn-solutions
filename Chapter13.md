# 第十三章 面向对象与C++基础

1.略

2.略

3.略

4.略

5.略

6.略

7.略

8.代码如下：
```cpp
class Vehicle {
protected:
    int wheels;
    double weight;
};

class Car : private Vehicle {
protected:
    Vehicle vehicle;
};

class Truck : private Vehicle {
protected:
    int passager_load;
    double payload;
};
```

9.代码如下：
```cpp
#include <iostream>
#include <utility>

using namespace std;

class Student {
private:
    int id;
    string name;
    int age;
public:
    Student(int id, string name, int age) {
        this->id = id;
        this->name = move(name);
        this->age = age;
    }
    void display() {
        cout << id << " " << name << " " << age;
    }
};

int main() {
    Student s = Student(5, "Sam", 10);
    s.display();
    return 0;
}
```

10.代码如下：
```cpp
#include <iostream>
#include <utility>
#include <math.h>

using namespace std;

class Point {
protected:
    double x;
    double y;
public:
    Point(double x, double y) {
        this->x = x;
        this->y = y;
    }
    double getX() const {
        return x;
    }
    double getY() const {
        return y;
    }
};

class Circle : Point {
private:
   double r;
public:
    Circle(double x, double y, double r) : Point(x, y) {
        this->x = x;
        this->y = y;
        this->r = r;
    }
    double getArea() const {
        return M_PI * r *r;
    }
    Point getPoint() {
        return {x, y};
    }
    double getRadius() const {
        return r;
    }
};

int main() {
    Circle circle = Circle(0.0, 0.0, 5.0);
    Point center = circle.getPoint();
    cout << "圆心坐标为：(" << center.getX() << "," << center.getY() << ")" << endl << "圆的半径为："
        << circle.getRadius() << endl << "圆的面积为：" << circle.getArea() << endl;
    return 0;
}
```

[返回首页](/README.md)
