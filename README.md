<h1 align="center"> 
  Docker Compose + keycloack + Postgrees  
</h1>

<p align="center">
  <img src="https://github.com/EduardoNofre/keycloack/blob/master/dockerCompose.jpg" alt="Sublime's custom image"/>  
</p>

<h1 align="left">
  Sumario  
</h1>
<p align="left">
• <a href="#---docker-compose-">Docker Compose</a><br>
• <a href="#---keycloack-">keycloack</a><br>
• <a href="#--requisito-mínimo-antes-de-começar">Requisito mínimo antes de começar</a><br>   
• <a href="#--arquivo-docker-composeyml">Arquivo docker-compose.yml</a><br>  	  
• <a href="#--arquivo-init-schemasql">Arquivo init-schema.sql</a><br>  				
• <a href="#--executando-o-arquivo-docker-composeyml-">Executando o arquivo 'docker-compose.yml'</a><br>  								
• <a href="#criação-da-conta-de-administrador-no-keycloak">Criação da conta de administrador no keycloak</a><br>  	
• <a href="#criando-um-realm">Criando um realm</a><br> 						
• <a href="#----criando-o-usuário----">Criando o usuario</a><br>  								
• <a href="#----criando-um-grupo--">Criando um grupo</a><br>   	
• <a href="#----criando-as-roles--">Criando as roles</a><br>  		
• <a href="#----adicionando-um-client--">Adicionando um Client</a><br>						
• <a href="#----adicionando-um-client-scopes--">Adicionando um Client scopes</a><br>				
• <a href="#----adicionando-um-client-resource_access--">Adicionando um Client resource_access</a><br>	
• <a href="#----vamos-testar-o-usuário---criado--">Vamos testar o usuario criado</a><br>	
</p>


<h1 align="center">
   Docker Compose 
</h1>

## O que é Docker Compose?
O Docker Compose é uma ferramenta que permite definir e gerir aplicações Docker que usam vários contêineres. Ele facilita a execução de serviços interconectados, como bancos de dados, APIs e front-end.<br>

## Como funciona?
Define serviços em um arquivo YAML e os inicia com um único comando.

## O que faz?
Orquestra containers, cria redes, constrói imagens e vincula contêineres.

## Quem usa?
Profissionais de desenvolvimento, teste, staging, CI e DevOps.

<h1 align="center">
   keycloack 
</h1>

## O que é o keycloack?
Keycloak é uma solução de gerenciamento de acesso e identidade de código aberto que visa principalmente aplicativos e serviços. Os usuários podem autenticar com o Keycloak em vez de aplicativos individuais. Assim, as aplicações não precisam lidar com formulários de login, autenticando e armazenando usuários.

## Funcionalidades?
Single Sign-On (SSO), autenticação centralizada, autorização e controle de acesso.

## Gerenciamento?
Console de gerenciamento de contas, console admin para gerenciamento central de usuários

## Suporte de protocolos?
Compatível com protocolos de autenticação como OAuth 2.0, OpenID Connect e SAML.

<h1 align="center">
  Requisito mínimo antes de começar!.
</h1>

## 1 - Requisito;
Ter o docker desktop instalado na maquina.
Para instalar é facil é só ir na pagina do docker fazer download de acordo com seu **S.O** e instalar.

## 2 - Requisito. 
Observação.<br>
Para executar o seu projeto, **o docker desktop deve esta sendo execuatdo** em sua maquina. Caso contrario vai da erro.

## 3 - Requisito.
Ao executar o seu projeto pela primiera vez **vai demorar um pouco pois o docker irá fazer o pull das imagens** para a sua maquina.

## 4 - Requisito.
ter o banco de dados postgrees instalado em seu computador.

# Requisito mínimo atendido.

<h1 align="center">
  Arquivo docker-compose.yml.
</h1>

