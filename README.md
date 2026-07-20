# GenIA: Classificador de Produtos — MBA em Engenharia de Dados | FIAP

**Disciplina:** Generative AI<br>
**Professor:** Anderson Vieira Dourado<br>
**Instituição:** FIAP<br>

## Autores

Ademir Franco de Brito <br>
Alef Anderson Fernandes Clarindo da Silva <br>
Gabriel Henrique Dias <br>
Heron Ignario Ando <br>
Thatiane Martins Batista Botelho <br>

---

## Estrutura de Arquivos

```
generative_ai/
├── README.md                                      # Este arquivo
├── genAI_classificador_de_produtos.ipynb          # Notebook principal
└── dataset/                                       # Notebook principal
   └── output/produtos_classificados_genai.csv        # Saída do processo
   └── input/produtos.csv                             # Dados de entrada
```

## Visão Geral do Projeto

Este projeto apresenta uma solução de **classificação e validação automática de produtos** para um marketplace utilizando **Inteligência Artificial Generativa (GenAI)**. O objetivo é mitigar problemas com categorização incorreta de produtos e reduzir o esforço manual necessário para revisar cadastros.

## Contexto e Motivação

Um marketplace enfrenta desafios significativos com produtos cadastrados em categorias incorretas por vendedores. Esses problemas incluem:

- Vendedores que cadastram produtos em categorias inadequadas
- Produtos que violam políticas de venda da plataforma
- Falta de descrições suficientes para validação manual
- Necessidade de revisar centenas de cadastros manualmente

Este trabalho propõe uma **solução automatizada** que utiliza modelos de linguagem para classificar produtos com base no nome e descrição, comparando com a categoria informada pelo vendedor e identificando possíveis inconsistências.

## Escopo e Objetivos

O projeto visa:

1. **Analisar** uma base de dados de produtos para entender sua estrutura e distribuição
2. **Explorar** diferentes técnicas de prompt engineering com IA Generativa
3. **Comparar** estratégias de classificação (prompts simples vs. refinados)
4. **Implementar** um pipeline final que classifique 100 produtos selecionados
5. **Validar** as classificações comparando com a categoria informada pelos vendedores
6. **Gerar** um relatório estruturado em CSV com os resultados

## Ambiente de Desenvolvimento

**Este projeto foi desenvolvido e otimizado para o Google Colab**, que fornece:

- Acesso a GPUs/TPUs (quando necessário)
- Pré-instalação de bibliotecas principais
- Integração simples com credenciais
- Facilidade de compartilhamento e colaboração

O notebook pode ser executado localmente com Python 3.8+, mas recomenda-se o Google Colab para melhor experiência.

## Arquitetura da Solução

### Componentes Principais

- **Azure OpenAI**: Serviço de IA Generativa utilizado para classificação
- **LangChain**: Framework para orquestração de prompts e cadeia de processamento
- **Python/Pandas**: Processamento e análise de dados
- **Google Colab**: Ambiente recomendado para execução

### Fluxo de Processamento

```
Extração de Dados
    ↓
Análise Exploratória
    ↓
Preparação (concatenação de nome + descrição)
    ↓
Criação de amostra (100 registros com seed=42)
    ↓
Testes de Prompts (Simples vs. Refinado)
    ↓
Classificação com Prompt Refinado
    ↓
Validação (comparação com categoria original)
    ↓
Geração de CSV com resultados
```

## Análise Exploratória

A base de dados contém produtos categorizados em diferentes categorias de um marketplace. A análise inicial revelou:

- **Estrutura**: colunas de nome, descrição e categoria
- **Dados Completos**: base sem valores nulos após tratamento
- **Distribuição**: categorias incluem brinquedo, game, livro e maquiagem
- **Tamanho dos Textos**: descrições com variação considerável de comprimento

