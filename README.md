## Exercícios da Seção 9.5.7

### Questão 1: Diferentes valores para num_examples

**Tente valores diferentes do argumento num_examples na função load_data_nmt. Como isso afeta os tamanhos do vocabulário do idioma de origem e do idioma de destino?**

Ao experimentar diferentes valores para o parâmetro `num_examples` na função `load_data_nmt`, observamos uma relação direta com os tamanhos dos vocabulários de origem e destino:

- **Valores pequenos (100-200)**: Resultam em vocabulários muito limitados que incluem apenas palavras de alta frequência. Isso leva a um modelo que reconhece poucos termos e frequentemente substitui palavras por tokens desconhecidos (`<unk>`), prejudicando significativamente a qualidade da tradução.

- **Valores médios (600-1000)**: Proporcionam um equilíbrio razoável entre cobertura do vocabulário e eficiência computacional. O modelo consegue aprender traduções para um número adequado de palavras sem exigir recursos excessivos.

- **Valores grandes (5000+)**: Aumentam substancialmente o tamanho do vocabulário, incluindo palavras menos frequentes. Isso pode melhorar a qualidade da tradução, mas também:
  - Aumenta consideravelmente o tempo de treinamento
  - Requer mais memória para armazenar os embeddings
  - Pode causar overfitting se a arquitetura do modelo não for suficientemente robusta

Ao realizar testes com diferentes valores, observamos que o crescimento do vocabulário não é linear em relação ao aumento de `num_examples`. Isso ocorre devido à distribuição de frequência de palavras em linguagem natural seguir a Lei de Zipf, onde poucas palavras aparecem com muita frequência e muitas palavras aparecem raramente.

### Questão 2: Tokenização para chinês e japonês

**O texto em alguns idiomas, como chinês e japonês, não tem indicadores de limite de palavras (por exemplo, espaço). A tokenização em nível de palavra ainda é uma boa ideia para esses casos? Por que ou por que não?**

A tokenização em nível de palavra não é ideal para idiomas como chinês e japonês que não utilizam espaços como delimitadores de palavras, por diversos motivos:

1. **Ambiguidade na segmentação**: Sem marcadores claros entre palavras, é difícil determinar onde uma palavra termina e outra começa. Um mesmo trecho de texto pode ter múltiplas segmentações válidas, dependendo do contexto.

2. **Complexidade morfológica**: Esses idiomas possuem estruturas morfológicas diferentes das línguas ocidentais. O chinês usa caracteres (hanzi) que podem ser palavras por si só ou combinar-se para formar palavras compostas. O japonês mistura múltiplos sistemas de escrita (kanji, hiragana e katakana).

3. **Vocabulário explosivo**: Uma tokenização baseada em palavras criaria um vocabulário excessivamente grande devido à natureza combinatória desses idiomas.

Para esses idiomas, abordagens alternativas são mais adequadas:

- **Tokenização baseada em caracteres**: Trata cada caractere como um token, o que funciona razoavelmente bem para o chinês.
- **Tokenização baseada em subpalavras**: Métodos como Byte-Pair Encoding (BPE) ou SentencePiece, que aprendem a dividir o texto em unidades menores que palavras.
- **Segmentadores específicos para o idioma**: Ferramentas como Jieba (para chinês) ou MeCab (para japonês) que utilizam dicionários e algoritmos específicos para realizar segmentação morfológica.

A tokenização adequada é crucial para o desempenho de modelos de tradução automática nesses idiomas, e geralmente as abordagens baseadas em subpalavras têm mostrado melhores resultados que a tokenização em nível de palavra.