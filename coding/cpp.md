# Method vs. Function

A method resides in a class, a function does not. For example, in C there are functions but in C++ there are methods and functions. A method is a function with an instance of an object as a first argument. This first argument is usally hidden but may be seen during a std::bind i.e.:

    void Class::method(std::string arg1){}

    int main()
    {
        using namespace std::placeholders; // _1, _2, ...

        std::unique_ptr<Class> class;
        class.reset(new Class {});

        std::bind(&Class:method, class, _1);
    }

# Virtual vs. Pure Virtual

> A virtual function or virtual method is a function or method whose behavior can be overridden within an inheriting class by a function with the same signature.

> A pure virtual function or pure virtual method is a virtual function that is required to be implemented by a derived class that is not abstract.

# Shared Pointers

## const

    shared_ptr<T> p;             ---> T * p;                                    : nothing is const
    const shared_ptr<T> p;       ---> T * const p;                              : p is const
    shared_ptr<const T> p;       ---> const T * p;       <=> T const * p;       : *p is const
    const shared_ptr<const T> p; ---> const T * const p; <=> T const * const p; : p and *p are const.

# Conversions

    # string to vec
    std::string str;
    std::vector<uint8_t> vec(str.begin(), str.end());
