# Implantacio_Serveis_Audio_Video_InnovateTech

## Página 1

2.1  —  Descripció  general
 

## Página 2

35.174.72.42.1  —  Descripció  General   
En  el  marc  del  projecte  transversal,  el  nostre  grup  ha  desplegat  una  infraestructura  
multimèdia
 
completa
 
dins
 
del
 
CPD
 
d’Innovate
 
Tech
 
utilitzant
 
servidors
 
independents
 
allotjats
 
a
 
AWS.
 
L’objectiu
 
principal
 
és
 
proporcionar
 
serveis
 
d’àudio,
 
vídeo
 
i
 
videoconferència
 
que
 
donin
 
suport
 
a
 
la
 
comunicació
 
interna,
 
la
 
formació
 
corporativa
 
i
 
la
 
distribució
 
de
 
continguts
 
als
 
clients
 
de
 
manera
 
fiable
 
i
 
accessible.
 
Per  aconseguir-ho,  hem  dissenyat  una  arquitectura  basada  en  tres  serveis  principals:  
Servei  d’Àudio  en  Streaming  
Hem  implementat  un  servidor  d’àudio  amb  Icecast2,  que  permet  emetre  contingut  en  directe  
i
 
sota
 
demanda
 
utilitzant
 
formats
 
estàndard
 
com
 
MP3
 
o
 
OGG.
 
Aquest
 
servei
 
és
 
accessible
 
des
 
de
 
qualsevol
 
navegador
 
web
 
i
 
està
 
pensat
 
per
 
a
 
comunicacions
 
internes,
 
anuncis
 
corporatius
 
i
 
contingut
 
formatiu.
 
La
 
configuració
 
garanteix
 
compatibilitat,
 
estabilitat
 
i
 
un
 
consum
 
moderat
 
d’amplada
 
de
 
banda.
 
Servei  de  Vídeo  en  Streaming  
Per  a  la  distribució  de  vídeo,  hem  desplegat  un  servidor  NGINX  amb  el  mòdul  RTMP,  que  
permet
 
l’emissió
 
en
 
temps
 
real
 
i
 
la
 
reproducció
 
via
 
HLS.
 
Aquesta
 
solució
 
és
 
compatible
 
amb
 
eines
 
com
 
OBS
 
Studio
 
per
 
a
 
l’emissió
 
i
 
amb
 
qualsevol
 
navegador
 
modern
 
per
 
a
 
la
 
visualització.
 
El
 
servei
 
està
 
orientat
 
a
 
formacions
 
internes,
 
demostracions
 
tècniques
 
i
 
contingut
 
audiovisual
 
corporatiu.
 
Servei  de  Videoconferència  
Per  cobrir  les  necessitats  de  reunions  internes  i  comunicació  remota,  hem  instal·lat  Jitsi  
Meet
 
en
 
un
 
servidor
 
dedicat.
 
Aquesta
 
plataforma
 
permet
 
realitzar
 
videotrucades
 
sense
 
necessitat
 
d’instal·lar
 
programari
 
addicional,
 
ja
 
que
 
funciona
 
directament
 
des
 
del
 
navegador.
 
A
 
més,
 
facilita
 
la
 
participació
 
de
 
múltiples
 
usuaris
 
i
 
s’integra
 
amb
 
la
 
resta
 
de
 
serveis
 
del
 
CPD.
 
Integració  amb  la  Infraestructura  Global  
Tots  els  serveis  multimèdia  estan  integrats  amb  la  resta  de  la  infraestructura  desplegada  al  
CPD:
 
el
 
servidor
 
web,
 
el
 
servei
 
SFTP,
 
el
 
servidor
 
de
 
logs
 
i
 
el
 
directori
 
actiu
 
LDAP.
 
Aquesta
 
integració
 
garanteix
 
una
 
gestió
 
centralitzada
 
d’usuaris,
 
un
 
control
 
d’accés
 
segur
 
i
 
una
 
experiència
 
unificada
 
per
 
als
 
treballadors
 
i
 
clients.
 
 
 

## Página 3

Objectiu  Final  
Amb  aquesta  arquitectura,  Innovate  Tech  disposa  d’un  sistema  multimèdia  complet,  
escalable
 
i
 
preparat
 
per
 
donar
 
suport
 
a
 
