# parcial1tallerpoo

Parte A. Conceptos y lectura de código
1) Selección múltiple
Dada la clase:
class A:
x = 1
_y = 2
__z = 3
a = A()

¿Cuáles de los siguientes nombres existen como atributos accesibles directamente desde
a?
A) a.x
B) a._y
C) a.__z
D) a._A__z

todos menos la c) porque python hace name mangling y lo convierte a _A__z 

2) Salida del programa
class A:
def __init__(self):
self.__secret = 42
a = A()
print(hasattr(a, '__secret'), hasattr(a, '_A__secret'))

¿Qué imprime?

imprime False True, porque hasattr es como un booleano entonces 
imprime false true, el primero es falso ya que no se reconoce
como atributo por lo que vimos en el anterior ejercicios,
_A__secret si se reconoce

3) Verdadero/Falso (explica por qué)
a) El prefijo _ impide el acceso desde fuera de la clase.
b) El prefijo __ hace imposible acceder al atributo.
c) El name mangling depende del nombre de la clase.

a) Falso porque es un atributo protegido osea que no deberia usarlo
fuera de la clase pero si se puede acceder desde fuera

b) Falso porque python aplica name mangling y renombra internamente 
el atributo a _Clase__atributo, no es imposible acceder pero si mas dificil
se puede con el nombre renombrado

class A:
    def __init__(self):
        self.__secret = 42
a = A() #aqui lo renombramos osea que si se puede acceder
print(a._A__secret) #aqui accedemos y imprime 42

c) Verdadero porque como ya vimos en ejemplos anteriores
_(nombreClase)__atributo

4) Lectura de código

class Base:
    def __init__(self):
        self._token = "abc"

class Sub(Base):
    def reveal(self):
        return self._token

print(Sub().reveal())

¿Qué se imprime y por qué no hay error de acceso?

se imprime abc, y no hay error de acceso porque ( _ ) realmente 
no restringe el acceso a diferencia de java c++, sub es una subclase 
entonces tiene acceso al atributo protegido en _token , entonces 
hereda abc de base, al final reveal muestra abc 

5) Name mangling en herencia

class Base:
    def __init__(self):
        self.__v = 1

class Sub(Base):
    def __init__(self):
        super().__v = 2

    def show(self):
        return (self.__v, self._Base__v)

print (Sub().show())

¿Cuál es la salida?

(2, 1)

6) Identifica el error
class Caja:
    __slots__ = ('x',)

c = Caja()
c.x = 10
c.y = 20

¿Qué ocurre y por qué?

El error ocurre porque __slots__ restringe los atributos de la clase.
ya que la restriccion es 'x' entonces no funcionaria c.y y si c.x
no se puede crear dinámicamente y Python lanza un AttributeError.

7) Rellenar espacios
Completa para que b tenga un atributo “protegido por convención”.

class B:
    def __init__(self):
        self ______ = 99

Escribe el nombre correcto del atributo.

self._valor = 99

8) Lectura de métodos “privados”

class M:
    def __init__(self):
        self._state = 0

    def _step(self):
        self._state += 1
        return self._state
    
    def __tick(self):
        return self._step()
    
m = M() 
print(hasattr(m, '_step'), hasattr(m, '__tick'), hasattr(m, '_M__tick'))

¿Qué imprime y por qué?

devuelve True False True

9) Acceso a atributos privados

class S:
    def __init__(self):
        self.__data = [1, 2]
    def size(self):
        return len(self.__data)
    
s = S()

Accede a __data (solo para comprobar), sin modificar el código de la clase:
Escribe una línea que obtenga la lista usando name mangling y la imprima.

Escribe la línea solicitada.

prit(s._S__data)
imprimira [1, 2]

10) Comprensión de dir y mangling

class D:
    def __init__(self):
        self.__a = 1
        self._b = 2
        self.c = 3

d = D()
names 0 [n for n in dir(d) if 'a' in n]
print(names)

¿Cuál de estos nombres es más probable que aparezca en la lista: __a, _D__a o a?
Explica.

_D__a porque es el nombre real tras el mangling, el rango esta solo en el a, 
entonces ya que el mangling tiene el valor de a, esa es la respuesta

11) Completar propiedad con validación
Completa para que saldo nunca sea negativo.

