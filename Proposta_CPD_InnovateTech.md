# Proposta_CPD_InnovateTech

## Página 1

Implementació  al  núvol  AWS  
Documentació  IP’s   
Servei  WEB  i  SFTP  →  3.213.220.104  Servei  Logs  →  32.197.66.8 Servei  Directori  Actiu  (LDAP)  →  3.227.167.255  
Domini  
innovate.tech.com   dc=innovate,dc=tech,dc=com  
LDAP  
 Aquí  lo  que  hacemos  con  Ansible  es  automatizar  todo  el  aprovisionamiento  de  la  instancia  
EC2
 
ldap01
 
en
 
AWS.
 
Nos
 
aseguramos
 
de
 
engancharla
 
a
 
la
 
subred
 
pública
 
que
 
nos
 
toca
 
dentro
 
de
 
la
 
VPC
 
y
 
le
 
metemos
 
la
 
clave
 
vockey.pem
 
para
 
poder
 
conectarnos
 
después
 
por
 
SSH
 
de
 
forma
 
segura.
 
Al
 
final,
 
mapeamos
 
la
 
IP
 
elástica
 
fija
 
3.227.167.255
 
para
 
que
 
el
 
servicio
 
de
 
directorio
 
activo
 
no
 
pierda
 
la
 
ruta
 
si
 
la
 
máquina
 
se
 
llega
 
a
 
reiniciar
 
en
 
la
 
consola.
  


## Página 3

     


## Página 4

  En  este  punto  del  despliegue,  nuestro  objetivo  es  purgar  cualquier  rastro  viejo  de  slapd  para  
montar
 
el
 
árbol
 
desde
 
cero.
 
Seteamos
 
el
 
sufijo
 
base
 
del
 
dominio
 
corporativo
 
de
 
la
 
empresa,
 
que
 
es
 
dc=innovate,dc=tech,dc=com
 
,
 
y
 
le
 
metemos
 
a
 
piñón
 
la
 
contraseña
 
de
 
administrador
 
(pirineus).
 
Como
 
buena
 
práctica
 
de
 
equipo,
 
dejamos
 
programado
 
un
 
script
 
de
 
backup
 
diario
 
y
 
un
 
health-check
 
automático
 
en
 
el
 
cron
 
para
 
que
 
el
 
servidor
 
aguante
 
en
 
producción.
 
  


## Página 7

Este  bloque  nos  sirve  para  poblar  el  directorio  inyectando  las  Unidades  Organizativas  de  
users
 
y
 
groups.
 
Primero
 
sacamos
 
el
 
hash
 
de
 
seguridad
 
con
 
slappasswd
 
y
 
luego
 
creamos
 
los
 
grupos
 
POSIX
 
users
 
(GID
 
5000)
 
y
 
admins
 
(GID
 
5001).
 
Después
 
metemos
 
en
 
bucle
 
las
 
cuentas
 
de
 
los
 
empleados
 
con
 
la
 
clase
 
inetOrgPerson
 
para
 
sus
 
correos
 
del
 
instituto
 
y
 
posixAccount
 
para
 
mapear
 
su
 
Home
 
directory
 
(/home/uid)
 
y
 
su
 
shell
 
predeterminada.


## Página 9

 
 
 
 
 
 
 


## Página 10

Security  group  
Es  crearà  un  Security  Group  a  AWS  per  permetre  que  totes  les  màquines  es  puguin  
comunicar
 
i
 
visualitzar
 
entre
 
elles.
 
 
 
 
 
 


## Página 11

DOCUMENTACIÓ  DEL  SERVEI  WEB  I  
SFTP
 
—
 
Innovate
 
Tech
 
1.  Descripció  general  del  servei  
El  servidor  WEB  +  SFTP  forma  part  de  la  infraestructura  desplegada  a  AWS  
per
 
al
 
projecte
 
Innovate
 
Tech
.
 
Aquesta  màquina  és  responsable  de:  
●  Servir  la  pàgina  web  corporativa  ●  Gestionar  l’accés  SFTP  per  a  usuaris  interns  ●  Emmagatzemar  i  publicar  contingut  web  ●  Permetre  la  pujada  de  fitxers  de  manera  segura  
IP  pública  assignada:  3.213.220.104    IP  privada:  172.31.22.229    Sistema  operatiu:  Ubuntu  Server  24.04  LTS  Tipus  d’instància:  t3.small  (2  vCPU,  2GB  RAM)  
Domini:  innovate.tech.com  
               

## Página 12

Creació  d’un  usuari  administrador  al  servidor  
               


## Página 13

2.  Creació  de  la  instància  a  AWS  
Segons  el  document  original,  la  instància  es  crea  amb:  
●  AMI:  Ubuntu  Server  24.04  LTS  ●  Tipus:  t3.small  ●  Clau  SSH:  linar.pem  ●  VPC:  vpc-078cea3f873b93c18  ●  Subnet:  subnet-08fc06193a72ea749  ●  Elastic  IP  associada:  3.213.220.104  
La  configuració  coincideix  amb  la  captura  del  document:  
“Ubuntu  Server  24.04  LTS  (HVM),  SSD  Volume  Type…  apto  para  la  capa  gratuita”    
“Dirección  IP  elástica:  3.213.220.104”  
 

## Página 14

 


## Página 15

 


## Página 16

  
Configuració  del  servei  SFTP   -  Crear  grup  d’usuaris  SFTP  -  Crear  usuari  SFTP  -  Crear  estructura  chroot   En  este  bloque  estructuramos  la  seguridad  del  almacenamiento  masivo  creando  un  grupo  
exclusivo
 
para
 
los
 
usuarios
 
de
 
SFTP
 
y
 
configurando
 
un
 
entorno
 
enjaulado
 
(chroot).
 
Hicimos
 
esto
 
para
 
bloquear
 
a
 
los
 
usuarios
 
en
 
sus
 
directorios
 
correspondientes
 
y
 
evitar
 
que
 
puedan
 
ponerse
 
a
 
navegar
 
por
 
el
 
resto
 
del
 
sistema
 
de
 
archivos
 
de
 
la
 
máquina
 
por
 
razones
 
obvias
 
de
 
seguridad.
 
 
 


## Página 17

     Configuració  SSH:  Aquí  configuramos  el  bloque  de  servidor  en  NGINX  para  mapear  el  dominio  corporativo  
innovate.tech.com.
 
Definimos
 
la
 
raíz
 
del
 
sitio
 
web
 
en
 
/var/www,
 
configuramos
 
el
 
index.php
 
para
 
que
 
sea
 
el
 
archivo
 
principal
 
de
 
carga
 
y
 
dejamos
 
lista
 
la
 
directiva
 
location
 
~
 
\.php$
 
pasándole
 
el
 
socket
 
fastcgi_pass
 
para
 
que
 
procese
 
correctamente
 
todo
 
el
 
código
 
backend
 
en
 
PHP.
  
 
 


## Página 18

 
         


## Página 19

Reiniciar  SSH  
    


## Página 20

  
Configuració  del  servidor  WEB  (NGINX)   -  Instal·lació  -  Directori  web   Después  de  preparar  el  SFTP,  instalamos  NGINX  como  servidor  web  principal,  apuntando  el  
directorio
 
raíz
 
a
 
/var/www
 
donde
 
pusimos
 
el
 
archivo
 
index
 
principal
 
en
 
PHP.
 
Además,
 
configuramos
 
el
 
VirtualHost
 
editando
 
el
 
fichero
 
/etc/nginx/sites-available/innovate_tech
 
para
 
enlazar
 
correctamente
 
nuestro
 
dominio
 
innovate.tech.com
 
con
 
el
 
servicio.
  
  
                    


## Página 21

  -  Fitxer  index.php   En  la  cabecera  del  archivo  montamos  la  lógica  en  PHP  para  validar  las  credenciales  de  la  
web
 
contra
 
nuestro
 
servidor
 
LDAP.
 
Usamos
 
ldap_connect
 
apuntando
 
a
 
la
 
IP
 
de
 
nuestra
 
instancia
 
(3.227.167.255),
 
forzamos
 
el
 
uso
 
del
 
protocolo
 
versión
 
3
 
y
 
armamos
 
dinámicamente
 
el
 
DN
 
del
 
usuario
 
concatenando
 
el
 
campo
 
del
 
formulario
 
con
 
nuestro
 
árbol
 
(uid=$user,ou=users,dc=innovate,dc=tech,dc=com).
 
Con
 
ldap_bind
 
comprobamos
 
si
 
la
 
contraseña
 
es
 
correcta:
 
si
 
conecta,
 
iniciamos
 
sesión
 
y
 
redirigimos;
 
si
 
falla,
 
capturamos
 
el
 
 ç5error  para  avisar  en  el  login.   
 
 


## Página 22

 
 


## Página 23

 
 


## Página 24

 
 


## Página 25

 


## Página 26

Esta  es  la  parte  del  código  que  renderiza  el  frontend  del  login  de  Innovate  Tech.  Es  un  
formulario
 
básico
 
estructurado
 
con
 
etiquetas
 
<form>
 
que
 
utiliza
 
el
 
método
 
POST
 
para
 
enviar
 
de
 
forma
 
segura
 
las
 
variables
 
user
 
y
 
password
 
al
 
propio
 
script.
 
También
 
le
 
incluimos
 
un
 
bloque
 
condicional
 
en
 
PHP
 
justo
 
arriba
 
para
 
que,
 
en
 
caso
 
de
 
que
 
la
 
variable
 
de
 
error
 
de
 
autenticación
 
LDAP
 
esté
 
activa,
 
pinte
 
un
 
mensaje
 
de
 
alerta
 
en
 
rojo
 
avisando
 
al
 
usuario
 
de
 
que
 
las
 
credenciales
 
no
 
son
 
válidas.
  
 
 


## Página 27

 
 


## Página 28

 
 
 
 
 
 
 
 


## Página 29

Configuració  del  VirtualHost  
Ruta:  /etc/nginx/sites-available/innovate_tech  
En  esta  captura  configuramos  Filebeat  en  la  máquina  web  para  recolectar  los  logs.  En  
filebeat.inputs
 
habilitamos
 
el
 
rastreo
 
de
 
rutas
 
nativas
 
apuntando
 
directamente
 
a
 
los
 
logs
 
de
 
NGINX
 