les
 
necessitats
 
actuals
 
de
 
comunicació
 
i
 
continguts
 
digitals
 
de
 
l’empresa,
 
assegurant
 
una
 
experiència
 
fluida
 
i
 
professional.
 
 

## Página 4

2.2  —  Servei  d’ÀUDIO  

## Página 5

2.2.1  Objectius  
L’objectiu  principal  és  desplegar  un  sistema  de  distribució  d’àudio  capaç  d’oferir  streaming  
en
 
directe
 
i
 
sota
 
demanda
 
dins
 
de
 
la
 
infraestructura
 
del
 
CPD
 
d’InnovateTech.
 
El
 
servei
 
ha
 
de
 
permetre
 
la
 
reproducció
 
de
 
continguts
 
d’àudio
 
des
 
de
 
diferents
 
tipus
 
de
 
clients,
 
garantir
 
l’accés
 
via
 
navegador
 
web
 
i
 
integrar-se
 
amb
 
l’entorn
 
corporatiu.
 
A  més,  el  sistema  ha  de  ser  escalable,  estable  i  compatible  amb  formats  d’àudio  digitals  
estàndard.
 
2.2.2  Requeriments  
  Es  crea  una  nova  instància  EC2  a  AWS  amb  el  nom  "SRV-AUDIO-VÍDEO".  S'utilitza  una  
AMI
 
d'Ubuntu
 
Server
 
24.04
 
LTS
 
(HVM)
 
de
 
64
 
bits,
 
seleccionada
 
des
 
de
 
la
 
pestanya
 
Quick
 
Start
 
de
 
la
 
consola
 
d'AWS.
 
Aquesta
 
imatge
 
és
 
apta
 
per
 
al
 
nivell
 
gratuït
 
i
 
proveïda
 
per
 
Canonical,
 
amb
 
suport
 
oficial
 
disponible.
 
  


## Página 6

Per  començar,  configurem  el  tipus  d'instància  com  a  t3.micro  (amb  2  vCPU  i  1  GiB  de  
memòria
 
RAM),
 
ja
 
que
 
és
 
la
 
que
 
entra
 
dins
 
del
 
nivell
 
gratuït
 
d'AWS.
 
Com
 
a
 
mètode
 
d'autenticació,
 
assignem
 
el
 
parell
 
de
 
claus
 
"grup3"
 
per
 
assegurar-nos
 
un
 
accés
 
SSH
 
totalment
 
segur.
 
A
 
la
 
secció
 
de
 
xarxa,
 
deixem
 
la
 
VPC
 
per
 
defecte,
 
habilitem
 
l'assignació
 
automàtica
 
d'IP
 
pública
 
i
 
seleccionem
 
un
 
grup
 
de
 
seguretat
 
que
 
ja
 
teníem
 
creat.
 


## Página 8

Ens  connectem  per  SSH  a  la  terminal  de  la  instància  i  executem  els  comandaments  sudo  
apt
 
update
 
i
 
sudo
 
apt
 
install
 
-y
 
icecast2
 
liquidsoap.
 
Durant
 
la
 
instal·lació,
 
se'ns
 
obre
 
l'assistent
 
de
 
configuració
 
d'Icecast2
 
i
 
ens
 
pregunta
 
si
 
volem
 
configurar
 
el
 
servei;
 
li
 
diem
 
que
 
sí
 
i
 
hi
 
introduïm
 
la
 
nostra
 
IP
 
pública
 
(35.174.72.4)
 
com
 
a
 
hostname
 
del
 
servidor,
 
que
 
serà
 
el
 
prefix
 
per
 
a
 
tots
 
els
 
nostres
 
streams.
 
  El  mateix  assistent  ens  demana  les  contrasenyes  per  al  servei.  Establim  "prineus"  com  a  
contrasenya
 
de
 
les
 
fonts
 
d'àudio
 
(source
 
password),
 
que
 
és
 
la
 
que
 
farem
 
servir
 
als
 
clients
 
emissors
 
per
 
connectar-nos
 
al
 
servidor.
 
Després,
 
posem
 
"pirineus"
 
com
 
a
 
contrasenya
 
dels
 
relays
 
(per
 
a
 
la
 
retransmissió
 
entre
 
servidors
 
Icecast)
 
i,
 
