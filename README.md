
# Um container do Docker que monitora sua rede doméstica
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

Pronto, agora é só instalar!

# Instalação Expressa

```
git clone https://github.com/dgstx/veloxpi
cd veloxpi/prometheus
docker-compose up
```

Goto [http://localhost:3030/d/velapi/veloxpi](http://localhost:3030/d/velapi/veloxpi) (change `localhost` to your docker host ip/name).

## Configuração
Para alterar em quais hosts você faz ping, altere a seção `targets` no arquivo [/prometheus/pinghosts.yaml](./prometheus/pinghosts.yaml).

Para o teste de velocidade, a única configuração relevante é a frequência com que você deseja que a verificação ocorra. É de 5 minutos por padrão, o que pode ser muito se você tiver limite de downloads. Isso é alterado editando `scrape_interval` em `speedtest` em [/prometheus/prometheus.yml](./prometheus/prometheus.yml).

Depois que as configurações estiverem concluídas, vamos iniciá-lo. No diretório do projeto /prometheus, execute o seguinte comando:

    $ docker-compose up -d

É só isso mesmo. O docker-compose vai montar todo Grafana e Prometheus automaticamente.

O Grafana Dashboard agora está acessível via: `http://<Host IP Address>:3030`

nome de usuário - administrador
senha - soma

Se tudo estiver funcionando, o dash vai estar em http://localhost:3030/d/velapi/veloxpi - se nenhum tudo ficar null, tente alterar o timeduration para algo menor.

<center><img src="images/dashboard.png" width="4600" heighth="500"></center>

## URLs interessantes

Nota: substitua `localhost` pelo ip/nome do host do docker se não estiver executando localmente.

http://localhost:9090/targets mostra o status dos alvos monitorados conforme visto do prometheus - neste caso, quais hosts estão recebendo o ping e o teste de velocidade. nota: o teste de velocidade demorará um pouco antes de aparecer como UP, pois leva cerca de 30 segundos para responder.

http://localhost:9090/graph?g0.expr=probe_http_status_code&g0.tab=1 mostra o valor prometheus para `probe_http_status_code` para cada host. Você pode editar com valores adicionais. Útil para verificar se tudo está ok no prometheus (caso o Grafana não esteja mostrando os dados que você espera).

http://localhost:9115 terminal do exportador de caixa preta. Permite que você veja o que falhou/conseguiu.

http://localhost:9696/metrics endpoint do exportador de teste de velocidade. Leva cerca de 30 segundos para mostrar seu resultado, pois executa um teste de velocidade real quando solicitado.


