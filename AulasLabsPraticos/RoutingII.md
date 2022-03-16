# Laboratório 2: Routing II / Firewall

O Laboratório 4 tem como objetivo **continuar a aprendizagem do sistema de endereçamento IP e routing e introduzir mecanismos de firewall** através de configurações em máquinas virtuais Linux (Ubuntu).

***Pré-requisito: Três máquinas virtuais - Instalação de pelo menos uma máquina virtual do SEED Project e dois clones; Ter o Laboratório 3: Routing I efetuado***

## 0. Notas Prévias

Para gerir regras de firewall em Linux utiliza-se o comando `iptables`. Este comando já foi utilizado no laboratório **Routing I** para definir regras de NAT mas não foi explorado.

Algumas opções uteis:

Para apagar todas as regras de firewall: `sudo /sbin/iptables –F`

Para apagar todas as regras de nat: `sudo /sbin/iptables -t nat –F`

## 1. Exemplo de utilização de *iptables* 

### 1.1 Testar ICMP (pings)

Em primeiro lugar devemos testar a conectividade através do comando ping a partir do **UE02** para a **UE01**:

```
$ ping 192.168.1.1
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.566 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.526 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.669 ms
64 bytes from 192.168.1.1: icmp_seq=4 ttl=64 time=0.658 ms
```
O comando `ping` utiliza o protocolo **ICMP (Internet Control Message Protocol)**, como verificado nas aulas teóricas. Para impedir que um *host* responda a *pings* deve-se cortar o protocolo ICMP. Podemos fazê-lo no **UE01** como regra de *input*, ou seja, cortando a entrada do protocolo (opção *-A INPUT*):

`$ sudo /sbin/iptables –A INPUT –p icmp –j DROP`

Após isso deve-se efetuar novamente o `ping` ao mesmo *host*. É interessante colocar `-O` no comando `ping` para devolver uma resposta negativa (quando não consegue realizar o ping):

```
$ ping -O 192.168.1.1
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
no answer yet for icmp_seq=1
no answer yet for icmp_seq=2
no answer yet for icmp_seq=3
no answer yet for icmp_seq=4
```

*Sugestão*: Também pode exprimentar no **UE02** mas nesse caso cortando o *output*, portanto fazendo *-A OUTPUT* (o -A significa *append*, isto é, acrescenta a regra no final das existentes).

Após o teste deve apagar a regra para impedir futuros problemas com testes de conectividade.

`$ sudo /sbin/iptables –D INPUT –p icmp –j DROP`

Se exprimentou a *sugestão* no **UE02** apague a regra nesse *host* também alterando o *-D INPUT* para *-D output* (o -D significa *delete*).

## 2. Utilização de *tcpdump* para capturar tráfego

### 2.1 Exemplos de captura de tráfego
Para capturar tráfego (funcionalidade mais desenvolvida no próximo laboratório) deverá usar o comando `tcpdump` no terminal. De notar que as placas de rede devem ter sido configuradas em modo *promiscuo*, como indicado anteriromente nos pré-requisitos dos laboratórios.

Os seguintes comandos *exemplo* permitem capturar o tráfego em cada uma das placas de rede anteriromente configuradas. Note-se que na **UE01** existem três placas: a primeira, `enp0s3`, que usa a rede de interligação com a **UE02** (*switch01*); a segunda, `enp0s8`, que usa a rede de interligação com a **UE03** (*switch02*); a terceira, `enp0s9`, que interliga o **UE01** à Internet.

Poderá testar os comandos (terá de gerar algum tipo de tráfego para aparecerem alguns dados, como indicado no passo **2.2**): 

Em **UE01**:

`$ sudo tcpdump -i enp0s3`

`$ sudo tcpdump -i enp0s8`

`$ sudo tcpdump -i enp0s9`

Em **UE02**:

`$ sudo tcpdump -i enp0s3`

Em **UE03**:

`$ sudo tcpdump -i enp0s3`

### Capturar pacotes ICMP (*pings*)

Vamos efetuar um `ping` da máquina **UE02** para a **UE01**

```
$ ping 192.168.1.1
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.566 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.526 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.669 ms
64 bytes from 192.168.1.1: icmp_seq=4 ttl=64 time=0.658 ms
```

Deixe o `ping`a funcionar e no **UE01** corra o tcpdump:

```
$ sudo tcpdump -i enp0s3
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes
12:44:43.255781 IP 192.168.1.2 > 192.168.1.1: ICMP echo request, id 2943, seq 1, length 64
12:44:43.255841 IP 192.168.1.1 > 192.168.1.2: ICMP echo reply, id 2943, seq 1, length 64
12:44:44.272172 IP 192.168.1.2 > 192.168.1.1: ICMP echo request, id 2943, seq 2, length 64
12:44:44.272225 IP 192.168.1.1 > 192.168.1.2: ICMP echo reply, id 2943, seq 2, length 64
12:44:45.274569 IP 192.168.1.2 > 192.168.1.1: ICMP echo request, id 2943, seq 3, length 64
12:44:45.274618 IP 192.168.1.1 > 192.168.1.2: ICMP echo reply, id 2943, seq 3, length 64
```

O output é simples e direto e indica que estão a chegar pacotes com *pedidos de echo* e são enviadas as respetivas respostas. Verifique o sentido da comunicação com o caracter ">" e o endereço de origem e destino. À frente dos endereços está descrito o protocolo (ICMP) e o tipo de ICMP (*echo request* e *echo reply*). Tal como no `ping` poderá interromper o output com *Ctrl-C*.


## 3. Testar serviço *ssh* (acesso remoto)

O serviço *ssh* permite o acesso remoto a outros sistemas. Vamos tentar aceder ao **UE01** a partir do **UE03**:

```
$ ssh seed@192.168.2.1
The authenticity of host '192.168.2.1 (192.168.2.1)' can't be established.
ECDSA key fingerprint is SHA256:p1zAio6c1bI+8HDp5xa+eKRi561aFDaPE1/xq1eYzCI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.2.1' (ECDSA) to the list of known hosts.
seed@192.168.2.1's password: 
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.8.0-36-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

1 package can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```
Como se verifica a ligação foi bem sucedida (a autenticidade do *host* só é solicitada a primeira vez que tenta fazer *ssh* para um certo *host*).

Podemos usar o `tcpdump` no **UE01** para ver o tráfego entre as máquinas (atenção ao interface que se *captura*, utiliza-se o *enp0s8* porque é o que está ligado no *switch* de ligação ao **UE03**)

`$ sudo tcpdump -i enp0s8`

Para forçar que haja *output* poderá ter que escrever algum comando na sessão de `ssh` que iniciou no **UE03** (como por exemplo *ls*).

```
$ sudo tcpdump -i enp0s8
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
13:46:43.938645 IP 192.168.2.2.35960 > 192.168.2.1.ssh: Flags [P.], seq 506794727:506794763, ack 758271861, win 579, options [nop,nop,TS val 2341144 ecr 1628711], length 36
13:46:43.939674 IP 192.168.2.1.ssh > 192.168.2.2.35960: Flags [P.], seq 1:37, ack 36, win 540, options [nop,nop,TS val 1781867 ecr 2341144], length 36
13:46:43.940729 IP 192.168.2.2.35960 > 192.168.2.1.ssh: Flags [.], ack 37, win 579, options [nop,nop,TS val 2341145 ecr 1781867], length 0
13:46:44.597821 IP 192.168.2.2.35960 > 192.168.2.1.ssh: Flags [P.], seq 36:72, ack 37, win 579, options [nop,nop,TS val 2341309 ecr 1781867], length 36
13:46:44.598234 IP 192.168.2.1.ssh > 192.168.2.2.35960: Flags [P.], seq 37:73, ack 72, win 540, options [nop,nop,TS val 1782032 ecr 2341309], length 36
13:46:44.600937 IP 192.168.2.2.35960 > 192.168.2.1.ssh: Flags [.], ack 73, win 579, options [nop,nop,TS val 2341310 ecr 1782032], length 0
```
O output poderá variar mas deverá ver as comunicações entre ambas as máquinas e a indicação do serviço *ssh*.

Para terminar a ligação *ssh* a partir do **UE03** deve-se fazer `$ exit` nessa janela.

Agora vamos cortar na firewall do **UE01** a entrada de pacotes **tcp** para a **porta 22**, ou seja, o serviço **ssh**

`sudo /sbin/iptables -A INPUT -p tcp --dport 22 -j DROP`

Após a adição desta regra o output deverá ser o seguinte (após alguns minutos de *timeout*):

```
$ ssh seed@192.168.2.1
ssh: connect to host 192.168.2.1 port 22: Connection timed out
```