A análise exploratória foi realizada na **base completa** para entender o contexto geral, enquanto os testes foram executados em uma **amostra de 100 registros** (random_state=42) para garantir comparabilidade e eficiência computacional.

## Estratégias de Classificação Testadas

### 1. Prompt Simples

**Abordagem:** Instruções diretas e objetivas para classificação

**Características:**
- Prompt minimalista
- Instruções claras para retornar apenas a categoria
- Menos restrições sobre casos ambíguos

**Resultados:**
- Acurácia: 96%
- Maior aderência ao dataset original
- Tende a reproduzir o padrão existente, incluindo possíveis erros

### 2. Prompt Refinado (Escolhido)

**Abordagem:** Instruções detalhadas com regras e critérios rigorosos

**Características:**
- Definições claras de cada categoria
- Critérios explícitos de decisão
- Comportamento conservador (retorna "fora_escopo" em casos ambíguos)
- Orientações sobre items ilícitos ou não permitidos

**Resultados:**
- Acurácia: 93%
- Comportamento mais validador
- Melhor para identificar inconsistências e produtos questionáveis
- Mais adequado para apoiar revisão manual

### Decisão e Justificativa

Embora o prompt simples apresente maior acurácia, o **prompt refinado foi escolhido** como abordagem final porque:

1. Funciona como mecanismo de **validação**, não apenas classificação
2. Identifica casos ambíguos ou com baixa confiança
3. Apoiar a identificação de possíveis **inconsistências de cadastro**
4. Comportamento mais conservador reduz falsos positivos
5. Alinha-se melhor com o objetivo de governance de dados

## Pipeline Final

### Processo de Classificação

A função final aplica as seguintes etapas:

1. **Recebe**: texto concatenado (nome + descrição)
2. **Processa**: com o Azure OpenAI e LangChain
3. **Retorna**: categoria validada ou "fora_escopo"
4. **Valida**: comparando com a categoria original
5. **Marca**: como "correta" ou "divergente"

### Saída

O arquivo `produtos_classificados_genai.csv` contém:

- `nome`: nome do produto
- `descricao`: descrição do produto
- `categoria`: categoria original (vendedor)
- `texto`: concatenação de nome + descrição
- `categoria_genai`: categoria atribuída pela IA
- `validacao_categoria`: "correta" ou "divergente"

## Resultados

### Métrica de Validação

A classificação final dos 100 produtos selecionados mostrou:

- **Casos Corretos**: produtos cuja categoria da IA coincide com a do vendedor
- **Casos Divergentes**: produtos cuja categoria foi diferente, sinalizando possível inconsistência

Esses casos divergentes não são necessariamente erros, mas sim **oportunidades de revisão manual** para garantir a qualidade dos dados no marketplace.

### Implicações Práticas

A solução proporciona:

- **Redução de esforço manual**: automação da primeira camada de validação
- **Detecção de inconsistências**: identificação de produtos questionáveis
- **Escabilidade**: pode ser aplicada a grandes volumes de cadastros
- **Governança de dados**: suporta políticas de qualidade de dados

## Tecnologias Utilizadas

| Tecnologia | Versão | Propósito |
|---|---|---|
| Python | 3.x | Linguagem principal |
| Pandas | Latest | Manipulação de dados |
| NumPy | Latest | Operações numéricas |
| LangChain | Latest | Orquestração de prompts |
| Azure OpenAI API | v1 | Acesso ao modelo GPT |
| Google Colab | - | Ambiente de execução (recomendado) |
| Jupyter | Latest | Notebook interativo |

## Instruções de Execução

### Executar no Google Colab (Recomendado)

1. **Abrir o notebook no Colab**
   - Faça upload do arquivo `genAI_classificador_de_produtos.ipynb` para o Google Colab
   - Ou abra diretamente do Google Drive se ele estiver sincronizado

