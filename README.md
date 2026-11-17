# Magister Hub

Plataforma educacional para professores e alunos, com ferramentas de ensino, avaliações, atividades e um módulo de aprendizagem de inglês (**English Master**).

🌐 **Produção:** https://magisterhub.web.app

## Funcionalidades

**Para alunos**
- Login com Google e cadastro de perfil (sujeito à aprovação do professor)
- **English Master**: módulo gamificado de inglês (question words, verb to be, numerais, vocabulário de tecnologia etc.) com XP, níveis, sequência diária e ranking
- Provas por turma: múltipla escolha, dissertativas e anexo ZIP/RAR, com cronômetro
- Atividades com prazo de entrega e envio de arquivo ZIP/RAR
- Referências de estudo da própria turma
- Vitrine de projetos dos alunos e publicações da comunidade da turma

**Para o professor**
- Gestão de turmas e matérias
- Criação, ativação e arquivamento de provas; consulta de resultados e dashboard de desempenho
- Atividades: criar, editar, ativar/desativar, arquivar, desarquivar e excluir permanentemente (inclui as entregas)
- Publicação de referências de estudo e aprovação de alunos

O conteúdo é isolado por turma, tanto na interface quanto nas regras do Firestore.

## Tecnologias

- JavaScript (ES modules) com [Vite](https://vite.dev)
- Firebase: Authentication (Google), Cloud Firestore e Hosting
- Testes com o runner nativo do Node.js

## Estrutura

```
main.js              SPA: rotas, estado e renderização das telas
style.css            Estilos e temas
src/services/        Regras de negócio e validações (com testes *.test.js)
src/data/            Conteúdo dos módulos de inglês
firestore.rules      Regras de segurança do Firestore
firebase.json        Configuração do Hosting (site magisterhub)
ARCHITECTURE.md      Detalhes de rotas, coleções e modelos de dados
```

## Como rodar localmente

Pré-requisito: Node.js 20 ou superior.

```bash
npm install
npm run dev       # servidor de desenvolvimento
npm test          # testes dos serviços
npm run build     # build de produção em dist/
npm run preview   # pré-visualiza o build
```

## Deploy

Todo push na branch `main` dispara o workflow [deploy.yml](.github/workflows/deploy.yml), que gera o build e publica o Hosting e as regras do Firestore no projeto Firebase `englishmaster-ea9b9`.

O workflow depende do secret `FIREBASE_SERVICE_ACCOUNT` configurado no repositório.

### Novo domínio

Domínios usados no login precisam estar em **Firebase Console → Authentication → Settings → Authorized domains**. Para usar um domínio próprio, configure-o em **Hosting → Adicionar domínio personalizado** e crie os registros DNS indicados pelo Firebase.

## Documentação

Veja [ARCHITECTURE.md](ARCHITECTURE.md) para a arquitetura detalhada, rotas, coleções do Firestore e como adicionar novos submódulos.
