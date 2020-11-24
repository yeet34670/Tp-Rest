JEAN-BAPTISTE Florian
IDO

# Tp-Rest

## Partie 1:

2) On peut piloter le shelly plug depuis son interface sur l'adresse suivante: http://10.202.255.252/
Il suffit de cliquer sur le bouton power afin de l'allumer/éteindre.

3) Pour récupérer la consommation actuelle il faut entrer:
curl http://10.202.255.252/meter/0

Pour allumer ou éteindre la pris il faut entrer:
allumer: curl http://10.202.255.252/relay/0?turn=on
eteindre: curl http://10.202.255.252/relay/0?turn=off

4) Pour avoir des informations triées losrqu'on allume la led:
curl --silent http://10.202.255.252/relay/0?turn=on | jq

et pour avoir uniquement son état allumé:
curl --silent http://10.202.255.252/relay/0?turn=on | jq .ison

5) J'ai créé un jeu de 5 scripts:
- allumerLED: permet de mettre en marche la LED
- eteindreLED: permet d'éteindre la LED
- etatLED: permet de savoir si la LED est allumée ou non
- powerstatus: permet de savoir la consommation actuelle de la LED 
- GererLED: Si la LED est éteinte, permet de l'allumer en ayant sa consommation et sinon permet de l'éteindre

6) Filtre de capture wireshark :
((host 10.202.14.1 and dst 10.202.255.252) or (host 10.202.255.252 and dst 10.202.14.1)) and port 80



Envoi de paquets pour allumer la LED:

```
No.     Time           Source                Destination           Protocol Length Info
      1 0.000000000    10.202.14.1           10.202.255.252        TCP      74     48742 → 80 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM=1 TSval=2520880645 TSecr=0 WS=1024

Frame 1: 74 bytes on wire (592 bits), 74 bytes captured (592 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 0, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      2 0.007495145    10.202.255.252        10.202.14.1           TCP      62     80 → 48742 [SYN, ACK] Seq=0 Ack=1 Win=2144 Len=0 MSS=536 SACK_PERM=1

Frame 2: 62 bytes on wire (496 bits), 62 bytes captured (496 bits) on interface 0
Ethernet II, Src: Espressi_6a:64:db (ec:fa:bc:6a:64:db), Dst: Dell_96:48:0a (d4:be:d9:96:48:0a)
Internet Protocol Version 4, Src: 10.202.255.252, Dst: 10.202.14.1
Transmission Control Protocol, Src Port: 80, Dst Port: 48742, Seq: 0, Ack: 1, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      3 0.007554273    10.202.14.1           10.202.255.252        TCP      54     48742 → 80 [ACK] Seq=1 Ack=1 Win=64240 Len=0

Frame 3: 54 bytes on wire (432 bits), 54 bytes captured (432 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 1, Ack: 1, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      4 0.007637924    10.202.14.1           10.202.255.252        HTTP     147    GET /relay/0?turn=on HTTP/1.1 

Frame 4: 147 bytes on wire (1176 bits), 147 bytes captured (1176 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 1, Ack: 1, Len: 93
Hypertext Transfer Protocol

No.     Time           Source                Destination           Protocol Length Info
      5 0.023043493    10.202.255.252        10.202.14.1           HTTP     290    HTTP/1.1 200 OK  (application/json)

Frame 5: 290 bytes on wire (2320 bits), 290 bytes captured (2320 bits) on interface 0
Ethernet II, Src: Espressi_6a:64:db (ec:fa:bc:6a:64:db), Dst: Dell_96:48:0a (d4:be:d9:96:48:0a)
Internet Protocol Version 4, Src: 10.202.255.252, Dst: 10.202.14.1
Transmission Control Protocol, Src Port: 80, Dst Port: 48742, Seq: 1, Ack: 94, Len: 236
Hypertext Transfer Protocol
JavaScript Object Notation: application/json

No.     Time           Source                Destination           Protocol Length Info
      6 0.023096090    10.202.14.1           10.202.255.252        TCP      54     48742 → 80 [ACK] Seq=94 Ack=237 Win=64004 Len=0

Frame 6: 54 bytes on wire (432 bits), 54 bytes captured (432 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 94, Ack: 237, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      7 0.023206735    10.202.14.1           10.202.255.252        TCP      54     48742 → 80 [FIN, ACK] Seq=94 Ack=237 Win=64004 Len=0

Frame 7: 54 bytes on wire (432 bits), 54 bytes captured (432 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 94, Ack: 237, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      8 0.028391240    10.202.255.252        10.202.14.1           TCP      60     80 → 48742 [ACK] Seq=237 Ack=95 Win=2050 Len=0

Frame 8: 60 bytes on wire (480 bits), 60 bytes captured (480 bits) on interface 0
Ethernet II, Src: Espressi_6a:64:db (ec:fa:bc:6a:64:db), Dst: Dell_96:48:0a (d4:be:d9:96:48:0a)
Internet Protocol Version 4, Src: 10.202.255.252, Dst: 10.202.14.1
Transmission Control Protocol, Src Port: 80, Dst Port: 48742, Seq: 237, Ack: 95, Len: 0

No.     Time           Source                Destination           Protocol Length Info
      9 0.030254420    10.202.255.252        10.202.14.1           TCP      60     80 → 48742 [FIN, ACK] Seq=237 Ack=95 Win=2050 Len=0

Frame 9: 60 bytes on wire (480 bits), 60 bytes captured (480 bits) on interface 0
Ethernet II, Src: Espressi_6a:64:db (ec:fa:bc:6a:64:db), Dst: Dell_96:48:0a (d4:be:d9:96:48:0a)
Internet Protocol Version 4, Src: 10.202.255.252, Dst: 10.202.14.1
Transmission Control Protocol, Src Port: 80, Dst Port: 48742, Seq: 237, Ack: 95, Len: 0

No.     Time           Source                Destination           Protocol Length Info
     10 0.030313441    10.202.14.1           10.202.255.252        TCP      54     48742 → 80 [ACK] Seq=95 Ack=238 Win=64004 Len=0

Frame 10: 54 bytes on wire (432 bits), 54 bytes captured (432 bits) on interface 0
Ethernet II, Src: Dell_96:48:0a (d4:be:d9:96:48:0a), Dst: Espressi_6a:64:db (ec:fa:bc:6a:64:db)
Internet Protocol Version 4, Src: 10.202.14.1, Dst: 10.202.255.252
Transmission Control Protocol, Src Port: 48742, Dst Port: 80, Seq: 95, Ack: 238, Len: 0
```
On peut voir qu'il y a les 3 paquets tcp pour établir la connexion, ensuite viens la requette HTTP afin d'allumer la LED, le serveur nous répond "OK".
Pour allumer un plug, le paquet en question envoyé fait 147 octets. (4eme trame)

7) Pour faire ce tag, il faut entrer : git tag -a partie_1 -m "partie_1"
Et ensuite le push: git push --tags


## Partie 2:

Notre LED a comme ip 10.202.0.107 et est connecté en tant que 6A6534 sur le brokerMQTT

Pour lancer une commande sur la led, il faut entrer:
mosquitto_pub -h 10.202.0.107 -t shellies/shellyplug-s-6A6534/relay/0/command -m "commande à effectuer"

Pour allumer la LED: mosquitto_pub -h 10.202.0.107 -t shellies/shellyplug-s-6A6534/relay/0/command -m "on"

Pour éteindre la LED: mosquitto_pub -h 10.202.0.107 -t shellies/shellyplug-s-6A6534/relay/0/command -m "off"

Pour afficher la consommation: mosquitto_sub -h 10.202.0.107 -t shellies/shellyplug-s-6A6534/relay/0/power -C 1

Pour afficher son etat: mosquitto_sub -h 10.202.0.107 -t shellies/shellyplug-s-6A6534/relay/0 -C 1


## Partie 3

Pour installer nginx: sudo apt install nginx

Pour lancer nginx: sudo service nginx
Penser à vérifier qu'il fonctionne bien : sudo service nginx status
Et ouvrir une page avec "localhost", une page "Welcome to nginx!" devrait etre affiché.




