class Cuenta:
    def __init__(Self, saldo):
        self._saldo = 0 
        self.saldo = saldo 

    @property
    def saldo(self):
        return self._saldo #devuelve el atributo protegido

    @saldo.setter
    def saldo(self, value):
        validar no-negarivo
        if value < 0:
            raise ValueError("El saldo no puede ser negativo")
        self._saldo = value

12) Propiedad de solo lectura

Convierte temperatura_f en un atributo de solo lectura que se calcula desde
temperatura_c.

class Termometro:
    def __init__(self, temperatura_c):
        self._c = float(temperatura_c)

Define aquí la propiedad temperatura_f: F = C * 9/5 + 32

Escribe la propiedad.

    @property
    def temperatura_f(self):
        return self._c * 9 / 5 + 32

13) Invariante con tipo
Haz que nombre sea siempre str. Si asignan algo que no sea str, lanza TypeError.

class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre
# Implementa property para nombre
    @property 
    def nombre(self):
        return self._nombre
    
    @nombre.setter
    def nombre(Self, value):
        if not isistance(value, str):
            raise TypeError("El nombre debe ser una cadena (str)")
        self._nombre = value

aqui el property permite acceder, nombre.setter intercepta la asignacion
el isinstance el para validar el tipo y lo validos como
str, sino es str lanza el type error

14) Encapsulación de colección

Expón una vista de solo lectura de una lista interna.

class Registro:
    def __init__(self):
        self.__items = []
    def add(self, x):
        self.__items.append(x)

# Crea una propiedad 'items' que retorne una tupla inmutable con el contenido
    @property
    def items(self):
    return tuple(self.__items)

15) Refactor a encapsulación

Refactoriza para evitar acceso directo al atributo
y validar que velocidad sea entre 0 y 200.

class Motor:
    def __init__(Self, velocidad)
        self._velocidad = 0
        self.velocidad = velocidad # refactor aquí

Escribe la versión con @property.

    @property 
    def velocidad(self):
        return self._velocidad

    @velocidad.setter
    def velocidad(self, value):
        if not (0 <= value <= 200):
            raise ValueError("La velocidad debe estar entre 0 y 200")
    self._velocidad = value

16) Elección de convención

Explica con tus palabras cuándo usarías _atributo frente 
a __atributo en una API pública de una librería.


_atributo seria para cosas internas que no forman parte de la API publica
pero que no se necesita ocultar por completo y __atributo es para evitar
accesos accidentales y porteger nombres o jerarquias de herencia

17) Detección de fuga de encapsulación

¿Qué problema hay aquí?

que se puede acceder a la lista y modificarla

class Buffer:
    def __init__(self, data):
        self._data = list(data)
    def get_data(self):
        return self._data

Propón una corrección.

class Buffer:
    def __init__(self, data):
        self._data = list(data)
    def get_data(self):
        return tuple(self._data)
    
18) Diseño con herencia y mangling

¿Dónde fallará esto y cómo lo arreglas?

class A:
    def __init__(self):
        self.__x = 1

class B(A):
    def get(self):
        return self.__x
    
falla cuando b intenta return self.__x siendo que aun no aplica
el name mangling que seria return self.__x, otra forma de arreglarlo
seria con el property

@property
def x(self):
    return self.__x

class B(A):
    def get(Self):
        return self.x 

entonces aqui se uso el property returnando el self.x
que seria el self.__x del A

19) Composición y fachada

Completa para exponer solo un método seguro de un objeto interno.

class _Repositorio:
    def __init__(self):
        self._datos = {}
    def guardar(self, k, v):
        self._datos[k] = v 
    def _dump(self):
        return dict(self._datos)
    
class Servicios:
        def __init__(self):
            self._repo = _Repositorio()

        # Expón un método 'guardar' que delegue en el repositorio,
        # pero NO expongas _dump ni __repo.
        def guardar(self, k, v):
            self.__repo.guardar(k, v)

aqui el repo esta con name mangling y solo 
se expone el guardar el cual es seguro

20) Mini-kata
Escribe una clase ContadorSeguro con:
• atributo “protegido” _n
• método inc() que suma 1
• propiedad n de solo lectura
• método “privado” __log() que imprima "tick" cuando se incrementa
Muestra un uso básico con dos incrementos y la lectura final.

class ContadorSeguro:
    def __init__(self):
        self._n = 0
    def inc(self):
        self._n += 1
        self.__log()
    
    @property
    def n(self)
        return self._n
    def __log(self)
        print("tick")

c = ContadorSeguro()
c.inc()
c.inc()
print(c.n)

resultado es  
tick
tick
2 -->


