# Be The Hero

Uma ONG cadastra um caso, descreve a ajuda de que precisa e informa o valor necessário. Quem quer ajudar encontra esses casos no aplicativo e entra em contato por WhatsApp ou e-mail.

Implementação do projeto da Semana OmniStack 11, da Rocketseat, com painel web para as ONGs, aplicativo mobile e API.

## Painel da ONG

O cadastro reúne nome, e-mail, WhatsApp, cidade e estado. Ao concluir, a API gera um ID usado para acessar o painel.

Pela web, a ONG pode:

- Entrar com seu ID.
- Consultar os próprios casos.
- Publicar um caso com título, descrição e valor.
- Excluir um caso de sua autoria.

## Aplicativo de quem ajuda

A tela inicial carrega os casos em páginas de cinco registros e busca a próxima página conforme a lista é percorrida.

Nos detalhes, o aplicativo mostra a ONG, sua cidade, o título do caso e o valor solicitado. Os botões de contato preparam uma mensagem com os dados do caso para WhatsApp ou e-mail. O apoio financeiro é combinado diretamente com a organização.

## Código e tecnologias

| Diretório | Responsabilidade | Tecnologias |
| --- | --- | --- |
| `aulas/backend/` | Cadastro de ONGs, acesso ao painel e gestão de casos | Node.js, Express, Knex e SQLite |
| `aulas/frontend/` | Login, cadastro da ONG, painel e novo caso | React 16, React Router e Axios |
| `aulas/mobile/` | Listagem, detalhes e contato com a ONG | React Native, Expo SDK 36, React Navigation e Expo Mail Composer |

As tabelas `ongs` e `incidents` são definidas nas [migrations](aulas/backend/src/database/migrations). Cada caso referencia a ONG que o publicou.

## Rotas da API

| Método | Rota | Operação |
| --- | --- | --- |
| POST | `/ongs` | Cadastrar uma ONG e retornar seu ID |
| GET | `/ongs` | Listar as ONGs |
| POST | `/sessions` | Consultar uma ONG pelo ID para acessar o painel |
| GET | `/profile` | Listar os casos da ONG identificada no cabeçalho |
| GET | `/incidents` | Listar casos com dados de contato da ONG |
| POST | `/incidents` | Publicar um caso |
| DELETE | `/incidents/:id` | Excluir um caso pertencente à ONG informada |

O cabeçalho `Authorization` recebe diretamente o ID da ONG nas operações de perfil, publicação e exclusão.

A listagem aceita `page`, começando em `1`, e devolve o total de casos no cabeçalho `X-Total-Count`.

## Para executar esta versão

Cada diretório tem suas próprias dependências. Instale-as dentro de `aulas/backend/`, `aulas/frontend/` e `aulas/mobile/`.

A configuração de desenvolvimento do Knex usa SQLite. Dentro de `aulas/backend/`, as migrations podem ser aplicadas com:

```sh
npx knex migrate:latest
```

O arquivo que inicia a API está na raiz do backend:

```sh
node index.js
```

O script `npm start` desse diretório aponta para `src/index.js`, caminho que não existe nesta versão. Por isso, o comando acima usa o ponto de entrada presente no repositório.

Para o frontend e o mobile, execute `npm start` nos respectivos diretórios. A API escuta na porta `3333`; o frontend usa `localhost`, enquanto o [cliente mobile](aulas/mobile/src/services/api.js) precisa do endereço da máquina acessível pelo aparelho.

Esta documentação descreve o código da edição original. A instalação, o banco e o aplicativo não foram executados nesta revisão; a compatibilidade das dependências com ambientes atuais não foi validada.
