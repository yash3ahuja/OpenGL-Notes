# Shaders
1. [Creating and Compiling Shaders](#shader-compile)
  - [Useful Functions](#shader-compile1)
  - [Checking Compilation Status](#shader-compile2)
  - [Example](#example)
2. [Using Shader Attributes](#shader-attrib)
  - [Declaring in GLSL](#shader-attrib1)
  - [Declaring in code](#shader-attrib2)
  - [Specifying VBO layout](#shader-attrib3)
3. [Using Shader Uniforms](#shader-uniform)
  - [Specifying in GLSL](#shader-uniform1)
  - [Specifying in code](#shader-uniform2)
4. [Fragment Shader Specifics](#shader-frag)
  - [Binding output in GLSL](#shader-frag1)
  - [Binding output in code](#shader-frag2)

* * *
 
<div id="shader-compile" />
##Creating and Compiling Shaders

<div id="shader-compile1" />
### Useful Functions
___
##### `GLuint glCreateShader(GLenum shaderType)`
- Creates a shader with the given *shaderType*.

##### `glShaderSource(GLuint id, GLsizei count, const GLchar **source, const GLint *length)`
- Sets the source code of the shader specified by *id* to *source*.
- count specifies the number of strings in *source*, while *length* is an array specifying the length of each string.
  Typically, *count* should be 1 and *length* should be NULL.
##### `glCompileShader(GLuint id)`
- Compiles the shader with the given *id*.

- - -

<div id="shader-compile2" />
###  Checking Compilation Status
```cpp
GLint status;
GLint logLength;
glGetShaderiv(shaderId, GL_COMPILE_STATUS, &status);
glGetShaderiv(shaderId, GL_INFO_LOG_LENGTH, &logLength);
if (status != GL_TRUE) {
  std::vector<char> log(logLength);
  // Store compilation log.
  glGetShaderInfoLog(shaderId, logLength, NULL, &log[0]);
}
```

- - -

<div id="shader-compile3" />
### Example
```cpp
GLuint shader = glCreateShader(GL_VERTEX_SHADER);

const GLchar* source = reinterpret_cast<const GLchar*>(getFileContents(filename).c_str());

glShaderSource(shader, 1, &source, NULL);
glCompileShader(shader);

// Check compilation status.
GLint status;
GLint logLength;
glGetShaderiv(shaderId, GL_COMPILE_STATUS, &status);
glGetShaderiv(shaderId, GL_INFO_LOG_LENGTH, &logLength);
if (status != GL_TRUE) {
  std::vector<char> log(logLength);
  glGetShaderInfoLog(shaderId, logLength, NULL, &log[0]);
  std::cerr << "Error processing shader " << filename << ": " << std::endl;
  for (const auto& c: log)
  	std::err << c;
  std::err << std::endl;
}
```
<br />

* * *

<div id="shader-attrib" />
## Using Shader Attributes

<div id = "shader-attrib1" />
### Declaring in GLSL
To use attributes in GLSL, the location of each variable must be in the variable declaration.
The index must then be enabled with the following function:

##### `void glEnableVertexAttribArray(GLuint index)`
- Enables or disables the vertex attribute specified by *index*.

##### Example
```cpp
	// GLSL
    layout(location = 0) in vec3 position;
    layout(location = 1) in vec4 color;

    // CPP
    // Enable position attribute.
    glEnableVertexAttribArray(0);
    
    // Enable color attribute.
    glEnableVertexAttribArray(1);
```
<br />
____

<div id="shader-attrib2" />
### Declaring in Code
Instead of specifying the layout in the shader, we instead use the following function to specify attribute locations:

##### `void glBindAttribLocation(GLuint program, GLuint index, const GLchar *name')`
- For the specified *program*, binds the *index* to the attribute given by *name*.

##### Example
```cpp
	// GLSL
    in vec3 position;
    in vec4 color;

    // CPP
    glBindAttribLocation(program, 0, "position");
    glEnableVertexAttribArray(0);

    glBindAttribLocation(program, 1, "color");
    glEnableVertexAttribArray(1);
```

<br />
____

<div id="shader-attrib3" />
### Specifying VBO layout

In order to get data to our attribute, we must specify the layout of the data in the vertex buffer using the following function:

##### `void glVertexAttribPointer(GLuint index, GLint size, GLenum   type, GLboolean normalized, GLsizei stride, const GLvoid * pointer)`
- *index* specifies the vertex attribute.
- *size* specifies the number of components for the attribute. Must be between 1 and 4. If we want to use a vec3, then it should be 3.
- *type* specifies the data type of the components in the array.
- *normalized* specifies whether the data should be normalized.
- *stride* specifies the byte offset between instances of a given component
- *pointer* specifies the offset of the first component for this attribute

##### Example
```cpp
// Vertex Data Layout: x, y, z, r, g, b, a
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE,
    7 * sizeof(GLfloat), // each vertex has 7 components
    0); // first attribute, so it has no offset

glVertexAttribPointer(0, 4, GL_FLOAT, GL_FALSE,
    7 * sizeof(GLfloat), // same stride as before
    (void*)(3 * sizeof(float))); // attributes start at fourth index
```

<br />

* * *

<div id="shader-uniform" />
## Using Shader Uniforms

- - -

<div id="shader-uniform1" />
### Useful Functions

- - -
<div id="shader-uniform2" />
### Example
```cpp

```
<br />

* * *

<div id="shader-frag" />
## Fragment Shader Specifics

- - -

<div id="shader-frag1" />
### Binding output in GLSL
Similarly to specifying attributes, all we have ot do is give a location to the output color of the fragment shader:

```cpp
// GLSL
layout(location = 0) out vec4 outColor
```

- - -

<div id="shader-frag2" />
### Binding output in code

If we want to specify the location of the output in code, then the following function can be used: 
##### `void glBindFragDataLocation(GLuint program, GLuint colorNumber, const char* name)`
- Binds the output variable with the given *name* in the fragment shader to the color number *colorNumber* for the given *program*.
- Typically, *colorNumber* will be 0.

##### Example
```cpp
// GLSL
...
out vec4 outColor;
...

// CPP
glBindFragDataLocation(program, 0, "outColor");

```
