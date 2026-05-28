# Base_de_Dades_InnovateTech

## Página 1

3.  Base  de  dades  3.1.  Context  del  Projecte   Per  a  la  nostra  empresa  Innovatetech,  la  base  de  dades  central  en  MariaDB  no  és  
només
 
un
 
magatzem
 
d'informació
 
aïllat,
 
sinó
 
el
 
nucli
 
que
 
connecta
 
i
 
dona
 
suport
 
a
 
tot
 
el
 
projecte
 
de
 
xarxa
 
i
 
serveis:
  Estructura  Interna  i  Personal:  Mapeja  tot  l'organigrama  de  l'empresa  dividida  en  
departaments
 
i
 
empleats,
 
guardant
 
també
 
les
 
dades
 
salarials
 
protegides
 
a
 
nomines.
  Servei  de  Telefonia  (Asterisk):  Es  connecta  directament  amb  el  servidor  VoIP  per  
monitorar
 
les
 
extensions
 
dels
 
empleats
 
i
 
registrar
 
cada
 
trucada
 
en
 
temps
 
real
 
dins
 
de
 
la
 
taula
 
registre_trucades.
  Plataforma  Web  i  Streaming:  Emmagatzema  els  catàlegs  de  videos  de  la  web,  la  
configuració
 
dels
 
servidors
 
(config_servidor)
 
i
 
les
 
mètriques
 
de
 
rendiment
 
de
 
la
 
xarxa
 
(mesures_ample_banda).
  3.2.1:  Executem  un  SELECT  *  de  les  taules  inicials  (departaments,  empleats,  etc.)  per  
demostrar
 
que
 
la
 
base
 
de
 
dades
 
està
 
correctament
 
creada,
 
relacionada
 
i
 
poblada.
  
   


## Página 2

3.2.2:  Revisamos  de  forma  exhaustiva  las  tablas  de  infraestructura  (config_servidor,  
videos,
 
mesures_ample_banda),
 
donde
 
mostramos
 
el
 
catálogo
 
de
 
vídeos
 
de
 
la
 
web,
 
los
 
parámetros
 
de
 
streaming
 
de
 
vídeo/audio
 
(Jitsi,
 
Icecast,
 
RTMP,
 
HLS)
 
y
 
el
 
registro
 
histórico
 
del
 
rendimiento
 
y
 
ancho
 
de
 
banda
 
de
 
la
 
red
 
corporativa.
 
Y
 
verificamos
 
que
 
el
 
planificador
 
de
 
tareas
 
de
 
MariaDB
 
está
 
activo
 
mediante
 
la
 
variable
 
event_scheduler
 
en
 
estado
 
ON
 
y
 
consultamos
 
la
 
tabla
 
control_backups
 
para
 
evidenciar
 
que
 
el
 
script
 
automatizado
 
genera
 
las
 
copias
 
de
 
seguridad
 
diarias
 
de
 
las
 
tablas
 
críticas
 
sin
 
errores.
  
 
 


## Página 3

  3.3.1:  Ejecutamos  desde  la  consola  de  Linux  el  script  interactivo  
/home/adminbd/crear_usuaris.sh
 
para
 
automatizar
 
de
 
golpe
 
la
 
creación
 
de
 
las
 
cuentas
 
de
 
base
 
de
 
datos
 
de
 
los
 
empleados.
  
 


## Página 4

   


## Página 5

3.3.2:  Consultamos  la  tabla  interna  mysql.user  del  motor  para  demostrar  de  forma  
empírica
 
que
 
todos
 
los
 
usuarios
 
y
 
los
 
roles
 
del
 
proyecto
 
se
 
han
 
registrado
 
correctamente
 
en
 
la
 
plataforma.
  
  3.3.3:  Ejecutamos  el  comando  SHOW  GRANTS  específicamente  para  el  perfil  de  
administración
 
(administracio),
 
demostrando
 
que
 
cuenta
 
con
 
los
 
permisos
 
necesarios
 
para
 
gestionar
 
las
 
tablas
 
de
 
gestión
 
