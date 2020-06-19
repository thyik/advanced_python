# Create C Extension Library for Python

Install `Python.h`

For Visual Studio, refer to [here](https://github.com/Microsoft/python-sample-vs-cpp-extension)

Always prefix with `Py`

`PyObject` type : pointer for data parsing between python and C

* PyArg_ParseTuple(args, format, ...) 
  + Handles getting arguments from Python

* PyArg_BuildValue(format, ...) 
  + Handles turning values into PyObject pointers

* PyModule_Create(moduleDef)
  + Initizes the module and wraps method pointers

* Py_None 
  + function reutrn nothing
  
* PyMethodDef
  + define definition of all exported methods
  + Pattern : pyMethodName, function, functionType, Docs
  
```
static PyMethodDef myMethods[] = {
   {"func1", func1, METH_NOARGS, "func1 doc"},
   {"func2", func2, METH_VARARGS, "func2 doc"},
   {NULL, NULL, 0, NULL}   
}
```

* PyModuleDef
  + tell PyModule_Create() what information to use to create the module
  
```
static struct PyModuleDef myModule = {
   PyModuleDef_HEAD_INIT,
   "myModule",          // name of module
   "Fibonnacci Module", // Module Docs
   -1,        // -1 the module is global
   myMethods  // method def structure
};
```

---------
Procedure (ubuntu)
1. `sudo apt-get install python-dev` to install `Python.h`
2. Create the C function
3. Add version function
4. Save as myModule.c

```
#include <Python.h>

int Cfib(int n)
{
  if (n < 2)
    return n;
  else
    return Cfib(n-1) + CFib(n-2);
}

static PyObject* fib(PyObject* self, PyObject* args)
{
  int n;
  
  if (!PyArg_ParseTuple(args, "i", &n))  // decode the integer arguments
    return NULL;

  // return C-value to python value
  return Py_BuildValue("i", Cfib(n));  // "i" : integer type
}

static PyObject* version(PyObject* self)
{
  return Py_BuildValue("s", "Version 1.0");  // "s" : string type
}

/// Methods definition
static PyMethodDef myMethods[] = {
  {"fib", fib, METH_VARARGS, "Calculates the fibonacci numbers"},
  {"version", version, METH_NOARGS, "Query the version"},
  {NULL, NULL, 0, NULL}
};

/// Module definition
static struct PyModuleDef myModule = {
  PyModuleDef_HEAD_INIT,
  "myModule",
  "Fibonnaci Module",
  -1,
  myMethods
};

/// Initializer function
PyMODINIT_FUNC PyInit_myModule(void)
{
  return PyModule_Create(&myModule);
}
```

5. Create setup.py script

```
from distutils.core import setp, Extension

module = Extension("myModule", source = ["myModule.c"])

setup(name="PackageName",
      version = "1.0",
	  description = "Fibonnacci Module",
	  ext_modules = [module])
```

6. build the module and copy the compiled library (`.so`) to your project

```
$ python3 setup.py build

# this will output to the ./build folder
```

7. alternatively, install directly to python directory

```
$ python3 setup.py install
```

-----
Create test.py to test the library

```
import myModule

print(myModule.fib(10))
print(myModule.version())
```

run the script `python3 test.py`