finalment,
 
també
 
definim
 
"pirineus"
 
per
 
a
 
l'administració
 
web,
 
a
 
la
 
qual
 
podem
 
accedir
 
des
 
de
 
http://localhost:8000.
 
 


## Página 9

 
 


## Página 10

 
   


## Página 11

El  següent  pas  és  crear  el  directori  /opt/music  i  donar-li  permisos  777.  Després,  pugem  els  
nostres
 
fitxers
 
MP3
 
("See
 
U
 
In
 
Hell.mp3",
 
"Bando
 
Boyz
 
Free
 
5.mp3"
 
i
 
"Barcelona.mp3")
 
des
 
del
 
nostre
 
client
 
local
 
al
 
servidor
 
utilitzant
 
el
 
comandament
 
scp.
 
Un
 
cop
 
dalt,
 
editem
 
el
 
fitxer
 
/etc/liquidsoap/radio.liq
 
per
 
configurar
 
una
 
playlist
 
que
 
llegeixi
 
aquests
 
arxius
 
de
 
/opt/music
 
i
 
els
 
emeti
 
en
 
MP3
 
a
 
128
 
kbps
 
cap
 
al
 
punt
 
de
 
muntatge
 
/radio
 
d'Icecast2,
 
amb
 
el
 
nom
 
"InnovateTech
 
Radio".
 
 
 
   


## Página 12

Editem  el  fitxer  /etc/nginx/sites-available/audio  per  crear  un  servidor  virtual  que  escolti  al  
port
 
80.
 
Fem
 
que
 
redirigeixi
 
les
 
peticions
 
de
 
/music/
 
cap
 
al
 
nostre
 
directori
 
/opt/music/
 
i
 
hi
 
afegim
 
la
 
capçalera
 
CORS
 
Access-Control-Allow-Origin
 
*
 
per
 
poder-hi
 
accedir
 
des
 
del
 
navegador
 
sense
 
problemes.
 
Per
 
tancar
 
aquesta
 
part,
 
fem
 
dues
 
proves
 
de
 
velocitat
 
amb
 
speedtest-cli:
  A  la  primera  obtenim  1645  Mbps  de  baixada,  1553  Mbps  de  pujada  i  1,367  ms  de  ping.   A  la  segona  aconseguim  1748  Mbps  de  baixada  i  1828  Mbps  de  pujada.  Amb  això  
confirmem
 
que
 
tenim
 
una
 
connexió
 
d'altíssima
 
capacitat
 
a
 
AWS.
  
 
          


## Página 13

                  

## Página 14

2.3  —  Servei  de  VÍDEO  +  
Videoconferència
 

## Página 15

La  funcionalitat  del  servei  de  vídeo  consisteix  a  proporcionar  una  plataforma  capaç  de  
distribuir
 
continguts
 
audiovisuals
 
de
 
manera
 
eficient,
 
estable
 
i
 
accessible
 
per
 
a
 
tots
 
els
 
usuaris
 
de
 
l’organització.
 
Aquest
 
servei
 
permet
 
oferir
 
reproducció
 
de
 
vídeo
 
en
 
streaming
 
tant
 
sota
 
demanda
 
com
 
en
 
temps
 
real,
 
utilitzant
 
protocols
 
estàndard
 
com
 
RTMP
 
o
 
HLS
 
que
 
garanteixen
 
una
 
transmissió
 
fluida
 
i
 
adaptada
 
a
 
diferents
 
amplades
 
de
 
banda.
 
El
 
sistema
 
treballa
 
amb
 
formats
 
i
 
còdecs
 
de
 
vídeo
 
àmpliament
 
utilitzats,
 
com
 
MP4
 
i
 
H.264,
 
que
 
asseguren
 
una
 
alta
 
qualitat
 
d’imatge
 
i
 
una
 
compressió
 
òptima
 
per
 
reduir
 
el
 
consum
 
de
 
recursos.
 
Els
 
usuaris
 
poden
 
accedir
 
als
 
continguts
 
directament
 
des
 
del
 
navegador
 
web
 
o
 
mitjançant
 
clients
 
multimèdia
 
compatibles,
 
sense
 
necessitat
 
d’instal·lar
 
programari
 
addicional,
 
i
 
poden
 
iniciar
 
la
 
reproducció
 
