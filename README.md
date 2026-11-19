# TripMaster AI — Planejador de Viagens Multiagente

Planejar uma viagem em família costuma exigir horas comparando destinos, passagens, hotéis e passeios para tentar encaixar tudo no orçamento. O **TripMaster AI** resolve esse problema transformando um único pedido em linguagem natural em um roteiro completo, coerente e financeiramente viável.

Em vez de depender de uma única inteligência artificial genérica tentando fazer tudo de uma vez, o sistema funciona como uma **agência de viagens digital composta por 8 agentes especialistas**. Cada agente possui uma personalidade, critérios técnicos e limites bem definidos, colaborando em etapas até chegar à proposta final.

---

## Como o Sistema Funciona na Prática

Tudo começa com um pedido simples do usuário, como:
> *"Quero viajar com minha família para o Nordeste em julho, com foco em praias e cultura local, por até R$ 8.000."*

A partir dessa entrada, o fluxo segue uma lógica de trabalho em equipe:

1. **Alinhamento inicial:** O **Agente Orquestrador** lê o pedido, organiza as premissas e aciona o **Agente de Perfil do Usuário**, que traduz o texto livre em restrições claras (viagem em família, alta temporada de julho, equilíbrio entre lazer e cultura, teto de R$ 8.000).
2. **Escolha do destino e logística:** Com o perfil definido, o **Agente de Destinos** seleciona lugares compatíveis com o clima e a época do ano (como João Pessoa/PB ou Aracaju/SE). Em seguida, os agentes de **Transporte**, **Hospedagem** e **Experiências Locais** montam as opções de voos, estadia e programação cultural.
3. **Validação financeira e ajustes:** Antes de fechar o plano, o **Agente Financeiro** soma todas as estimativas. Se o pacote inicial ultrapassar os R$ 8.000 (por exemplo, chegando a R$ 8.900), ele barra o orçamento e o Orquestrador pede ajustes — como trocar a categoria do hotel ou substituir um passeio pago por uma atração cultural gratuita — até a conta fechar com margem de segurança (ex.: R$ 7.650).
4. **Entrega ao cliente:** Por fim, o **Agente de Apresentação** reúne apenas o que foi aprovado e escreve o roteiro final de forma clara e organizada.

---

## Arquitetura Técnica Local

Para testar e validar essa orquestração com controle total e sem depender de APIs externas pagas, o ambiente foi estruturado localmente:

* **LM Studio:** Responsável por carregar o modelo de linguagem (Gemma) na máquina e disponibilizar uma API local compatível com o padrão OpenAI (`http://127.0.0.1:1234/v1`).
* **n8n (via Node.js):** Responsável pela camada de orquestração visual, conectando a interface de chat (*On chat message*) aos nós de inteligência artificial e gerenciando o fluxo de mensagens.

### Arquivos deste Repositório
* `README.md` — Documentação completa do projeto, arquitetura, prompts e reflexões.
* `tripmaster-workflow.json` — Exportação do fluxo desenvolvido no n8n.
* `evidencias/` — Capturas de tela demonstrando a execução do fluxo no n8n e o servidor ativo no LM Studio.

---

## Os 8 Agentes e Seus Prompts

Cada especialista abaixo é configurado com um *System Prompt* próprio para evitar que um invada a função do outro:

### 1. Agente Orquestrador
*Coordena a equipe, define a ordem das tarefas e garante a coerência entre roteiro e orçamento.*
```text
Você é o Agente Orquestrador do projeto TripMaster AI, um planejador de viagens inteligente baseado em múltiplos agentes.

Sua função é:
1. Analisar o pedido inicial do cliente (briefing) e extrair os pontos-chave.
2. Coordenar a sequência de trabalho entre os agentes especializados (Perfil do Usuário, Destinos, Transporte, Hospedagem, Financeiro, Experiências Locais e Apresentação).
3. Garantir que o plano final seja completo, coerente com o perfil/época do ano e viável dentro do orçamento informado.
4. Apontar o que cada agente deve avaliar e quais conflitos precisam ser resolvidos antes da proposta final.

Estilo de fala: Tom profissional, colaborativo e objetivo.
Estruture a resposta em: 1. Resumo do Briefing e Premissas; 2. Sequência de Tarefas aos Agentes; 3. Pontos de Atenção / Conflitos; 4. Próximo Passo.
