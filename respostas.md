# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Eduardo Alberto da Rocha  
Matrícula: 26128461  
Usuário do GitHub: odudu07  
Usuário do Docker Hub: odudu07

Responda com suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?

Imagem base: `nginx:1.27-alpine`.

Tamanho final: preencher depois do `docker build` com o valor mostrado em `docker images`.

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
conferir que o `index.html` está lá dentro.

O Nginx procura os arquivos do site em `/usr/share/nginx/html`. Para conferir, usei:
`docker exec teste-portal ls -la /usr/share/nginx/html`

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.

`odudu07/agrovale-portal:1.0-26128461`

Link: `https://hub.docker.com/r/odudu07/agrovale-portal`

4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?

Porque o token pode ser usado para autenticar no Docker Hub sem precisar informar a senha principal da conta. Ele também pode ter permissões específicas e ser revogado separadamente.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | `COPY site/ /usr/share/nginx/html/` | A página `site/index.html` não era copiada para a pasta servida pelo Nginx. | O Nginx não encontrava a página de manutenção e apresentava a página padrão. | Adicionei o `COPY` para `/usr/share/nginx/html/`. |
| 2 | `WORKDIR` | O `WORKDIR /usr/share/nginx` não define a pasta que o Nginx usa para servir o site. | Mesmo com os arquivos no projeto, eles não eram servidos pela pasta padrão do Nginx. | Removi o `WORKDIR` e copiei os arquivos diretamente para `/usr/share/nginx/html/`. |
| 3 | `EXPOSE 80` | A porta 80 do container não estava documentada no Dockerfile. | O container podia funcionar, mas o Dockerfile não documentava a porta usada pelo serviço. | Adicionei `EXPOSE 80`. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

Em `-p 7042:80`, a porta `7042` é a porta do host e a `80` é a porta do container. Em `-p 80:7042`, a porta `80` é a do host e `7042` seria a porta do container. A porta do container é sempre o número depois dos dois-pontos.

## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?

Porque `db` é o nome do serviço do MariaDB na rede criada pelo Compose. Os containers conseguem encontrar o banco pelo nome do serviço. `localhost` dentro do container do WordPress apontaria para o próprio container do WordPress, e não para o MariaDB.

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar
a porta? Mostre o comando.

Não é necessário publicar a porta porque o WordPress acessa o MariaDB pela rede interna do Compose. Para consultar o banco, posso entrar no container do MariaDB com:
`docker compose exec db mariadb -uagrovale -p agrovale_blog`

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou,
e por quê?

Para derrubar a stack usei `docker compose down` e para subir novamente usei `docker compose up -d`.

O comando `docker compose down -v` apagaria também os volumes nomeados, removendo os dados persistidos do MariaDB e do WordPress. Por isso ele poderia apagar o post criado.

10. Código de conclusão impresso pelo verificador:

```
PREENCHER APÓS EXECUTAR scripts/verificar.ps1 OU scripts/verificar.sh
```