de
 
manera
 
immediata
 
gràcies
 
a
 
la
 
transcodificació
 
automàtica
 
del
 
servidor.
 
El
 
servei
 
és
 
capaç
 
de
 
gestionar
 
múltiples
 
connexions
 
simultànies
 
i
 
manté
 
una
 
experiència
 
estable
 
fins
 
i
 
tot
 
en
 
entorns
 
corporatius
 
amb
 
alta
 
demanda.
 
A
 
més,
 
la
 
infraestructura
 
incorpora
 
funcionalitats
 
de
 
comunicació
 
en
 
temps
 
real
 
mitjançant
 
el
 
protocol
 
WebRTC,
 
que
 
permet
 
establir
 
videoconferències
 
directament
 
des
 
del
 
navegador
 
amb
 
baixa
 
latència,
 
transmissió
 
segura
 
i
 
compatibilitat
 
amb
 
la
 
majoria
 
de
 
dispositius
 
moderns.
 
Aquest
 
conjunt
 
de
 
funcionalitats
 
converteix
 
el
 
servei
 
de
 
vídeo
 
en
 
una
 
eina
 
completa
 
per
 
a
 
la
 
distribució
 
de
 
continguts
 
multimèdia
 
i
 
la
 
comunicació
 
interactiva
 
entre
 
usuaris
 
dins
 
l’entorn
 
de
 
treball.
 
Per  a  aquest  servei  s'ha  creat  una  nova  instància  AWS  EC2  independent.  La  raó  principal  
és
 
que
 
Jitsi
 
Meet
 
instal·la
 
el
 
seu
 
propi
 
servidor
 
NGINX,
 
cosa
 
que
 
entraria
 
en
 
conflicte
 
si
 
s'hagués
 
reutilitzat
 
una
 
instància
 
existent.
 
Configuració  de  la  instància:  
●  AMI:  Ubuntu  26.04  LTS  (amd64)  ●  Tipus:  t3.medium  (2  vCPU,  4  GB  RAM)  —  mínim  necessari  per  a  Jitsi  Meet  ●  Emmagatzematge:  20  GiB  (gp3)  ●  IP  pública:  44.223.73.232  ●  Clau  SSH:  server-video.pem  (RSA)  
 
 
 
 
 
 
 
 
 
 

## Página 16

Ports  oberts  al  Security  Group:  
Port  Protocol  Servei  
22  TCP  SSH  
80  TCP  HTTP  (NGINX)  
443  TCP  HTTPS  (Jitsi)  
1935  TCP  RTMP  (Streaming)  
4443  TCP  Jitsi  JVB  
8090  TCP  Reproductor  de  vídeo  
10000  UDP  Jitsi  Media  (WebRTC)  
3478  UDP  Jitsi  STUN/TURN  
 

## Página 17

Mostrem  les  regles  del  grup  de  seguretat  de  la  nova  instància  que  hem  configurat  
expressament
 
per
 
al
 
servei
 
de
 
vídeo
 
i
 
videoconferència.
 
Com
 
es
 
pot
 
veure,
 
obrim
 
els
 
ports
 
UDP
 
10000
 
i
 
3478
 
per
 
al
 
tràfic
 
WebRTC
 
de
 
Jitsi
 
Meet
 
(mèdia
 
i
 
STUN/TURN),
 
així
 
com
 
els
 
ports
 
TCP
 
1935
 
per
 
a
 
RTMP
 
i
 
4443
 
per
 
a
 
la
 
connexió
 
del
 
Jitsi
 
Video
 
Bridge
 
(JVB).
 
Deixem
 
tots
 
els
 
ports
 
oberts
 
a
 
qualsevol
 
IP
 
d'origen
 
(0.0.0.0/0).
 
 
 
 


## Página 18

 
 
Un  cop  connectat  al  servidor  via  SSH,  s'ha  actualitzat  el  sistema  i  instal·lat  les  eines  
bàsiques
 
necessàries
 
(curl,
 
wget,
 
git
 
i
 
ffmpeg
 
per
 
al
 
processament
 
de
 
vídeo):
 
 S'ha  instal·lat  NGINX  juntament  amb  el  mòdul  RTMP,  que  permet  rebre  streams  en  protocol  
RTMP
 
i
 
distribuir-los
 
en
 
