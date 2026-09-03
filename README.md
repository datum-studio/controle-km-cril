# Gestão de Frota · CRIL

PWA para gestão do veículo da Central de Regulação Interestadual de Leitos (Polo Track, placa SWA 7G41) — controle de quilometragem, abastecimento e checklist diário, substituindo a planilha impressa.

## Ajustes mais recentes

- **Checklist reformulado para seguir a determinação oficial da FESFSUS**: agora é diário (não mais semanal), com os 26 itens exatos do documento oficial (pneus, faróis, freios, direção, extintor, estepe, macaco, avarias na lataria, etc.), cada um marcado como **OK / Não / N/A** — sem mais os termos mecânicos específicos que tínhamos antes
- Formulário do motorista agora tem dois campos gerais no final — **Observações/Irregularidades** e **Providências adotadas** — em vez de uma observação por item
- **Novo PDF de checklist**: em vez de um relatório histórico por período, agora cada checklist enviado tem seu próprio botão **"Imprimir"** na aba Manutenção, gerando uma ficha de 2 páginas que replica exatamente o padrão visual da FESFSUS (logo, rodapé, campos, tabela de checkboxes, campo de assinatura do motorista) — já preenchida com os dados digitados no sistema, para o motorista só assinar no papel
- Aviso do dashboard atualizado: agora sinaliza itens marcados como **"Não"** (antes chamados de "defeito")
- PWA confirmado funcionando no celular; linhas por página dos relatórios aumentadas (20 registros / 24 combustível) já que a causa raiz das páginas em branco foi corrigida

## O que já está pronto (v1)

- Login com dois perfis: **motorista** (registra viagens) e **admin** (edita, vê dashboard, gera impressão)
- Motorista escolhe primeiro o **tipo de deslocamento**; só depois aparece o campo de nome do usuário (que fica oculto quando o tipo é "outro", já que nesse caso não é necessário)
- **Múltiplas paradas por viagem**: enquanto a viagem está aberta, o motorista registra cada parada (local + KM ao chegar), quantas forem necessárias. Em visitas hospitalares, o local é escolhido numa lista de hospitais de Juazeiro-BA e Petrolina-PE (sempre com opção de digitar outro local manualmente)
- **Retorno à CRIL obrigatório em todos os tipos**: pra finalizar qualquer viagem (visita hospitalar, viagem ou outro), o motorista precisa confirmar a chegada na CRIL informando a KM, ou explicitamente confirmar que está finalizando sem retornar (com um alerta de confirmação)
- **KM não pode ficar igual**: cada parada (incluindo a chegada na CRIL) exige uma KM maior que a última registrada — se o motorista esquecer de olhar o painel e digitar o mesmo número de antes, o app bloqueia e avisa
- Trava de uma viagem aberta por vez
- **Abastecimento avulso**: pode ser registrado a qualquer momento, mesmo com uma viagem em aberto (fica vinculado a ela se houver). Registra litros e valor por litro, calcula o valor total gasto e a autonomia (km/l) desde o último abastecimento
- Motorista vê a KM atual do veículo e a KM do último abastecimento na tela
- Admin: dashboard com KM total, viagens registradas, paradas registradas e gasto com combustível; aba **Registros** com KM inicial/final editável, edição completa das paradas (local, KM, horário) e do nome do usuário de cada viagem; aba **Combustível** com litros, gasto total, autonomia média e tabela detalhada
- Geração de página de impressão em **paisagem**, dividida em páginas de 20 linhas cada, com numeração de página confiável no rodapé de cada página (calculada e escrita diretamente pelo sistema, não depende do navegador). Cada parada vira sua própria linha, com data/hora/KM daquele trecho específico; a coluna Itinerário mostra o tipo de deslocamento antes do local, separado por traço (ex.: "VISITA HOSPITALAR - HOSPITAL REGIONAL DE JUAZEIRO (KATIA)"); em viagens, o destino (município) também aparece nessa coluna (ex.: "VIAGEM - PAULO AFONSO - HOTEL X (KATIA)"); a parada de chegada na CRIL aparece como "RETORNO À CRIL", sem repetir tipo/destino; sem litros/valor de combustível, que fica só no painel admin
- **Se o motorista esquecer de registrar algo**: (1) o motorista pode escolher manualmente o horário de cada parada, não fica preso ao "agora"; (2) o admin pode adicionar, corrigir ou remover paradas de qualquer viagem já registrada, na aba Registros; (3) o painel do admin mostra um aviso quando existe uma viagem em aberto há mais de 12h, sinal de que o motorista pode ter esquecido de finalizar
- **KM do cartão de abastecimento**: existe um deslocamento fixo (resto de KM de um carro anterior) que se soma à KM real do carro para dar o número que o motorista informa ao posto — mas só quando o **cartão Coringa** é usado. No abastecimento, o motorista escolhe entre cartão **Normal** (informa a KM real do carro, sem soma nenhuma) ou **Coringa** (informa a KM real + deslocamento). O admin configura esse deslocamento na aba Combustível; o motorista só vê o resultado já somado, em destaque, e apenas quando o Coringa está selecionado
- **Relatório de abastecimentos em PDF**: na aba Combustível, o admin pode gerar um relatório (via impressão do navegador, "salvar como PDF") com data, KM, litros, valor/litro, total, autonomia e qual cartão foi usado em cada abastecimento do período — sem vincular à viagem. Termina com um resumo de totais e um campo de assinatura da Gestão
- **Checklist diário do carro (padrão oficial FESFSUS)**: todo dia, o motorista faz a vistoria com os 26 itens do documento oficial da FESFSUS — pneus, faróis, lanternas, setas, luzes de freio/ré, limpador/lavador do para-brisa, retrovisores, vidros, para-brisa, freios, direção, painel, nível de combustível, documento do veículo, extintor, estepe, macaco, chave de roda, triângulo, limpeza interna/externa e avarias na lataria. Cada item é marcado **OK / Não / N/A** — nenhum vem pré-marcado, o motorista precisa marcar todos ou o envio é bloqueado. No final, dois campos gerais: **Observações/Irregularidades** e **Providências adotadas**. A tela inicial do motorista mostra um aviso se o checklist de hoje ainda não foi feito, e a data do último enviado
- **Controle de troca de óleo**: separado do checklist. Motorista registra quando trocou o óleo (KM); o admin configura os intervalos (por KM rodado e por meses — vale o que vencer primeiro). O sistema avisa tanto o motorista (tela inicial) quanto o admin (aba Manutenção) quando a troca está vencida
- **Aba Manutenção (admin)**: mostra o status da troca de óleo (última troca, próxima prevista, se está vencida), as irregularidades (itens marcados "Não") do último checklist, e o histórico completo de checklists enviados (data, horário, motorista, itens marcados "Não", botão de imprimir a ficha) — assim a gestão sempre consegue ver se o motorista sinalizou algum problema
- Estrutura de PWA (manifest + service worker) pronta para "instalar" no celular/tablet do motorista, com ícones reais da CRIL em `icon-192.png` e `icon-512.png`, na raiz do projeto (arquivos de verdade, não mais em base64 — mais confiável para o Android reconhecer como instalável)
- Em visita hospitalar, a lista de nomes de usuário é restrita a Eliete Castro, Katia Kalene e Maria Grasiela; em viagem, continua com a lista completa, e permite **selecionar mais de um usuário** (Ctrl/Cmd+clique para selecionar vários), já que uma viagem longa pode levar mais de uma pessoa