(/var/log/nginx/*.log).
 
Más
 
abajo,
 
deshabilitamos
 
la
 
salida
 
directa
 
a
 
Elasticsearch
 
(output.elasticsearch)
 
y
 
activamos
 
output.logstash
 
apuntando
 
a
 
la
 
IP
 
de
 
nuestro
 
servidor
 
centralizado
 
de
 
logs
 
(32.197.66.8:5044)
 
para
 
procesar
 
y
 
limpiar
 
las
 
cadenas
 
antes
 
de
 
indexarlas.
 
   


## Página 30

Activació:  Aquí  ejecutamos  un  systemctl  status  filebeat  para  validar  que  el  agente  arrancó  bien  y  sin  
errores
 
de
 
sintaxis
 
tras
 
modificar
 
el
 
YAML.
 
Como
 
se
 
ve
 
en
 
la
 
salida
 
de
 
la
 
terminal,
 
el
 
servicio
 
está
 
en
 
estado
 
active
 
(running)
 
y
 
ya
 
empezó
 
a
 
abrir
 
los
 
archivos
 
de
 
log
 
de
 
NGINX
 
para
 
enviar
 
en
 
tiempo
 
real
 
todo
 
el
 
flujo
 
de
 
eventos
 
de
 
la
 
web
 
hacia
 
el
 
pipeline
 
de
 
Logstash.
  
 
 


## Página 31

 
 
 


## Página 32

 


## Página 33

   
Logs:  En  este  primer  bloque  lo  que  hacemos  es  actualizar  los  repositorios  locales  del  servidor  con  
apt
 
e
 
instalar
 
la
 
versión
 
17
 
de
 
Java
 
(openjdk-17-jdk),
 
la
 
cual
 
es
 
obligatoria
 
para
 
poder
 
arrancar
 
las
 
herramientas
 
de
 
Elastic.
 
Aprovechamos
 
también
 
para
 
meter
 
dependencias
 
de
 
red
 
básicas
 
como
 
curl,
 
certificados,
 
gnupg
 
para
 
el
 
cifrado
 
y
 
el
 
demonio
 
rsyslog
 
que
 
nos
 
servirá
 
para
 
gestionar
 
toda
 
la
 
telemetría
 
local.
  
   


## Página 34

Aquí  tiramos  un  comando  curl  -XGET  contra  la  API  local  de  Elasticsearch  en  el  puerto  9200  
para
 
comprobar
 
que
 
el
 
motor
 
de
 
logs
 
está
 
levantado
 
y
 
respondiendo
 
correctamente.
 
Como
 
se
 
puede
 
ver
 
en
 
la
 
salida
 
de
 
la
 
terminal,
 
el
 
JSON
 
nos
 
devuelve
 
los
 
metadatos
 
del
 
clúster,
 
el
 
nombre
 
que
 
le
 
asignamos
 
en
 
el
 
playbook
 
(innovatetech-logs)
 
y
 
la
 
versión
 
de
 
Elastic
 
8.x
 
en
 
funcionamiento,
 
lo
 
que
 
nos
 
confirma
 
que
 
la
 
base
 
de
 
datos
 
documental
 
está
 
lista
 
para
 
empezar
 
a
 
indexar
 
y
 
almacenar
 
toda
 
la
 
telemetría
 
del
 
proyecto.
  
 
                       


## Página 35

En  esta  captura  mostramos  cómo  ejecutamos  en  terminal  nuestro  playbook  de  
automatización
 
con
 
ansible-playbook
 
para
 
configurar
 
de
 
arriba
 
a
 
abajo
 
el
 
servidor
 
srv-03-logs.
 
Como
 
se
 
ve
 
en
 
el
 
resumen
 
final
 
(PLAY
 
RECAP),
 
el
 
script
 
completó
 
con
 
éxito
 
todas
 
las
 
tareas
 
sin
 
lanzar
 
fallos:
 
actualizó
 
los
 
repositorios,
 
instaló
 
Java
 
17,
 
montó
 
de
 
golpe
 
todo
 
el
 
Stack
 
ELK
 
junto
 
con
 
Filebeat,
 
y
 
aplicó
 
los
 
cambios
 
de
 
memoria
 
necesarios
 
para
 
que
 
Elasticsearch
 
no
 
se
 
rompa
 
por
 
falta
 
de
 
RAM
 
en
 
AWS.
 
Al
 
automatizarlo
 
de
 
esta
 
manera,
 
garantizamos
 
que
 
toda
 
la
 
estructura
 
de
 
recolección
 
y
 
parseo
 
de
 
logs
 
se
 
levante
 
con
 
las
 
mismas
 
directivas,
 
configurando
 
las
 
interfaces
 
en
 
escucha
 
y
 
asegurando
 
que
 
los
 
servicios
 
arranquen
 
solos
 
si
 
la
 
máquina
 
se
 
reinicia.
  
   


## Página 36

  PLAYBOOK  de  logs:   -  name:  ELK  Stack  -  SRV-03  Logs  InnovateTech    hosts:  logs    become:  yes     tasks:       -  name:  Actualitzar  sistema        apt:          update_cache:  yes          upgrade:  yes       -  name:  Instal·lar  Java  i  dependències        apt:          name:            -  openjdk-17-jdk            -  apt-transport-https            -  ca-certificates            -  curl            -  gnupg            -  rsyslog          state:  present  


## Página 37

cat  >  ~/ansible/playbook_logs.yml  <<  'EOF'   ---     -  name:  Crear  directori  keyrings        file:          path:  /etc/apt/keyrings          state:  directory          mode:  '0755'       -  name:  Afegir  clau  GPG  Elastic        shell:  |          curl  -fsSL  https://artifacts.elastic.co/GPG-KEY-elasticsearch  |  gpg  --dearmor  -o  
/etc/apt/keyrings/elasticsearch.gpg
       args:          creates:  /etc/apt/keyrings/elasticsearch.gpg       -  name:  Afegir  repositori  Elastic        copy:          dest:  /etc/apt/sources.list.d/elastic-8.x.list          content:  |            deb  [signed-by=/etc/apt/keyrings/elasticsearch.gpg]  
https://artifacts.elastic.co/packages/8.x/apt
 
stable
 
main
      -  name:  Instal·lar  Elasticsearch  Kibana  Logstash  Filebeat        apt:          name:            -  elasticsearch            -  kibana            -  logstash            -  filebeat          state:  present          update_cache:  yes       -  name:  Ajustar  virtual  memory  (vm.max_map_count)  per  a  Elasticsearch        sysctl:          name:  vm.max_map_count          value:  "262144"          state:  present          reload:  yes       -  name:  Eliminar  dades  antigues  indexades  per  evitar  conflicte  de  nom  de  cluster        file:          path:  /var/lib/elasticsearch          state:  absent       -  name:  Resetear  i  forçar  permisos  absoluts  en  directoris  clau        file:          path:  "{{  item  }}"          state:  directory  

## Página 38

        owner:  elasticsearch          group:  elasticsearch          recurse:  yes        loop:          -  /var/lib/elasticsearch          -  /var/log/elasticsearch          -  /etc/elasticsearch       -  name:  Configurar  Elasticsearch        copy:          dest:  /etc/elasticsearch/elasticsearch.yml          owner:  elasticsearch          group:  elasticsearch          mode:  '0644'          content:  |            cluster.name:  innovatetech-logs            node.name:  srv-03-logs            path.data:  /var/lib/elasticsearch            path.logs:  /var/log/elasticsearch            network.host:  0.0.0.0            http.port:  9200            discovery.type:  single-node            xpack.security.enabled:  false       -  name:  Configurar  heap  JVM  al  minim  absolut  per  evitar  OOM  Kill        copy:          dest:  /etc/elasticsearch/jvm.options.d/heap.options          owner:  elasticsearch          group:  elasticsearch          mode:  '0644'          content:  |            -Xms256m            -Xmx256m       -  name:  Configurar  Kibana        copy:          dest:  /etc/kibana/kibana.yml          owner:  root          group:  kibana          mode:  '0644'          content:  |            server.port:  5601            server.host:  "0.0.0.0"            server.name:  "innovatetech-kibana"            elasticsearch.hosts:  ["http://localhost:9200"]       -  name:  Configurar  Logstash        copy:  

## Página 39

        dest:  /etc/logstash/conf.d/syslog.conf          content:  |            input  {              syslog  {                port  =>  5144                type  =>  "syslog"              }              beats  {                port  =>  5044                type  =>  "beats"              }            }            filter  {              if  [type]  ==  "syslog"  {                grok  {                  match  =>  {                    "message"  =>  "%{SYSLOGTIMESTAMP:timestamp}  
%{SYSLOGHOST:hostname}
 
%{DATA:program}:
 
%{GREEDYDATA:msg}"
                 }                }              }            }            output  {              elasticsearch  {                hosts  =>  ["localhost:9200"]                index  =>  "innovatetech-%{+YYYY.MM.dd}"              }            }       -  name:  Configurar  Filebeat        copy:          dest:  /etc/filebeat/filebeat.yml          content:  |            filebeat.inputs:              -  type:  log                enabled:  true                paths:                  -  /var/log/syslog                  -  /var/log/auth.log                  -  /var/log/*.log                fields:                  server:  srv-03                  environment:  production            output.logstash:              hosts:  ["localhost:5044"]            setup.kibana:              host:  "localhost:5601"   

## Página 40

    -  name:  Configurar  Rsyslog  recepcio  remota        copy:          dest:  /etc/rsyslog.d/10-remote.conf          content:  |            module(load="imudp")            input(type="imudp"  port="514")            module(load="imtcp")            input(type="imtcp"  port="514")            $template  RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"            *.*  ?RemoteLogs       -  name:  Crear  directori  logs  remots        file:          path:  /var/log/remote          state:  directory          owner:  syslog          group:  adm          mode:  '0755'       -  name:  Forçar  recàrrega  de  daemon  de  systemd        systemd:          daemon_reload:  yes       -  name:  Arrencar  tots  els  serveis        systemd:          name:  "{{  item  }}"          state:  started          enabled:  yes        loop:          -  elasticsearch          -  kibana          -  logstash          -  filebeat          -  rsyslog   
 

## Página 41

Configuració  dels  documenrs  .php  sudo  cat  /var/www/innovate_tech/vendes/peticions.php  <?php  session_start();  require_once  __DIR__  .  '/../includes/rols.php';  require_once  __DIR__  .  '/../includes/db.php';  requereix('peticions_serveis');   $role     =  $_SESSION['rol']  ??  'usuari';  $user_id  =  $_SESSION['user_id']  ??  0;  $missatge  =  '';  $error     =  '';  $pdo       =  get_pdo();   //  ELIMINAR  if  (isset($_GET['eliminar'])  &&  is_numeric($_GET['eliminar']))  {      try  {          $pdo->prepare("DELETE  FROM  peticions_serveis  WHERE  id  =  
?")->execute([$_GET['eliminar']]);
         $missatge  =  'Petició  eliminada.';      }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }  }   //  CANVIAR  ESTAT  if  ($_SERVER['REQUEST_METHOD']  ===  'POST'  &&  isset($_POST['canviar_estat']))  {      $id     =  (int)($_POST['id']  ??  0);      $estat  =  $_POST['estat']  ??  '';      if  ($id  &&  in_array($estat,  ['pendent','en_curs','completada','rebutjada']))  {          try  {              $pdo->prepare("UPDATE  peticions_serveis  SET  estat  =  ?  WHERE  id  =  
?")->execute([$estat,
 
$id]);
             $missatge  =  'Estat  actualitzat.';          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  }   //  NOVA  PETICIÓ  if  ($_SERVER['REQUEST_METHOD']  ===  'POST'  &&  isset($_POST['nova']))  {      $client_nom    =  trim($_POST['client_nom']  ??  '');      $client_email  =  trim($_POST['client_email']  ??  '');      $servei        =  trim($_POST['servei']  ??  '');      $descripcio    =  trim($_POST['descripcio']  ??  '');       if  ($client_nom  &&  $servei)  {          try  {              $pdo->prepare("INSERT  INTO  peticions_serveis  (id_usuari,  client_nom,  client_email,  
servei,
 
descripcio)
 
VALUES
 
(?,?,?,?,?)")
 

## Página 42

                ->execute([$user_id,  $client_nom,  $client_email,  $servei,  $descripcio]);              $missatge  =  "Petició  per  a  \"$client_nom\"  creada  correctament.";          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  else  {  $error  =  'Omple  client  i  servei.';  }  }   $peticions  =  $pdo->query("SELECT  *  FROM  peticions_serveis  ORDER  BY  created_at  
DESC")->fetchAll();
 ?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Peticions  ·  Innovate  Tech</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="/css/styles.css">    <style>      body  {  background:#030712;  color:white;  font-family:sans-serif;  }      .page-wrap  {  max-width:1000px;  margin:0  auto;  padding:40px  20px;  }      .page-header  {  display:flex;  align-items:center;  gap:16px;  margin-bottom:32px;  }      .page-header  a  {  color:#6366f1;  text-decoration:none;  font-size:14px;  }      h1  {  font-size:26px;  margin:0;  }      .badge  {  font-size:11px;  padding:2px  10px;  border-radius:10px;  
background:rgba(34,197,94,0.2);
 
color:#22c55e;
 
font-weight:bold;
 
}
     .layout  {  display:grid;  grid-template-columns:1fr  300px;  gap:24px;  }      @media(max-width:700px){  .layout  {  grid-template-columns:1fr;  }  }      .card  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:12px;  padding:24px;  
}
     .card  h2  {  font-size:15px;  margin:0  0  16px  0;  color:#9ca3af;  }      label  {  display:block;  font-size:13px;  color:#9ca3af;  margin-bottom:5px;  margin-top:14px;  }      input[type=text],  input[type=email],  select,  textarea  {        width:100%;  padding:10px  12px;  background:#0b1020;  border:1px  solid  #374151;        border-radius:8px;  color:white;  font-size:14px;  box-sizing:border-box;      }      input:focus,  select:focus,  textarea:focus  {  outline:none;  border-color:#4f46e5;  }      textarea  {  resize:vertical;  min-height:70px;  }      .btn-submit  {  margin-top:16px;  width:100%;  padding:11px;  background:#4f46e5;  
border:none;
 
border-radius:8px;
 
color:white;
 
font-size:14px;
 
cursor:pointer;
 
transition:.2s;
 
}
     .btn-submit:hover  {  background:#6366f1;  }      .alert-ok   {  background:rgba(34,197,94,0.15);  color:#22c55e;  border:1px  solid  #22c55e;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     .alert-err  {  background:rgba(239,68,68,0.15);  color:#ef4444;  border:1px  solid  #ef4444;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     table  {  width:100%;  border-collapse:collapse;  }      th  {  color:#6366f1;  font-size:11px;  text-transform:uppercase;  letter-spacing:.05em;  
padding:10px
 
12px;
 
text-align:left;
 
border-bottom:1px
 
solid
 
#1f2937;
 
background:#0b1020;
 
}
     td  {  padding:10px  12px;  border-top:1px  solid  #1f2937;  color:#d1d5db;  font-size:13px;  
vertical-align:middle;
 
}
 

## Página 43

    tr:hover  td  {  background:#0b1020;  }      .estat-pendent     {  color:#fbbf24;  font-weight:bold;  }      .estat-en_curs     {  color:#6366f1;  font-weight:bold;  }      .estat-completada  {  color:#22c55e;  font-weight:bold;  }      .estat-rebutjada   {  color:#ef4444;  font-weight:bold;  }      .btn-del  {  background:none;  border:1px  solid  #374151;  color:#ef4444;  border-radius:6px;  
padding:4px
 
10px;
 
cursor:pointer;
 
font-size:12px;
 
transition:.2s;
 
}
     .btn-del:hover  {  background:#ef4444;  color:white;  border-color:#ef4444;  }      .estat-form  {  display:flex;  gap:6px;  align-items:center;  }      .estat-form  select  {  padding:4px  8px;  font-size:12px;  width:auto;  }      .btn-save  {  background:#4f46e5;  border:none;  color:white;  border-radius:6px;  padding:4px  
10px;
 
cursor:pointer;
 
font-size:12px;
 
}
     .btn-save:hover  {  background:#6366f1;  }      .empty  {  text-align:center;  padding:40px;  color:#6b7280;  font-size:14px;  }    </style>  </head>  <body>  <div  class="page-wrap">    <div  class="page-header">      <a  href="/index.php">←  Tornar</a>      <h1> 📝  Peticions  de  serveis</h1>      <span  class="badge"><?=  strtoupper(htmlspecialchars($role))  ?></span>    </div>     <?php  if  ($missatge):  ?><div  class="alert-ok"> ✅  <?=  htmlspecialchars($missatge)  
?></div><?php
 
endif;
 
?>
   <?php  if  ($error):     ?><div  class="alert-err"> ❌  <?=  htmlspecialchars($error)  
?></div><?php
 
endif;
 
?>
    <div  class="layout">      <div  class="card">        <h2>Peticions  registrades  (<?=  count($peticions)  ?>)</h2>        <?php  if  (!empty($peticions)):  ?>        <table>          
<thead><tr><th>Client</th><th>Servei</th><th>Estat</th><th>Data</th><th></th></tr></the
ad>
         <tbody>            <?php  foreach  ($peticions  as  $p):  ?>            <tr>              <td>                <?=  htmlspecialchars($p['client_nom'])  ?>                <?php  if  ($p['client_email']):  ?>                  <br><span  style="color:#6b7280;font-size:12px;"><?=  
htmlspecialchars($p['client_email'])
 
?></span>
               <?php  endif;  ?>              </td>              <td><?=  htmlspecialchars($p['servei'])  ?></td>  

## Página 44

            <td>                <form  method="POST"  class="estat-form">                  <input  type="hidden"  name="canviar_estat"  value="1">                  <input  type="hidden"  name="id"  value="<?=  $p['id']  ?>">                  <select  name="estat">                    <option  value="pendent"     <?=  $p['estat']==='pendent'     ?  'selected':''  
?>>Pendent</option>
                   <option  value="en_curs"     <?=  $p['estat']==='en_curs'     ?  'selected':''  ?>>En  
curs</option>
                   <option  value="completada"  <?=  $p['estat']==='completada'  ?  'selected':''  
?>>Completada</option>
                   <option  value="rebutjada"   <?=  $p['estat']==='rebutjada'   ?  'selected':''  
?>>Rebutjada</option>
                 </select>                  <button  type="submit"  class="btn-save"> ✓ </button>                </form>              </td>              <td><?=  htmlspecialchars(substr($p['created_at'],  0,  16))  ?></td>              <td>                <a  href="?eliminar=<?=  $p['id']  ?>"  class="btn-del"  onclick="return  confirm('Eliminar  
aquesta
 
petició?')">
🗑
</a>
             </td>            </tr>            <?php  endforeach;  ?>          </tbody>        </table>        <?php  else:  ?><div  class="empty">No  hi  ha  peticions  encara.</div><?php  endif;  ?>      </div>       <div  class="card">        <h2>Nova  petició</h2>        <form  method="POST">          <input  type="hidden"  name="nova"  value="1">          <label>Nom  del  client  *</label>          <input  type="text"  name="client_nom"  placeholder="Nom  del  client"  required>          <label>Email  del  client</label>          <input  type="email"  name="client_email"  placeholder="correu@empresa.com">          <label>Servei  sol·licitat  *</label>          <select  name="servei"  required>            <option  value="">—  Selecciona  —</option>            <option>CPD  /  Infraestructura</option>            <option>Streaming</option>            <option>Videoconferència</option>            <option>Manteniment</option>            <option>Consultoria</option>            <option>Altres</option>          </select>          <label>Descripció</label>  

## Página 45

        <textarea  name="descripcio"  placeholder="Detalls  addicionals..."></textarea>          <button  type="submit"  class="btn-submit"> ➕  Crear  petició</button>        </form>      </div>    </div>  </div>  </body>  </html>    sudo  cat  /var/www/innovate_tech/index.php  <?php  session_start();  require_once  __DIR__  .  '/includes/rols.php';   $logged  =  isset($_SESSION['user']);  $rol     =  $_SESSION['rol']  ??  'usuari';   $pot_video   =  $logged  &&  pot('veure_videos');  $pot_audio   =  $logged  &&  pot('veure_audio');  $pot_stream  =  $logged  &&  pot('veure_streaming');  $pot_meet    =  $logged  &&  pot('entrar_meet');   $login_error  =  $_GET['login_error']  ??  '';  $login_errors  =  [      'camps_buits'  =>  'Omple  el  nom  d\'usuari  i  la  contrasenya.',      'ldap_error'   =>  'Error  de  connexió  al  servidor  d\'autenticació.',      'credencials'  =>  'Usuari  o  contrasenya  incorrectes.',      'bloquejat'    =>  'El  teu  compte  està  bloquejat.  Contacta  amb  l\'administrador.',  ];  $error_msg  =  $login_errors[$login_error]  ??  '';  $rol_info   =  info_rol($rol);  $role  =  $rol;  ?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Innovate  Tech  ·  CPD  al  Núvol</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="css/styles.css">    <script  src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>    <style>      .media-nav  {        display:  flex;        justify-content:  center;        gap:  10px;        background:  rgba(2,6,23,0.95);  

## Página 46

      padding:  12px  20px;        border-bottom:  1px  solid  rgba(79,99,241,0.3);      }      .media-nav  a  {        padding:  10px  24px;        border:  2px  solid  #4f46e5;        border-radius:  25px;        color:  white;        text-decoration:  none;        font-size:  15px;        transition:  0.3s;      }      .media-nav  a:hover  {  background:  #4f46e5;  }      .media-nav  a.disabled  {        opacity:  0.4;  cursor:  not-allowed;  pointer-events:  none;        border-color:  #374151;      }       .media-page  {  display:  none;  padding:  40px  5vw;  min-height:  80vh;  }      .media-page.active  {  display:  block;  }      .home-page  {  display:  block;  }      .home-page.hidden  {  display:  none;  }       video  {  width:  100%;  border-radius:  10px;  border:  2px  solid  #4f46e5;  margin:  20px  0;  }      .live-badge  {        display:  inline-block;  background:  #ef4444;        padding:  4px  14px;  border-radius:  20px;  font-size:  13px;  margin-bottom:  10px;      }      .video-list  {  display:  flex;  gap:  15px;  flex-wrap:  wrap;  margin-bottom:  20px;  }      .video-card  {        background:  #111827;  border:  2px  solid  #1f2937;        border-radius:  10px;  padding:  15px  25px;        cursor:  pointer;  transition:  0.3s;  flex:  1;  text-align:  center;  color:  white;      }      .video-card:hover,  .video-card.active  {  border-color:  #4f46e5;  background:  #0b1020;  }      .video-card  h3  {  color:  #6366f1;  margin-bottom:  5px;  }       /*  AUDIO  */      .audio-list  {  display:  flex;  flex-direction:  column;  gap:  12px;  margin:  20px  0;  }      .audio-card  {        background:  #111827;  border:  2px  solid  #1f2937;        border-radius:  10px;  padding:  16px  24px;        cursor:  pointer;  transition:  0.3s;  color:  white;        display:  flex;  align-items:  center;  gap:  15px;      }      .audio-card:hover,  .audio-card.active  {  border-color:  #4f46e5;  background:  #0b1020;  }      .audio-card  h3  {  color:  #6366f1;  margin:  0;  font-size:  16px;  }      .audio-card  p  {  margin:  4px  0  0  0;  color:  #9ca3af;  font-size:  13px;  }  

## Página 47

    .audio-icon  {  font-size:  28px;  }       /*  LOCKED  */      .content-locked  {        text-align:  center;  padding:  80px  20px;  color:  #9ca3af;      }      .content-locked  .lock-icon  {  font-size:  48px;  margin-bottom:  16px;  }      .content-locked  h3  {  color:  #4f46e5;  margin-bottom:  10px;  font-size:  22px;  }      .content-locked  p  {  margin:  6px  0;  font-size:  15px;  }      .content-locked  a  {        display:  inline-block;  margin-top:  20px;  padding:  12px  28px;        background:  #4f46e5;  color:  white;  border-radius:  8px;        text-decoration:  none;  font-size:  15px;  transition:  0.2s;      }      .content-locked  a:hover  {  background:  #6366f1;  }      .meet-locked  {        text-align:  center;  padding:  80px  20px;  color:  #9ca3af;      }      .meet-locked  .lock-icon  {  font-size:  48px;  margin-bottom:  16px;  }      .meet-locked  h3  {  color:  #ef4444;  margin-bottom:  10px;  font-size:  22px;  }       /*  CONTACTE  */      #contacte  {  min-height:  60vh;  display:  flex;  flex-direction:  column;  justify-content:  center;  
padding:
 
60px
 
10vw;
 
}
     .contact-form  {  width:  100%;  max-width:  100%;  }      .contact-form  .form-row  {  display:  flex;  gap:  15px;  width:  100%;  margin-bottom:  15px;  }      .contact-form  .form-row  input  {  flex:  1;  width:  100%;  padding:  14px;  font-size:  16px;  }      .contact-form  textarea  {  width:  100%;  padding:  14px;  font-size:  16px;  resize:  vertical;  
min-height:
 
180px;
 
}
     .contact-form  button  {  margin-top:  15px;  padding:  14px  40px;  font-size:  16px;  }       /*  USER  AVATAR  DROPDOWN  */      .user-menu  {  position:  relative;  display:  inline-block;  }      .user-avatar  {        width:  38px;  height:  38px;  border-radius:  50%;        background:  #4f46e5;  color:  white;  font-size:  16px;  font-weight:  bold;        display:  flex;  align-items:  center;  justify-content:  center;        cursor:  pointer;  border:  2px  solid  #6366f1;  transition:  0.2s;        user-select:  none;      }      .user-avatar:hover  {  background:  #6366f1;  }      .user-dropdown  {        display:  none;  position:  absolute;  right:  0;  top:  48px;        background:  #111827;  border:  1px  solid  #4f46e5;        border-radius:  10px;  min-width:  240px;  z-index:  1000;        box-shadow:  0  8px  24px  rgba(0,0,0,0.4);  overflow:  hidden;      }      .user-dropdown.open  {  display:  block;  }  

## Página 48

    .dd-info  {  padding:  14px  16px;  border-bottom:  1px  solid  #1f2937;  }      .dd-name  {  color:  white;  font-size:  14px;  font-weight:  bold;  margin:  0  0  3px  0;  }      .dd-email  {  color:  #9ca3af;  font-size:  12px;  margin:  0  0  5px  0;  }      .dd-role  {  font-size:  11px;  padding:  2px  8px;  border-radius:  10px;  display:  inline-block;  
margin-top:
 
4px;
 
font-weight:
 
bold;
 
}
     .role-admin          {  background:  rgba(239,68,68,0.2);    color:  #ef4444;  }      .role-vendes         {  background:  rgba(34,197,94,0.2);    color:  #22c55e;  }      .role-administracio  {  background:  rgba(251,191,36,0.2);   color:  #fbbf24;  }      .role-treballador    {  background:  rgba(99,102,241,0.2);   color:  #6366f1;  }      .role-usuari         {  background:  rgba(156,163,175,0.2);  color:  #9ca3af;  }      .dd-section-title  {        padding:  8px  16px  4px  16px;        font-size:  11px;  color:  #6366f1;        text-transform:  uppercase;  letter-spacing:  0.08em;  font-weight:  bold;        border-top:  1px  solid  #1f2937;  margin-top:  2px;      }      .dd-player  {  padding:  10px  16px;  border-bottom:  1px  solid  #1f2937;  display:  none;  }      .dd-player.visible  {  display:  block;  }      .dd-song  {  color:  #6366f1;  font-size:  12px;  margin:  0  0  8px  0;  white-space:  nowrap;  
overflow:
 
hidden;
 
text-overflow:
 
ellipsis;
 
}
     .dd-controls  {  display:  flex;  align-items:  center;  gap:  10px;  }      .dd-controls  button  {  background:  none;  border:  none;  color:  white;  font-size:  18px;  cursor:  
pointer;
 
padding:
 
4px;
 
transition:
 
0.2s;
 
}
     .dd-controls  button:hover  {  color:  #6366f1;  }      .dd-progress  {  flex:  1;  height:  4px;  background:  #1f2937;  border-radius:  2px;  overflow:  
hidden;
 
cursor:
 
pointer;
 
}
     .dd-progress-bar  {  height:  100%;  background:  #4f46e5;  width:  0%;  transition:  width  0.5s;  }      .user-dropdown  a  {  display:  block;  padding:  10px  16px;  color:  #d1d5db;  text-decoration:  
none;
 
font-size:
 
13px;
 
transition:
 
0.2s;
 
}
     .user-dropdown  a:hover  {  background:  #1f2937;  color:  white;  }      .dd-logout  {  color:  #ef4444  !important;  border-top:  1px  solid  #1f2937;  }    </style>  </head>  <body>     <!--  HEADER  -->    <header  class="main-header">      <div  class="logo"  onclick="showHome()"  
style="cursor:pointer;">Innovate<span>Tech</span></div>
     <nav  class="main-nav">       <?php  if($logged):  ?>        <a  href="#serveis">Serveis</a>        <a  href="#cpd">CPD  AWS</a>        <a  href="#contacte">Contacte</a>        <div  class="user-menu">          <div  class="user-avatar"  onclick="toggleDropdown()">            <?=  strtoupper(substr($_SESSION['name']  ??  $_SESSION['user'],  0,  1))  ?>          </div>  

## Página 49

        <div  class="user-dropdown"  id="user-dropdown">             <!--  Info  usuari  -->            <div  class="dd-info">              <p  class="dd-name"><?=  htmlspecialchars($_SESSION['name']  ??  
$_SESSION['user'])
 
?></p>
             <p  class="dd-email"><?=  htmlspecialchars($_SESSION['email']  ??  '')  ?></p>              <span  class="dd-role  role-<?=  htmlspecialchars($role)  ?>"><?=  
strtoupper(htmlspecialchars($role))
 
?></span>
           </div>             <!--  Mini  reproductor  -->            <div  class="dd-player"  id="dd-player">              <p  class="dd-song"  id="dd-song-title">—</p>              <div  class="dd-controls">  
              
<button
 
onclick="prevSong()">
⏮ 
</button>
               <button  onclick="togglePlay()"  id="btn-play"> ▶ </button>  
              
<button
 
onclick="nextSong()">
⏭ 
</button>
               <div  class="dd-progress"  onclick="seekAudio(event)">                  <div  class="dd-progress-bar"  id="dd-progress-bar"></div>                </div>              </div>            </div>             <!--  TREBALLADOR  -->            <?php  if(pot('historial_trucades')  ||  pot('registrar_activitat')  ||  pot('docs_interns')):  ?>              <div  class="dd-section-title"> 👷  Treballador</div>              <?php  if(pot('historial_trucades')):  ?>                <a  href="/treballador/historial.php"> 📋  El  meu  historial</a>              <?php  endif;  ?>              <?php  if(pot('registrar_activitat')):  ?>                <a  href="/treballador/activitat.php"> ✏  Registrar  activitat</a>              <?php  endif;  ?>              <?php  if(pot('docs_interns')):  ?>                <a  href="/treballador/docs.php"> 📁  Docs  interns</a>              <?php  endif;  ?>            <?php  endif;  ?>             <!--  VENDES  -->            <?php  if(pot('peticions_serveis')  ||  pot('gestionar_contactes')):  ?>              <div  class="dd-section-title"> 💼  Vendes</div>              <?php  if(pot('peticions_serveis')):  ?>                <a  href="/vendes/peticions.php"> 📝  Peticions  de  serveis</a>              <?php  endif;  ?>              <?php  if(pot('gestionar_contactes')):  ?>                <a  href="/vendes/contactes.php"> 👥  Contactes  clients</a>              <?php  endif;  ?>            <?php  endif;  ?>  

## Página 50

           <!--  ADMINISTRACIÓ  -->            <?php  if(pot('gestionar_usuaris')):  ?>              <div  class="dd-section-title"> 🏢  Administració</div>              <a  href="/admin/usuaris.php"> ⚙  Gestionar  usuaris</a>            <?php  endif;  ?>            <a  href="logout.php"  class="dd-logout"> 🚪  Tancar  sessió</a>          </div>        </div>       <?php  else:  ?>        <a  href="crear_usuari.php"  class="btn-outline">Crear  usuari</a>        <button  id="btn-login"  class="btn-outline">Inicia  sessió</button>       <?php  endif;  ?>      </nav>    </header>     <!--  BARRA  DE  PESTANYES  MULTIMÈDIA  -->    <div  class="media-nav">      <?php  if($logged):  ?>        <?php  if($pot_audio):  ?>          <a  href="#"  onclick="showPage('audio')"> 🎵  Àudio</a>        <?php  else:  ?>          <a  class="disabled"> 🎵  Àudio</a>        <?php  endif;  ?>        <?php  if($pot_stream):  ?>          <a  href="#"  onclick="showPage('streaming')"> 📡  Streaming</a>        <?php  else:  ?>          <a  class="disabled"> 📡  Streaming</a>        <?php  endif;  ?>        <?php  if($pot_video):  ?>          <a  href="#"  onclick="showPage('video')"> 🎬  Vídeo</a>        <?php  else:  ?>          <a  class="disabled"> 🎬  Vídeo</a>        <?php  endif;  ?>        <?php  if($pot_meet):  ?>          <a  href="#"  onclick="showPage('meet')"> 📹  Meet</a>        <?php  else:  ?>          <a  class="disabled"  title="El  teu  rol  no  té  accés  a  Meet"> 📹  Meet</a>        <?php  endif;  ?>      <?php  else:  ?>        <a  class="disabled"> 🎵  Àudio</a>        <a  class="disabled"> 📡  Streaming</a>        <a  class="disabled"> 🎬  Vídeo</a>        <a  class="disabled"> 📹  Meet</a>      <?php  endif;  ?>    </div>     <!--  PAGINA  AUDIO  -->  

## Página 51

  <div  id="page-audio"  class="media-page">      <h2> 🎵  Àudio</h2>      <p>Selecciona  una  cançó  per  reproduir-la.  Format  MP3  ·  Streaming  en  temps  real.</p>      <div  class="audio-list">        <div  class="audio-card"  onclick="loadAudio(0,  this)">          <span  class="audio-icon"> 🎵 </span>          <div><h3>See  U  In  Hell</h3><p>Papa  Roach  &  Hanumankind  ·  Devil  May  Cry  
S2</p></div>
       </div>        <div  class="audio-card"  onclick="loadAudio(1,  this)">          <span  class="audio-icon"> 🎵 </span>          <div><h3>Barcelona</h3><p>Pikeras,  KICKBOMBO</p></div>        </div>        <div  class="audio-card"  onclick="loadAudio(2,  this)">          <span  class="audio-icon"> 🎵 </span>          <div><h3>Bando  Boyz  Free  5</h3><p>Kidd  Keo</p></div>        </div>      </div>    </div>     <!--  PAGINA  STREAMING  -->    <div  id="page-streaming"  class="media-page">      <h2> 📡  Streaming  en  Viu  <span  class="live-badge">LIVE</span></h2>      <p>Canal  de  televisió  en  directe  en  bucle  continu.  Còdec  H.264  ·  Format  HLS</p>      <video  id="videoLive"  controls  autoplay  muted></video>      <script>        const  vLive  =  document.getElementById('videoLive');        const  hlsUrl  =  '/stream/hls/stream.m3u8';        if  (Hls.isSupported())  {          const  hls  =  new  Hls();  hls.loadSource(hlsUrl);  hls.attachMedia(vLive);        }  else  if  (vLive.canPlayType('application/vnd.apple.mpegurl'))  {          vLive.src  =  hlsUrl;        }      </script>    </div>     <!--  PAGINA  VIDEO  -->    <div  id="page-video"  class="media-page">      <h2> 🎬  Vídeos</h2>      <p>Selecciona  un  vídeo  per  reproduir-lo.  Còdec  H.264  ·  Format  MP4</p>      <div  class="video-list">        <div  class="video-card  active"  onclick="loadVod('video_real.mp4',  this)"><h3> 🎬  
Final</h3></div>
       <div  class="video-card"  onclick="loadVod('video2.mp4',  this)"><h3> 🎬  MUI</h3></div>        <div  class="video-card"  onclick="loadVod('video3.mp4',  this)"><h3> 🎬  
Caulifla</h3></div>
       <div  class="video-card"  onclick="loadVod('video5.mp4',  this)"><h3> 🎬  
Gogeta</h3></div>
 

## Página 52

    </div>      <video  id="videoVod"  controls></video>      <script>        document.getElementById('videoVod').src  =  '/stream/video/video_real.mp4';        function  loadVod(filename,  card)  {          document.getElementById('videoVod').src  =  '/stream/video/'  +  filename;          document.getElementById('videoVod').play();          document.querySelectorAll('.video-card').forEach(c  =>  c.classList.remove('active'));          card.classList.add('active');        }      </script>    </div>     <!--  PAGINA  MEET  -->    <div  id="page-meet"  class="media-page">      <?php  if($pot_meet):  ?>      <h2> 📹  Videoconferència  —  Jitsi  Meet</h2>      <p>Crea  o  uneix-te  a  una  sala  de  videoconferència.  Protocol  WebRTC.</p>      <div  style="margin-top:30px;  text-align:center;">        <p  style="color:#9ca3af;  margin-bottom:20px;">Fes  clic  per  obrir  Jitsi  Meet  en  una  nova  
finestra.</p>
       <a  href="https://35.153.48.218"  target="_blank"          style="padding:16px  40px;  background:#4f46e5;  color:white;  border-radius:10px;                 text-decoration:none;  font-size:18px;  display:inline-block;">          📹  Iniciar  videoconferència        </a>      </div>      <?php  else:  ?>      <div  class="meet-locked">        <div  class="lock-icon"> 🚫 </div>        <h3>Accés  restringit</h3>        <p>El  teu  rol  <strong><?=  strtoupper(htmlspecialchars($role))  ?></strong>  no  té  
permisos
 
per
 
iniciar
 
videoconferències.</p>
       <p  style="font-size:13px;  margin-top:8px;">Contacta  amb  un  administrador  si  necessites  
accés.</p>
     </div>      <?php  endif;  ?>    </div>     <!--  PAGINA  INICI  -->    <div  id="home-page"  class="home-page">      <section  class="hero">        <div  class="hero-content">          <h1>Solucions  Tecnològiques  Avançades  al  Núvol</h1>          <p>Infraestructura  AWS,  streaming  d'àudio  i  vídeo,  videoconferència  i  gestió  de  dades  
amb
 
seguretat
 
empresarial.</p>
         <div  class="hero-actions">            <a  href="#serveis"  class="btn-primary">Serveis</a>  

## Página 53

          <a  href="#cpd"  class="btn-secondary">Arquitectura</a>          </div>          <div  class="hero-tags">            <span>CPD  
AWS</span><span>Streaming</span><span>Videoconferència</span><span>LDAP/AD</s
pan>
         </div>        </div>        <div  class="hero-visual">          <div  class="card  kpi"><h3>99.95%  uptime</h3><p>Alta  disponibilitat</p></div>          <div  class="card  kpi"><h3>-30%  consum</h3><p>Infraestructura  sostenible</p></div>          <div  class="card  kpi"><h3>+1M</h3><p>Sessions  gestionades</p></div>        </div>      </section>       <section  id="serveis"  class="section">        <h2>Serveis  Principals</h2>        <div  class="cards">          <article  class="card"><h3>Servei  Web</h3><p>Portal  corporatiu  integrat  amb  BBDD  i  
LDAP.</p></article>
         <article  class="card"><h3>SFTP  Segur</h3><p>Accés  restringit  amb  chroot  i  
autenticació
 
AD.</p></article>
         <article  class="card"><h3>Streaming</h3><p>Àudio  i  vídeo  en  temps  real  amb  
adaptació
 
de
 
qualitat.</p></article>
         <article  class="card"><h3>Monitorització</h3><p>Logs  centralitzats  i  
alertes.</p></article>
       </div>      </section>       <section  id="cpd"  class="section  section-alt">        <h2>Arquitectura  del  CPD  AWS</h2>        <p  class="section-intro">Infraestructura  distribuïda  amb  servidors  dedicats  per  Web,  
SFTP,
 
BBDD,
 
LDAP
 
i
 
Streaming.</p>
       <div  class="cpd-grid">          <div  class="cpd-item"><h3>Seguretat</h3><p>Firewalls,  IAM,  backups  i  triggers  
d'auditoria.</p></div>
         <div  class="cpd-item"><h3>Escalabilitat</h3><p>Instàncies  EC2  
optimitzades.</p></div>
         <div  class="cpd-item"><h3>Sostenibilitat</h3><p>Reducció  de  consum  i  petjada  
ecològica.</p></div>
         <div  class="cpd-item"><h3>Integració</h3><p>LDAP/AD  +  BBDD  +  serveis  
multimèdia.</p></div>
       </div>      </section>       <section  id="contacte"  class="section  section-alt">        <h2>Contacte</h2>        <form  class="contact-form">  

## Página 54

        <div  class="form-row">            <input  type="text"  placeholder="Nom"  required>            <input  type="email"  placeholder="Correu"  required>          </div>          <textarea  rows="6"  placeholder="Missatge"></textarea>          <button  class="btn-primary">Enviar</button>        </form>      </section>       <footer  class="main-footer">        <p>©  2026  Innovate  Tech  ·  Projecte  Transversal  ASIXc1</p>      </footer>    </div>     <!--  MODAL  LOGIN  -->    <div  id="login-modal"  class="modal  hidden">      <div  class="modal-content">        <button  id="login-close"  class="modal-close">&times;</button>        <h2>Inicia  sessió</h2>        <p>Accés  per  a  usuaris  del  directori.</p>        <form  method="POST"  action="login.php">          <input  type="text"  name="username"  placeholder="Usuari"  required>          <input  type="password"  name="password"  placeholder="Contrasenya"  required>          <button  class="btn-primary">Entrar</button>        </form>        <?php  if($error_msg):  ?>        <p  style="color:#ef4444;font-size:13px;margin-top:8px;"><?=  
htmlspecialchars($error_msg)
 
?></p>
       <?php  endif;  ?>        <p  class="login-hint">Aquest  formulari  validarà  contra  LDAP/AD  quan  estigui  
configurat.</p>
     </div>    </div>     <audio  id="audioPlayer"  style="display:none;"></audio>     <script  src="js/script.js"></script>    <script>      const  playlist  =  [        {  file:  'See  U  In  Hell.mp3',  title:  'See  U  In  Hell',  artist:  'Papa  Roach  &  Hanumankind'  },        {  file:  'Barcelona.mp3',      title:  'Barcelona',      artist:  'Pikeras,  KICKBOMBO'  },        {  file:  'Bando  Boyz  Free  5.mp3',  title:  'Bando  Boyz  Free  5',  artist:  'Kidd  Keo'  }      ];      let  currentIndex  =  -1;      const  audio  =  document.getElementById('audioPlayer');       audio.addEventListener('timeupdate',  function()  {        if  (audio.duration)  {  

## Página 55

        const  pct  =  (audio.currentTime  /  audio.duration)  *  100;          const  bar  =  document.getElementById('dd-progress-bar');          if  (bar)  bar.style.width  =  pct  +  '%';        }      });      audio.addEventListener('ended',  function()  {  nextSong();  });       function  loadAudio(index,  card)  {        currentIndex  =  index;        const  song  =  playlist[index];        audio.src  =  '/audio/'  +  song.file;        audio.play();        document.getElementById('dd-song-title').textContent  =  ' 🎵  '  +  song.title  +  '  ·  '  +  
song.artist;
       document.getElementById('dd-player').classList.add('visible');        document.getElementById('btn-play').textContent  =  ' ⏸ ';        document.querySelectorAll('.audio-card').forEach(c  =>  c.classList.remove('active'));        if  (card)  card.classList.add('active');      }       function  togglePlay()  {        if  (audio.paused)  {  audio.play();  document.getElementById('btn-play').textContent  =  ' ⏸ ';  
}
       else  {  audio.pause();  document.getElementById('btn-play').textContent  =  ' ▶ ';  }      }       function  prevSong()  {        if  (currentIndex  >  0)  {          const  cards  =  document.querySelectorAll('.audio-card');          loadAudio(currentIndex  -  1,  cards[currentIndex  -  1]  ||  null);        }      }       function  nextSong()  {        if  (currentIndex  <  playlist.length  -  1)  {          const  cards  =  document.querySelectorAll('.audio-card');          loadAudio(currentIndex  +  1,  cards[currentIndex  +  1]  ||  null);        }      }       function  seekAudio(e)  {        const  pct  =  e.offsetX  /  e.currentTarget.offsetWidth;        audio.currentTime  =  pct  *  audio.duration;      }       function  showPage(id)  {        document.querySelectorAll('.media-page').forEach(p  =>  p.classList.remove('active'));        document.getElementById('home-page').classList.add('hidden');  

## Página 56

      document.getElementById('page-'  +  id).classList.add('active');      }       function  showHome()  {        document.querySelectorAll('.media-page').forEach(p  =>  p.classList.remove('active'));        document.getElementById('home-page').classList.remove('hidden');      }       function  toggleDropdown()  {        document.getElementById('user-dropdown').classList.toggle('open');      }       document.addEventListener('click',  function(e)  {        const  menu  =  document.querySelector('.user-menu');        if  (menu  &&  !menu.contains(e.target))  {          const  dd  =  document.getElementById('user-dropdown');          if  (dd)  dd.classList.remove('open');        }      });    </script>  </body>  </html>    sudo  cat  /var/www/innovate_tech/admin/usuaris.php  <?php  session_start();  require_once  __DIR__  .  '/../includes/rols.php';  require_once  __DIR__  .  '/../includes/db.php';  requereix('gestionar_usuaris');   $role      =  $_SESSION['rol']  ??  'usuari';  $pdo       =  get_pdo();  $missatge  =  '';  $error     =  '';  $tab       =  $_GET['tab']  ??  'usuaris';   //  BLOQUEJAR  /  DESBLOQUEJAR  if  (isset($_GET['bloquejar'])  &&  is_numeric($_GET['bloquejar']))  {      try  {          $pdo->prepare("UPDATE  usuaris_comunicacio  SET  estat='bloquejat'  WHERE  
id_usuari=?")->execute([$_GET['bloquejar']]);
         $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  (?,?,?,?)")              ->execute([$_SESSION['user'],  'bloquejar',  $_SERVER['REMOTE_ADDR'],  
'id='.$_GET['bloquejar']]);
         $missatge  =  'Usuari  bloquejat.';      }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }  }  

