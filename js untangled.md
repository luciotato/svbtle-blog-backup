#Javascript, untangled

###Background
*I'm working on LiteScript, a compile-to-js-language, based in Javascript design (the good parts). LiteScript is also a compile-to-C-language. In order to compile to fast C, it is required to untangle some JS concepts and limit some JS dynamic features*

##Untangling Javascript

Javascript is extremely powerful, and this power allows a programmer to write very clever code, and also shoot herself in the foot in very imaginative ways. 

##Classes are Functions, and Functions are Classes

In JS a "class" is simply a Function and every Function is a class. This is the first concept to untangle.
In LiteScript, a "Class" is a type of  Object, different from a "Function".

_JS Function_:  Object containing executable code. 
Functions objects have properties: *name, prototype, length*.
Functions objects have several methods: *apply, call, bind, toString, etc.*

_JS Class_:  see Function.


_LiteScript Function_: executable code, Function objects have no properties, and have two methods: *apply* and *call*

_LiteScript Class_: model for instances. Class objects have two properties: *name* and *prototype*.

We're simply untangling the two concepts, but keeping JS design. Since we have now a differentiated type "Class", we moved the properties "name" and "prototype" to the new type.

In JS, the Function-Class is used as the instance initialization (the code initializing the instance property values). In LS, we separate the instance initialization as the class *constructor*.

##to new or not to new

In JS you may call a function with "new" or you may not. Sometimes, if the function is mainly a class, the function will take care of this and will create a instance for you (by calling itself with new), sometimes it will not. There are some core JS classes which do not respect this "initialize instance" model, most notably the "Error" Function-Class.

in LiteScript (if you want to compile-to-C later) the only way to create a instance is to use "new" with a Class, also, all core classes respect the "initialize provided instance" model. Moreover: the instance initialization function do not return a value, so it cannot return a different "this" than the provided one.

##Untangling Objects and Dictionaries

This is one of the most powerful features of JS. JS is self-reflective by default. All Objects are really "bags of dynamic properties". This is also one of the reasons why normal JS code is slow compared to compiled code, despite great advances of JS engines like V8.

Many JS programs use object as "Dictionaries", to have fast access to a name:value pair. 

In the EcmaScript6 specification, a new concept "Map" is incorporated to JS. From the Map's viewpoint, a Object is just a Map string=>any

##Litescript: Objects and Maps

in LiteScript, since we want to compile-to-C, we need to untangle Map and Object.

_JS Object_: bag of dynamic properties. You can access properties with the *Property Access Operator*, a dot. You can add or delete properties at runtime. 

###Example of the PropertyAccess operator:
```
obj.someProp.anotherSubProp
```

_JS Map_ (ES6): bag of dynamic properties.


_LiteScript Object_: instance of a Class. It has a Predefined list of properties. You can access properties with the *Property Access Operator*, a dot.  You can not add new properties at runtime.

_LiteScript Map_: bag of dynamic properties. a Dictionary.

So in LiteScript, you use objects as you define them in Class declarations, and, if you need a "Dictionary", you use a Map. In order to rewrite JS code in LiteScript, you need to use defined classes instead of patched Objects, and you can't rely on being able to add or remove new Object properties at runtime. Normally you have to declare and extend classes instead of add or remove properties from Objects.

##Performance gains

The LiteScript compiler is written in LiteScript. Today, at v0.8.5, the LiteScript compiler can compile itself to-js code and also to-c code.
  
LiteScript compiler can be compiled to-js in order to execute it under node.js or the browser. LiteScript compiler can be compiled to-c in order to execute it as a standalone executable. When compiled-to-c,  LiteScript compiler can self-compile 5x-7x faster than the js version of the same source.

##Conclusion

Untangling concepts, incorporating  "Class" and limiting "Object", we're able to have a language, LiteScript, which is heavily inspired by Javascript, but which can be compiled-to-C when a better performance is required.

##Status

LiteScript is beta, and compile-to-C is in alpha stage. Hackers help is required and will be very well received. http://github.com/luciotato/litescript


