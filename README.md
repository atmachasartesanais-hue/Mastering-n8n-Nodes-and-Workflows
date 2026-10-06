# Mastering-n8n-Nodes-and-Workflows
Objetivo de estudo, tema:
Conhecendo principais nodes e automação com n8n - o objetivo é aprender a utilizar os principais nodes e como integrar 
eles no workflow dentro do N8N


Principais fontes inseridas:

https://www.youtube.com/watch?v=AQgAB0sGEAE
https://www.youtube.com/watch?v=uOx_xQALHRA
https://www.youtube.com/watch?v=ituvsQdBDo8
https://www.youtube.com/watch?v=68ETRmlCUR4
https://www.youtube.com/watch?v=D9MIGseFB3g
https://www.youtube.com/watch?v=GgMEkB2Wl0k

https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.n8n
https://docs.n8n.io/integrations/builtin/node-types
https://www.npmjs.com/package/n8n-nodes-evolution-api
https://community.n8n.io/t/master-list-of-every-n8n-node/155146

As fontes pesquisadas vieram de canais do youtube com grande engajamento e bons conteudos e as 
fontes de artigos vieram do proprio repositório de documentos do N8N


A diretriz de comportamento;
utilizei o seguinte prompt para inicio de atuação dos estudos:
comporte-se como um especialista em n8n e me informe os principais nodes utilizados para 
implementação de automação de whatsapp utilizados em clinicas de odontologia, nutrição e psicologia, como 
forma de agendamento de consultas


As perguntas que você fez, o que ele respondeu e de quais fontes;
me informe os principais nodes utilizados para implementação de automação de 
whatsapp utilizados em clinicas de odontologia, nutrição e psicologia, como forma de agendamento de consultas

resposta: Para estruturar uma automação robusta de agendamento de consultas via WhatsApp em clínicas de **Odontologia, Nutrição e Psicologia**, o n8n utiliza um conjunto de nodes estratégicos divididos entre captura, inteligência, lógica e integração. 

Abaixo estão os principais nodes necessários para construir essa automação end-to-end:

---

### 1. Entrada e Gatilhos (Triggers)
* **Webhook**: Funciona como a "porta de entrada" ou campainha da automação. É o nó que escuta e recebe em tempo real as mensagens enviadas pelos pacientes através do WhatsApp ou de formulários do site da clínica.
* **Schedule Trigger**: Funciona como um temporizador ou despertador programado. É fundamental para fluxos ativos de **lembretes de consulta** e **confirmações prévias**. Ele dispara mensagens automáticas (ex: 24h ou 2h antes do atendimento) para reduzir a taxa de absenteísmo nas consultas odontológicas, nutricionais ou psicológicas.

---

### 2. Comunicação com o WhatsApp
* **Evolution API (Community Node)**: Nó da comunidade (`n8n-nodes-evolution-api`) que facilita o envio e o recebimento de mensagens de texto, áudios e arquivos no WhatsApp sem a necessidade de montar requisições manuais complexas.
* **HTTP Request**: Atua como o "mensageiro" da automação para conversar com APIs externas. Caso não utilize um nó de comunidade, o HTTP Request é configurado para realizar as chamadas para a API do WhatsApp ou para a API do sistema de prontuário/gestão da clínica.

---

### 3. Inteligência Artificial e Tomada de Decisão
* **AI Agent**: Atua como o "cérebro" do fluxo. Em clínicas médicas, ele é configurado com modelos de linguagem (como OpenAI) para interpretar mensagens em linguagem natural. Ele entende a intenção do paciente (agendar, remarcar, tirar dúvidas sobre convênios ou procedimentos), mantém a memória do histórico do diálogo e executa ferramentas (*tools*) para interagir com a agenda.
* **Switch**: Permite rotear o fluxo em múltiplos caminhos. É ideal para categorizar o atendimento por tipo de mensagem (texto, áudio, foto de documento) ou triar a especialidade desejada na clínica (ex: Rota 1 = Odontologia, Rota 2 = Nutrição, Rota 3 = Psicologia).
* **If**: Avalia decisões binárias de sim ou não. Por exemplo: verificar se o paciente já possui cadastro prévio no sistema ou se um horário solicitado está disponível.

---

### 4. Organização de Dados e Agendamento
* **Edit Fields (Set)**: O "organizador" de dados da automação. É usado para limpar, padronizar e renomear as informações extraídas do paciente (como nome completo, CPF, telefone e o horário pretendido) antes de enviá-las para os próximos passos.
* **Google Calendar / Google Sheets**: Utilizados para consultar a disponibilidade de horários dos profissionais da clínica, gravar o novo agendamento e manter um registro centralizado das consultas.
* **Code (JavaScript/Python)**: Utilizado quando é necessário criar lógicas personalizadas de manipulação de datas, calcular intervalos entre consultas ou formatar fusos horários específicos dos profissionais.

---

### Exemplo de Lógica do Fluxo de Agendamento