## Página 57

 if  (isset($_GET['desbloquejar'])  &&  is_numeric($_GET['desbloquejar']))  {      try  {          $pdo->prepare("UPDATE  usuaris_comunicacio  SET  estat='actiu'  WHERE  
id_usuari=?")->execute([$_GET['desbloquejar']]);
         $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  (?,?,?,?)")              ->execute([$_SESSION['user'],  'desbloquejar',  $_SERVER['REMOTE_ADDR'],  
'id='.$_GET['desbloquejar']]);
         $missatge  =  'Usuari  desbloquejat.';      }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }  }   //  CANVIAR  ROL  if  ($_SERVER['REQUEST_METHOD']  ===  'POST'  &&  isset($_POST['canviar_rol']))  {      $id       =  (int)$_POST['id'];      $nou_rol  =  $_POST['rol']  ??  '';      $rols_valids  =  ['usuari','treballador','vendes','administracio','admin'];      if  ($id  &&  in_array($nou_rol,  $rols_valids))  {          try  {              $pdo->prepare("UPDATE  usuaris_comunicacio  SET  rol=?  WHERE  
id_usuari=?")->execute([$nou_rol,
 
$id]);
             $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  
(?,?,?,?)")
                 ->execute([$_SESSION['user'],  'canviar_rol',  $_SERVER['REMOTE_ADDR'],  
"id=$id
 
nou_rol=$nou_rol"]);
             $missatge  =  'Rol  actualitzat.';          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  }   //  ELIMINAR  USUARI  if  (isset($_GET['eliminar'])  &&  is_numeric($_GET['eliminar']))  {      $eid  =  (int)$_GET['eliminar'];      if  ($eid  !==  (int)($_SESSION['user_id']  ??  0))  {          try  {              $pdo->prepare("DELETE  FROM  usuaris_comunicacio  WHERE  
id_usuari=?")->execute([$eid]);
             $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  
(?,?,?,?)")
                 ->execute([$_SESSION['user'],  'eliminar',  $_SERVER['REMOTE_ADDR'],  "id=$eid  
eliminat"]);
             $missatge  =  'Usuari  eliminat.';          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  else  {  $error  =  'No  pots  eliminar  el  teu  propi  usuari.';  }  }   $usuaris  =  $pdo->query("SELECT  *  FROM  usuaris_comunicacio  ORDER  BY  nom_complet  
ASC")->fetchAll();
 

## Página 58

$logs     =  $pdo->query("SELECT  *  FROM  logs_acces  ORDER  BY  data  DESC  LIMIT  
200")->fetchAll();
  $colors_rol  =  [      'admin'          =>  ['bg'=>'rgba(239,68,68,0.15)',    'color'=>'#ef4444'],      'administracio'  =>  ['bg'=>'rgba(251,191,36,0.15)',   'color'=>'#fbbf24'],      'vendes'         =>  ['bg'=>'rgba(34,197,94,0.15)',    'color'=>'#22c55e'],      'treballador'    =>  ['bg'=>'rgba(99,102,241,0.15)',   'color'=>'#6366f1'],      'usuari'         =>  ['bg'=>'rgba(156,163,175,0.15)',  'color'=>'#9ca3af'],  ];  ?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Gestió  Usuaris  ·  Innovate  Tech</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="/css/styles.css">    <style>      body  {  background:#030712;  color:white;  font-family:sans-serif;  }      .page-wrap  {  max-width:1100px;  margin:0  auto;  padding:40px  20px;  }      .page-header  {  display:flex;  align-items:center;  gap:16px;  margin-bottom:24px;  }      .page-header  a  {  color:#6366f1;  text-decoration:none;  font-size:14px;  }      h1  {  font-size:26px;  margin:0;  }      .badge  {  font-size:11px;  padding:2px  10px;  border-radius:10px;  
background:rgba(239,68,68,0.2);
 
color:#ef4444;
 
font-weight:bold;
 
}
     .tabs  {  display:flex;  gap:4px;  margin-bottom:24px;  border-bottom:1px  solid  #1f2937;  }      .tab-btn  {  padding:10px  22px;  background:none;  border:none;  color:#6b7280;  
font-size:14px;
 
cursor:pointer;
 
border-bottom:2px
 
solid
 
transparent;
 
transition:.2s;
 
}
     .tab-btn.active  {  color:#6366f1;  border-bottom-color:#6366f1;  }      .tab-btn:hover  {  color:white;  }      .tab-panel  {  display:none;  }      .tab-panel.active  {  display:block;  }      .card  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:12px;  
overflow:hidden;
 
}
     table  {  width:100%;  border-collapse:collapse;  }      th  {  background:#0b1020;  color:#6366f1;  font-size:11px;  text-transform:uppercase;  
letter-spacing:.05em;
 
padding:12px
 
14px;
 
text-align:left;
 
}
     td  {  padding:11px  14px;  border-top:1px  solid  #1f2937;  color:#d1d5db;  font-size:13px;  
vertical-align:middle;
 
}
     tr:hover  td  {  background:#0b1020;  }      .rol-badge  {  display:inline-block;  padding:2px  9px;  border-radius:8px;  font-size:11px;  
font-weight:bold;
 
}
     .estat-actiu      {  color:#22c55e;  font-weight:bold;  }      .estat-bloquejat  {  color:#ef4444;  font-weight:bold;  }      .btn-sm  {  background:none;  border:1px  solid  #374151;  border-radius:6px;  padding:4px  
10px;
 
cursor:pointer;
 
font-size:12px;
 
transition:.2s;
 
color:#d1d5db;
 
}
     .btn-sm:hover  {  background:#374151;  }  

## Página 59

    .btn-danger  {  color:#ef4444;  }      .btn-danger:hover  {  background:#ef4444;  color:white;  border-color:#ef4444;  }      .btn-warn  {  color:#f97316;  }      .btn-warn:hover  {  background:#f97316;  color:white;  border-color:#f97316;  }      .btn-ok  {  color:#22c55e;  }      .btn-ok:hover  {  background:#22c55e;  color:#030712;  border-color:#22c55e;  }      .rol-form  {  display:flex;  gap:6px;  align-items:center;  }      .rol-form  select  {  padding:4px  8px;  font-size:12px;  background:#0b1020;  border:1px  solid  
#374151;
 
border-radius:6px;
 
color:white;
 
}
     .btn-save-rol  {  background:#4f46e5;  border:none;  color:white;  border-radius:6px;  
padding:4px
 
10px;
 
cursor:pointer;
 
font-size:12px;
 
}
     .btn-save-rol:hover  {  background:#6366f1;  }      .alert-ok   {  background:rgba(34,197,94,0.15);  color:#22c55e;  border:1px  solid  #22c55e;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     .alert-err  {  background:rgba(239,68,68,0.15);  color:#ef4444;  border:1px  solid  #ef4444;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     /*  LOGS  */      .logs-table  th  {  color:#ef4444;  }      td.action-login      {  color:#22c55e;  font-weight:bold;  }      td.action-logout     {  color:#9ca3af;  }      td.action-error      {  color:#fbbf24;  font-weight:bold;  }      td.action-bloquejar  {  color:#f97316;  }      td.action-desbloquejar  {  color:#22c55e;  }      td.action-canviar_rol   {  color:#6366f1;  }      td.action-eliminar   {  color:#ef4444;  font-weight:bold;  }      .log-ip  {  font-family:monospace;  color:#6b7280;  }      .empty  {  text-align:center;  padding:50px;  color:#6b7280;  }      .stats-row  {  display:flex;  gap:14px;  margin-bottom:20px;  flex-wrap:wrap;  }      .stat-box  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:10px;  
padding:16px
 
22px;
 
text-align:center;
 
flex:1;
 
min-width:130px;
 
}
     .stat-val  {  font-size:30px;  font-weight:bold;  color:#6366f1;  }      .stat-val.green  {  color:#22c55e;  }      .stat-val.red    {  color:#ef4444;  }      .stat-label  {  font-size:12px;  color:#9ca3af;  margin-top:3px;  }    </style>  </head>  <body>  <div  class="page-wrap">    <div  class="page-header">      <a  href="/index.php">←  Tornar</a>      <h1> ⚙  Gestió  d'usuaris</h1>      <span  class="badge"><?=  strtoupper(htmlspecialchars($role))  ?></span>    </div>     <?php  if  ($missatge):  ?><div  class="alert-ok"> ✅  <?=  htmlspecialchars($missatge)  
?></div><?php
 
endif;
 
?>
   <?php  if  ($error):     ?><div  class="alert-err"> ❌  <?=  htmlspecialchars($error)  
?></div><?php
 
endif;
 
?>
 

## Página 60

   <?php    $total     =  count($usuaris);    $actius    =  count(array_filter($usuaris,  fn($u)  =>  $u['estat']  ===  'actiu'));    $bloquejats  =  $total  -  $actius;    $logs_count  =  count($logs);    ?>    <div  class="stats-row">      <div  class="stat-box"><div  class="stat-val"><?=  $total  ?></div><div  
class="stat-label">Total
 
usuaris</div></div>
     <div  class="stat-box"><div  class="stat-val  green"><?=  $actius  ?></div><div  
class="stat-label">Actius</div></div>
     <div  class="stat-box"><div  class="stat-val  red"><?=  $bloquejats  ?></div><div  
class="stat-label">Bloquejats</div></div>
     <div  class="stat-box"><div  class="stat-val"><?=  $logs_count  ?></div><div  
class="stat-label">Entrades
 
al
 
log</div></div>
   </div>     <div  class="tabs">      <button  class="tab-btn  <?=  $tab==='usuaris'  ?  'active':''  ?>"  
onclick="showTab('usuaris')">
👤
 
Usuaris
 
(<?=
 
$total
 
?>)</button>
     <button  class="tab-btn  <?=  $tab==='logs'  ?  'active':''  ?>"  onclick="showTab('logs')"> 🪵  
Logs
 
sistema
 
(<?=
 
$logs_count
 
?>)</button>
   </div>     <!--  TAB  USUARIS  -->    <div  id="tab-usuaris"  class="tab-panel  <?=  $tab==='usuaris'  ?  'active':''  ?>">      <div  class="card">        <table>          <thead>            <tr><th>Nom</th><th>Email</th><th>Rol</th><th>Estat</th><th>Accions</th></tr>          </thead>          <tbody>            <?php  foreach  ($usuaris  as  $u):              $rc  =  $colors_rol[$u['rol']]  ??  $colors_rol['usuari'];            ?>            <tr>              <td><?=  htmlspecialchars($u['nom_complet'])  ?></td>              <td><?=  htmlspecialchars($u['email'])  ?></td>              <td>                <form  method="POST"  class="rol-form">                  <input  type="hidden"  name="canviar_rol"  value="1">                  <input  type="hidden"  name="id"  value="<?=  $u['id_usuari']  ?>">                  <select  name="rol">                    <?php  foreach  (['usuari','treballador','vendes','administracio','admin']  as  $r):  ?>                    <option  value="<?=  $r  ?>"  <?=  $u['rol']===$r  ?  'selected':''  ?>><?=  $r  ?></option>                    <?php  endforeach;  ?>                  </select>  

## Página 61

                <button  type="submit"  class="btn-save-rol"> ✓ </button>                </form>              </td>              <td  class="estat-<?=  htmlspecialchars($u['estat'])  ?>"><?=  
htmlspecialchars($u['estat'])
 
?></td>
             <td  style="white-space:nowrap;  display:flex;  gap:6px;">                <?php  if  ($u['estat']  ===  'actiu'):  ?>                  <a  href="?bloquejar=<?=  $u['id_usuari']  ?>&tab=usuaris"  class="btn-sm  btn-warn"  
onclick="return
 
confirm('Bloquejar
 
aquest
 
usuari?')">
🔒
 
Bloquejar</a>
               <?php  else:  ?>                  <a  href="?desbloquejar=<?=  $u['id_usuari']  ?>&tab=usuaris"  class="btn-sm  
btn-ok">
🔓
 
Desbloquejar</a>
               <?php  endif;  ?>                <?php  if  ($u['id_usuari']  !=  ($_SESSION['user_id']  ??  0)):  ?>                <a  href="?eliminar=<?=  $u['id_usuari']  ?>&tab=usuaris"  class="btn-sm  btn-danger"  
onclick="return
 
confirm('Eliminar
 
usuari
 
<?=
 
htmlspecialchars($u['nom_complet'])
 
?>?')">
🗑
</a>
               <?php  endif;  ?>              </td>            </tr>            <?php  endforeach;  ?>          </tbody>        </table>      </div>    </div>     <!--  TAB  LOGS  -->    <div  id="tab-logs"  class="tab-panel  <?=  $tab==='logs'  ?  'active':''  ?>">      <div  class="card  logs-table">        <?php  if  (!empty($logs)):  ?>        <table>          <thead>            <tr><th>Data</th><th>Usuari</th><th>Acció</th><th>IP</th><th>Detall</th></tr>          </thead>          <tbody>            <?php  foreach  ($logs  as  $l):  ?>            <tr>              <td><?=  htmlspecialchars($l['data'])  ?></td>              <td><?=  htmlspecialchars($l['usuari'])  ?></td>              <td  class="action-<?=  htmlspecialchars($l['accio'])  ?>"><?=  
htmlspecialchars($l['accio'])
 
?></td>
             <td  class="log-ip"><?=  htmlspecialchars($l['ip'])  ?></td>              <td><?=  htmlspecialchars($l['detall'])  ?></td>            </tr>            <?php  endforeach;  ?>          </tbody>        </table>        <?php  else:  ?>  

## Página 62

      <div  class="empty"> 🪵  No  hi  ha  logs  encara.  Els  logins  i  accions  d'administració  
apareixeran
 
aquí.</div>
       <?php  endif;  ?>      </div>    </div>  </div>   <script>  function  showTab(name)  {    document.querySelectorAll('.tab-panel').forEach(p  =>  p.classList.remove('active'));    document.querySelectorAll('.tab-btn').forEach(b  =>  b.classList.remove('active'));    document.getElementById('tab-'  +  name).classList.add('active');    event.target.classList.add('active');    history.replaceState(null,'','?tab='+name);  }  //  Activar  tab  correcte  si  ve  per  URL  document.addEventListener('DOMContentLoaded',  ()  =>  {    const  tab  =  new  URLSearchParams(window.location.search).get('tab')  ||  'usuaris';    const  panel  =  document.getElementById('tab-'  +  tab);    if  (panel)  {      document.querySelectorAll('.tab-panel').forEach(p  =>  p.classList.remove('active'));      document.querySelectorAll('.tab-btn').forEach(b  =>  b.classList.remove('active'));      panel.classList.add('active');      document.querySelectorAll('.tab-btn').forEach(b  =>  {        if  (b.getAttribute('onclick')?.includes(tab))  b.classList.add('active');      });    }  });  </script>  </body>  </html>    sudo  cat  /var/www/innovate_tech/admin/logs.php  <?php  header('Location:  /admin/usuaris.php?tab=logs');  exit;   sudo  cat  /var/www/innovate_tech/admin/estadistiques.php  <?php  header('Location:  /index.php');  exit;   sudo  cat  /var/www/innovate_tech/vendes/contactes.php  <?php  session_start();  require_once  __DIR__  .  '/../includes/rols.php';  require_once  __DIR__  .  '/../includes/db.php';  requereix('gestionar_contactes');   $role      =  $_SESSION['rol']  ??  'usuari';  $missatge  =  '';  

## Página 63

$error     =  '';  $pdo       =  get_pdo();   //  ELIMINAR  if  (isset($_GET['eliminar'])  &&  is_numeric($_GET['eliminar']))  {      try  {          $pdo->prepare("DELETE  FROM  contactes_clients  WHERE  id  =  
?")->execute([$_GET['eliminar']]);
         $missatge  =  'Contacte  eliminat.';      }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }  }   //  EDITAR  if  ($_SERVER['REQUEST_METHOD']  ===  'POST'  &&  isset($_POST['editar']))  {      $id       =  (int)($_POST['id']  ??  0);      $nom      =  trim($_POST['nom']  ??  '');      $empresa  =  trim($_POST['empresa']  ??  '');      $email    =  trim($_POST['email']  ??  '');      $telefon  =  trim($_POST['telefon']  ??  '');      if  ($id  &&  $nom)  {          try  {              $pdo->prepare("UPDATE  contactes_clients  SET  nom=?,  empresa=?,  email=?,  
telefon=?
 
WHERE
 
id=?")
                 ->execute([$nom,  $empresa,  $email,  $telefon,  $id]);              $missatge  =  'Contacte  actualitzat.';          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  }   //  NOU  CONTACTE  if  ($_SERVER['REQUEST_METHOD']  ===  'POST'  &&  isset($_POST['nou']))  {      $nom      =  trim($_POST['nom']  ??  '');      $empresa  =  trim($_POST['empresa']  ??  '');      $email    =  trim($_POST['email']  ??  '');      $telefon  =  trim($_POST['telefon']  ??  '');      if  ($nom)  {          try  {              $pdo->prepare("INSERT  INTO  contactes_clients  (nom,  empresa,  email,  telefon)  
VALUES
 
(?,?,?,?)")
                 ->execute([$nom,  $empresa,  $email,  $telefon]);              $missatge  =  "Contacte  \"$nom\"  afegit  correctament.";          }  catch  (Exception  $e)  {  $error  =  $e->getMessage();  }      }  else  {  $error  =  'El  nom  és  obligatori.';  }  }   $contactes   =  $pdo->query("SELECT  *  FROM  contactes_clients  ORDER  BY  nom  
ASC")->fetchAll();
 $editar_id   =  isset($_GET['editar'])  ?  (int)$_GET['editar']  :  0;  

## Página 64

?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Contactes  ·  Innovate  Tech</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="/css/styles.css">    <style>      body  {  background:#030712;  color:white;  font-family:sans-serif;  }      .page-wrap  {  max-width:1000px;  margin:0  auto;  padding:40px  20px;  }      .page-header  {  display:flex;  align-items:center;  gap:16px;  margin-bottom:32px;  }      .page-header  a  {  color:#6366f1;  text-decoration:none;  font-size:14px;  }      h1  {  font-size:26px;  margin:0;  }      .badge  {  font-size:11px;  padding:2px  10px;  border-radius:10px;  
background:rgba(34,197,94,0.2);
 
color:#22c55e;
 
font-weight:bold;
 
}
     .layout  {  display:grid;  grid-template-columns:1fr  280px;  gap:24px;  }      @media(max-width:700px){  .layout  {  grid-template-columns:1fr;  }  }      .card  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:12px;  padding:24px;  
}
     .card  h2  {  font-size:15px;  margin:0  0  16px  0;  color:#9ca3af;  }      label  {  display:block;  font-size:13px;  color:#9ca3af;  margin-bottom:5px;  margin-top:14px;  }      input[type=text],  input[type=email]  {  width:100%;  padding:10px  12px;  
background:#0b1020;
 
border:1px
 
solid
 
#374151;
 
border-radius:8px;
 
color:white;
 
font-size:14px;
 
box-sizing:border-box;
 
}
     input:focus  {  outline:none;  border-color:#4f46e5;  }      .btn-submit  {  margin-top:16px;  width:100%;  padding:11px;  background:#22c55e;  
border:none;
 
border-radius:8px;
 
color:#030712;
 
font-weight:bold;
 
font-size:14px;
 
cursor:pointer;
 
transition:.2s;
 
}
     .btn-submit:hover  {  background:#16a34a;  }      .btn-edit-submit  {  margin-top:16px;  width:100%;  padding:11px;  background:#4f46e5;  
border:none;
 
border-radius:8px;
 
color:white;
 
font-size:14px;
 
cursor:pointer;
 
transition:.2s;
 
}
     .btn-edit-submit:hover  {  background:#6366f1;  }      .alert-ok   {  background:rgba(34,197,94,0.15);  color:#22c55e;  border:1px  solid  #22c55e;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     .alert-err  {  background:rgba(239,68,68,0.15);  color:#ef4444;  border:1px  solid  #ef4444;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     table  {  width:100%;  border-collapse:collapse;  }      th  {  color:#22c55e;  font-size:11px;  text-transform:uppercase;  letter-spacing:.05em;  
padding:10px
 
12px;
 
text-align:left;
 
border-bottom:1px
 
solid
 
#1f2937;
 
background:#0b1020;
 
}
     td  {  padding:10px  12px;  border-top:1px  solid  #1f2937;  color:#d1d5db;  font-size:13px;  
vertical-align:middle;
 
}
     tr:hover  td  {  background:#0b1020;  }      .avatar  {  width:28px;  height:28px;  border-radius:50%;  background:#22c55e;  
color:#030712;
 
font-weight:bold;
 
font-size:13px;
 
display:inline-flex;
 
align-items:center;
 
justify-content:center;
 
}
     .btn-del   {  background:none;  border:1px  solid  #374151;  color:#ef4444;  border-radius:6px;  
padding:4px
 
10px;
 
cursor:pointer;
 
font-size:12px;
 
transition:.2s;
 
}
 

## Página 65

    .btn-del:hover   {  background:#ef4444;  color:white;  border-color:#ef4444;  }      .btn-edit  {  background:none;  border:1px  solid  #374151;  color:#6366f1;  border-radius:6px;  
padding:4px
 
10px;
 
cursor:pointer;
 
font-size:12px;
 
text-decoration:none;
 
transition:.2s;
 
display:inline-block;
 
}
     .btn-edit:hover  {  background:#6366f1;  color:white;  border-color:#6366f1;  }      .empty  {  text-align:center;  padding:40px;  color:#6b7280;  font-size:14px;  }      .edit-highlight  td  {  background:#0f172a  !important;  border-color:#4f46e5;  }      .btn-cancel  {  display:block;  margin-top:8px;  text-align:center;  color:#6b7280;  
font-size:13px;
 
text-decoration:none;
 
}
     .btn-cancel:hover  {  color:white;  }    </style>  </head>  <body>  <div  class="page-wrap">    <div  class="page-header">      <a  href="/index.php">←  Tornar</a>      <h1> 👥  Contactes  clients</h1>      <span  class="badge"><?=  strtoupper(htmlspecialchars($role))  ?></span>    </div>     <?php  if  ($missatge):  ?><div  class="alert-ok"> ✅  <?=  htmlspecialchars($missatge)  
?></div><?php
 
endif;
 
?>
   <?php  if  ($error):     ?><div  class="alert-err"> ❌  <?=  htmlspecialchars($error)  
?></div><?php
 
endif;
 
?>
    <div  class="layout">      <div  class="card">        <h2>Directori  (<?=  count($contactes)  ?>)</h2>        <?php  if  (!empty($contactes)):  ?>        <table>          
<thead><tr><th></th><th>Nom</th><th>Empresa</th><th>Email</th><th>Telèfon</th><th>
</th></tr></thead>
         <tbody>            <?php  foreach  ($contactes  as  $c):  ?>            <tr  <?=  $c['id']==$editar_id  ?  'class="edit-highlight"'  :  ''  ?>>              <td><div  class="avatar"><?=  strtoupper(substr($c['nom'],0,1))  ?></div></td>              <td><?=  htmlspecialchars($c['nom'])  ?></td>              <td><?=  htmlspecialchars($c['empresa']  ??  '—')  ?></td>              <td><?=  htmlspecialchars($c['email']  ??  '—')  ?></td>              <td><?=  htmlspecialchars($c['telefon']  ??  '—')  ?></td>              <td  style="white-space:nowrap;">                <a  href="?editar=<?=  $c['id']  ?>"  class="btn-edit"> ✏ </a>                <a  href="?eliminar=<?=  $c['id']  ?>"  class="btn-del"  onclick="return  confirm('Eliminar  
<?=
 
htmlspecialchars($c['nom'])
 
?>?')">
🗑
</a>
             </td>            </tr>            <?php  endforeach;  ?>  

## Página 66

        </tbody>        </table>        <?php  else:  ?><div  class="empty">No  hi  ha  contactes  encara.</div><?php  endif;  ?>      </div>       <div  class="card">        <?php        //  Si  estem  editant,  mostrem  formulari  d'edició        $edit_contact  =  null;        if  ($editar_id)  {            foreach  ($contactes  as  $c)  {  if  ($c['id']  ==  $editar_id)  {  $edit_contact  =  $c;  break;  }  }        }        if  ($edit_contact):        ?>        <h2> ✏  Editar  contacte</h2>        <form  method="POST">          <input  type="hidden"  name="editar"  value="1">          <input  type="hidden"  name="id"  value="<?=  $edit_contact['id']  ?>">          <label>Nom  *</label>          <input  type="text"  name="nom"  value="<?=  htmlspecialchars($edit_contact['nom'])  ?>"  
required>
         <label>Empresa</label>          <input  type="text"  name="empresa"  value="<?=  
htmlspecialchars($edit_contact['empresa']
 
??
 
'')
 
?>">
         <label>Email</label>          <input  type="email"  name="email"  value="<?=  htmlspecialchars($edit_contact['email']  
??
 
'')
 