interna.
  
   3.3.4:  Realizamos  el  SHOW  GRANTS  para  los  roles  operativos  como  el  de  ventas  o  
trabajadores,
 
evidenciando
 
la
 
aplicación
 
estricta
 
del
 
principio
 
de
 
menor
 
privilegio,
 
donde
 
se
 
les
 
restringe
 
el
 
acceso
 
a
 
datos
 
sensibles
 
que
 
no
 
competen
 
a
 
su
 
función.
  


## Página 6

  Script  crear  usuarios  a  BD:   Desarrollamos  un  script  interactivo  en  Bash  (crear_usuaris.sh)  que  agiliza  y  securiza  
la
 
gestión
 
de
 
accesos
 
en
 
MariaDB
 
al
 
solicitar
 
secuencialmente
 
al
 
administrador
 
el
 
nombre,
 
host,
 
contraseña
 
y
 
rol
 
del
 
nuevo
 
empleado.
 
Una
 
vez
 
introducidos
 
los
 
datos,
 
el
 
script
 
se
 
conecta
 
automáticamente
 
al
 
motor
 
para
 
ejecutar
 
los
 
comandos
 
nativos
 
CREATE
 
USER
 
y
 
GRANT.
 
Este
 
desarrollo
 
elimina
 
errores
 
manuales
 
en
 
la
 
configuración
 
de
 
credenciales,
 
optimiza
 
el
 
despliegue
 
de
 
permisos
 
y
 
garantiza
 
el
 
cumplimiento
 
inmediato
 
del
 
principio
 
de
 
menor
 
privilegio
 
en
 
la
 
plantilla.
  
  


## Página 7

 

## Página 8

Diagrama  ER  BD:   Para  organizar  la  lógica  de  Innovatetech,  diseñamos  una  arquitectura  relacional  
dividida
 
en
 
tres
 
bloques
 
funcionales.
 
El
 
primero
 
gestiona
 
la
 
Estructura
 
Interna
 
y
 
Recursos
 
Humanos
 
mediante
 
las
 
entidades
 
departaments,
 
empleats
 
y
 
nomines.
 
Vinculamos
 
departamentos
 
y
 
empleados
 
en
 
una
 
relación
 
de
 
uno
 
a
 
muchos
 
(1:N)
 
para
 
estructurar
 
la
 
plantilla,
 
mientras
 
que
 
aislamos
 
los
 
sueldos
 
en
 
una
 
tabla
 
independiente
 
con
 
una
 
relación
 
uno
 
a
 
uno
 
(1:1)
 
ligada
 
por
 
el
 
DNI;
 
esta
 
separación
 
estratégica
 
nos
 
permite
 
restringir
 
los
 
privilegios
 
de
 
acceso
 
y
 
aplicar
 
de
 
manera
 
directa
 
los
 
disparadores
 
que
 
protegen
 
los
 
datos
 
salariales
 
de
 
cualquier
 
modificación
 
fraudulenta.
  El  segundo  bloque  corresponde  al  Módulo  de  Comunicaciones  e  Integración  con  
Asterisk,
 
compuesto
 
por
 
usuaris_comunicacio
 
y
 
registre_trucades.
 
Enlazamos
 
a
 
cada
 
empleado
 
con
 
su
 
terminal
 
telefónica
 
mediante
 
una
 
relación
 
1:1
 
para
 
gestionar
 
parámetros
 
críticos
 
en
 
tiempo
 
real
 
como
 
las
 
extensiones
 
o
 
el
 
estado
 
de
 
suspensión
 
(activo/bloqueado).
 
Desde
 
esta
 
base,
 
conectamos
 
la
 
tabla
 
registre_trucades
 
con
 
una
 
relación
 
de
 
uno
 
a
 
muchos
 
(1:N)
 
que
 
hereda
 
tanto
 
el
 
identificador
 
del
 
originador
 
como
 
del
 
destinatario;
 
este
 
diseño
 
es
 
clave
 
para
 
que
 
los
 
triggers
 
calculen
 
la
 
concurrencia
 
diaria
 
y
 
apliquen
 
bloqueos
 
