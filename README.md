![Página inicial do Fortnite API em execução](documentation/screenshot.png)

# Fortnite API — aplicação web

Aplicação web acadêmica com temática de Fortnite, desenvolvida em React. Reúne páginas públicas, cadastro e login via Firebase, cadastros de personagens, recados e contatos, além de uma consulta externa de escolas por notas do ENEM. Apesar do nome, este repositório contém um frontend, não uma API própria de Fortnite.

[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
[![License MIT](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

## 📋 Table of contents

- [✨ Features](#features)
- [🧪 Technologies](#technologies)
- [🛠️ Environment setup](#environment-setup)
- [🚀 Getting started](#getting-started)
- [🗂️ Project structure](#project-structure)
- [✅ Testing and quality](#testing-and-quality)
- [🔎 Troubleshooting](#troubleshooting)
- [🤝 Contributing](#contributing)
- [📜 License](#license)

<a id="features"></a>

## ✨ Features

- Página inicial e navegação com `HashRouter`.
- Cadastro e login de usuários por e-mail/senha no Firebase Authentication.
- Cadastro, consulta e exclusão de personagens por usuário, e listagem pública agregada.
- Formulário de contato e consulta de mensagens armazenadas no Realtime Database.
- Cadastro e consulta de recados.
- Consulta de escolas de Passo Fundo com nota ENEM mínima de 550, por serviço externo.

As guardas de navegação usam estado de sessão no navegador; a autorização de dados depende das regras do Firebase. Os fluxos com dados e autenticação não foram validados nesta entrega.

<a id="technologies"></a>

## 🧪 Technologies

| Tecnologia | Versão declarada | Finalidade |
| --- | --- | --- |
| [React](https://react.dev/) | `^16.12.0` | Interface |
| [Create React App](https://create-react-app.dev/) | `react-scripts` `3.2.0` | Desenvolvimento, build e testes |
| [React Router](https://v5.reactrouter.com/) | `^5.0.1` | Rotas com hash |
| [Firebase](https://firebase.google.com/docs) | `^7.2.1` | Authentication e Realtime Database |
| [Bootstrap](https://getbootstrap.com/docs/4.3/) | `^4.3.1` | Estilos |
| [Axios](https://axios-http.com/) | `^0.19.0` | Consulta externa |

As faixas são as declaradas no manifesto. O projeto não fixa Node.js; Node.js 14.21.3/npm 6.14.18 foram escolhidos para reproduzir CRA 3 sem a divergência OpenSSL dos runtimes recentes.

<a id="environment-setup"></a>

## 🛠️ Environment setup

Requer Git, Node.js e npm. O projeto não fixa Node.js em `engines`, `.nvmrc` ou `.node-version`. **Node.js 14.21.3 com npm 6.14.18** foi escolhido para a reprodução da pilha histórica; não é uma versão declarada pelo repositório nem uma atualização de dependências.

```bash
nvm install 14.21.3
nvm use 14.21.3
git clone https://github.com/paulodaluz/FortniteApi.git
cd FortniteApi
npm install
```

Não há lockfile versionado: use `npm install`, não `npm ci`. Este guia adota npm, disponível com o Node.js, sem prometer uma resolução de dependências idêntica entre instalações. Não há Husky, hooks versionados ou script `prepare` para ativar.

### Firebase and external services

O objeto de configuração Firebase está em `src/services/CrudContact.js`, onde ocorre `firebase.initializeApp`. Use a configuração Web de um projeto próprio, habilite login por e-mail/senha no **Authentication** e prepare o **Realtime Database**. As árvores acessadas são `/personagens/{uid}`, `/recados` e `/contatoMessages`. As regras de dados precisam corresponder aos acessos pretendidos; não torne o banco público para contornar falhas.

`public/index.html` carrega uma folha Bootstrap 4.3.1 por CDN, além dos arquivos locais do template. O código não lê configuração Firebase de `.env`. A integração ENEM de `src/services/getEnemResults.js` usa o proxy CORS Anywhere e a API de educação; sua disponibilidade não é garantida nem foi validada. Não há backend local a iniciar.

<a id="getting-started"></a>

## 🚀 Getting started

```bash
npm start
```

O Create React App usa `http://localhost:3000` por padrão; o roteamento usa URLs como `http://localhost:3000/#/login`. `PORT=4301 BROWSER=none npm start` seleciona uma porta temporária em shell POSIX e evita abrir o navegador automaticamente. Isso não altera a porta padrão do projeto.

Para gerar arquivos estáticos:

```bash
npm run build
```

A saída fica em `build/`; o repositório não define script `start:prod`. Sirva essa pasta em uma hospedagem estática configurada por você. As rotas declaradas incluem `/`, `/login`, `/register`, `/contact`, `/receivedMessages`, `/changeCharacter`, `/charcacterList`, `/changeErrands`, `/errands` e `/acessDenied`; as grafias refletem o código.

Validação com Node.js 14.21.3/npm 6.14.18: a instalação foi concluída após uma tentativa inicial interrompida por tempo limite. `CI=false npm run build` gerou `build/` com avisos existentes; `CI=true npm run build` falhou porque trata esses avisos como erros. O teste de montagem passou. O servidor de desenvolvimento iniciou na porta temporária 4301, compilou com avisos e respondeu HTTP 200. A captura real mostra a página inicial compilada, servida localmente. Nenhum login foi enviado nem foram acessados dados do Firebase; os processos de validação foram encerrados.

<a id="project-structure"></a>

## 🗂️ Project structure

| Caminho | Responsabilidade |
| --- | --- |
| `src/App.js` | Rotas e guardas de navegação |
| `src/pages/` | Telas públicas e administrativas |
| `src/components/Moleculas/` | Cabeçalho e rodapé |
| `src/services/` | Firebase e consulta ENEM |
| `public/` | HTML, imagens, estilos e bibliotecas do template |
| `public/woodrox-doc/` | Documentação do template visual |
| `src/App.test.js` | Teste de montagem do componente principal |

<a id="testing-and-quality"></a>

## ✅ Testing and quality

| Comando | Finalidade |
| --- | --- |
| `npm test -- --watchAll=false --runInBand` | Executar o teste existente sem observação |
| `npm run build` | Compilar e empacotar para `build/` |

Não há scripts separados de lint ou formatação. `npm run eject` altera a configuração do CRA e não é uma etapa de instalação ou validação.

<a id="troubleshooting"></a>

## 🔎 Troubleshooting

| Situação | Verificação |
| --- | --- |
| Login ou gravação falha | Confira o projeto Firebase próprio, provedor de e-mail/senha e regras do Realtime Database. |
| Consulta ENEM indisponível | A integração depende do proxy público e da API de educação; nenhum substituto foi implementado. |
| Dependências incompatíveis ao instalar | Não há lockfile; registre a resolução efetiva e avalie atualizações separadamente. |

O build com `CI=true` falha nos avisos `no-unused-vars` para `nome` e `idade` em `src/pages/Register/Register.js`. O teste passa com avisos React sobre as propriedades inline `text-transform` e `font-family` em `Home.js`. As correções permanecem fora desta entrega. A renderização inicial foi verificada; autenticação, operações de dados e consulta ENEM não foram verificadas. As chamadas aos serviços de dados ficaram bloqueadas durante a captura.

<a id="contributing"></a>

## 🤝 Contributing

Crie uma branch a partir de `master`, mantenha as instruções compatíveis com os arquivos do projeto e descreva a alteração em um pull request. Execute as verificações aplicáveis descritas acima; registre limitações de serviços externos e ambiente. Não inclua credenciais, dados pessoais, `node_modules` ou arquivos gerados na entrega.

<a id="license"></a>

## 📜 License

O código do projeto possui licença **MIT** em [LICENSE](LICENSE). Os arquivos de template, fontes e bibliotecas em `public/` mantêm seus próprios avisos e créditos; consulte também `public/woodrox-doc/`. Esses avisos não foram alterados.

---

<p align="center">Made with 💜 by <a href="https://github.com/paulodaluz">Paulo da Luz</a></p>
