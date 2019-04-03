# Installatie

## Benodigdheden
* Flask (app framework)
* Uwsgi (application server)
* Nginx (web server)
* Python virtual environment
* Eventueel skeleton boilerplate
* Sensor DHT11 of DHT22
* Adafruit_DHT
* Plotly
* SQLite3
* Arrow voor date/time
* Datetime picker widget

## Stappenplan (uitgebreid)
1. Kopieer en plak uit het volgende bestand 
https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/360_seting_up_system_Python_notes.txt

2.  Kopieer en plak uit het volgende bestand 
https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/365_seting_up_the_Python_Virtual_Environment_notes.txt

3.  Kopieer en plak uit het volgende bestand 
https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/460_sample_SQLite3_session.txt

4. pip install uwsgi (als root en in python venv).
    check: ls -al bin.  Daarin moet uwsgi voorkomen.

5.   Kopieer en plak uit het volgende bestand 
https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/430_setup_nginx_and_flask_notes.txt

6. Create lab_app_nginx.conf and create symbolic link for lab_app_nginx.conf:
    ln -s /var/www/lab_app/lab_app_nginx.conf /etc/nginx/conf.d/
    check met:  ls -al /etc/nginx/conf.d/

7. Restart nginx:
    /etc/init.d/nginx restart

8. Create uwsgi file (zie https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/lab_app_uwsgi.ini) en vergeet niet mkdir /var/log/uwsgi
Start handmatig om te checken: bin/uwsgi --ini /var/www/lab_app/lab_app_uwsgi.ini

9. Om uwsgi automatisch te launchen kan je systemd gebruiken.
      - vim /etc/systemd/system/emperor.uwsgi.service
      - inhoud van file kopiëren en opslaan:
      https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/emperor.uwsgi.service
      - Uit vim gaan, om (eenmalig) te starten:
         systemctl start emperor.uwsgi.service
      - Om te checken of het runt: 
         systemctl status emperor.uwsgi.service 
      - Om bij boot te starten:
         systemctl enable emperor.uwsgi.service

10. Voeg Skeleton indien gewenst toe. 

11. Referentie sqlite3: https://github.com/futureshocked/RaspberryPi-FullStack/blob/master/Sample%20SQLite3%20session.txt

12. crontab -e en kopieer dan uit https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/cron_schedule
cron_schedule_example is hier ter referentie in deze map.

### Debugging
- Via poort 8080 als debugging aanstaat
- Voor uwsgi: tail -n 100 /var/log/uwsgi/lab_app_uwsgi.log (let op dat je zelf het pad hier naartoe moet aanmaken). In lab_app-uwsgi.ini kan je het pad aanmaken voor logging. Referentie: https://uwsgi-docs.readthedocs.io/en/latest/Logging.html

### Paden
Er wordt hier vanuit gegaan dat de app in /var/www/lab_app staat. Hou er rekening mee als je dit pad verandert dat je het ook in de code aanpast (zoals in lab_app_uwsgi.ini, lab_app_nginx.conf,lab_app.py, env_log.py).

### Let op
Let op dat je soms root toegang nodig hebt en dat je de Python virtual environment moet activeren ( . bin/activate )
Na wijzigingen in python systemctl restarten: systemctl restart emperor.uwsgi.service

### Timezones
De timezones worden de code geconverteerd. De Raspberry Pi moet op UTC staan. 

### Browser compatibility
Werkt in alle browsers, alleen in Firefox laden pas na uitschakelen blokkering vsn trackers (schildje naast adresbalk). Trackers, in dit geval gcharts, worden in FF automatisch geblokkeerd.

### Bron
* https://www.udemy.com/raspberry-pi-full-stack-raspbian/
* https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/