**Fora do escopo desta v1** (combinado com a Débora): OCR de leitura da KM (testado e removido — não funcionou bem), GPS, assinatura digital no app — ficam para uma v2, ou não avançam mais.

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

### 4. Definir o perfil (e o nome) de cada usuário no Firestore
Crie manualmente a coleção `usuarios` com um documento por usuário, usando o **UID** de cada um (copiado da tela de Authentication) como ID do documento. Além do `perfil`, cadastre também o campo `nome` — é ele que aparece na saudação da tela inicial e identifica quem registrou cada viagem/abastecimento/checklist nos relatórios (em vez do e-mail). Isso já deixa o sistema pronto para quando houver mais de um motorista: cada um com seu próprio login e seu próprio nome:

```
usuarios/{uid-do-motorista}
  perfil: "motorista"
  nome: "NOME DO MOTORISTA"

usuarios/{uid-do-admin}
  perfil: "admin"
  nome: "NOME DO ADMIN"
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
Mesmo padrão dos outros projetos Datum Studio: subir esta pasta para um repositório e ativar o GitHub Pages apontando para a branch/pasta raiz. **Importante**: os arquivos `icon-192.png` e `icon-512.png` precisam subir junto, soltos na raiz do repositório (não dentro de nenhuma subpasta) — sem eles, o Android não reconhece o app como instalável.

### 8. Se o PWA não aparecer como instalável no Android
- **Teste direto e definitivo**: abra `https://datum-studio.github.io/controle-km-cril/icon-192.png` direto no navegador. Se aparecer a imagem do ícone, o arquivo está publicado corretamente; se der erro 404, ele não está na raiz do repositório
- No Chrome do celular, abra o menu (⋮) → deve aparecer "Instalar aplicativo" ou "Adicionar à tela inicial" mesmo sem o banner automático aparecer sozinho. No computador, o ícone de instalação normalmente aparece dentro da barra de endereço, à direita
- Se quiser conferir o que está travando, no computador abra a URL publicada no Chrome, aperte F12 → aba **Application** → **Manifest**: ali aparece qualquer erro de ícone ou configuração
- Depois de qualquer atualização de arquivo (incluindo o `sw.js`), é bom fechar e reabrir a aba (ou limpar o cache do site) antes de tentar instalar de novo, já que o service worker guarda uma cópia antiga em cache