Agora repita esse ultimo comando *ssh* a partir do **UE03** e coloque um *tcpdump* a correr no destinatário, ou seja, no **UE01**:

```
$ sudo tcpdump -i enp0s8 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
14:03:26.347023 IP 192.168.2.2.35964 > 192.168.2.1.ssh: Flags [S], seq 1049652504, win 29200, options [mss 1460,sackOK,TS val 2591746 ecr 0,nop,wscale 6], length 0
14:03:27.372489 IP 192.168.2.2.35964 > 192.168.2.1.ssh: Flags [S], seq 1049652504, win 29200, options [mss 1460,sackOK,TS val 2592000 ecr 0,nop,wscale 6], length 0
14:03:29.374618 IP 192.168.2.2.35964 > 192.168.2.1.ssh: Flags [S], seq 1049652504, win 29200, options [mss 1460,sackOK,TS val 2592504 ecr 0,nop,wscale 6], length 0
14:03:33.502870 IP 192.168.2.2.35964 > 192.168.2.1.ssh: Flags [S], seq 1049652504, win 29200, options [mss 1460,sackOK,TS val 2593536 ecr 0,nop,wscale 6], length 0
```
Como se verifica os pacotes *ssh* chegam a **UE01** e não há resposta de volta.

No final não esquecer de apagar a regra que nega a entrada de pacotes do serviço *ssh*

`$ sudo /sbin/iptables -D INPUT -p tcp --dport 22 -j DROP`

## 4. Testar serviço *http* (web)

Para testar o acesso Web poderemos testar o acessoo ao **UE01** a partir do **UE02** (ou **UE03** na verdade, mas para efeitos de exercicio utilizaremos o **UE02**).

Abra o *Firefox* (menu lateral) no **UE02** e coloque o endereço `http://192.168.1.1`. Deverá aceder à página do *Apache2 Ubuntu Default Page* que está alojada no **UE01** (o endereço colocado).

Feche o browser e corra o *tcpdump* no **UE01**:

`$ sudo tcpdump -i enp0s3`

Agora abra novamente o Firefox no **UE02** e aceda ao mesmo endereço. Veja o output:

```
14:29:32.239836 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [S], seq 587415982, win 29200, options [mss 1460,sackOK,TS val 3022504 ecr 0,nop,wscale 6], length 0
14:29:32.239908 IP 192.168.1.1.http > 192.168.1.2.57674: Flags [S.], seq 596709831, ack 587415983, win 28960, options [mss 1460,sackOK,TS val 2423942 ecr 3022504,nop,wscale 6], length 0
14:29:32.240748 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [.], ack 1, win 457, options [nop,nop,TS val 3022504 ecr 2423942], length 0
14:29:32.240814 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [P.], seq 1:318, ack 1, win 457, options [nop,nop,TS val 3022504 ecr 2423942], length 317: HTTP: GET / HTTP/1.1
14:29:32.240844 IP 192.168.1.1.http > 192.168.1.2.57674: Flags [.], ack 318, win 470, options [nop,nop,TS val 2423943 ecr 3022504], length 0
14:29:32.243368 IP 192.168.1.1.http > 192.168.1.2.57674: Flags [P.], seq 1:3526, ack 318, win 470, options [nop,nop,TS val 2423943 ecr 3022504], length 3525: HTTP: HTTP/1.1 200 OK
14:29:32.243782 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [.], ack 3526, win 567, options [nop,nop,TS val 3022505 ecr 2423943], length 0
14:29:32.283449 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [P.], seq 318:596, ack 3526, win 567, options [nop,nop,TS val 3022515 ecr 2423943], length 278: HTTP: GET /icons/ubuntu-logo.png HTTP/1.1
14:29:32.283699 IP 192.168.1.1.http > 192.168.1.2.57674: Flags [P.], seq 3526:7149, ack 596, win 486, options [nop,nop,TS val 2423953 ecr 3022515], length 3623: HTTP: HTTP/1.1 200 OK
14:29:32.284093 IP 192.168.1.2.57674 > 192.168.1.1.http: Flags [.], ack 7149, win 680, options [nop,nop,TS val 3022515 ecr 2423953], length 0
14:29:32.591509 IP 192.168.1.2.60852 > a23-47-188-17.deploy.static.akamaitechnologies.com.http: Flags [.], ack 1, win 30192, length 0
14:29:32.591555 IP 192.168.1.2.60854 > a23-47-188-17.deploy.static.akamaitechnologies.com.http: Flags [.], ack 1, win 30192, length 0
14:29:32.591734 IP a23-47-188-17.deploy.static.akamaitechnologies.com.http > 192.168.1.2.60852: Flags [.], ack 1, win 32332, length 0
14:29:32.591746 IP a23-47-188-17.deploy.static.akamaitechnologies.com.http > 192.168.1.2.60854: Flags [.], ack 1, win 32332, length 0
14:29:34.639637 IP 192.168.1.2.52382 > 82.221.107.34.bc.googleusercontent.com.http: Flags [.], ack 221, win 32160, length 0
14:29:34.640289 IP 82.221.107.34.bc.googleusercontent.com.http > 192.168.1.2.52382: Flags [.], ack 294, win 31886, length 0
``` 

