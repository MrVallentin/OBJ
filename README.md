
# LoadOBJ

LoadOBJ is a minimal single-header OBJ loader written in C.


## Example

```c
LoadOBJMesh mesh;
loadOBJ(&mesh, ...);

LoadOBJTriangleMesh trimesh;
loadOBJTriangulate(&trimesh, &mesh);

loadOBJDestroyMesh(&mesh);
loadOBJDestroyTriangleMesh(&trimesh);
```


## Implementation

In one C or C++ file, define `LOADOBJ_IMPLEMENTATION` prior to inclusion to create the implementation.

```c
#define LOADOBJ_IMPLEMENTATION
#include "loadobj.h"
```


## OpenGL Example

```c
#include <stddef.h> // offsetof

// Load OBJ

LoadOBJMesh mesh;
loadOBJ(&mesh, ...);

LoadOBJTriangleMesh trimesh;
loadOBJTriangulate(&trimesh, &mesh);

loadOBJDestroyMesh(&mesh);

// Setup Vertex Buffer and Vertex Arrays

const GLsizei vertexCount = trimesh.vertexCount;

GLuint vao, vbo;

glGenVertexArrays(1, &vao);
glBindVertexArray(vao);

glCreateBuffers(1, &vbo);
glBindBuffer(GL_ARRAY_BUFFER, vbo);
glBufferData(GL_ARRAY_BUFFER, vertexCount * sizeof(LoadOBJTriangleVertex), trimesh.vertices, GL_STATIC_DRAW);

glEnableVertexAttribArray(ATTRIBUTE_POSITION);
glVertexAttribPointer(ATTRIBUTE_POSITION, 3, GL_FLOAT, GL_FALSE, sizeof(LoadOBJTriangleVertex), (const GLvoid*) offsetof(LoadOBJTriangleVertex, x));

glEnableVertexAttribArray(ATTRIBUTE_TEXCOORD);
glVertexAttribPointer(ATTRIBUTE_TEXCOORD, 2, GL_FLOAT, GL_FALSE, sizeof(LoadOBJTriangleVertex), (const GLvoid*) offsetof(LoadOBJTriangleVertex, u));

glEnableVertexAttribArray(ATTRIBUTE_NORMAL);
glVertexAttribPointer(ATTRIBUTE_NORMAL, 3, GL_FLOAT, GL_FALSE, sizeof(LoadOBJTriangleVertex), (const GLvoid*) offsetof(LoadOBJTriangleVertex, nx));

loadOBJDestroyTriangleMesh(&trimesh);

// Draw

glDrawArrays(GL_TRIANGLES, 0, vertexCount);
```