format
 
HLS:
 
          


## Página 19

    Configuració  del  bloc  RTMP  (/etc/nginx/nginx.conf):  
Editem  el  fitxer  /etc/nginx/nginx.conf  per  afegir  el  nostre  bloc  RTMP  (sempre  fora  del  bloc  
http{}).
 
A
 
la
 
captura
 
es
 
veu
 
la
 
configuració
 
estàndard
 
de
 
NGINX
 
amb
 
els
 
workers,
 
el
 
PID,
 
el
 
log
 
d'errors
 
i
 
les
 
opcions
 
SSL
 
configurades
 
amb
 
TLSv1.2
 
i
 
TLSv1.3.
 
Al
 
final
 
d'aquest
 
fitxer
 
és
 
on
 
afegim
 
el
 
bloc
 
rtmp{}
 
per
 
indicar
 
al
 
servidor
 
que
 
escolti
 
pel
 
port
 
1935
 
i
 
transcodifiqui
 
el
 
flux
 
que
 
rebi
 
en
 
segments
 
HLS
 
de
 
3
 
segons,
 
guardant-los
 
a
 
/tmp/hls/.
 


## Página 20

 
 


## Página 21

Configuració  HLS  (/etc/nginx/sites-available/default):  
S'ha  afegit  un  bloc  location  /hls  dins  del  servidor  HTTP  per  servir  els  fitxers  HLS  generats  
amb
 
els
 
tipus
 
MIME
 
correctes:
 
 Verificació  dels  serveis:  
 
 
 
 


## Página 22

S'han  utilitzat  els  següents  formats  i  còdecs:  
H.264  (libx264):  còdec  de  vídeo  estàndard  àmpliament  compatible,  alta  compressió  
mantenint
 
qualitat.
 
Perfil
 
High,
 
level
 
3.1,
 
resolució
 
1280x720
 
(HD
 
720p)
 
a
 
1500
 
kb/s.
 
MP4:  format  contenidor  estàndard  per  al  vídeo  original  emmagatzemat  al  servidor.  
HLS  (HTTP  Live  Streaming):  protocol  de  distribució  desenvolupat  per  Apple.  El  vídeo  es  
divideix
 
en
 
segments
 
.ts
 
(MPEG-2
 
Transport
 
Stream)
 
de
 
3
 
segons
 
cadascun,
 
amb
 
una
 
playlist
 
.m3u8
 
que
 
els
 
referencia.
 
Permet
 
adaptació
 
de
 
qualitat
 
i
 
reproducció
 
des
 
de
 
qualsevol
 
navegador
 
modern.
 
AAC:  còdec  d'àudio,  44100  Hz,  estèreo,  128  kb/s.  
Per  generar  el  vídeo  de  prova  i  emetre'l  en  streaming  s'ha  utilitzat  ffmpeg:


## Página 23

Editem  el  fitxer  /var/www/html/index.html  per  muntar  una  aplicació  web  dividida  en  tres  
seccions:
 
"Streaming
 
en
 
Viu"
 
(amb
 
un
 
reproductor
 
HLS.js
 
que
 
apunta
 
al
 
nostre
 
flux
 
en
 
temps
 
real),
 
"Vídeos"
 
(una
 
llista
 
amb
 
quatre
 
vídeos
 
MP4
 
sota
 
demanda:
 
Final,
 
MUI,
 
Caulifla
 
i
 
Gogeta)
 
i
 
"Informació"
 
del
 
servidor.
 
Al
 
mateix
 
temps,
 
executem
 
ffmpeg
 
en
 
mode
 
nohup
 
per
 
emetre
 
el
 
vídeo
 
en
 
bucle
 
cap
 
al
 
nostre
 
servidor
 
RTMP
 
local,
 
de
 
manera
 
que
 
va
 
generant
 
els
 
fragments
 
HLS
 
a
 
/tmp/hls/
 
contínuament.


## Página 25

 
 


## Página 26

 
 


## Página 27

 
 
S'ha  creat  una  pàgina  web  a  /var/www/html/index.html  amb  un  reproductor  HLS.js  que  
permet
 
visualitzar
 
el
 
vídeo
 
en
 
streaming
 
des
 
de
 
qualsevol
 
navegador
 
sense
 
plugins
 
addicionals.
 