Pode ver claramente o tráfego *http* entre as duas máquinas.

Exprimente a cortar o acesso *http* no **UE01** com o seguinte comando e tente aceder noovamente ao endereço.

`sudo /sbin/iptables -A INPUT -p tcp --dport 80 -j DROP`

Já não deverá conseguir. Pode tentar correr novamente o *tcpdump* no **UE01** e verificar algo semelhante ao serviço *ssh*, ou seja, tráfego a chegar mas não há resposta de volta.

Não se esqueça de apagar a regra para poder voltar a aceder:

`sudo /sbin/iptables -D INPUT -p tcp --dport 80 -j DROP`


## 5. Redirecionar tráfego

Finalmente irá ser testado o redirecionamento de tráfego através de regras na firewall. O teste será efetuado através do acesso ao serviço *ssh*.

Vamos inicialmente aceder a partir do **UE02** ao **UE03**. Uma vez que estão em redes diferentes, o tráfego passa no *gateway* das redes, ou seja, o **UE01**. Isto permite que nesse local o tráfego possa ser redirecionado.

Inicialmente deverá testar o acesso sem redirecionamento:

No **UE02**:

```
$ ssh seed@192.168.2.2
The authenticity of host '192.168.2.2 (192.168.2.2)' can't be established.
ECDSA key fingerprint is SHA256:p1zAio6c1bI+8HDp5xa+eKRi561aFDaPE1/xq1eYzCI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.2.2' (ECDSA) to the list of known hosts.
seed@192.168.2.2's password: 
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.8.0-36-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

1 package can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

[03/15/22]seed@VM:~$ ifconfig enp0s3
enp0s3    Link encap:Ethernet  HWaddr 08:00:27:52:ed:ad  
          inet addr: 192.168.2.2  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe52:edad/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16557 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12730 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:12266493 (12.2 MB)  TX bytes:1321147 (1.3 MB)

```

Correu-se o comando `ifconfig enp0s3` dentro da sessão *ssh* para verificar o endereço IP da máquina de destino. Verificou-se que a ligação estava mesmoo estabelecida ao **192.168.2.2**.

Agora coloque-se a regra de redirecionamento de tráfego no **UE01**:

`$ sudo /sbin/iptables -t nat -A PREROUTING --dst 192.168.2.2 -p tcp --dport 22 -j DNAT --to-destination 192.168.1.1`

Fazendo novamente o acesso *ssh* ao mesmo endereço, executa-se novamente o `ipconfig` dentro da sessão *ssh*:

```
$ ssh seed@192.168.2.2
seed@192.168.2.2's password: 
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.8.0-36-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

1 package can be updated.
0 updates are security updates.

Last login: Tue Mar 15 15:00:13 2022 from 192.168.1.2
[03/15/22]seed@VM:~$ ifconfig enp0s3
enp0s3    Link encap:Ethernet  HWaddr 08:00:27:28:47:74  
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe28:4774/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:14160 errors:0 dropped:0 overruns:0 frame:0
          TX packets:19458 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1558882 (1.5 MB)  TX bytes:20934815 (20.9 MB)
```

Verifica-se que o tráfego foi redirecionado para o endereço **192.168.1.1**, como configurado na regra no **UE01**.

Não se esqueça de apagar a regra para poder voltar a aceder via ssh à máquina adequada:

`$ sudo /sbin/iptables -t nat -D PREROUTING --dst 192.168.2.2 -p tcp --dport 22 -j DNAT --to-destination 192.168.1.1`



