# Votação DevFest Porto Alegre 2026

Ferramenta de votação cega para avaliar as propostas de conteúdo submetidas ao
[DevFest Porto Alegre 2026](https://www.devfestportoalegre.com.br/) — tema
**"Criar, proteger, escalonar: desenvolvedores e criadores na era agêntica."**

Cada avaliador vota nota (1–5) e critério de desempate (0–2) por proposta, em
ordem embaralhada e sem nenhum nome de autor exibido. Os votos são salvos em
tempo real no Firebase (Firestore), com fallback automático para o
armazenamento local do navegador caso o Firestore esteja indisponível.

## Como usar

Abra o `index.html` em qualquer navegador (ou publique via GitHub Pages —
veja abaixo). Cada pessoa digita um apelido e começa a votar. A tela de
"Resultados" mostra o ranking agregado de todos os votantes e permite baixar
um backup em CSV e em JSON.

## Publicar com GitHub Pages (opcional)

1. No repositório no GitHub, vá em **Settings → Pages**.
2. Em "Source", selecione a branch `main` e a pasta `/ (root)`.
3. Salve. Em alguns minutos o site fica disponível em
   `https://SEU-USUARIO.github.io/NOME-DO-REPO/`.

## Configuração do Firebase

O arquivo já vem com a configuração do projeto Firebase
`palestrasdevfest26` embutida no topo do `<script>`, dentro de
`index.html` (constante `firebaseConfig`). Para usar com outro projeto,
edite esses valores e a coleção `devfest2026_votes` (constante
`FIRESTORE_COLLECTION`).

Regras do Firestore usadas (Console do Firebase → Firestore Database →
Regras):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /devfest2026_votes/{docId} {
      allow read, write: if true;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

> Depois do evento, recomenda-se trocar `allow read, write: if true;` por
> `if false;` nessa coleção para fechar o banco.

## Dados

As 60 propostas (título, resumo, formato, nível, conexão com o tema, tags
geradas automaticamente por palavra-chave) estão embutidas em `index.html`
na constante `TALKS`. Nenhum nome de autor é armazenado — a planilha
original de submissões já não incluía essa informação, o que torna a
votação cega por natureza.
