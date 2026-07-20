# Controle de KM · CRIL

PWA para controle de quilometragem e abastecimento do veículo da Central de Regulação Interestadual de Leitos (Polo Track, placa SWA 7G41), substituindo a planilha impressa.

## O que já está pronto (v1)

- Login com dois perfis: **motorista** (registra viagens) e **admin** (edita, vê dashboard, gera impressão)
- Motorista: iniciar/finalizar viagem, com data/hora automáticas e KM inicial puxada do último registro
- Trava de uma viagem aberta por vez
- Admin: dashboard com KM total, viagens por tipo; tabela de registros editável por mês/ano
- Geração de página de impressão no layout da planilha original, para colher assinatura física
- Estrutura de PWA (manifest + service worker) pronta para "instalar" no celular/tablet do motorista

**Fora do escopo desta v1** (combinado com a Débora): OCR de leitura da KM, GPS, assinatura digital no app — ficam para uma v2.

## Passo a passo antes do primeiro deploy

### 1. Criar o projeto no Firebase
1. Acesse [console.firebase.google.com](https://console.firebase.google.com) → **Criar projeto**
2. Ative **Authentication** → método **E-mail/senha**
3. Ative **Firestore Database** → modo produção

### 2. Pegar as credenciais do projeto
No Firebase Console → ⚙️ **Configurações do projeto** → seção "Seus apps" → criar um app Web (`</>`).
Copie o objeto `firebaseConfig` e cole em `index.html`, substituindo o bloco:

```js
const firebaseConfig = {
  apiKey: "SUBSTITUIR",
  authDomain: "SUBSTITUIR.firebaseapp.com",
  projectId: "SUBSTITUIR",
  storageBucket: "SUBSTITUIR.appspot.com",
  messagingSenderId: "SUBSTITUIR",
  appId: "SUBSTITUIR"
};
```

### 3. Criar os usuários (motorista e admin)
No Firebase Console → Authentication → **Adicionar usuário**, crie:
- Um usuário para o motorista (ex.: `motorista@cril.local` + um PIN numérico como senha)
- Um usuário para o admin (seu e-mail + senha)

### 4. Definir o perfil de cada usuário no Firestore
Crie manualmente a coleção `usuarios` com um documento por usuário, usando o **UID** de cada um (copiado da tela de Authentication) como ID do documento:

```
usuarios/{uid-do-motorista}
  perfil: "motorista"

usuarios/{uid-do-admin}
  perfil: "admin"
```

### 5. Criar o documento inicial do veículo
Crie a coleção `veiculos` com o documento:

```
veiculos/polo-track
  kmAtual: <coloque aqui a KM atual real do carro>
```

Esse número é o ponto de partida — a partir daí o app assume o controle sozinho.

### 6. Regras de segurança do Firestore
Regra mínima recomendada (ajustar depois se necessário):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 7. Deploy no GitHub Pages
Mesmo padrão dos outros projetos Datum Studio: subir esta pasta para um repositório e ativar o GitHub Pages apontando para a branch/pasta raiz.

## Estrutura de dados (Firestore)

```
veiculos/polo-track
  kmAtual: number

registros/{id}
  status: "aberto" | "fechado"
  tipo: "visita_hospitalar" | "viagem" | "abastecimento" | "outro"
  itinerario: string
  kmInicial: number
  kmFinal: number | null
  kmRodado: number | null
  horaInicial: timestamp
  horaFinal: timestamp | null
  registradoPor: string (e-mail)
  editadoPor: string | null
  editadoEm: timestamp | null

usuarios/{uid}
  perfil: "motorista" | "admin"
```

## Próximos passos sugeridos (v2)
- OCR de leitura da KM via foto (Tesseract.js ou Google Cloud Vision)
- Ícones reais do PWA (`icons/icon-192.png`, `icons/icon-512.png` — hoje ausentes)
