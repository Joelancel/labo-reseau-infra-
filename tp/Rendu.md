#Tp1


-Connectez-vous en SSH aux machines : 
ssh-keygen : génération de la clefs

- Changez le nom d'hôte des machines pour avoir respectivement vm-landing1 et vm-landing2 : 
echo 'vm-landing1' | sudo tee /etc/hostname
echo 'vm-landing2' | sudo tee /etc/hostname

-Trouvez l'adresse IP locale des machines : 
ip a :  inet 172.16.64.2/24 vm1
ip a :  inet 172.16.64.2/24 vm2

- Quelle est l'adresse de broadcast ? 
 ip a : brd 172.16.64.255 vm1
 ip a :  brd 172.16.64.255 vm2
 
 - Trouvez le masque de sous-réseau des machines : 
 sudo /etc/sysconfig/network-scripts ifcfg-enp0s8 : NETMASK=255.255.255.0
 
 -Trouvez l'adresse MAC des machines : 
 ip a : 08:00:27:96:20:f0 vm2
 ip a : 08:00:27:9e:90:67 vm1
 
 -Pingez l'adresse publique du site www.ynov.com avec une des deux machines : 
 [user@vm-landing2 ~]$ ping ynov.com
PING ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=56 time=17.2 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=2 ttl=56 time=17.8 ms
^C64 bytes from 104.26.11.233: icmp_seq=3 ttl=56 time=17.6 ms

--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 10115ms
rtt min/avg/max/mdev = 17.235/17.542/17.775/0.226 ms

-Sur chaque machine, créez respectivement un utilisateur labo-user1 et labo-user2 : 
sudo useradd labo-user1 
sudo useradd labo-user2

🎰 Essayez de lire le contenu du fichier /var/log/tallylog
🎯 Que se passe-t-il ? Pourquoi ?

Cela ne m'ouvre pas de fichier car je ne suis pas autorisé

Ajoutez ces utilisateurs au groupe wheel et retentez
🎯 Que se pase-t-il ? Pourquoi ?
[alexandre@vm-landing2 ~]$ sudo usermod -aG wheel labo-user1
[alexandre@vm-landing2 ~]$ sudo usermod -aG wheel labo-user2

je peux pas ouvrir car je ne suis pas administrateur 

Retentez avec la commande sudo cat
🎯 Il n'y aura plus d'erreur. Pourquoi ?

L'erreur n'est plus visible car je suis autorisé a utilisé sudo cat 

Retentez en changeant d'utilisateur et en passant root
🎯 Ca devrait marcher. Pourquoi ?

Car on est administrateur 


Ajoutez l'utilisateur labo-user1 au sudoers (petit coup de Google), puis retentez la commande sudo cat
🎯 Ca devrait marcher. Pourquoi ?

Car labo-user1 est devenue un administrateur et peux utilisé sudo 

🎰 Pingez vm-landing2 avec vm-landing1
```[user@vm-landing2 ~]$ ping 172.16.64.2
PING 172.16.64.2 (172.16.64.2) 56(84) bytes of data.
64 bytes from 172.16.64.2: icmp_seq=1 ttl=64 time=1.37 ms
64 bytes from 172.16.64.2: icmp_seq=2 ttl=64 time=0.804 ms
64 bytes from 172.16.64.2: icmp_seq=3 ttl=64 time=0.703 ms
64 bytes from 172.16.64.2: icmp_seq=4 ttl=64 time=0.927 ms
64 bytes from 172.16.64.2: icmp_seq=5 ttl=64 time=1.08 ms
64 bytes from 172.16.64.2: icmp_seq=6 ttl=64 time=0.631 ms
^C
--- 172.16.64.2 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5055ms
rtt min/avg/max/mdev = 0.631/0.920/1.372/0.250 ms
```

-Sur la machine landing-vm1 :

*    Installez les paquets sl, dnsmasq et htop. 🎰 Vérifiez leur version* : 
 
    ```[user@vm-landing2 ~]$ sudo dnf install htop fail2ban unzip```

*    Changez le DNS pour celui de CloudFlare puis 🎰 pingez google.com*
    
    [user@vm-landing1 ~]$ ping google.com
```PING google.com (142.250.179.110) 56(84) bytes of data.
64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=1 ttl=115 time=28.8 ms
64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=2 ttl=115 time=29.2 ms
64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=3 ttl=115 time=28.9 ms
^C64 bytes from 142.250.179.110: icmp_seq=4 ttl=115 time=28.7 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 15193ms
rtt min/avg/max/mdev = 28.663/28.872/29.165/0.184 ms
```


Sur la machine landing-vm2 :

   *** Installez les paquets hotop, fail2ban et unzip. 🎰 Vérifiez leur version

*    Changez le DNS pour celui de Google puis 🎰 pingez cloudflare.com



```[user@vm-landing2 ~]$ ping google.com
PING google.com (216.58.214.174) 56(84) bytes of data.
64 bytes from mad01s26-in-f14.1e100.net (216.58.214.174): icmp_seq=1 ttl=115 time=29.3 ms
64 bytes from par10s42-in-f14.1e100.net (216.58.214.174): icmp_seq=2 ttl=115 time=31.0 ms
64 bytes from 216.58.214.174: icmp_seq=3 ttl=115 time=31.2 ms

--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 10136ms
rtt min/avg/max/mdev = 29.289/30.520/31.237/0.874 ms```

```[user@vm-landing2 ~]$ ping cloudflare.com
PING cloudflare.com (104.16.132.229) 56(84) bytes of data.
64 bytes from 104.16.132.229 (104.16.132.229): icmp_seq=1 ttl=56 time=19.2 ms
64 bytes from 104.16.132.229 (104.16.132.229): icmp_seq=2 ttl=56 time=21.4 ms
64 bytes from 104.16.132.229 (104.16.132.229): icmp_seq=3 ttl=56 time=19.7 ms
^C64 bytes from 104.16.132.229: icmp_seq=4 ttl=56 time=23.0 ms

--- cloudflare.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 14052ms
rtt min/avg/max/mdev = 19.205/20.815/23.016/1.503 ms```

 *   🎰 Trouvez et affichez la route par défaut présente sur la machine :  
 - [user@vm-landing2 ~]$ ip route show
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
172.16.64.0/24 dev enp0s8 proto kernel scope link src 172.16.64.3 metric 101

Sur la machine landing-vm1, changez la carte réseau en Host-Only.
🎯 Quelle est l'utilité de ce type de carte réseau ?

ça permet de crée un réseaux priver entre machine et carte réseau 


🎰 Pingez landing-vm2 avec landing-vm1, que se passe-t-il ?
```[user@vm-landing2 ~]$ ping 172.16.64.2
PING 172.16.64.2 (172.16.64.2) 56(84) bytes of data.
64 bytes from 172.16.64.2: icmp_seq=1 ttl=64 time=0.765 ms
64 bytes from 172.16.64.2: icmp_seq=2 ttl=64 time=0.723 ms
64 bytes from 172.16.64.2: icmp_seq=3 ttl=64 time=0.812 ms
64 bytes from 172.16.64.2: icmp_seq=4 ttl=64 time=0.575 ms
64 bytes from 172.16.64.2: icmp_seq=5 ttl=64 time=0.972 ms
64 bytes from 172.16.64.2: icmp_seq=6 ttl=64 time=0.899 ms
64 bytes from 172.16.64.2: icmp_seq=7 ttl=64 time=0.798 ms
^C
--- 172.16.64.2 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 5994ms
rtt min/avg/max/mdev = 0.575/0.792/0.972/0.117 ms```
la connexion rapide 