
# Uma container do Docker que monitora sua rede dom�stica
Usando [Prometheus](http://prometheus.io/), Grafana com [blackbox-exporter](https://github.com/prometheus/blackbox_exporter) e [speedtest -exporter](https://github.com/stefanwalther/speedtest-exporter)

# Importante
Instalar docker:
```
wget https://get.docker.com -O get-docker.sh
sudo sh get-docker.sh
```

Adicione o user no grupo do docker:
```
sudo usermod -aG docker pi
```
Desconecte e Conecte novamente

Instalar o Docker Composer
```
sudo apt-get install -y libffi-dev libssl-dev python3-dev python3-pip git
sudo pip3 install docker-compose
````

Pronto, agora � s� instalar!

# Instala��o Expressa

```
git clone https://github.com/dgstx/veloxpi
cd veloxpi/prometheus
docker-compose up
```


## Configura��o
Para alterar em quais hosts voc� faz ping, altere a se��o `targets` no arquivo [/prometheus/pinghosts.yaml](./prometheus/pinghosts.yaml).

Para o teste de velocidade, a �nica configura��o relevante � a frequ�ncia com que voc� deseja que a verifica��o ocorra. � de 5 minutos por padr�o, o que pode ser muito se voc� tiver limite de downloads. Isso � alterado editando `scrape_interval` em `speedtest` em [/prometheus/prometheus.yml](./prometheus/prometheus.yml).

Depois que as configura��es estiverem conclu�das, vamos inici�-lo. No diret�rio do projeto /prometheus, execute o seguinte comando:

    $ docker-compose up -d

� s� isso mesmo. O docker-compose vai montar todo Grafana e Prometheus automaticamente.

O Grafana Dashboard agora est� acess�vel via: `http://<Host IP Address>:3030`

nome de usu�rio - administrador
senha - soma

Se tudo estiver funcionando, o dash vai estar em http://localhost:3030/d/velapi/veloxpi - se nenhum todos graficos ficararem null, tente alterar o timeduration para algo menor.


## URLs interessantes

Nota: substitua `localhost` pelo ip/nome do host do docker se n�o estiver executando localmente.

http://localhost:9090/targets mostra o status dos alvos monitorados conforme visto do prometheus - neste caso, quais hosts est�o recebendo o ping e o teste de velocidade. nota: o teste de velocidade demorar� um pouco antes de aparecer como UP, pois leva cerca de 30 segundos para responder.

http://localhost:9090/graph?g0.expr=probe_http_status_code&g0.tab=1 mostra o valor prometheus para `probe_http_status_code` para cada host. Voc� pode editar com valores adicionais. �til para verificar se tudo est� ok no prometheus (caso o Grafana n�o esteja mostrando os dados que voc� espera).

http://localhost:9115 terminal do exportador de caixa preta. Permite que voc� veja o que falhou/conseguiu.

http://localhost:9696/metrics endpoint do exportador de teste de velocidade. Leva cerca de 30 segundos para mostrar seu resultado, pois executa um teste de velocidade real quando solicitado.


