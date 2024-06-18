# entregaDocker
Projeto final de Docker

# Turorial

## Passo 1: Preparando o ambiente

1. Tenha o Docker e o Docker Compose instalados na sua máquina.
2. Ao decorrer do projeto certifqui-se de ter as portas que serão utilizadas disponíveis.

## Passo 2: Preparar o ambiente Docker

### 2.1 Criar Diretório do Projeto

1. Crie um diretório para o seu projeto e navegue até ele:
   ```bash
   mkdir projdocker
   cd projdocker
   ```

### 2.2 Clone o Projeto

1. Clone o projeto dentro do diretório criado anteriormente:
   ```bash
   git clone https://github.com/MedeirosMatheus/entregaDocker.git
   ```

## Passo 3: Iniciar o projeto

1. Acesse o diretório do projeto que foi clonado:
   ```bash
   cd entregaDocker 
   ```
2. Após acessar o diretório, inicie os containers e aguarde o fim do processo:
   ```bash
   docker-compose up -d
   ```
3. Após o fim do processo, verifique se os containers estão rodando:
   ```bash
   docker ps
   ```

## Passo 4: Configurar o WordPress

### 4.1 Acessar o WordPress

1. Abra o navegador e acesse `http://localhost:8000` para completar a configuração do WordPress. Siga as instruções na tela para configurar o seu site WordPress.
2. Favor fazer a instalação na língua português do Brasil.

### 4.2 Instalar o Plugin Redis Object Cache

1. No painel de administração do WordPress, vá para `Plugins > Adicionar Novo`.
2. Na barra de busca, digite  `Redis Object Cache`.
3. Encontre o plugin "Redis Object Cache" (desenvolvido por Till Krüss) e clique em `Instalar Agora`(caso de falha na instalação tente novamente).
4. Após a instalação, clique em ativar.

### 4.3 Editar o Arquivo `wp-config.php`

#### Acessar o Arquivo `wp-config.php`

1. Acesse o contêiner do WordPress:
    ```bash
    docker-compose exec wordpress bash
    ```

2. Navegue até o diretório do WordPress:
    ```bash
    cd /var/www/html
    ```
3. Instale o editor de código nano:
    ```bash
    apt-get update
    apt-get install nano
    ```

4. Edite o arquivo `wp-config.php` usando um editor de texto, como `nano`:
    ```bash
    nano wp-config.php
    ```

5. Adicione as seguintes linhas ao arquivo, logo acima da linha que diz `/* That's all, stop editing! Happy publishing. */`:

    ```php
    // Enable Redis Object Cache
    define('WP_CACHE', true);
    define('WP_REDIS_HOST', 'redis');
    define('WP_REDIS_PORT', 6379);
    ```

6. Salve e feche o arquivo:
   Pressione `CTRL+X`, depois `Y` e `ENTER` para salvar e sair.
   Depois de um `exit` no prompt do WSl.

### 4.4 Verificar a Configuração do Redis

1. No painel de administração do WordPress, vá para `Configurações > Redis`.
2. Na página de configurações do Redis, clique em `Ativar Cache de Objetos`.
3. Verifique se o status está como conectado, isso indica que o Redis está conectado e funcionando.

## Passo 5: Verificar a Conexão do WordPress com o MySQL

### 5.1 Verificar a Instalação do WordPress

Se você conseguiu completar a instalação inicial do WordPress e acessar o painel de administração, a conexão com o MySQL está funcionando corretamente.

### 5.2 Usar um Plugin de Administração de Banco de Dados

1. No painel de administração do WordPress, vá para `Plugins > Adicionar Plugin`.
2. Pesquise por `WP phpMyAdmin` e instale o plugin (caso de falha na instalação tente novamente).
3. Ative o plugin e vá para `Ferramentas > WP phpMyAdmin`.
4. Desmarque a opção `Restrict access only to current IP ` e clique em `salvar as alterações`.
5. Clique no botão `Enter local phpMyAdmin`.
6. Verifique se você consegue acessar e visualizar as tabelas do banco de dados.

### 5.3 Testar a Conexão Diretamente no Contêiner do WordPress

1. Acesse o contêiner do WordPress:
    ```bash
    docker-compose exec wordpress bash
    ```

2. Instale o `mysql-client` no contêiner do WordPress (se não estiver instalado):
    ```bash
    apt-get update
    apt-get install -y default-mysql-client
    ```

3. Conecte-se ao banco de dados MySQL:
    ```bash
    mysql -h db -u wpuser -ppassword wordpress
    ```

4. Execute uma consulta simples:
    ```sql
    SHOW TABLES;
    ```

    Se você ver uma lista de tabelas, a conexão está funcionando corretamente.
    Depois de um `exit` no prompt do WSl. para fechar o banco de dados.
    E mais um para sair do bash.


## Passo 6: Verificar o Prometheus

### 6.1 Acessar o Prometheus

1. Abra o navegador e acesse `http://localhost:9090` para verificar o Prometheus métricas.

### 6.2 Consultar metricas do MYSQL
Para utilizar os comandos os escreva na `Expression` e após isso presione `Execute` para ver as metricas, aqui estão alguns exemplos:
1. `mysql_global_variables_metadata_locks_cache_size`
2. `mysql_global_status_bytes_sent`
3. `mysql_global_status_connections`

### 6.3 Consultar metricas do Redis
Para utilizar os comandos os escreva na `Expression` e após isso presione `Execute` para ver as metricas, aqui estão alguns exemplos:
1. `redis_allocator_resident_bytes`
2. `redis_commands_duration_seconds_total`
3. `redis_aof_last_write_status`
   
### 6.4 Consultar metricas do Node
Para utilizar os comandos os escreva na `Expression` e após isso presione `Execute` para ver as metricas, aqui estão alguns exemplos:
1. `node_memory_MemAvailable_bytes`
2. `node_hwmon_temp_celsius`
3. `node_procs_running`

### 6.5 Visualizar os targets aplicados
Para visualizar os targets selecione no `status` (canto superior esquerdo) e selecione o campo `Targets`.

## Passo 7: Verificar o cadivisor
Para visualizar algumas métricas do container com o cadvisor abra uma nova janela no navegador `http://localhost:8080`.
Você também pode consultar métricas com o cadvisor pelo prometheus.

Fim! :)
