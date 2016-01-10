# Vertex Array Object (VAO)
- [Useful Functions](#vao1)
- [Example](#vao2)

* * *

<div id="vao1" />
##  Useful Functions
##### `void glGenVertexArrays(GLint n, GLuint* ids)`
- Generates *n* vertex arrays and stores their object names in *ids*.



##### `void glBindVertexArray(GLuint id)`
- Binds the vertex array with the given *id*.

* * *
 
<div id="vao2" />
##  Example
```cpp
GLuint vao;
glGenVertexArrays(1, &vao);
// Bind vao as the active Vertex Array Object.
glBindVertexArray(vao);
```