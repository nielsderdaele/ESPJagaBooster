This code is meant for equipping standard ("Jaga") radiators with fans, and contols their speed based on the heating/cooling demand. This allows low-temperature heat sources (e.g. heat pumps) to be used with normal radiators, and still reach reasonable heat output.

----

Voor Nederlanders/Belgen zijn bijpassende modules hier verkrijgbaar: https://www.sdr-engineering.nl/webshop/

----

Let op: deze firmware is alleen geschikt voor de ESP8266-versie van de modules (modules R04 en ouder)!


Beknopte handleiding
================

Benodigdheden:
----------------

1. Bijbehorende module.
2. Micro-USB kabel tbv eerste keer programmeren van module. Alle opvolgende keren kan de module "over the air" geprogrammeerd worden indien gebruik gemaakt wordt van WiFi-integratie door in de Arduino IDE de netwerkpoort met de nodenaam te selecteren.
3. Laptop met Arduino IDE v1.8.x geinstalleerd (deze instructies zijn niet van toepassing op de nieuwe versie IDE). Bovenaan de programmacode (ESPJagaBooster.ino op deze pagina) tref je de benodigde libraries en instellingen.
4. Ventilatoren met 4 pinnen (PWM-gestuurd), zoals bijvoorbeeld de Arctic P14 PWM PST fans.
5. Een 12V voeding met 5.5/2.1mm DC connector (voor module versie R04 en nieuwer) of een 5.5/2.5mm DC connector (voor oudere modules).

Optioneel, om de "smart home" functies te gebruiken:

6. Een WiFi router
7. Home Assistant op een Raspberry Pi (https://www.home-assistant.io/) met MQTT integratie ingeschakeld -OF- een MQTT broker/server (e.g. mosquitto) met bijvoorbeeld Node Red


Software download
----------------
- Arduino IDE: https://downloads.arduino.cc/arduino-1.8.19-windows.exe



Montage
----------------

1. Plaats rubber tochtstrip (zoals deze: https://www.hornbach.nl/shop/ELLEN-Tochtstrip-siliconen-universeel-wit-6-m/7571056/artikel.html) onder de ventilatoren ter demping van vibraties. Zorg ervoor dat de pijl voor de luchtstroming aan de zijkant van de ventilator omhoog wijst, plaats de rubber tochtstrip dan aan de onderzijde.

![](<https://raw.githubusercontent.com/BeaverUI/ESPJagaBooster/main/images/fan_suspension.jpg?>)

2. Plaats de fans op de convector, laat de kabels richting de kappen van de Jaga wijzen. Let erop dat de luchtstroming opwaarts dient te zijn.
3. Gebruik tiewraps en 4-pin fan verlengkabels om de bedrading netjes in de kap weg te werken.

![](<https://raw.githubusercontent.com/BeaverUI/ESPJagaBooster/main/images/fan_mounting_1.jpg?>)
![](<https://raw.githubusercontent.com/BeaverUI/ESPJagaBooster/main/images/fan_mounting_2.jpg?>)
![](<https://raw.githubusercontent.com/BeaverUI/ESPJagaBooster/main/images/extension_cable.jpg?>)


4. Plak tochtstrip op de onderzijde van de module zodat de pinnen geen contact maken met de radiatorbehuizing.
5. Monteer de besturingsmodule aan de zijde van de thermostaatknop (buiten de luchtstroming), gebruik een tiewrap om de module vast te maken.
Let op: de WiFi ontvangst wordt beinvloed door het metaal in de radiator. Zorg ervoor dat de antenne niet te dicht tegen de metalen beplating zit om het ontvangst te optimaliseren. Wanneer de module draait kan de RSSI gecontroleerd worden via MQTT, deze dient boven de -80 dB te liggen voor een betrouwbare werking.

![](<https://raw.githubusercontent.com/BeaverUI/ESPJagaBooster/main/images/module_mounting.jpg?>)

6. Sluit de fans aan op de module.
7. Sluit de temperatuursensoren aan op de module.

 - CN0 = omgevingslucht, deze moet vrij hangen onder de radiator
 - CN1 = aanvoer, trek de temperatuursensor met een tiewrap strak tegen de aanvoer-waterleiding
 - CN2 = retour, trek deze met een tiewrap strak tegen de retour-waterleiding
 
 Eventueel kun je de temperatuursensoren die aan de leidingen getiewrapped zitten nog voorzien van wat katoentape ter isolatie van de omgevingslucht.

8. Sluit de +12V voeding aan op de module.
9. Programmeer de module via de micro-USB kabel. Zie notities in de broncode voor meer informatie.
10. Controleer de werking: kijk via de Arduino IDE naar de Serial Monitor (onder Tools) om activiteit van de module te zien. Als deze goed functioneert moet er regelmatig "Sending MQTT update..." voorbij komen.
11. Gebruik MQTT Explorer (https://github.com/thomasnordquist/MQTT-Explorer) om de berichten van de module te checken. Controleer met name de temperaturen en RSSI (dient boven de -85 a -80 dB te liggen, of beter nog boven de -75 dB). Activeer de ventilatoren handmatig (fan_controlmode_ref = 1, fan_dutycycle_ref = 0 ... 100) en controleer of ze allemaal functioneren en netjes in snelheid geregeld worden.
