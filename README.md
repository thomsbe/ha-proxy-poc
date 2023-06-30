# Beispiel HA-Proxy-Dockerstack

Dies ist ein kleiner Dockerstack der dazu dient, Experimente mit dem HA-Proxy zu machen. Es handelt sich um 2 Container die den kleinsten möglichen Webserver bereitstellen. static1 und static2. Diese liefern den HTML aus, der im entsprechenden Order ist.

Ein simple auf Debian 12 installierter HA-Proxy liest seine Konfig haproxy.cfg aus dem Root und startet dann. Diese Konfig ist hier static1 als Server und static2 als Backup. Erkennbar an der HTML-Seite welche 1 oder 2 zeigt.

Nichts wildes. Aber man kann experimentieren. Mit `watch -n 1 curl --silent locahost` kann man sich in einem Terminal immer die HTML-Seite anzeigen. Evtl. könnte man da auch kein-HTML nehmen, sondern nur Text...

In einem anderen Terminal kann man jetzt in den HA-Proxy container attachen und dort findet man eben doch etwas Spezielles!

Im Container ist `netcat` und `socat` installiert. Außerdem ist der HA-Proxy so konfiguriert, dass er seine Runtime-API über den Socket zugänglich hat. Man kann also jetzt im Container Dinge tun wie:

`echo "help" | nc -U /var/run/haproxy.sock` (Das zeigt alle möglichen Befehle an.)

`echo "set server web_servers/s1 state maint" | nc -U /var/run/haproxy.sock` (Das stellt den Server s1 aus dem Backend web_servers auf den Status MAINT, also absichtliches herausnehmen aus dem Balancer. In dem Falle wird der Backup aktiv geschaltet und auf der HTML-Seite steht nun 2.)

`echo "set server web_servers/s1 state ready" | nc -U /var/run/haproxy.sock` (Das stellt den s1 wieder auf ready und er bekommt die Requests, s2 wird wieder Backup.)

So kann es gehen...