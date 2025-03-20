Esto es un codigo simple el cual tiene la capacidad de extrar informacion de la red en donde se ejecute y enviarlos a un bot-chat de telegram el cual es BotFather que es bastante simple de usar ya que las intrucciones te las entrega el mismo chat, una ves creado el bot solo tienes ingregar tu api key en la linea #24 donde dira BOT_TOKEN y ingresar tu chat id en la linea #25 CHAT_ID ambos tiene que ir entre comillas para que funcionen, despues solo es ejecutar y listo.

Tambien cuenta con otras funciones como una captura de video de camara y captura de video del escritorio totalmente configurable en su duraccion por defecto esta en 5 segundos pero creo que pueden aumentar mucho mas la duraccion de igual manera hay que considerar que esto se envia como archivo a un chat de telegram y no sabria cual es el limite de peso que puede tener el chat al menos con 5 segundos no hay problemas

la ultima funcion es un keyloger simple el cual tambien es completamente configurable en su duraccion por defecto lo deje en 50 segundos para que no tenga que durar una eternidad ejecutando el test pero funciona al 100% solo funciona para el teclado no hace lectura de mouse, este archivo de texto lo manda como un .txt a el chat_bot y se descarga para su lectura

>[!warning]
>los archivos se suben al chat pero no se borran del disco y se almacenan en la carpeta desde donde se ejecute el codigo, por si tiene poco espacio la opcion 2 puede ser un problema.

>[!important]
>ten en cuenta que los datos solo van a ser de la maquina en cual se ejecute el script esto no escanea tu red en general solo entrega los datos de una maquina en el siguiente formato:

          Información de red:
        - Nombre de host: TEST
        - Tipo de nodo: TEST
        - Adaptador Ethernet:
          * Dirección física (MAC): E0-E0-00-00-0C-0E
          * IPv4: 000.000.000.00
          * Máscara de subred: 255.255.0.0
          * IPv6: fe00::baf:ae00:0aa0:000
        - DHCP habilitado: No
        - Puerta de enlace predeterminada: Puerta de enlace predeterminada . . . . . : 198.00.00.1
        - Servidores DNS: Servidores DNS. . . . . . . . . . . . . . : 0.0.0.0
        - NetBIOS TCP/IP habilitado: 
        Información de IP pública y geolocalización:
        - IP Pública: 000.000.000.00
        - Ciudad: TEST
        - Región: TEST
        - País: TEST
        - Ubicación (Latitud, Longitud): TEST

>[!TIP] 
Para no tener que estar ejecutando desde un editor el codigo recomiendo trasformarlo en un .exe de igual manera dejare el que hice yo por si les da peresa.

>[!important]
>DEPENDECIAS
keyboard                  0.13.5
numpy                     2.2.3
opencv-python             4.11.0.86
platformdirs              4.3.6
psutil                    7.0.0
requests                  2.32.3


        