#  Arquivo docker-compose.yml
## 1 - Vamos criar o arquivo 'docker-compose.yml' como no exemplo abaixo.

                  services:
                    postgres:
                      image: postgres
                      environment:
                        POSTGRES_DB: keycloak
                        POSTGRES_USER: keycloak
                        POSTGRES_PASSWORD: keycloak
                      volumes:
                        - C:/java-estudos-2025/banco_db_postgres_docker:/var/lib/postgresql/data
                        - ./init-schema.sql:/docker-entrypoint-initdb.d/init-schema.sql
                      ports:
                       - 3333:5432
                      networks:
                        - keycloak_network
                   
                    keycloak:
                      image: quay.io/keycloak/keycloak:legacy
                      environment:
                        DB_VENDOR: POSTGRES
                        DB_ADDR: postgres
                        DB_DATABASE: keycloak
                        DB_USER: keycloak
                        DB_SCHEMA: keycloak_schema
                        DB_PASSWORD: keycloak
                        KEYCLOAK_USER: admin
                        KEYCLOAK_PASSWORD: admin
                      ports:
                        - 8080:8080
                      depends_on:
                        - postgres
                      networks:
                        - keycloak_network
                        
                  networks:
                    keycloak_network:
                      driver: bridge

# 1.1 - O Arquivo compose 'docker-compose.yml'.
##  Arquivo  'docker-compose.yml'
Compose é uma ferramenta para gerenciar múltiplos contêineres.<br>
No exemplo acima 'docker-compose.yml' podemos observar dois serviços são eles.<br>
        - postgres<br>
        - keycloak<br>
Cada serviço com as suas configuraçoes, portas, redes ... e assim por diante<br>

<h1 align="center">
  Arquivo init-schema.sql.
</h1>

#  Arquivo  'init-schema.sql'
## 2 - Vamos criar o arquivo o 'init-schema.sql' como no exemplo abaixo.

              DO $$
                BEGIN
                    IF NOT EXISTS(
                        SELECT schema_name
                        FROM information_schema.schemata
                        WHERE schema_name = 'keycloak_schema'
                    ) THEN
                        EXECUTE 'CREATE SCHEMA keycloak_schema';
                    END IF;
                END
                $$;

# 2.1 - O Arquivo compose 'init-schema.sql'.
  **SQL** é um script de criação de schema_name e nome da schema_name.<br>
  O Arquivo compose **'docker-compose.yml'** faz a chamada deste sql após a instalação do postgrees.<br>

<h1 align="center">
  Executando o arquivo 'docker-compose.yml' 
</h1>

  1 - Va ate o diretorio onde se encontra o arquivo  **'docker-compose.yml'** via promtp de comando.

  2 - No diretorio onde esta o arquivo  execute o comando **docker-compose up -d**.

  3 - Após a execução do comando isso pode levar alguns minutos.

#  • Saida:

<p align="center">
  <img src="https://github.com/EduardoNofre/keycloack/blob/master/Captura%20de%20tela%202025-04-12%20012446.png" alt="Sublime's custom image"/>  
</p>
      
 4 -  Com isso o Keycloak estará no ar e o schema e tabelas deve esta criado em seu banco de dados.<br>  
        1 - O Keycloak ira a tabelas necessaria para o seu controles e gerencimaneto.<br>
        2 - A imagem Keycloak deve esta em execução.
        3 - A imagem do postgrees também deve esta em execução.

 5 - Verificando se as imagens estão rodando execute o comando 'docker ps':

  #  • Saida:

<p align="center">
  <img src="https://github.com/EduardoNofre/keycloack/blob/master/dockerps.png" alt="Sublime's custom image"/>  
</p>
        
<h1 align="center">
Criação da conta de administrador no keycloak
</h1>

  1 - O usuário   administrador console foi criado em seu arquivo **'docker-compose.yml'** <br> 
     - usuário  : admin <br>
     - senha: admin <br>

  2 - Abra http://localhost:8080/auth
    
  3 - A página de boas-vindas é aberta, confirmando que o servidor está funcionando.Página de boas-vindas
  
  5 - E faça login no console de administração do Keycloak usando a conta de console administrador..
  
  ![ciar](criarUsua.PNG)  
 

