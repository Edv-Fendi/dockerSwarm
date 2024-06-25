Este guia deve lhe ajudar a configurar e rodar :)

```
Índice 
    1. Inicialização do Swarm 
    2. Gerenciamento de Serviços
      Implantando Serviços (Stacks)
      Listando Serviços Ativos
      Removendo Serviços (Stacks)
      Monitorando Containers
      Acessando Containers
    3. Acessando seus Serviços
    4. Integrando WordPress e Redis para Melhor Desempenho
    5. Observações Importantes/Curiosidades
```
    
1. Inicialização do Swarm
Comece configurando o Swarm na sua aplicação Docker. Substitua "ip da sua placa de rede" pelo endereço IP correto da sua interface de rede:

```
docker swarm init --advertise-addr "ip da sua placa de rede"
```

2. Gerenciamento de Serviços
Implantando Serviços (Stacks)

```
docker stack deploy -c docker-swarm2.yml app
```

Listando Serviços Ativos

```
docker service ls
```

Removendo Serviços (Stacks)
Remova uma stack completa e todos os seus serviços associados:

```
docker stack rm app
```

Monitoando Containers
Monitore o desempenho e os recursos consumidos pelos seus containers em tempo real:

```
docker stats
```
Acessando Containers
Acesse um container para interagir diretamente com ele. Primeiro, identifique seu ID (por exemplo, usando docker stats) e então execute:

```
docker exec -it {id_do_container} bash
```

3. Acessando seus Serviços
Após a implantação, seus serviços estarão acessíveis nos seguintes endereços:
```
WordPress: http://localhost:8080/
Grafana: http://localhost:3000/ (lembre-se de configurar o Prometheus)
Prometheus: http://localhost:9090/
cAdvisor: http://localhost:8081/
```
4. Integrando WordPress e Redis para Melhor Desempenho
Instale o Plugin: Adicione o plugin "Redis Object Cache" ao seu WordPress.
Acesse o Container: Entre no container do WordPress:

```
docker exec -it {id_do_container_wordpress} bash
```

Prepare o Ambiente: Atualize o sistema e instale o editor de texto nano:

```
apt-get update && apt-get install nano
```

Configure o WordPress:
Abra o arquivo wp-config.php com o nano.
Antes da linha /* That's all, stop editing! Happy publishing. */, adicione:
```
define('WP_CACHE', true);
define('WP_REDIS_HOST', 'redis');
define('WP_REDIS_PORT', 6379);
```

Salve o arquivo e ative o cache de objeto no painel do WordPress.

5. Observações Importantes
Grafana: Para visualizar métricas, configure a conexão com o Prometheus no Grafana e crie painéis com as métricas desejadas.
Segurança: Em ambientes de produção, considere medidas adicionais de segurança, como o uso de HTTPS e autenticação.
Escalabilidade: O Docker Swarm permite escalar seus serviços facilmente para atender à demanda. Explore as opções de replicação de serviços.
