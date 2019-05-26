# Excepciones en Python

## Algunos conceptos iniciales

Antes de comenzar a explayarnos sobre el tema de este artículo, es necesario repasar algunos conceptos que tal vez ya conocían, pero que son muy importantes a la hora de entender las excepciones en python.

En primer lugar, podemos decir que las excepciones son **errores**, **anomalías** ocurridas durante la ejecución de un programa, es decir, en tiempo de ejecución.

Python es un lenguaje que tiene **tipado dinámico**, que significa que resuelve los tipos (integer, string, list, dict, etc) de las variables en tiempo de ejecución, en base a los valores que se le asignan. Por ejemplo, 'saludo = 'Hola'' asignará en tiempo de ejecución a la variable *saludo* el tipo *string*.

Por otro lado, podemos decir que es un lenguaje **fuertemente tipado**, lo que determina que las variables a las que ya se les ha asignado un tipo no puedan ser tratadas como si tuviesen un tipo de dato distinto. Por ejemplo si definimos una variable 'mi_variable = 3' se le asignará automáticamente el tipo *int* y no vamos a poder considerarlo como el string *'3'*. Para poder combinar y cambiar de tipos, es necesario hacer **conversiones explícitas** de tipos: por ejemplo 'str()', 'int()', 'bool', etc.

Es importante entender estos dos conceptos ya que muchos de los errores que vamos a intentar comprender son o estan relacionados con los tipos, por no tener conversiones automáticas y por resolver dinámicamente las estructuras.

## Excepciones

Decíamos que las excepciones son **errores** que ocurren en tiempo de ejecución. Cuando el intérprete de Python encuentra un error levanta una excepción que interrumpe el flujo de ejecución (¡a menos que lo capturemos!).

Veamos un ejemplo para el caso de intentar acceder a una clave de un diccionario que no existe:

    diccionario = { 'mi_clave': 'valor' }
    print(diccionario['clave_que_no_existe'])

Al querer acceder a esa clave inexistente cortará la ejecución del programa y levantará una excepción:

    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    KeyError: 'clave_que_no_existe'

Podemos observar dos partes claves en la impresión anterior. Por un lado el **Traceback** (trazado de pila) que nos indica el lugar del archivo en donde ocurrió el error y como se desencadenó el mismo. Por otro lado, y muy relevante para el manejo de excepciones, es el **Tipo de error** que ocurrió y detalles: en este caso, *KeyError* (error de clave) y la clave que no encontró en el diccionario.

Como decíamos antes, las excepciones cortan la ejecución del programa, a menos que las capturemos y las procesemos debidamente. A continuación veremos como se hace. Hay lenguajes (como *Pascal*) que no poseen manejo de excepciones, pero en el caso de Python, posee un manejo de excepciones muy completo.

Para lograr capturar una excepción, Python nos provee un bloque llamado **try-except** donde incluiremos el código que estará bajo "observación" de que ocurra algún error. Dentro de esas claves pondremos todas las instrucciones a ejecutar y debajo colocaremos los **manejadores** de las distintas excepciones que puedan ocurrir. Si no encuentra el manejador correspondiente, la ejecución se cortará.

Volvamos al ejemplo del diccionario, con las nuevas instrucciones se vería de la siguiente forma:

    try:
        diccionario = { 'mi_clave': 'valor' }
        print(diccionario['clave_que_no_existe'])
    except KeyError:
        print('La clave del diccionario no existe.')

Con la palabra clave **except *KeyError*** indicamos que si ocurre dicha excepción, el código incluido debajo se va a ejecutar. Si corremos el código anterior imprimirá ''La clave del diccionario no existe.''.

Python nos permite utilizar varios bloques **except *Nombre de excepción***. Esto dependerá de la cantidad de excepciones que puedan ocurrir en el código observado y queramos capturar. Además, pueden manejarse dos excepciones distintas con el mismo código de procesamiento.

    try:
        diccionario = { 'mi_clave': 'valor' }
        print(diccionario['clave_que_no_existe'])
    except KeyError:
        print('La clave del diccionario no existe.')
    except TypeError:
        print('Ocurrió un error de tipos.')
    except (RuntimeError, NameError):
        print('Ocurrió un error inesperado.')

Existen otras dos cláusulas disponibles para usar:
- ***finaly***, que se ejecuta siempre, se hayan levantado excepciones o no. Esta sentencia suele usarse para hacer tareas de limpieza.
- ***else*** que se ejecuta solo si no ocurrió ninguna excepción.

En el código de abajo se verán en uso.

    try:
        diccionario = { 'mi_clave': 'valor' }
        print(diccionario['clave_que_no_existe'])
    except KeyError:
        print('La clave del diccionario no existe.')
    finally:
        print('Limpiando datos...')
    else:
        print('No ocurrió ningún error.')