<h1 align="center">
Criando um realm
</h1>
   
  ## O que é o realm?<br>
   R: O **realm** e como se fosse uma work space um agrupamento de recursos.<br>
   recursos:<br>
        usuário  s.<br>
        Roles.<br>
        Grupos.<br>
        Regras.<br>

         
  1 - Vá para o endereço  http://localhost:8080/auth/.

  2 - Por default a master ja vem criada.No menu Master, clique em **Add Realm**. Quando você está conectado ao domínio master, este menu lista todos os outros reinos.
   
  3 - Assim que você se logar na aplicação do lado esquerdo superior você verá o nome **Master**.
   
  4 - Ao passar o curso do mouse por cima do nome master será exibido um botão com a opção de **Add um realm**.
   
  5 - clique no botão **add realm**.    
   
  6 - No campo name iremos colocar o nome **"REALM_SISTEMA"**.
    
  7 - Após a definição do nome do seu realm clique no botão **create**.E pronto!
  

  <h1 align="center">
    Criando o usuário  .
  </h1>
  
  1 - No realm **"REALM_SISTEMA"**, você cria um novo usuário.
  
  2 - No menu, clique em usuários para abrir a página da lista de usuários.
    
  3 - No lado direito da lista de usuários vazia, clique em **Adicionar usuário** para abrir a página Adicionar usuário.
    
  4 - Insira um **nome no campo** nome de usuário e um email no **campo Email** e clique em salvar.<br>
        **nome usuário  : eduardo1**<br>
        **email: eduardo1@email**com<br>              
 
  5 - Clique na ABA**credentials** para definir uma senha para o novo usuário.
          **senha: 123123**<br>
  
  7 - Digite uma nova senha e confirme-a.
  
  8 - Clique em Definir senha para definir a senha do usuário como a nova que você especificou.


  <h1 align="center">
    Criando um grupo
  </h1>

 1 - Vá em **groups**.
 
 2 - Clique no botão **'create group'**.
 
 3 - Defina um nome para o grupo no caso **'Administrador'**.
 
 4 - URL root: nome do host do aplicativo

 5 - Vá novamente na opção **usuário  s** e selecione o usuário   **'eduardo1'** selecione a **'ABA Groups'**.

 6 - Selecione o **Groups** que você acabou de criar no caso  **'Administrador'** será feita **associação**do usuário   **'eduardo1'** ao grupo **'Administrador'** client em join.

 7  - Validar volte para a opção **'Groups'** selecione o grupo  **'Administrador'** vá na ABA **Menbers** la estará o usuário   **'eduardo1'** associado ao grupo **'Administrador'**.

  <h1 align="center">
    Criando as roles
  </h1>

 1 - Vá em **Realm roles**.
 
 2 - Clique no botão **'create role'**.
 
 3 - Defina um nome para a role no caso **'Administrador_role'**.
 
 4 - Coloque na descrição o que essa role permite fazer. 
 Exemplo: **'acesso full na aplicação' e clique no botão save**.

 5 - Volte para a opção **'Groups'** selecione a ABA **'Role mapping'** e faça a **associação com a role que você acabou** de criar no caso **'Administrador_role'**.

 6 - Selecione o groups que você acabou de criar no caso  **'Administrador'** será feita **associação** do usuário   **'eduardo1'** ao grupo **'Administrador'** client em join.
      
  <h1 align="center">
    Adicionando um Client
  </h1>

 1 - Vá em **Clients**.
 
 2 - Clique no botão **Create client**. defina um nome **client_sistema**.
 
 3 - Vamos usar o protocolo: **OpenID-Connect**
 
 4 - Clique no **botão next**.

 5 - Ative a opçao **Client authentication** e **Authorization** e deixe marcado o check box **Standard flow** e **Direct access grants**.

 6 - Volte para a opçao **Clients** e clique na ABA **settings**.<br>
   - 6.1 - Campo **Name** defina o valor **${client_account}**.<br>
   - 6.2 - Campo **Root url** defina o valor **${authBaseUrl}**<br>
   - 6.3 - Campo **Home url** <br>
     - 6.3.1 - Observação: <br>
          Padrão -> **Home url /realms/SEU-REALM/account/** <br>
          Exemplo como fica -> **/realms/REALM_SISTEMA/account/** realm que criamos logo no inicio **REALM_SISTEMA**.<br>
  - 6.4 - Campo **Valid redirect URIs** <br>
    - 6.4.1 - Observação: <br>
          Padrão -> **Valid redirect URIs /realms/SEU-REALM/account/*.** <br>
          Exemplo como fica -> **/realms/REALM_SISTEMA/account/*.** realm que criamos logo no inicio **REALM_SISTEMA**.<br>           
                

  <h1 align="center">
    Adicionando um Client scopes
  </h1>

 1 - Vá em **Client**.
 
 2 - Clique na opção  **Roles**.
 
 3 - Clique na ABA **scopes**.
 
 4 - Clique no **botão Assign**.

 5 - Nesta ABA estará todas as roles que você criou no nosso caso só **'Administrador_role'** seleciona a sua role e clique no botão **Assign**.

 6 - Volte para a **opção Client scopes** e  Clique na **opção Roles**  selecione a **ABA Mappers** na opçao **client roles** no **campo Client ID** selecione o client que você criou **client_sistema**.


   <h1 align="center">
    Adicionando um Client resource_access
  </h1>

 1 - Vá em **Client scopes**.
 
 2 - selecione na client que acabou de criar **client_sistema**.
 
 3 - Clique na ABA **roles**.

 4 - Crie roles com o nome **app-backend-admin**.

 5 - Vá em **Users > eduardo1 > Role Mappings**

 6 - Faça a **associação**  da role que acabou de ser criada **app-backend-admin** ao usuario **eduardo1** 

 7 - A role será exibida da seguinte forma exemplo: **nome do client** e **nome da role** como mostra o exemplo abaixo.
        
  <p align="center">
    <img src="https://github.com/EduardoNofre/DockerCompose_keycloack_postgrees/blob/master/resource.png" alt="Sublime's custom image"/>  
  </p>

 8 - Observção **resource_access**.

    8.1 Antes da alteração o token.

 <p align="center">
    <img src="https://github.com/EduardoNofre/DockerCompose_keycloack_postgrees/blob/master/resource_padr%C3%A3o.png" alt="Sublime's custom image"/>  
  </p>

    8.2 Após a alteração o token.

 <p align="center">
    <img src="https://github.com/EduardoNofre/DockerCompose_keycloack_postgrees/blob/master/resourceApos.png" alt="Sublime's custom image"/>  
  </p>

  PODEMOS NOTA QUE NO RESOURCE FOI ADICIONADO  O **CLIENT** CLIENT_SISTEMA E A SUA ROLE **app-backend-admin**.


  <h1 align="center">
    Vamos testar o usuário   criado.
  </h1>

 1 - Vá em **Client**.
 
 2 - Selecione o client que foi criado por nos  **client_sistema** .
 
 3 - Na linha do nosso cliente existe uma URL **http://localhost:8080/auth/realms/REALM_SISTEMA/account/**.
 
 4 - Clique na url **http://localhost:8080/auth/realms/REALM_SISTEMA/account/** logo você será redirecionado para uma tela de login de teste.

 Será exibida a seguinte tela.

 <p align="center">
  <img src="https://github.com/EduardoNofre/keycloack/blob/master/teste.png" alt="Sublime's custom image"/>  
</p>

5 - clique em **Personal info** e digite:<br>
        **usuário  : eduardo1**<br>
        **senha: 123123**<br>
Se tudo der certo você sera direcionado para a tela principal do usuário  .<br>

  <h1 align="center">
    Definir regras para as senhas
  </h1>

  1 - Vá em **Authentication**.
 
  2 - Selecione a **ABA Policies** .
 
  3 - Selecione a opção **Password policies**.
 
  4 - Na tela de **Password policies** podemos adicionar varias regras para a senha.

  5 - Depois de definir as regras de email .


# Fim


