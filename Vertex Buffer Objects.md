# Vertex Buffer Object (VBO)
- [Useful Functions](#vbo1)
- [Example](#vbo2)

* * *
 
<div id="vbo1" />
##   Useful Functions
##### `void glGenBuffers(GLint n, GLuint ids)`
- Generates *n* buffers and stores their object names in *ids*.

##### `void glBindBuffer(GL_ARRAY_BUFFER, GLuint id)`
- Binds the buffer specified by *id* as the active vertex buffer.

##### `void glBufferData(GL_ARRAY_BUFFER, GLsizeiptr size, const GLvoid* data, GLenum usage)`
- Creates and initializes the vertex buffer object with the supplied *data* and *size*.

- *usage* describes how often the data will be written / used.
For most use cases, GL_STATIC_DRAW should be used.

* * *
 
<div id="vbo2" />
##  Example
```cpp
GLuint vbo;
glGenBuffers(1, &vbo):
GLfloat vertices[] = {
  1.0f, 1.0f, 1.0f
};
glBindBuffer(GL_ARRAY_BUFFER, vbo);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```