1. **Recepção**: O paciente envia uma mensagem no WhatsApp \\(\rightarrow\\) O **Webhook** aciona o fluxo.
2. **Triagem**: O **Switch** identifica o tipo de mensagem e o **AI Agent** qualifica a necessidade do paciente (agendamento para Nutrição, Psicologia ou Odontologia).
3. **Consulta de Agenda**: O **AI Agent** consulta as vagas disponíveis na agenda (via **Google Calendar** ou **HTTP Request**).
4. **Confirmação e Cadastro**: O **Edit Fields** organiza os dados pessoais, o horário é reservado, e a **Evolution API** envia a mensagem de confirmação com as orientações pré-consulta ao paciente.
5. **Lembrete Automático**: O **Schedule Trigger** roda diariamente para buscar as consultas do dia seguinte e disparar o lembrete de confirmação via WhatsApp.

💡 *Se desejar, posso desenhar um passo a passo do prompt do AI Agent ou mostrar como estruturar a requisição para a agenda da clínica.*

pergunta 2
como iniciar o workflow com o trigger e posteriormente utilizar o fluxo para agendamento automatico na planilha google e confirmação de agendamento no whatsapp do paciente, logo após isso inserindo o dado de data e hora do agendamento no calendario google

resposta:Para implementar um fluxo completo de agendamento automático no n8n que registra os dados em uma planilha, envia confirmação por WhatsApp e cria o compromisso no Google Calendar, você pode estruturar os nós na ordem descrita abaixo:

---

### Passo 1: Início do Fluxo com o Gatilho (**Webhook Trigger**)

1. **Adicionar o nó Webhook**: O nó **Webhook** é utilizado como gatilho (*trigger*) inicial do fluxo, atuando como a porta de entrada para receber as informações enviadas pelo formulário ou chatbot[1].
2. **Configuração do Webhook**:
  * Defina o método como **POST** para receber a estrutura de dados contendo o nome do paciente, telefone, data e horário da consulta[4].
  * Durante os testes, utilize a **Test URL** e execute o teste no n8n; após finalizar a automação, altere para a **Production URL** e ative o workflow[4][5].
3. **Organização dos Dados com Edit Fields (Set)**:
  * Conecte o nó **Edit Fields (Set)** logo após o Webhook[6].
  * Ele servirá para renomear, filtrar e padronizar os valores recebidos (como `nome_paciente`, `telefone`, `data_inicio` e `data_fim`) antes de enviá-los para as etapas seguintes[7][8].

---

### Passo 2: Registro do Agendamento na Planilha (**Google Sheets**)

1. **Adicionar o nó Google Sheets**: Conecte o nó **Google Sheets** após o nó **Edit Fields**[9][10].
2. **Configuração da Ação**:
  * **Operation**: Selecione a operação **Append Row** (ou *Append or Update Row*) para adicionar uma nova linha[9].
  * **Document ID / Sheet**: Selecione a planilha e a aba correspondente aos agendamentos da clínica.
  * **Mapeamento de Colunas**: Vincule cada coluna da planilha às variáveis organizadas no passo anterior (ex.: Coluna A = `nome_paciente`, Coluna B = `telefone`, Coluna C = `data_inicio`, Coluna D = `Status: Agendado`)[8][9].

---

### Passo 3: Envio da Confirmação de Agendamento no WhatsApp

1. **Escolha do Nó de Mensageria**:
  * Utilize o nó de comunidade **Evolution API** (`n8n-nodes-evolution-api`)[11][12] ou o nó **HTTP Request** configurado em método **POST** para realizar a chamada à API do WhatsApp[13].
2. **Configuração do Envio**:
  * **Resource / Operation**: Escolha a ação de enviar mensagem de texto (*Send Message*)[16].
  * **Destinatário**: Mapeie o número de telefone do paciente vindo do nó anterior[8].
  * **Mensagem**: Escreva o texto dinâmico de confirmação utilizando as variáveis do fluxo (ex.: *"Olá {{* $json.nome\_paciente }}, sua consulta foi agendada com sucesso para {{ $*json.data\_inicio }}."*).

---

### Passo 4: Inserção de Data e Hora no Calendário (**Google Calendar**)

1. **Adicionar o nó Google Calendar**: Insira o nó nativo do **Google Calendar** na sequência do fluxo[15].
2. **Configuração do Evento**:
  * **Resource**: Selecione **Event**.
  * **Operation**: Selecione **Create**[15].
  * **Calendar ID**: Escolha a agenda do profissional ou da clínica.
  * **Start Time / End Time**: Mapeie as variáveis de data e hora de início e término formatadas em padrão ISO 8601[8].
  * **Title (Summary)**: Defina o nome do compromisso (ex.: *"Consulta - {{ $json.nome\_paciente }}"*).


O link do notebook compartilhado.
https://notebook.google.com/notebook/5ecb457d-3df4-4666-b500-171167af6545