## Estrutura de dados (Firestore)

```
veiculos/polo-track
  kmAtual: number
  kmUltimoAbastecimento: number
  deslocamentoCartao: number (deslocamento fixo somado à KM real no abastecimento, só o admin edita)
  kmUltimaTrocaOleo: number
  dataUltimaTrocaOleo: timestamp
  intervaloKmOleo: number (padrão 10000, só o admin edita)
  intervaloMesesOleo: number (padrão 6, só o admin edita)

registros/{id}
  status: "aberto" | "fechado"
  tipo: "visita_hospitalar" | "viagem" | "outro"
  nomeUsuario: string (CAIXA ALTA — um nome por viagem inteira, lista pré-cadastrada + digitação livre)
  destino: string (CAIXA ALTA — só para tipo "viagem"; município de destino, lista pré-cadastrada + digitação livre)
  paradas: array de { local: string (CAIXA ALTA), km: number, hora: Date }
  kmInicial: number
  kmFinal: number | null (KM da última parada, no momento de finalizar)
  kmRodado: number | null
  horaInicial: timestamp
  horaFinal: timestamp | null
  registradoPor: string (nome do usuário logado, com e-mail como reserva se o nome não estiver cadastrado)
  editadoPor: string | null
  editadoEm: timestamp | null

abastecimentos/{id}
  data: timestamp
  km: number
  litros: number
  valorLitro: number
  valorTotal: number (litros × valorLitro)
  kmRodadoDesdeUltimo: number | null
  autonomia: number | null (km/l desde o abastecimento anterior)
  cartao: "NORMAL" | "CORINGA"
  viagemId: string | null (registro de viagem em aberto no momento, se houver)
  registradoPor: string (nome do usuário logado, com e-mail como reserva se o nome não estiver cadastrado)

usuarios/{uid}
  perfil: "motorista" | "admin"
  nome: string (usado na saudação e para identificar quem registrou algo nos relatórios)

checklists/{id}
  dataStr: string ("YYYY-MM-DD" do dia da vistoria, usado para saber se já foi feito hoje)
  data: timestamp (momento do envio)
  motorista: string (nome de quem fez a vistoria, aparece na ficha impressa)
  registradoPor: string (nome do usuário logado, com e-mail como reserva se o nome não estiver cadastrado)
  itens: array de { item: string, status: "OK" | "NAO" | "NA" } — os 26 itens fixos do padrão FESFSUS
  observacoes: string (Observações/Irregularidades gerais do checklist)
  providencias: string (Providências adotadas, quando houver)
  assinaturaMotorista: string (imagem da assinatura desenhada no celular, em base64 — obrigatória para enviar)
```

Observações:
- Uma viagem pode ter quantas paradas forem necessárias (ex.: visitar 3 hospitais diferentes, ou parar no hotel + reunião + hospital numa mesma viagem). Cada parada registra local e KM.
- Em visita hospitalar e outro, finalizar exige confirmar a chegada na CRIL (com KM) ou confirmar explicitamente que não retornou. Em viagem, finaliza direto com a KM da última parada.
- "abastecimento" é uma ação independente — disponível a qualquer momento, inclusive durante uma viagem em aberto.
- Todo abastecimento gera automaticamente um **registro de deslocamento fechado** (na coleção `registros`), com KM inicial = última KM conhecida do carro e KM final = KM do abastecimento, itinerário "Outro - Abastecimento", sem valores de combustível — assim ele aparece na planilha/relatório de KM, já que toda movimentação do carro precisa constar ali. Isso vale mesmo quando já existe uma viagem em aberto (a política é registrar todo deslocamento, mesmo que se sobreponha a uma viagem em andamento). O abastecimento guarda o ID desse registro em `registroDeslocamentoId`, permitindo excluir os dois juntos pela aba Combustível (botão "Excluir" na tabela). Abastecimentos criados antes dessa vinculação existir não têm esse campo — excluí-los remove só o abastecimento, e o registro de deslocamento correspondente (se houver) precisa ser apagado manualmente na aba Registros.
- **Abastecimento retroativo (aba Configurações, admin)**: pra corrigir ou lançar um abastecimento de data passada, sem precisar mexer direto no Firestore Console. Cria de uma vez o documento em `abastecimentos` e o registro de deslocamento correspondente em `registros` (mesma lógica do abastecimento normal, mas com KM inicial informada manualmente em vez de puxada automaticamente). Não atualiza a KM atual do veículo, já que é um lançamento do passado.
- As listas de hospitais (`HOSPITAIS`) e usuários (`USUARIOS`) sugeridos ficam no início do `<script>` do `index.html` — para adicionar/editar nomes, é só editar esses dois arrays diretamente no código.

## Próximos passos sugeridos (v2)
- OCR de leitura da KM via foto (Tesseract.js ou Google Cloud Vision) — já testado uma vez e removido por não funcionar bem; pode valer revisitar com outra abordagem
