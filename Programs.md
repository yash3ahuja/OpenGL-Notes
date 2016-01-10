# Programs:
- [Creating Programs](#program1)
- [Attaching Shaders](#program2)
- [Linking and Using Programs](#program3)
- [Checking Link Status](#program4)

*** 
<div id="program1" />
## Creating Programs

##### `GLuint glCreateProgram()`
- Creates a program object and returns its id.

*** 

<div id="program2" />
## Attaching Shaders
##### `void glAttachShader(GLuint program, GLuint shader)`
- Attaches the *shader* object to the given *program*.

***

## Example:
```cpp
// foo.vert
#version 330

void main() {
  gl_Position = vec4(0.0);
}

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

* * *

<div id="program3" />
## Linking and Using Programs

##### `void glLinkProgram(GLuint programId)`
- Links the program with the given *programId*.

___

##### `glDetachShader(GLuint program, GLuint shader)`
- Detaches *shader* from the *program*.
- Should be called **after** the program has been linked.

___

##### `glUseProgram(GLuint program)`
- Sets the given *program* as the program to be used for rendering.

* * *

<div id="program4" />
## Checking Link Status
```cpp
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