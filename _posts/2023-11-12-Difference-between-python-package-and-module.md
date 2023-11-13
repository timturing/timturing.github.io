python的执行是从第一行往后数的(所以import实际是在执行别的文件)，而C是有明确的入口地址的，这不一样

Module就是单独的py file，且python xxx.py已经表明python是只能直接运行py file的。而package是一个目录，如果我们想要运行一个目录显然没有入口点(entry point)，所以我们需要在这个目录下创建一个`__main__.py`文件，这个文件就是入口文件，这样我们就可以通过python -m 目录了。(所以-m就是以module形式去运行的意思)。注意这里与`__init__.py`没有直接关系，即使没有照样可以运行。

`__init__.py`的作用是将一个目录变成一个package，这样我们就可以`import`这个package了。也就是说，当我们`import`一个目录的时候，实际上会立刻去执行这个目录下的`__init__.py`文件，这样就可以将package理解为module使用

同样的道理，如果我们直接`import`一个module，那么实际上也是立刻去执行这个module。而为了将`import`时才需要的代码与直接运行的代码区分开，我们可以在module中加入`if __name__ == '__main__':`，这样就可以区分开了。

如果我们使用from package import module的方式直接引用一个module，就会既执行package下的`__init__.py`，又执行module。如果没有`__init__.py`，就只会执行module。(默认__init__.py是空的)

而Script可以理解成是一个独立的程序，一般直接运行(python xxx.py)。而module和package都是被其他程序调用的，所以它们都是library。

综上可知，所有的import都是引入了一个module，即使是package也不过是引入了__init__这个module而已。