Problema  trobat  i  solució:  En  instal·lar  Jitsi  Meet,  aquest  va  ocupar  el  port  80  i  443  amb  el  
seu
 
propi
 
NGINX.
 
Per
 
solucionar
 
el
 
conflicte,
 
es
 
va
 
crear
 
un
 
nou
 
virtual
 
host
 
de
 
NGINX
 
al
 
port
 
8090:
 


## Página 28

 
Protocol  WebRTC:  
Jitsi  Meet  utilitza  WebRTC  (Web  Real-Time  Communication),  un  estàndard  obert  que  permet  
comunicació
 
en
 
temps
 
real
 
directament
 
des
 
del
 
navegador
 
sense
 
plugins.
 
Característiques
 
principals:
 
●  Comunicació  peer-to-peer  o  via  servidor  SFU  ●  Xifrat  DTLS-SRTP  ●  Adaptació  automàtica  del  bitrate  ●  Negociació  de  connexions  via  ICE/STUN/TURN  
Instal·lació:  
S'ha  configurat  el  hostname,  instal·lat  Java  (requerit  per  Jitsi)  i  afegit  el  repositori  oficial:  
 


## Página 29

 
 
Durant  la  instal·lació  s'han  configurat:  
●  Hostname:  35.153.48.218  (IP  pública  del  servidor)  ●  Certificat  SSL:  Generate  a  new  self-signed  certificate  


## Página 30

 
         


## Página 31

Verificació  dels  serveis:  
  Prova  de  videoconferència:  S'ha  creat  una  sala  a  https://44.223.73.232/Grup3  i  s'hi  ha  
connectat
 
des
 
de
 
dues
 
pestanyes
 
del
 
navegador
 
simultàniament,
 
comprovant
 
que
 
l'àudio
 
i
 
el
 
vídeo
 
funcionaven
 
correctament
 
entre
 
els
 
dos
 
participants.
  
 


## Página 32

 
 


## Página 33

2.4  —  Comprovacions  d’amplada  de  
banda
 

## Página 34

2.4.1  Objectiu  L’objectiu  d’aquesta  secció  és  verificar  que  la  infraestructura  de  xarxa  disponible  és  capaç  
de
 
suportar
 
el
 
servei
 
d’àudio
 
en
 
streaming
 
desplegat
 
amb
 
Icecast2
 
sense
 
degradació
 
del
 
servei.
 
A  més,  es  comprova  que  l’amplada  de  banda,  la  latència  i  l’estabilitat  de  la  connexió  són  
suficients
 
per
 
garantir
 
una
 
reproducció
 
fluida
 
i
 
sense
 
interrupcions.
 
2.4.2  Requeriments   
 
 
2.4.3  Proves  realitzades    Audio  →    Prova  1  —  Estat  normal  del  servidor   


## Página 35

 Prova  2  —  Amb  Icecast  reproduint  música   
 
 Conclusió  
Streaming  d’àudio  (Icecast2)  
-  Consum  aproximat:  0,1  –  0,5  Mbps  per  usuari.  -  Amb  90  Mbps  disponibles,  el  servidor  podria  suportar  més  de  150  usuaris  
simultanis
 
sense
 
problemes.
 -  La  latència  inferior  a  20  ms  garanteix  una  resposta  immediata  en  la  
reproducció.
 
Classificació  del  sistema   
La  infraestructura  és  més  que  suficient  per  al  servei  d’àudio  i  compleix  
àmpliament
 
els
 
requisits.
 
(Acceptable)
 


## Página 36

  Video/streaming  →    Prova  1  —  Estat  normal  del  servidor   
 Prova  2  —  Reproduint  video   
 
   Conclusió  
Streaming  d’àudio   
-  Consum  aproximat:  0,1  –  0,5  Mbps  per  usuari.  -  Amb  90  Mbps  disponibles,  el  servidor  podria  suportar  més  de  150  usuaris  
simultanis
 
sense
 
problemes.
 -  La  latència  inferior  a  20  ms  garanteix  una  resposta  immediata  en  la  
reproducció.
 


## Página 37

Classificació  del  sistema   
La  infraestructura  és  més  que  suficient  per  al  servei  d’àudio  i  compleix  
àmpliament
 
els
 
requisits.
 
(Acceptable)
 
  

