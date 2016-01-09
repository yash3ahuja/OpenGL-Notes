#Table of Contents
1. [Vertex Array Objects](#vao)
  - [Useful Functions](#vao1)
  - [Example](#vao2)
2. [Vertex Buffer Objects](#vbo)
  - [Useful Functions](#vbo1)
  - [Example](#vbo2)
3. [Creating and Compiling Shaders](#shader-compile)
  - [Useful Functions](#shader-compile1)
  - [Checking Compilation Status](#shader-compile2)
  - [Example](#shader-compile3)
4. [Programs](#program)
  - [Creating Programs](#program1)
  - [Attaching Shaders](#program2)
  - [Linking and Using Programs](#program3)


<div id="vao" />
## Vertex Array Object (VAO)

<div id="vao1" />
###  Useful Functions
##### `void glGenVertexArrays(GLint n, GLuint* ids)`
- Generates *n* vertex arrays and stores their object names in *ids*.



##### `void glBindVertexArray(GLuint id)`
- Binds the vertex array with the given *id*.

_ _ _

<div id="vao2" />
###  Example
    GLuint vao;
    glGenVertexArrays(1, &vao);
    glBindVertexArray(vao);

` `

* * *

<div id="vbo" />
###  Example
## Vertex Buffer Object (VBO)
<div id="vbo1" />
###   Useful Functions
___
##### `void glGenBuffers(GLint n, GLuint ids)`
- Generates *n* buffers and stores their object names in *ids*.
___

##### `void glBindBuffer(GL_ARRAY_BUFFER, GLuint id)`
- Binds the buffer specified by *id* as the active vertex buffer.
___

##### `void glBufferData(GL_ARRAY_BUFFER, GLsizeiptr size, const GLvoid* data, GLenum usage)`
- Creates and initializes the vertex buffer object with the supplied *data* and *size*.

- *usage* describes how often the data will be written / used.
For most use cases, GL_STATIC_DRAW should be used.

___

<div id="vbo2" />
###  Example
    GLuint vbo;
    glGenBuffers(1, &vbo):
    GLfloat vertices[] = {
      1.0f, 1.0f, 1.0f
    };
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

` `
* * *

<div id="shader-compile" />
##Creating and Compiling Shaders
<div id="shader-compile1" />
### Useful Functions
___
##### `GLuint glCreateShader(GLenum shaderType)`
- Creates a shader with the given *shaderType*.

____
##### `glShaderSource(GLuint id, GLsizei count, const GLchar **source, const GLint *length)`
- Sets the source code of the shader specified by *id* to *source*.
- count specifies the number of strings in *source*, while *length* is an array specifying the length of each string.
  Typically, *count* should be 1 and *length* should be NULL.

____
##### `glCompileShader(GLuint id)`
- Compiles the shader with the given *id*.


___

<div id="shader-compile2" />
###  Checking Compilation Status
```
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

___

<div id="shader-compile3" />
### Example
```
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

` `
* * *

<div id="program" />
## Programs:

<div id="program1" />
### Creating Programs

##### `GLuint glCreateProgram()`
- Creates a program object and returns its id.
___


<div id="program2" />
### Attaching Shaders
##### `void glAttachShader(GLuint program, GLuint shader)`
- Attaches the *shader* object to the given *program*.
___
##### `void glBindFragDataLocation(GLuint program, GLuint colorNumber, const char* name)`
- Binds the output variable with the given *name* in the fragment shader to the color number *colorNumber* for the given *program*. 
- Typically, *colorNumber* will be 0.

#### Example:
```
// foo.frag
#version 330

out vec4 outColor;
void main() {
	outColor = vec4(1.0, 0.0, 0.0, 1.0);
}

// foo.cpp
GLuint program = glCreateProgram();
glAttachShader(program, vertexShader);
glAttachShader(program, fragmentShader);
glBindFragDataLocation(program, 0, "outColor");

```
___

<div id="program3" />
### Linking and Using Programs

##### `void glLinkProgram(GLuint programId)`
- Links the program with the given *programId*.

___

##### `glDetachShader(GLuint program, GLuint shader)`
- Detaches *shader* from the *program*.
- Should be called **after** the program has been linked.

___

##### `glUseProgram(GLuint program)`
- Sets the given *program* as the program to be used for rendering.

___

### Checking Link Status
```
GLint isLinked;
GLint logLength;
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &isLinked);
glGetProgramiv(shaderProgram, GL_INFO_LOG_LENGTH, &logLength);
if (isLinked == GL_FALSE) {
  vector<char> log(logLength);
  glGetProgramInfoLog(shaderProgram, logLength, NULL, &log[0]);
  std::cerr << "Error linking: " << std::endl;
  for (const auto& c : log) 
  	std::cerr << buffer
  std::cerr << std::endl;
}
```

` `
* * *

## Specifying Shader Attributes:

` `
* * *

## Specifying Shader Uniforms:

` `
* * *

## Texture:

` `
* * *

## Depth Test:

` `
* * *


## Stencil Test:

` `
* * *

## Framebuffers:


