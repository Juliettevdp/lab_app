
Benodigdheden:
flask (app framework)
uwsgi (application server)
nginx (web server)
python virtual environment
eventueel skeleton boilerplate

Bron:
https://www.udemy.com/raspberry-pi-full-stack-raspbian/
https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/

1. https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/360_seting_up_system_Python_notes.txt

2. https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/365_seting_up_the_Python_Virtual_Environment_notes.txt

3. https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/460_sample_SQLite3_session.txt

4. pip install uwsgi (als root en in python venv).
	check: ls -al bin.  Daarin moet uwsgi voorkomen.

5.  https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/CommandLineInstructions/430_setup_nginx_and_flask_notes.txt

6. Create lab_app_nginx.conf and create symbolic link for lab_app_nginx.conf:
	ln -s /var/www/lab_app/lab_app_nginx.conf /etc/nginx/conf.d/
	check met:  ls -al /etc/nginx/conf.d/

7. restart nginx:
    /etc/init.d/nginx restart

8. create uwsgi file (https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/lab_app_uwsgi.ini) en vergeet niet mkdir /var/log/uwsgi
Start handmatig om te checken: bin/uwsgi --ini /var/www/lab_app/lab_app_uwsgi.ini


9. Om uwsgi automatisch te launchen kan je systemd gebruiken.
	1) vim /etc/systemd/system/emperor.uwsgi.service
	2) inhoud van file kopiëren en opslaan:
	https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/emperor.uwsgi.service
	3) Uit vim gaan, om (eenmalig) te starten:
	   systemctl start emperor.uwsgi.service
	4) Om te checken of het runt: 
	   systemctl status emperor.uwsgi.service 
	5) Om bij boot te starten:
	   systemctl enable emperor.uwsgi.service

10. Voeg skeleton indien nodig toe. Dit is een zeer compact CSS framework en ggeeft basisstyling.

11. Referente sqlite3: https://github.com/futureshocked/RaspberryPi-FullStack/blob/master/Sample%20SQLite3%20session.txt
LET OP
Als je start soms nodig: sudo su (root) en doe . bin/activate (python virtual environment activeren)
vanuit /var/www/lab_app. Na wijzigingen in python: 
systemctl restart emperor.uwsgi.service 

12. crontab -e en dan https://github.com/futureshocked/RaspberryPiFullStack_Raspbian/blob/master/cron_schedule
output is bijv /tmp/crontab.HzlVVt/crontab 
cron_schedule is hier ter referentie in deze map.
DEBUGGING
- Via poort 8080 als debugging aanstaat
- tail -n 100 /var/log/uwsgi/lab_app_uwsgi.log