automatizados
 
ante
 
abusos
 
de
 
la
 
cuota
 
mensual
 
de
 
minutos.
  El  tercer  bloque  integra  la  Infraestructura  Web,  Streaming  y  Auditoría  a  través  de  las  
tablas
 
videos,
 
config_servidor,
 
mesures_ample_banda,
 
avisos_auditoria
 
y
 
control_backups.
 
Definimos
 
la
 
entidad
 
de
 
vídeos
 
de
 
forma
 
aislada
 
para
 
que
 
funcione
 
como
 
un
 
catálogo
 
global
 
y
 
ágil
 
para
 
la
 
web,
 
mientras
 
que
 
centralizamos
 
las
 
variables
 
de
 
red
 
(IPs,
 
puertos
 
y
 
protocolos
 
como
 
Jitsi
 
o
 
Icecast)
 
y
 
las
 
métricas
 
de
 
rendimiento
 
en
 
tablas
 
técnicas
 
de
 
soporte.
 
Como
 
cierre
 
de
 
seguridad,
 
dejamos
 
los
 
logs
 
de
 
avisos_auditoria
 
y
 
control_backups
 
totalmente
 
desacoplados
 
de
 
claves
 
foráneas
 
de
 
negocio,
 
garantizando
 
que
 
el
 
Event
 
Scheduler
 
y
 
los
 
triggers
 
almacenen
 
sus
 
registros
 
de
 
forma
 
asíncrona
 
y
 
segura
 
sin
 
poner
 
en
 
riesgo
 
la
 
integridad
 
referencial
 
del
 
sistema.
 

## Página 9

 Triggers:   Para  verificar  la  robustez  de  la  base  de  datos  ante  posibles  ataques  o  malas  
prácticas,
 
realizamos
 
una
 
batería
 
de
 
siete
 
comprobaciones
 
técnicas
 
(checks)
 
estructuradas
 
entre
 
consultas
 
de
 
control
 
y
 
pruebas
 
de
 
error
 
sobre
 
el
 
comportamiento
 
del
 
motor.
 
En
 
este
 
bloque
 
demostramos
 
de
 
forma
 
empírica
 
cómo
 
nuestros
 
cuatro
 
disparadores
 
(triggers)
 
oficiales
 
de
 
producción
 
interceptan
 
y
 
abortan
 
en
 
tiempo
 
real
 
acciones
 
no
 
permitidas
 
,
 
tales
 
como
 
el
 
registro
 
de
 
llamadas
 
de
 
usuarios
 
suspendidos
 
en
 
la
 
plataforma
 
VoIP
 
,
 
la
 
manipulación
 
fraudulenta
 
de
 
salarios
 
en
 
la
 
tabla
 
de
 
nòmines
 
o
 
el
 
exceso
 
en
 
la
 
cuota
 
máxima
 
de
 
500
 
minutos
 
mensuales
 
por
 
cuenta.
 
Cada
 
una
 
de
 
estas
 
infracciones
 
devuelve
 
limpiamente
 
un
 
código
 
de
 
ERROR
 
1644
 
en
 
la
 
terminal
 
y
 
evidencia,
 
mediante
 
consultas
 
al
 
diccionario
 
de
 
datos
 
information_schema
 
y
 
a
 
la
 
tabla
 
histórica
 
de
 
avisos_auditoria
 
,
 
que
 
el
 
sistema
 
mantiene
 
una
 
trazabilidad
 
absoluta
 
y
 
un
 
blindaje
 
total
 
de
 
la
 
infraestructura
 
de
 
Innovatetech.
 
  Check  trigger  1  (Prueba  de  Error):  


## Página 10

    Check  trigger  2  (Consulta  de  apoyo):  
  Check  trigger  3  (Consulta  de  diseño):  
  Check  trigger  4  (Consulta  de  tabla  de  logs):  
  Check  trigger  5  (Consulta  +  Prueba  de  Error):  
  Check  trigger  6  (Prueba  de  Error):  
 


## Página 11

  

## Página 12

Check  trigger  7  (Mapa  final)  
 


