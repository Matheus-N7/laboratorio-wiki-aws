# 📝 Resposta do Laboratório: A Wiki Perdida dos Arquivos Corporativos

> Preencha este arquivo com a sua proposta de solução.
>
> Sua resposta deve explicar como transformar os documentos brutos da pasta `raw/` em uma Wiki Corporativa Inteligente, pesquisável e segura usando apenas serviços da AWS.

---

## 👤 Identificação

**Nome:**  
Matheus Leonardo

**Data:**  
03/10/2026

**Link do repositório:**  
(https://github.com/Matheus-N7/laboratorio-wiki-aws/tree/main)

---

# ✅ Quest 1: O Mapa dos Arquivos Perdidos

## 1.1 Formatos encontrados na pasta `raw/`

Descreva quais tipos de arquivos existem dentro da pasta `raw/`.

```md
Exemplo de como responder, com o formato e o que ele implica:
- <extensao>: <nasce digital ou precisa de OCR?>, <o que da para extrair>
```

> Abra a pasta e liste o que voce encontrou de fato. Esta quest avalia a sua
> leitura do acervo, entao a resposta certa e a que corresponde aos arquivos.

**Sua resposta:**

```md
Verifica-se que os documentos estão em três tipos de formato (PDF, os digitalizados em PNG e CSV). Implica no uso de duas maneiras para a extração das informações, sendo o uso de OCR (Textract) para o PDF e PNG. E uso do Lambda para o CSV..
```

---

## 1.2 Principais desafios encontrados

Explique quais dificuldades esses documentos podem apresentar.

```md
Exemplo:
- Arquivos sem padrão de nomenclatura
- Documentos escaneados com baixa qualidade
- Textos manuscritos ou parcialmente ilegíveis
- Atas com estruturas diferentes
- Informações importantes espalhadas em vários formatos
```

**Sua resposta:**

```md
Arquivos em formatos e estruturas diferentes e imagens escaneadas podem vir borradas e o CSV pode vir com colunas faltando.
```

---

## 1.3 Informações importantes a serem extraídas

Liste quais informações precisam ser identificadas para transformar os documentos em conhecimento pesquisável.

**Sua resposta:**

```md
As informações importantes para a extração são:  nome dos envolvidos, datas, locais, assuntos tratados e suas decisões, valores financeiros, prazos, riscos, pendências, projetos, áreas e segmentos organizacional.
```

---

## 1.4 Estratégia de classificação inicial

Como você classificaria os documentos sem depender de subpastas dentro de `raw/`?

**Sua resposta:**

```md
Optou-se por não realizar classificação ou separação manual prévia dos arquivos. Todos serão enviados juntos, e a classificação (roteamento por extensão) ocorrerá de forma automatizada na nuvem via Lambda.
```

---

# ✅ Quest 2: O Portal de Entrada na AWS

## 2.1 Armazenamento dos arquivos brutos

Explique como os arquivos da pasta `raw/` seriam enviados e armazenados na AWS.

Serviços que você pode considerar:

- Amazon S3
- AWS IAM
- AWS KMS
- Amazon S3 Versioning
- Amazon S3 Lifecycle

**Sua resposta:**

```md
Os arquivos serão enviados no seu estado original sem modificações por meio do Console de Gerenciamento da AWS via upload, com destino a um sistema de armazenamento Amazon S3.
```

---

## 2.2 Preservação dos arquivos originais

Explique como garantir que os arquivos originais sejam mantidos intactos e rastreáveis.

**Sua resposta:**

```md
A preservação será feita por meio Amazon S3 inicial, o qual manterá os arquivos originais.
```

---

## 2.3 Extração de texto dos documentos

Explique como cada tipo de arquivo seria processado.

Considere:

- PDFs escaneados;
- Imagens;
- PDFs digitais;
- Arquivos `.txt`;
- Arquivos `.docx`;
- Arquivos `.md`.

Serviços que você pode considerar:

- Amazon Textract
- AWS Lambda
- AWS Step Functions
- Amazon S3
- Amazon CloudWatch

**Sua resposta:**

```md
A extração dos textos dos documentos será feita com base no seu tipo de arquivo (PDF, PNG e CSV). Onde o Textract fica responsável dos PDFs e PNGs, enquanto o Lambda com uma função de conversão de tabela para texto cuida do CSV. E apesar de na pasta não conter arquivos nos formatos TXT, MD e DOCX no futuro se fossem adicionados os TXT e MD seguiriam a rota do Lambda (pois já são texto). Arquivos DOCX exigiriam uma biblioteca de extração no Lambda ou envio para o Textract..
```

---

## 2.4 Tratamento de falhas

Explique como sua solução identificaria e registraria erros de processamento.

**Sua resposta:**

```md
Quando um arquivo dá erro (ex: o Textract não conseguiu ler a imagem), esse arquivo segue para uma "Fila de Erros", conhecida como DLQ (Dead Letter Queue), ou o movemos para um Armazenamento "S3-Erros". Assim, o sistema não trava e um administrador humano pode olhar o arquivo defeituoso depois.
```

---

# ✅ Quest 3: A Relíquia dos Metadados

## 3.1 Padronização dos textos processados

Explique como os textos extraídos seriam limpos, normalizados e preparados para consulta.

**Sua resposta:**

```md
Por meio do Comprehend os textos extraídos serão analisados e devolvidos e estruturado no formato JSON, com as informações em blocos classificados.
```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
| --- | --- |
| Nome do documento | É uma forma de identificar o documento. |
| Tipo do documento | Classificação do formato (ex: PDF, PNG, CSV) ou documento (Ata, Relatório). |
| Data identificada | Marca o registro de quando os eventos ocorreram ou vão ocorrer. |
| Tema principal | O assunto principal tratado no documento. |
| Participantes | Pessoas que participaram do contexto ou reunião. |
| Decisões tomadas | Representa as ações decididas pelos envolvidos. |
| Responsáveis | Pessoas responsáveis por determinada tarefa. |
| Próximos passos | Ações e tarefas a serem cumpridas. |
| Nível de confidencialidade | O Comprehend detecta PII (como CPF e Cartão de Crédito). Se achar, a tag vira "Alta". |
| Caminho do arquivo original | Link referente ao arquivo original armazenado no S3. |

Adicione outros metadados, se necessário.

---

## 3.3 Uso de IA para enriquecimento dos documentos

Explique como o Amazon Bedrock poderia ajudar a identificar temas, decisões, responsáveis, pendências e resumos dos documentos.

**Sua resposta:**

```md
Amazon Bedrock consegue cruzar informações de documentos diferentes e gerar um resumo completo (parágrafo único) para o usuário, poupando a pessoa de ler páginas e páginas de arquivos.
```

---

## 3.4 Armazenamento dos metadados

Explique onde os metadados seriam armazenados e como seriam conectados aos documentos originais.

Serviços que você pode considerar:

- Amazon S3
- Amazon DynamoDB
- AWS Glue Data Catalog
- Amazon Bedrock Knowledge Bases

**Sua resposta:**

```md
Os metadados seriam armazenados no S3 “processados” e terão sua conexão aos dados originais através do Kendra quando houver a busca/ solicitação dos dados.
```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

Explique como os documentos seriam divididos em trechos menores e preparados para busca semântica.

**Sua resposta:**

```md
Os documentos seriam divididos em trechos menores, onde o Kendra transforma palavras em números para entender o significado por exemplo: entende que "Cachorro" e "Cão" têm números parecidos.
```

---

## 4.2 Busca semântica e base vetorial

Explique como embeddings seriam gerados e onde seriam armazenados.

Serviços que você pode considerar:

- Amazon Bedrock Knowledge Bases
- Amazon OpenSearch Serverless
- Amazon Aurora PostgreSQL com pgvector
- Amazon S3 Vectors
- Modelos de embeddings no Amazon Bedrock

**Sua resposta:**

```md
O Kendra já gerencia a criação de embeddings e a base vetorial nativamente.
```

---

## 4.3 Geração de respostas com IA

Explique como a Wiki responderia perguntas em linguagem natural com base nos documentos originais.

Considere explicar:

- Como a pergunta do usuário seria recebida;
- Como os trechos relevantes seriam recuperados;
- Como o Amazon Bedrock geraria a resposta;
- Como a resposta indicaria as fontes utilizadas.

**Sua resposta:**

```md
O sistema pega a pergunta do usuário, o Amazon Kendra vasculha os JSONs do S3 processado e devolve apenas 2 ou 3 parágrafos mais relevantes sobre o assunto. Depois com apenas a pergunta e esses parágrafos relevantes o Bedrock formula a resposta clara e resumida, bem como o link para o arquivo original. Caracterizando uma arquitetura RAG (Retrieval-Augmented Generation)..
```

---

## 4.4 Interface de consulta

Proponha como os usuários acessariam essa Wiki Inteligente.

Serviços que você pode considerar:

- Amazon Q Business
- AWS Amplify
- Amazon API Gateway
- AWS Lambda
- Amazon Cognito

**Sua resposta:**

```md
Através do Bedrock , a interface de consulta será um Chatbot Web Customizado. Essa aplicação receberá a pergunta do funcionário, fará a orquestração entre o Kendra e o Bedrock na AWS e exibirá a resposta final na tela, permitindo que o usuário 'converse' com o acervo disponível.
```

---

## 4.5 Segurança, auditoria e monitoramento

Explique como controlar acesso, proteger dados, auditar consultas e monitorar custos, erros e qualidade das respostas.

Serviços que você pode considerar:

- AWS IAM
- AWS KMS
- Amazon Cognito
- AWS CloudTrail
- Amazon CloudWatch
- Amazon Macie
- AWS Cost Explorer

**Sua resposta:**

```md
IAM para garantir que só funcionários autorizados acessem o painel de busca.
```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

Explique em poucas linhas a ideia central da sua arquitetura.

**Sua resposta:**

```md
A visão Geral da arquitetura é ser simples e funcional de modo que os objetivos estabelecidos no desafio sejam cumpridos utilizando somente serviços AWS.
```

---

## 2. Serviços AWS utilizados

| Serviço AWS | Papel na solução |
| --- | --- |
| Amazon S3 | Sistema para a armazenagem dos documentos brutos e dos processados. |
| AWS Lambda | Roteia arquivos e limpa/formata dados. |
| Amazon Textract | Extração de texto de formatos que precisam de OCR (Imagens e PDFs). |
| Amazon Comprehend | Faz a análise estruturada e extrai as tags/entidades dos textos. |
| Amazon Kendra | Motor de busca inteligente varrendo o S3 Processado e recuperando os melhores trechos. |
| Amazon Bedrock | Responsável pela interação e formulação da resposta humana final para o usuário. |

Adicione, remova ou ajuste os serviços conforme sua proposta.

---

## 3. Fluxo de dados de ponta a ponta

Descreva o caminho dos dados desde a pasta `raw/` até a Wiki Inteligente.

```md
Exemplo de estrutura:

1. Arquivos estão inicialmente na pasta raw/
2. Arquivos são enviados para o Amazon S3
3. Documentos escaneados passam pelo Amazon Textract
4. Arquivos digitais têm seus textos extraídos
5. Textos são limpos e padronizados
6. Metadados são extraídos
7. Conteúdos são indexados em uma base pesquisável
8. Usuário pesquisa na Wiki
9. IA responde com base nos documentos originais
```

**Sua resposta:**

```md
1. Os arquivos na pasta `raw/` são enviados para o Amazon S3 sem alterações.
2. Os documentos são roteados por uma função Lambda com base no seu formato, ocorrendo uma divisão de rotas.
3. **Caminho 1 (CSV):** Passa por uma segunda função Lambda que faz a leitura e extração do texto limpo.
4. **Caminho 2 (PDF e PNG):** Passam pelo Textract e seus textos são extraídos.
5. Os caminhos se unem e os textos passam pelo Amazon Comprehend, onde ocorre a análise semântica e a classificação das tags.
6. Os dados enriquecidos e formatados em JSON são armazenados em um outro Amazon S3 (Camada Processada).
7. O usuário faz uma pergunta no Chatbot Web da empresa.
8. O Amazon Kendra busca na pergunta, extrai os trechos mais relevantes do S3 Processado e manda para o Bedrock.
9. O Amazon Bedrock formula a resposta humanizada e a entrega ao usuário, juntamente com o link do arquivo original como fonte.
```

---

## 4. Diagrama textual da arquitetura

Crie um diagrama simples usando texto.

```md
Exemplo:

raw/ → Amazon S3 → Lambda/Step Functions → Textract → S3 Processado → Bedrock Knowledge Bases → Interface de Consulta → Usuário Final
```

**Sua resposta:**

```md
<img width="830" height="323" alt="diagrama-wiki" src="https://github.com/user-attachments/assets/d8540740-29f8-4aac-a246-50b5109ab32c" />.

```

---

## 5. Riscos e limitações

Liste possíveis desafios da sua solução.

```md
Exemplo:
- Documentos ilegíveis podem prejudicar a extração de texto.
- OCR pode gerar erros em documentos com baixa qualidade.
- Custos podem aumentar conforme o volume de documentos.
- Metadados inferidos por IA podem precisar de validação humana.
- Respostas geradas por IA devem sempre referenciar documentos de origem.
```

**Sua resposta:**

```md
Documentos ilegíveis podem prejudicar a extração de texto ou OCR gerar erros em documentos com baixa qualidade. 
Custos podem aumentar conforme o volume de documentos, portanto é recomendado estabelecer um orçamento e configurar o AWS Budgets para enviar um alarme se o projeto ultrapassar o valor estipulado.
Segurança alguém vazar dados do S3.
```

---

## 6. Melhorias futuras

Descreva como a solução poderia evoluir.

```md
Exemplo:
- Criar uma interface web para consulta.
- Criar um chat interno para perguntas sobre atas.
- Adicionar controle de acesso por departamento.
- Criar dashboard de decisões e pendências.
- Gerar alertas automáticos sobre ações em aberto.
- Integrar com ferramentas corporativas.
```

**Sua resposta:**

```md
Adicionar o AWS CloudTrail para aumentar a segurança do processo, sendo este um serviço que grava um histórico/auditoria de quem pesquisou o quê.
```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [x] Como transformar documentos escaneados em texto?
- [x] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [x] Como armazenar os documentos originais?
- [x] Como preservar a rastreabilidade entre resposta e documento fonte?
- [x] Como organizar metadados?
- [x] Como criar busca semântica?
- [x] Como usar Amazon Bedrock na solução?
- [x] Como proteger documentos sensíveis?
- [x] Como monitorar falhas?
- [x] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

Escreva uma breve conclusão defendendo sua solução como se estivesse apresentando para uma liderança técnica ou de negócio.

**Sua resposta:**

```md
Ao propor uma solução para um problema comum das empresas, o desafio demonstra a complexidade e as variadas alternativas presentes nos serviços da AWS para sua resolução e a importância da arquitetura em nuvem.
```