?>">
         <label>Telèfon</label>          <input  type="text"  name="telefon"  value="<?=  htmlspecialchars($edit_contact['telefon']  
??
 
'')
 
?>">
         <button  type="submit"  class="btn-edit-submit"> 💾  Guardar  canvis</button>        </form>        <a  href="contactes.php"  class="btn-cancel">←  Cancel·lar  edició</a>         <?php  else:  ?>        <h2>Nou  contacte</h2>        <form  method="POST">          <input  type="hidden"  name="nou"  value="1">          <label>Nom  *</label>          <input  type="text"  name="nom"  placeholder="Nom  complet"  required>          <label>Empresa</label>          <input  type="text"  name="empresa"  placeholder="Nom  de  l'empresa">          <label>Email</label>          <input  type="email"  name="email"  placeholder="correu@empresa.com">          <label>Telèfon</label>          <input  type="text"  name="telefon"  placeholder="+34  600  000  000">          <button  type="submit"  class="btn-submit"> ➕  Afegir  contacte</button>        </form>  

## Página 67

      <?php  endif;  ?>      </div>    </div>  </div>  </body>  </html>   sudo  cat  /var/www/innovate_tech/treballador/activitat.php  <?php  session_start();  require_once  __DIR__  .  '/../includes/rols.php';  require_once  __DIR__  .  '/../includes/db.php';  requereix('registrar_activitat');   $role     =  $_SESSION['rol']  ??  'usuari';  $user_id  =  $_SESSION['user_id']  ??  0;  $missatge  =  '';  $error     =  '';   if  ($_SERVER['REQUEST_METHOD']  ===  'POST')  {      $tipus       =  trim($_POST['tipus']  ??  '');      $descripcio  =  trim($_POST['descripcio']  ??  '');      $durada      =  trim($_POST['durada']  ??  '');       if  ($tipus  &&  $descripcio)  {          try  {              $pdo   =  get_pdo();              $stmt  =  $pdo->prepare("INSERT  INTO  historial_activitat  (id_usuari,  tipus,  descripcio,  
durada)
 
VALUES
 
(?,
 
?,
 
?,
 
?)");
             $stmt->execute([$user_id,  $tipus,  $descripcio,  $durada]);              $missatge  =  'Activitat  registrada  correctament.';          }  catch  (Exception  $e)  {              $error  =  'Error  en  guardar:  '  .  $e->getMessage();          }      }  else  {          $error  =  'Omple  els  camps  obligatoris.';      }  }  ?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Registrar  Activitat  ·  Innovate  Tech</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="/css/styles.css">    <style>      body  {  background:#030712;  color:white;  font-family:sans-serif;  }  

## Página 68

    .page-wrap  {  max-width:600px;  margin:0  auto;  padding:40px  20px;  }      .page-header  {  display:flex;  align-items:center;  gap:16px;  margin-bottom:32px;  }      .page-header  a  {  color:#6366f1;  text-decoration:none;  font-size:14px;  }      h1  {  font-size:26px;  margin:0;  }      .badge  {  font-size:11px;  padding:2px  10px;  border-radius:10px;  
background:rgba(99,102,241,0.2);
 
color:#6366f1;
 
font-weight:bold;
 
}
     .form-card  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:12px;  
padding:28px;
 
}
     label  {  display:block;  font-size:13px;  color:#9ca3af;  margin-bottom:6px;  margin-top:18px;  }      select,  input[type=text],  textarea  {  width:100%;  padding:10px  14px;  background:#0b1020;  
border:1px
 
solid
 
#374151;
 
border-radius:8px;
 
color:white;
 
font-size:14px;
 
box-sizing:border-box;
 
}
     select:focus,  input:focus,  textarea:focus  {  outline:none;  border-color:#4f46e5;  }      textarea  {  resize:vertical;  min-height:100px;  }      .btn-submit  {  margin-top:22px;  width:100%;  padding:12px;  background:#4f46e5;  
border:none;
 
border-radius:8px;
 
color:white;
 
font-size:15px;
 
cursor:pointer;
 
transition:.2s;
 
}
     .btn-submit:hover  {  background:#6366f1;  }      .btn-hist  {  display:inline-block;  margin-top:14px;  width:100%;  padding:11px;  
background:transparent;
 
border:1px
 
solid
 
#374151;
 
border-radius:8px;
 
color:#9ca3af;
 
font-size:14px;
 
cursor:pointer;
 
text-align:center;
 
text-decoration:none;
 
transition:.2s;
 
box-sizing:border-box;
 
}
     .btn-hist:hover  {  border-color:#6366f1;  color:white;  }      .alert-ok   {  background:rgba(34,197,94,0.15);  color:#22c55e;  border:1px  solid  #22c55e;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
     .alert-err  {  background:rgba(239,68,68,0.15);  color:#ef4444;  border:1px  solid  #ef4444;  
border-radius:8px;
 
padding:10px
 
14px;
 
margin-bottom:20px;
 
font-size:14px;
 
}
   </style>  </head>  <body>  <div  class="page-wrap">    <div  class="page-header">      <a  href="/index.php">←  Tornar</a>      <h1> ✏  Registrar  activitat</h1>      <span  class="badge"><?=  strtoupper(htmlspecialchars($role))  ?></span>    </div>     <?php  if  ($missatge):  ?><div  class="alert-ok"> ✅  <?=  htmlspecialchars($missatge)  
?></div><?php
 
endif;
 
?>
   <?php  if  ($error):     ?><div  class="alert-err"> ❌  <?=  htmlspecialchars($error)  
?></div><?php
 
endif;
 
?>
    <div  class="form-card">      <form  method="POST">        <label>Tipus  d'activitat  *</label>        <select  name="tipus"  required>          <option  value="">—  Selecciona  —</option>          <option  value="trucada"> 📞  Trucada</option>          <option  value="reunio"> 🤝  Reunió</option>  

## Página 69

        <option  value="tasca"> 📝  Tasca</option>        </select>         <label>Descripció  *</label>        <textarea  name="descripcio"  placeholder="Descriu  breument  l'activitat  realitzada..."  
required></textarea>
        <label>Durada  (opcional)</label>        <input  type="text"  name="durada"  placeholder="Ex:  30  min,  1h  15min">         <button  type="submit"  class="btn-submit"> 💾  Guardar  activitat</button>      </form>      <a  href="/treballador/historial.php"  class="btn-hist"> 📋  Veure  el  meu  historial</a>    </div>  </div>  </body>  </html>   sudo  cat  /var/www/innovate_tech/treballador/historial.php  <?php  session_start();  require_once  __DIR__  .  '/../includes/rols.php';  require_once  __DIR__  .  '/../includes/db.php';  requereix('historial_trucades');   $role      =  $_SESSION['rol']  ??  'usuari';  $user_id   =  $_SESSION['user_id']  ??  0;  $historial  =  [];   try  {      $pdo   =  get_pdo();      $stmt  =  $pdo->prepare("SELECT  *  FROM  historial_activitat  WHERE  id_usuari  =  ?  
ORDER
 
BY
 
data
 
DESC
 
LIMIT
 
100");
     $stmt->execute([$user_id]);      $historial  =  $stmt->fetchAll();  }  catch  (Exception  $e)  {}  ?>  <!DOCTYPE  html>  <html  lang="ca">  <head>    <meta  charset="UTF-8">    <title>Historial  ·  Innovate  Tech</title>    <meta  name="viewport"  content="width=device-width,  initial-scale=1.0">    <link  rel="stylesheet"  href="/css/styles.css">    <style>      body  {  background:#030712;  color:white;  font-family:sans-serif;  }      .page-wrap  {  max-width:900px;  margin:0  auto;  padding:40px  20px;  }      .page-header  {  display:flex;  align-items:center;  gap:16px;  margin-bottom:32px;  }  

## Página 70

    .page-header  a  {  color:#6366f1;  text-decoration:none;  font-size:14px;  }      .page-header  a:hover  {  text-decoration:underline;  }      h1  {  font-size:26px;  margin:0;  }      .badge  {  font-size:11px;  padding:2px  10px;  border-radius:10px;  
background:rgba(99,102,241,0.2);
 
color:#6366f1;
 
font-weight:bold;
 
}
     .card-table  {  background:#111827;  border:1px  solid  #1f2937;  border-radius:12px;  
overflow:hidden;
 
}
     table  {  width:100%;  border-collapse:collapse;  }      th  {  background:#0b1020;  color:#6366f1;  font-size:12px;  text-transform:uppercase;  
letter-spacing:.05em;
 
padding:12px
 
16px;
 
text-align:left;
 
}
     td  {  padding:12px  16px;  border-top:1px  solid  #1f2937;  color:#d1d5db;  font-size:14px;  }      tr:hover  td  {  background:#0b1020;  }      .tag  {  display:inline-block;  padding:2px  8px;  border-radius:6px;  font-size:12px;  }      .tag-trucada  {  background:rgba(34,197,94,0.15);  color:#22c55e;  }      .tag-reunio   {  background:rgba(99,102,241,0.15);  color:#6366f1;  }      .tag-tasca    {  background:rgba(251,191,36,0.15);  color:#fbbf24;  }      .empty  {  text-align:center;  padding:60px  20px;  color:#6b7280;  }      .empty  .icon  {  font-size:40px;  margin-bottom:12px;  }      .btn-new  {  display:inline-block;  margin-bottom:20px;  padding:10px  22px;  
background:#4f46e5;
 
color:white;
 
border-radius:8px;
 
text-decoration:none;
 
font-size:14px;
 
transition:.2s;
 
}
     .btn-new:hover  {  background:#6366f1;  }    </style>  </head>  <body>  <div  class="page-wrap">    <div  class="page-header">      <a  href="/index.php">←  Tornar</a>      <h1> 📋  El  meu  historial</h1>      <span  class="badge"><?=  strtoupper(htmlspecialchars($role))  ?></span>    </div>     <a  href="/treballador/activitat.php"  class="btn-new"> ✏  Registrar  nova  activitat</a>     <?php  if  (!empty($historial)):  ?>    <div  class="card-table">      <table>        <thead>          <tr><th>Data</th><th>Tipus</th><th>Descripció</th><th>Durada</th></tr>        </thead>        <tbody>          <?php  foreach  ($historial  as  $h):  ?>          <tr>            <td><?=  htmlspecialchars($h['data'])  ?></td>            <td><span  class="tag  tag-<?=  htmlspecialchars($h['tipus'])  ?>"><?=  
htmlspecialchars($h['tipus'])
 
?></span></td>
           <td><?=  htmlspecialchars($h['descripcio'])  ?></td>            <td><?=  htmlspecialchars($h['durada']  ??  '—')  ?></td>  

## Página 71

        </tr>          <?php  endforeach;  ?>        </tbody>      </table>    </div>    <?php  else:  ?>    <div  class="empty">      <div  class="icon"> 📭 </div>      <p>No  hi  ha  activitats  registrades  encara.</p>      <p  style="font-size:13px;color:#4b5563;margin-top:8px;">Registra  la  teva  primera  activitat  
amb
 
el
 
botó
 
de
 
dalt.</p>
   </div>    <?php  endif;  ?>  </div>  </body>  </html>   sudo  cat  /var/www/innovate_tech/logout.php  <?php  session_start();  require_once  __DIR__  .  '/includes/db.php';   //  Log  logout  real  if  (!empty($_SESSION['user']))  {      try  {          $pdo  =  get_pdo();          $ip   =  $_SERVER['REMOTE_ADDR']  ??  '—';          $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  (?,  
'logout',
 
?,
 
'Sessió
 
tancada')")
             ->execute([$_SESSION['user'],  $ip]);      }  catch  (Exception  $e)  {}  }   session_destroy();  header('Location:  /index.php');  exit;   sudo  cat  /var/www/innovate_tech/login.php  <?php  session_start();  require_once  __DIR__  .  '/includes/db.php';  require_once  __DIR__  .  '/includes/rols.php';   if  ($_SERVER['REQUEST_METHOD']  !==  'POST')  {      header('Location:  /index.php');  exit;  }   $username  =  trim($_POST['username']  ??  '');  $password  =  $_POST['password']  ??  '';  

## Página 72

 if  ($username  ===  ''  ||  $password  ===  '')  {      header('Location:  /index.php?login_error=camps_buits');  exit;  }   $pdo   =  get_pdo();  $stmt  =  $pdo->prepare("      SELECT  id_usuari,  nom_complet,  email,  rol,  estat,  password_hash      FROM    usuaris_comunicacio      WHERE   email  =  ?  OR  nom_complet  =  ?      LIMIT   1  ");  $stmt->execute([$username,  $username]);  $user  =  $stmt->fetch();   if  (!$user  ||  !password_verify($password,  $user['password_hash']))  {      //  Log  intent  fallit      try  {          $ip  =  $_SERVER['REMOTE_ADDR']  ??  '—';          $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  (?,  'error',  
?,
 
'Credencials
 
incorrectes')")
             ->execute([$username,  $ip]);      }  catch  (Exception  $e)  {}      header('Location:  /index.php?login_error=credencials');  exit;  }   if  ($user['estat']  ===  'bloquejat')  {      header('Location:  /index.php?login_error=bloquejat');  exit;  }   session_regenerate_id(true);  $_SESSION['user']     =  $user['nom_complet'];  $_SESSION['name']     =  $user['nom_complet'];  $_SESSION['email']    =  $user['email'];  $_SESSION['rol']      =  $user['rol'];  $_SESSION['role']     =  $user['rol'];  $_SESSION['user_id']  =  $user['id_usuari'];   //  Log  login  real  try  {      $ip  =  $_SERVER['REMOTE_ADDR']  ??  '—';      $pdo->prepare("INSERT  INTO  logs_acces  (usuari,  accio,  ip,  detall)  VALUES  (?,  'login',  ?,  
'Login
 
correcte')")
         ->execute([$user['nom_complet'],  $ip]);  }  catch  (Exception  $e)  {}   header('Location:  /index.php');  exit;   

## Página 73

  
Vinculació  entre  LDAP  i  rsyslog  
1.  Configurarem  el  nivel  de  logs  de  LDAP  a  estadístiques  (nivell  256),  que  registra  
connexions,
 
operacions
 
i
 
resultats.
 
Aquest
 
és
 
un
 
bon
 
equilibri
 
per
 
a
 
un
 
servidor
 
de
 
producció.
 
 2.  Crear  una  regla  de  reenviament  rsyslog  dedicada.  Crearem  un  fitxer  de  configuració  
nou
 
per
 
reenviar
 
els
 
registres
 
local4.*
 
al
 
servidor
 
remot
 
mitjançant
 
TCP.
 
 3.  Reiniciar  rsyslog  per  aplicar  els  canvis.  
  Els  nostres  registres  LDAP  ara  començaran  a  fluir  al  nostre  servidor  central  rsyslog  al  port  
514
 
a
 
través
 
de
 
TCP.
          
   


