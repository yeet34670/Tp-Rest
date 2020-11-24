JEAN-BAPTISTE Florian
IDO

# Tp-Rest

## partie 1

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

5) 
