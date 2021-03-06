
## Lua5.1 Plugin
This can be used to generate lua binding code for UE4 Engine.That means you can write game logic by lua.
## How to build
This plugin can be used in UE4 4.14 version.Checkout branch **demo_firstperson_umg** to see how to implement.

basically:

1.copy all the file and folder of this repository to your project folder.

2.build lua lib manually.(I have build mac64 and win64 lib for you)

3.add lua include path and libarary path to youproject.build.cs(have a look to Source/sample.build.cs)

4.copy Source/TableUtil.h, TableUtil.cpp, Luautils.h, Luautils.cpp to your cpp source folder

5.Edit TableUtil.cpp and Luautils.cpp to include your project header file.

6.put lua file into projectname/LuaSource folder.At least need one file name main.lua.


## Communication between C++ and Lua:  
Checkout branch **demo_firstperson_umg** or **demo_shootergame**, there are some example.

1.call lua function in c++, There are Two way, In both method, No matter how many parameters:

  without return value:

  ```
  UTableUtil::call(function_name, parameter1, parameter2, ...);
  ```

  with return value:

  ```
  UTableUtil::callr<returntype>(function_name, parameter1, parameter2, ...);
```

2.call c++ in lua is similar with Blueprint.You can call c++ class function which with UFUNCTION() and You can get or set c++ member data which with UPROPERTY().

## Communication between Blueprint and Lua:
1.Lua can't not comunicate with Blueprint derectly.But Lua can call C++ class's blueprint implement function.

2.Blueprint can call lua function derectly, But It's not convenient.For example, there is a function in lua name Tick(delta), It accept one parameter.First Blueprint need to push an argument and then to call the function.

![tickexample](https://github.com/asqbtcupid/asqbtcupid.github.com/blob/master/_image/TickExamplebp.gif?raw=true)

there are different push function to push different type parameter:

![pushfunction](https://github.com/asqbtcupid/asqbtcupid.github.com/blob/master/_image/pushmethodbp.png?raw=true)

 and different call function to get different type result:
 
![callfunction](https://github.com/asqbtcupid/asqbtcupid.github.com/blob/master/_image/callmethodbp.png?raw=true)

K2Node may help to do this more simple.

## Export Class, Struct, Enum to Lua

1.config:In the Config/luaconfig.ini, "SupportedModules" mean The UCLASS of this module are exported to Lua, But for Now I only test CoreUobject, Engine, SlateCore, Slate, UMG."SupportedStruct" mean the USTRUCT exported to Lua.Please ignore "NoPropertyStruct" for now.All UENUM are exported.

2.if you want to export your class or struct or enum,you can add "meta=(Lua=1)" to the macro, such as UCLASS(meta=(Lua=1)).
## How to know what function or property of class are exported?
The lua binding code is generate to the folder base on your visual studio project config.If you use Development Editor config,please check your project_name/Intermediate/Build/Win64/UE4Editor/Inc/project_name folder.each class or struct has their corresponding file.Enum are in allEnum.script.h.

## What other type are suppoted?

1.TArray and C array is supported.TSet and TMap is not supported.

2.TWeakObjectPtr is supported,There are some example code in branch **demo_shootergame**.

3.MulticastDelegate is supported, There are some example code In **branch demo_fristperson_umg**.

4.UInterface is supported in branch demo_firstperson_umg.I will merge to master later.

## Featrue

1.Lua Hot Reload, You can change the lua logic during game running.This is my another repositery about lua hot reload:[luahotupdate](https://github.com/asqbtcupid/lua_hotupdate).you can try it in branch **demo_firstperson_umg**.Project path can't contain space.

2.When lua call c++ function which contains default argument.you dont't have to pass all arguments.For an example,In c++,you declare _void example(int i, int j =1, int k = 2)_.When you call this function in lua,you can just pass one argument:example(0).

3.When lua call c++ function which contains reference parameter, such as _void example(int &i, FVector &v)_,lua can get the reference value after call.In this case, you can write lua code:local i, v = example(argument1, argument2).
## Finally
1.There are some lua code in LuaSource such as luaclass.lua, It imitate the c++ object-oriented.And TimerMgr.lua, It works like a timer.But all of these are not necessary, you can implement better one.  

2.All my work are base on the official plugin ScriptPlugin.https://forums.unrealengine.com/showthread.php?3958-Scripting-Language-extensions-via-plugins

3.It's an experimental project,If you has any good idea or advise, welcome to fallback.Contact:15244611841@163.com
