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

