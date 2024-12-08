OpenGL

Que es un vector

==Producto escalar (dot product)==
El resultat es un valor que son els angles
- podem calcular-ho de dos maneres element a element
	- Element a element
	- Multiplicar el mòdul de cada un amb el coseno de l'angle que formen
(Forward vector)

El arcocoseno es el contrari del seno. El resultat esta dins de 0 a 180

Per calcular si dos vectors estan a la vista


==Cross Product== (Producto vectorial)
Es un producte entre vectors que ens dona un vector perpendicular
Obtenim quan es de perpendicular un vector
Uso real del Cross Product

# Trigonometria
Es la branca que estudia els angles
Les RAONS 
#### Circunferencia Gonometrica 
- Es una circunferencia que te 1 com a radi
#### Radianes i Graus
- $2 * pi$ = $360º$

# OpenGL

Instalar
### GLFW
https://www.glfw.org/download.html

Baixar els Windows pre-compiled binaries de 64bits
### Glad
https://glad.dav1d.de/
![[Pasted image 20241031085252.png|450]]

En el projecte crear carpeta "Dependencies"
- Include
	- GLFW (copiat de GLFW)
	- glad (copiat de Glad)
	- KHR (copiat de Glad)
- lib
	- glfw3.lib
Copiar glad.c a la root del projecte
![[Pasted image 20241031091623.png]]
#### Project Properties

Include
![[Pasted image 20241031091109.png|500]]
Lib
![[Pasted image 20241031091252.png|500]]
Al linker
![[Pasted image 20241031091533.png|675]]


#4/12/2024

# Texturas
[https://github.com/nothings/stb/blob/master/stb_image.h](https://github.com/nothings/stb/blob/master/stb_image.h)

![[Pasted image 20241204113614.png]]


Els arxius .jpg van de la ma del parametre GL_RGB -> Si el numero de pixels es 3 es RBG si es 4 es RGBA

```cpp
//TEXTURAS  
//------------------------------------------------------------------------------------------------------------------  
int widthImg, heighImg, PixelNum;  

stbi_set_flip_vertically_on_load(true);
unsigned char* image = stbi_load("Image.jpg", &widthImg, &heighImg, &PixelNum, 0);  
  
GLuint textureID;  
glGenTextures(1, &textureID); //numero de textures  
glBindTexture(GL_TEXTURE_2D, textureID);  
  
//Com es comporta el suavitzat de entre pixels  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);  
  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);  
  
if (image)  
{  
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, widthImg, heighImg, 0, GL_RGB, GL_UNSIGNED_BYTE, image);  
}  
else  
{  
    std::cout << "Failed to load image" << std::endl;  
}  
stbi_image_free(image);  
glBindTexture(GL_TEXTURE_2D, 0); //Evitem que la imatge es modifiqui  
  
//UNIFORM  
GLuint texUniform = glGetUniformLocation(shaderProgram, "u_texture");  
glUseProgram(shaderProgram);  
glUniform1i(texUniform, 0);  
  
//------------------------------------------------------------------------------------------------------------------
```