2. **Configurar Credenciais do Azure OpenAI**
   - Navegue até a seção "API Key da OpenAI via Azure AI Foundry"
   - Insira as seguintes informações na célula correspondente:
     - `AZURE_OPENAI_API_KEY`: sua chave da API
     - `AZURE_ENDPOINT`: endpoint do seu serviço
     - `AZURE_API_VERSION`: versão da API
     - `AZURE_DEPLOYMENT_MODEL`: nome da implantação (ex: gpt-4.1)

3. **Executar o Notebook**
   - Seção 1: Análise exploratória (base completa)
   - Seção 2: Desenvolvimento e testes (amostra de 100)
   - Seção 3: Pipeline final (amostra de 100)
   - Seção 4: Conclusões

4. **Obter o Arquivo de Saída**
   - Após executar a seção "Carregamento do relatório final"
   - Faça download do arquivo `produtos_classificados_genai.csv` gerado

### Executar Localmente (Alternativa)

Se preferir executar em seu ambiente local:

1. **Clonar ou fazer download do repositório**
   ```bash
   git clone [seu-repositorio]
   cd generative_ai
   ```

2. **Criar ambiente virtual (opcional, mas recomendado)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   venv\Scripts\activate     # Windows
   ```

3. **Instalar Dependências**
   ```bash
   pip install openai langchain langchain-openai pandas numpy jupyter
   ```

4. **Configurar Variáveis de Ambiente**
   - Crie um arquivo `.env` com suas credenciais do Azure OpenAI
   - Ou configure as variáveis de ambiente do sistema

5. **Executar o Jupyter Notebook**
   ```bash
   jupyter notebook genAI_classificador_de_produtos.ipynb
   ```

### Pré-requisitos

### Observações Importantes

- **Manter seed=42**: Essencial para reproduzibilidade e comparação com outros grupos
- **Não expor credenciais**: Remover API keys antes de publicar
- **Executar todas as células**: Manter histórico completo de execução
- **Respeitar estrutura**: Seguir o template fornecido pelo professor

## Principais Aprendizados

1. **Prompt Engineering**: A estrutura e detalhe do prompt impactam significativamente os resultados
2. **Trade-off Aderência vs. Validação**: Maior acurácia nem sempre significa melhor capacidade de identificar problemas
3. **Comportamento Conservador**: Ser restritivo em classificações ambíguas é melhor que forçar uma categoria incorreta
4. **GenAI como Validador**: IA pode ser usada não apenas para classificação, mas para governance de dados
5. **Importância do Contexto**: Fornecer definições claras ao modelo melhora significativamente a qualidade das respostas

## Possíveis Extensões

Esta solução pode ser estendida para:

- Identificar **descrições insuficientes** ou incompletas
- Detectar **produtos potencialmente ilícitos** ou não permitidos
- Validar **informações enganosas ou irrelevantes**
- Implementar **múltiplos modelos especializados** por categoria
- Criar **pipeline de feedback** para melhorar o treinamento

## Impacto Esperado

Para o marketplace:

- Redução significativa de **tempo de revisão manual**
- Melhoria na **qualidade dos dados cadastrais**
- **Detecção automática** de inconsistências
- Suporte a **decisões de moderação** mais informadas
- **Escalabilidade** para grandes volumes

## Referências e Recursos

- **Azure OpenAI Documentation**: https://learn.microsoft.com/pt-br/azure/ai-foundry/
- **LangChain Documentation**: https://python.langchain.com/
- **OpenAI Python Repository**: https://github.com/openai/openai-python
- **Dataset de Produtos**: https://dados-ml-pln.s3-sa-east-1.amazonaws.com/produtos.csv

## Considerações de Segurança

- Credenciais não foram incluídas no notebook publicado
- API keys devem ser gerenciadas via variáveis de ambiente
- Dados sensíveis foram tratados conforme políticas
- Código foi revisado para remover informações confidenciais

## Licença e Uso

Este projeto é parte da avaliação da disciplina Cloud Strategy no MBA em Engenharia de Dados da FIAP.