## Creando nuestras propias excepciones

Python nos permite crear nuestras propias excepciones, heredando de la clase **Exception**. Con la palabra clave **raise** se levantará la excepción creada.

    class MayorAVeinte(Exception):
        def __init__(self):
            self.valor = 'Mayor a veinte.'
        def __str__(self):
            return “Error: “ + self.valor

    try:
        if resultado > 20:
            raise MayorAVeinte()
    except MayorAVeinte, e:
        print e

Si bien esto errores pueden ser manjeados por la lógica misma del programa, puede ser útil definir excepciones propias y que luego sean capturadas por los bloques try-catch.

## Listado de posibles excepciones

    BaseException: Clase de la que heredan todas las excepciones.

    Exception(BaseException): Super clase de todas las excepciones que no sean de salida.

    GeneratorExit(Exception): Se pide que se salga de un generador.

    StandardError(Exception): Clase base para todas las excepciones que no tengan que ver con salir del intérprete.

    ArithmeticError(StandardError): Clase base para los errores aritméticos.

    FloatingPointError(ArithmeticError): Error en una operación de coma flotante.

    OverflowError(ArithmeticError): Resultado demasiado grande para poder representarse.

    ZeroDivisionError(ArithmeticError): Lanzada cuando el segundo argumento de una operación de división o módulo era 0.

    AssertionError(StandardError): Falló la condición de un estamento assert.

    AttributeError(StandardError): No se encontró el atributo.

    EOFError(StandardError): Se intentó leer más allá del final de fichero.

    EnvironmentError(StandardError): Clase padre de los errores relacionados con la entrada/salida.

    IOError(EnvironmentError): Error en una operación de entrada/salida.

    OSError(EnvironmentError): Error en una llamada a sistema.

    WindowsError(OSError): Error en una llamada a sistema en Windows.

    ImportError(StandardError): No se encuentra el módulo o el elemento del módulo que se quería importar.

    LookupError(StandardError): Clase padre de los errores de acceso.

    IndexError(LookupError): El índice de la secuencia está fuera del rango posible.

    KeyError(LookupError): La clave no existe.

    MemoryError(StandardError): No queda memoria suficiente.

    NameError(StandardError): No se encontró ningún elemento con ese nombre.

    UnboundLocalError(NameError): El nombre no está asociado a ningun variable.

    ReferenceError(StandardError): El objeto no tiene ninguna referencia fuerte apuntando hacia él.

    RuntimeError(StandardError): Error en tiempo de ejecución no especificado.

    NotImplementedError(RuntimeError): Ese método o función no está implementado.

    SyntaxError(StandardError): Clase padre para los errores sintácticos.

    IndentationError(SyntaxError): Error en la indentación del archivo.

    TabError(IndentationError): Error debido a la mezcla de espacios y tabuladores.

    SystemError(StandardError): Error interno del intérprete.

    TypeError(StandardError): Tipo de argumento no apropiado.

    ValueError(StandardError): Valor del argumento no apropiado.

    UnicodeError(ValueError): Clase padre para los errores relacionados con unicode.

    UnicodeDecodeError(UnicodeError): Error de decodificación unicode.

    UnicodeEncodeError(UnicodeError): Error de codificación unicode.

    UnicodeTranslateError(UnicodeError): Error de traducción unicode.

    StopIteration(Exception): Se utiliza para indicar el final del iterador.

    Warning(Exception): Clase padre para los avisos.

    DeprecationWarning(Warning): Clase padre para avisos sobre características obsoletas.

    FutureWarning(Warning): Aviso. La semántica de la construcción cambiará en un futuro.

    ImportWarning(Warning): Aviso sobre posibles errores a la hora de importar.

    PendingDeprecationWarning(Warning): Aviso sobre características que se marcarán como obsoletas en un futuro próximo.

    RuntimeWarning(Warning): Aviso sobre comportmaientos dudosos en tiempo de ejecución.

    SyntaxWarning(Warning): Aviso sobre sintaxis dudosa.

    UnicodeWarning(Warning): Aviso sobre problemas relacionados con Unicode, sobre todo con problemas de conversión.

    UserWarning(Warning): Clase padre para avisos creados por el programador.

    KeyboardInterrupt(BaseException): El programa fué interrumpido por el usuario.

    SystemExit(BaseException): Petición del intérprete para terminar la ejecución.

## Referencias

- [Material de la cátedra](https://catedras.info.unlp.edu.ar)
- [Documentación oficial de Python](http://docs.python.org.ar/tutorial/3/errors.html)
- [Libro: "Python para todos"](http://www.utic.edu.py/citil/images/Manuales/Python_para_todos.pdf)