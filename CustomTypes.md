
## Creating a class

To create a class, we use `DECLARE_CLASS`, defined as below:
```cpp
#define DECLARE_CLASS(namespaze, name, baseNamespaze, baseName, baseSize, impl)
```

`impl` specifies the declarations for fields and methods in the class using `DECLARE_INSTANCE_FIELD`, `DECLARE_STATIC_FIELD`, `DECLARE_INSTANCE_FIELD_DEFAULT`, `DECLARE_METHOD_SPECIAL`, `DECLARE_METHOD`, and `DECLARE_OVERRIDE_METHOD`.

### Example
```cpp
// class MyType : UnityEngine.MonoBehaviour
DECLARE_CLASS(Il2CppNamespace, MyType, "UnityEngine", "MonoBehaviour", sizeof(Il2CppObject) + sizeof(void*),
    // Declare a constructor
    DECLARE_CTOR(ctor);

    // Declare our methods
    // void Start();
    DECLARE_METHOD(void, Start);
    // static void Test(int x, float y);
    DECLARE_METHOD(static void, Test, int x, float y);
    // int asdf(int q);
    DECLARE_METHOD(int, asdf, int q);

    // Overriding UnityEngine.Object.ToString
    DECLARE_OVERRIDE_METHOD(Il2CppString*, ToString, il2cpp_utils::FindMethod("UnityEngine", "Object", "ToString"));

    // `REGISTER_FUNCTION` will create a static inline _register function used to register our type within il2cpp
    REGISTER_FUNCTION(MyType,
        // implementation of the _register function
        modLogger().debug("Registering MyType!");

        // Register constructor
        REGISTER_METHOD(ctor);
        // Register methods
        REGISTER_METHOD(Start);
        REGISTER_METHOD(Test);
        REGISTER_METHOD(asdf);
        // Register the override method
        REGISTER_METHOD(ToString);
    )
)

DEFINE_CLASS(Il2CppNamespace::MyType);
```

## Implementing an interface

To implement an interface, we use `DECLARE_CLASS_INTERFACES`, defined as below:
```cpp
#define DECLARE_CLASS_INTERFACES(namespaze, name, baseNamespaze, baseName, baseSize, interfaces, impl)
```

### Example
```cpp
DECLARE_CLASS_INTERFACES(Il2CppNamespace, OrganizeEnumerator, "System", "Object", sizeof(Il2CppObject),
    il2cpp_utils::GetClassFromName("System.Collections", "IEnumerator"),

    DECLARE_INSTANCE_FIELD(Il2CppObject*, current);
    DECLARE_INSTANCE_FIELD(bool, hasWaited);

    DECLARE_OVERRIDE_METHOD(bool, MoveNext, il2cpp_utils::FindMethod("System.Collections", "IEnumerator", "MoveNext"));
    DECLARE_OVERRIDE_METHOD(Il2CppObject*, get_Current, il2cpp_utils::FindMethod("System.Collections", "IEnumerator", "get_Current"));
    DECLARE_OVERRIDE_METHOD(void, Reset, il2cpp_utils::FindMethod("System.Collections", "IEnumerator", "Reset"));

    REGISTER_FUNCTION(OrganizeEnumerator,
        getLogger().debug("Registering OrganizeEnumerator!");

        REGISTER_FIELD(current);
        REGISTER_FIELD(hasWaited);

        REGISTER_METHOD(MoveNext);
        REGISTER_METHOD(get_Current);
        REGISTER_METHOD(Reset);
    )
)

DEFINE_CLASS(Il2CppNamespace::OrganizeEnumerator);
```