# michellundell/7a C++

## Agenda
1. C++ class constructor
2. C++ class destructor
3. C++ inheritance
4. C++ overloading methods
5. C++ Reference
6. C++ friend
7. C++ overloading input output operators << and >>
8. C++ Function Templates
9. C++ Class Templates
10. C++ std::list
11. C++ List with custom class, std::list
12. Todays assignments

## 1. C++ class constructor
A c++ class constructor is public and returns nothing!

```
#include <iostream>
using namespace std;

class BaseClass {
        public:
                BaseClass() { cout << "BaseClass constructor called" << endl; }
};

int main(int argc, char **argv)
{
        BaseClass bc;
        return 0;
}
```

## 2. C++ class destructor

A class destructor is defined by having a ~ infront of it and return nothing.
```
#include <iostream>
using namespace std;

class BaseClass {
	public:
		BaseClass() { cout << "BaseClass constructor called" << endl; }
		~BaseClass() { cout << "BaseClass destructor called" << endl; }
};

int main(int argc, char **argv)
{
        BaseClass bc;
        return 0;
}
```

## 3. C++ inheritance

Classes can inherit from other classes.

```
#include <iostream>
using namespace std;

class BaseClass {
        char data[20];
        public:
                BaseClass() { cout << "BaseClass created" << endl; strcpy(data,"initiated"); }
                const char *getData() { return(this->data); }
                void doit() { cout << "BaseClass::doit()" << endl; }
};

class SubClass: public BaseClass {
        public:
                SubClass() { cout << "SubClass created, BaseClass::data=" << this->getData() << endl; }
};

int main(int argc,char **argv)
{
        SubClass sc;
        sc.doit();
        return 0;
}
```

## 4. C++ overloading methods

```
#include <iostream>
using namespace std;

class BaseClass {
        public:
                BaseClass() { cout << "BaseClass object created" << endl; }
                ~BaseClass() { cout << "BaseClass object deleted" << endl; }
                void doit() { cout << "BaseClass:doit() here " << endl; }
                void dontDoit() { cout << "BaseClass:dontDoit() here " << endl; }
};

class SubClass: public BaseClass {
        public:
                SubClass() { cout << "SubClass object created" << endl; }
                ~SubClass() { cout << "SubClass object deleted" << endl; }
                void doit() { cout << "SubClass:doit() here " << endl; }
};

int main(int argc,char **argv)
{
        BaseClass *bcp = NULL;
        SubClass *scp = NULL;
        SubClass sc;             // sc is both a BaseClass and a SubClass!

        bcp = &sc;               // access only BaseClass ...
        bcp->doit();             // call BaseClass:doit()

        scp = &sc;               // access derived SubClass ...
        scp->doit();             // call overloaded method, SubClass::doit()
        scp->dontDoit();         // call inherited method, BaseClass::dontDoit()

        return 0;
}
```

## 5. C++ Reference

A reference is a alias for another variable.

```
#include <iostream>
using namespace std;

int main(int argc, char **argv)
{
	double agent = 0.028;
	double &bond = agent;
	bond /= 4.0;
	cout << "agent " << agent << endl;
	return 0;
}
```
Passing a large object reference to a function will not copy it onto the stack.
```
void someFunction( LargeClass &large )
{
	// large is not copied onto the stack
}
```

References can be updated without using pointer or adresses
```
#include <iostream>
using namespace std;

void swap(int &v1, int &v2)
{
	int t=v1;
	v1=v2;
	v2=t;
}

int main(int argc, char **argv)
{
	int a = 10;
	int b = 2;
	cout << "a=" << a << endl;
	cout << "b=" << b << endl;
	swap(a,b);
	cout << "a=" << a << endl;
	cout << "b=" << b << endl;
}
```

## 6. C++ friend

```
#include <iostream>
using namespace std;

class BaseClass {
	private:
		char data[20];
		int a;
        public:
                BaseClass() { cout << "BaseClass created" << endl; strcpy(this->data,"initiated"); this->a=42; }
                void info() { cout << "BaseClass:info(): " << this->data << endl; }
                friend class aFriendClass;
                friend void aFriendFunction(BaseClass&);
};


class aFriendClass: public BaseClass {
        public:
                aFriendClass() { this->a++; cout << "aFriendClass data:" << this->data << ", a:" << this->a << endl; }
};

void aFriendFunction( BaseClass &b )
{
	cout << "aFriendFunction b.data=" << b.data << ", a:" << b.a << endl;
}

int main(int argc,char **argv)
{
        aFriendClass fc;
        fc.info();
	aFriendFunction(fc);
        return 0;
}
```

## 7. C++ overloading input output operators << and >>

```
#include <iostream>
#include <ostream>
using namespace std;

class BaseClass {
        char data[20];
        public: 
                BaseClass() { cout << "BaseClass created" << endl; strcpy(data,"initiated"); }
                char *getData() { return this->data; }
                
                friend ostream &operator<<( ostream &output, const BaseClass &B ) {
                        output << "BaseClass data : " << B.data;
                        return output;
                }
                
                friend istream &operator>>( istream  &input, BaseClass &B ) {
                        input >> B.data;
                        return input;
                }
};

int main(int argc, char **argv)
{
        BaseClass object;
        cout << object << endl;
        cout << "Enter data:";
        cin >> object;
        cout << object << endl;
        return 0;
}
```

