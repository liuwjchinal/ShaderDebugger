# ShaderDebugger

ShaderDebugger is a library built for [SHADERed](https://github.com/dfranx/SHADERed).
It can be used to run shaders on CPU or debug them (get variable values, run them line by line, etc...).

**NOTE: This library is currently not finished/doesn't work! Please wait.**

## How?
ShaderDebugger uses [glslang](https://github.com/graphitemaster/glsl-parser) to parse 
GLSL into AST. The AST is converted to the bytecode with the [aGen](https://github.com/dfranx/aGen) library. The bytecode can then be executed using the [BlueVM](https://github.com/dfranx/BlueVM).

## Example
**NOTE: this is a 'prototype'**
```c++
sd::ShaderDebugger vs;
vs.SetSource(vertexCode);
vs.SetUniformValue("matVP", glm::mat4(1));

for (int i = 0; i < 3; i++) {
    vs.SetInputValue("position", vertices[i]);
    vs.SetInputValue("color", glm::vec3(i\*0.1f, i\*0.2f, i\*0.3f, 1.0f));
    vs.Execute("main"); // execute whole vertex shader

    glm::vec3 oPos = vs.GetOutputValue<glm::vec3>("oPos");
    ...
}
```

Or to execute line by line:
```c++
while (!vs.IsFinished()) {
    vs.Step();

    // get various info now using these functions: GetCurrentFunction, GetFunctionStack, GetVariableValue, etc...
}
```

## Limitations
### GLSL
- doesn't support macros, interface blocks, local & structure member arrays & global variable initalization
- parsed, but not stored & not implemented: storage, auxilary, precision, memory, invariant
- parsed & stored but not implemented: smooth, flat, noperspective

### HLSL
- not implemented yet

### Both
- function overloading