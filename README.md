# Pipeline creacion de usuario y asignacion de Grupos

Se ingresan 4 parametros: 

Login
Nombre
Apellido
Departamento

El pipeline tiene diferentes stages 

Validacion de existencia del grupo, elegido en departamento. Si existe no hace nada y sigue su flujo, si no, lo crea
Validacion de existencia del usuario por Login. Si existe lo informa y sale del script, en caso que no:

Lo crea, le asigna uNombre y Apellido y el Departamento a eleccion
Crea una contraseña aleatoria que se tiene que cambiar despues del primer login y la muestra 
