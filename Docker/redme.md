tenho atualmente uma visao geral sobre o Docker, é uma arquetura que compila uma maquina virtual como se fosse um processo no sistema operacional, e dentro desse processo vai ter o ambiente perfeito
para podemos utilizar um ambiente de desemvolvimento com todas as ferramentas ja instaladas.

a grande questao é interagir com esse ambiente, se temos uma aplicação em nossa maquina local temos que acessar a imagem desse outro ambiente por codigo, e o estudo para o docker esta focado para essa parte.

intalei o docker em minha maquina pelo site:
https://hub.docker.com/

docker pull mcr.microsoft.com/mssql/server:2012 #primeiro baixei a imagem sqlserver com o seguinte comando:
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=SuaSenhaAqui" -p 1433:1433 --name sqlserver -d mcr.microsoft.com/mssql/server #depois criei o conteiner usando a imagem:
docker ps -a # para ver se o status do conteiner
docker start nome_do_conteiner #inicia o conteiner docker criado
docker logs nome_do_contêiner # verifica logs do conteiners
docker cp C:\Users\gabriel.santana\Documents\Parametrizacao.bak sqlserver:/tmp/Parametrizacao.bak # nessa primeira aplicação estou subindo um db sqlserver em um ambiente docker para recuperar uma tabela esse comando faz eu copiar o arquivo bak pra dentro do conteiner docker
dentro do terminal docker vou usar o codigo abaixo para restaurar um DB com o arquivo bak ja transferido para o conteiner: #tive dificuldades de colocar o diretorio correto para subir o backup, mas é como esta em baixo.
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'evt!123456' -Q "RESTORE DATABASE Parametrizacao FROM DISK='/tmp/Parametrizacao.bak' WITH MOVE 'Parametrizacao' TO '/var/opt/mssql/data/Parametrizacao.mdf', MOVE 'Parametrizacao_log' TO '/var/opt/mssql/data/Parametrizacao_log.ldf'"
eu usei o sql server studio para transferir a tabela do banco de dados importado no docker para o db em produção, com isso foi criado uma nova tabela no banco em produção e essa tabela vai ser 
comparada com a tabela igual que esta em produção. agora vou ter que subistituir uma das colunas do banco em produção que teve os valores alterados indevidamente para ficaram com os mesmos valores da tabela
do db docker pego do backup