## 8. C++ Function Templates

```
#include <iostream>
using namespace std ;

//max returns the maximum of the two elements
template <class T> T max(T a, T b)
{
	return a > b ? a : b ;
}

int main(int argc,char **argv)
{
	cout << "max(10, 15) = " << max(10, 15) << endl ;
	cout << "max('k', 's') = " << max('k', 's') << endl ;
	cout << "max(10.1, 15.2) = " << max(10.1, 15.2) << endl ;
	return 0;
}
```
When the compiler sees an instantiation of the function template, for example: the call max(10, 15) in function main, the compiler generates a function max(int, int). Similarly the compiler generates definitions for max(char, char) and max(float, float) in this case.


## 9. C++ Class Templates
Similar to template functions, one can also make a template class

```
#include <iostream>
using namespace std;

class notTemplateClass { // hard coded to use int
        int v1;
        public:
                notTemplateClass(int init) { v1 = init;}
                int doit() { return( v1 * 3.14 ); }
};

template <class T> class aTemplateClass { // flexible
        T v1;
        public:
                aTemplateClass(T init) { v1=init; }
                T doit() { return( v1 * 3.14 ); }
};


int main(int argc, char **argv)
{
        notTemplateClass ntc(1);

        aTemplateClass<float> atc_float(1.0);
        aTemplateClass<int> atc_int(1);
        aTemplateClass<double> atc_double(1.2);

        cout << ntc.doit() << endl;
        cout << atc_float.doit() << endl;
        cout << atc_int.doit() << endl;
        cout << atc_double.doit() << endl;
        return 0;
}
```

## 10. C++ std::list

Having objects in a list is a good way to not forget them ;-)

c++ have a handy standard list class, std::list.

Here is an example of using a integer list:
```
#include <iostream>
#include <cstdlib>
#include <iterator>
#include <list>
using namespace std;

// function for printing the elements in a list
void showlist(list<int> g)
{
    list<int>::iterator it;
    for (it = g.begin(); it != g.end(); ++it)
        cout << "  " << *it;
    cout << '\n';
}

int main(int argc, char **argv)
{
    list<int> my_list;

    for (int i = 0; i < 10; ++i) {
        my_list.push_back( (rand()%100) ); // Add 10 random numbers between 0 and 100
    }

    cout << "my_list is          : ";
    showlist(my_list);

    cout << "my_list.front()     :   " << my_list.front() << endl;
    cout << "my_list.back()      :   " << my_list.back() << endl;

    cout << "my_list.pop_front() : ";
    my_list.pop_front();
    showlist(my_list);

    cout << "my_list.sort()      : ";
    my_list.sort();
    showlist(my_list);

    cout << "my_list.reverse()  : ";
    my_list.reverse();
    showlist(my_list);

    return 0;
}

```

## 11. C++ List with custom class, std::list

Having objects in a list is a good way to not forget them ;-)

C++ have a handy list class, std::list.

Here is an example of using a custom class list:
```
#include <iostream>
#include <cstdlib>
#include <iterator>
#include <list>
using namespace std;

class myClass {
	int x;
	public:
		myClass(int x) { this->x = x; }
		int getValue() { return(x); }
};
  
// std::list.sort needs a "compare" function to work with custom objects
// is a less than b? 
bool compareMyClass(myClass *a, myClass *b) 
{ 
	return( a->getValue() < b->getValue() ); 
}

// function for printing the elements in a list
void showlist(list<myClass *> g)
{
    list<myClass *>::iterator it;
    for (it = g.begin(); it != g.end(); ++it) {
	myClass *mcp = *it;
        cout << "  " << mcp->getValue();
    }
    cout << '\n';
}
  
int main(int argc, char **argv)
{
    list<myClass *> my_list;
  
    for (int i = 0; i < 10; ++i) {
        my_list.push_back( new myClass(rand()%100) ); // Add 10 random numbers between 0 and 100
    }
    cout << "my_list is          : ";
    showlist(my_list);
  
    myClass *mcp = my_list.front();
    cout << "my_list.front()     :   " << mcp->getValue() << endl; 
    mcp = my_list.back();
    cout << "my_list.back()      :   " << mcp->getValue() << endl;
  
    cout << "my_list.pop_front() : ";
    my_list.pop_front();
    showlist(my_list);

    cout << "my_list.sort()      : ";
    my_list.sort(compareMyClass);
    showlist(my_list);
  
    cout << "my_list.reverse()   : ";
    my_list.reverse();
    showlist(my_list);

    return 0;
}
```

## 12. Todays assignments

1. Compile the examples above and look at the code and output. Try to remember these C++ features by creating some other 
examples of each feature **without looking/copy-paste!**

2. If you are finished with the ticket program in C, start making a C++ version of it!

Maybe some of the features we learnt today could be used in your C++ ticket system.
(Using the features in a "right place" will collect points!)

