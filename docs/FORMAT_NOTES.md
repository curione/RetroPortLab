# Notas provisórias sobre o formato

## Contexto

Os arquivos examinados têm a extensão `.ccs` e incluem ocorrências de marcadores ASCII `CCMF`, além de referências textuais como `TEX_sel01`. A estrutura completa do contêiner ainda não foi formalmente especificada.

## Observações medidas

| Arquivo | Tamanho | Observação |
|---|---:|---|
| `charsel1.ccs` | 2,187,468 bytes (`0x2160CC`) | 32 marcadores CCMF reportados |
| `custom.ccs` | 153,364 bytes (`0x25714`) | 28 marcadores CCMF reportados |

No `custom.ccs`, quatro blocos consecutivos em `0x7638`, `0x77D0`, `0x7968` e `0x7B00` têm comprimento de 0x198 bytes entre inícios de bloco.

## Hipóteses para validação

1. **CCMF como marcador de estrutura:** o marcador parece delimitar regiões de dados, mas é necessário confirmar se cada ocorrência é um objeto completo ou se há estruturas compostas/aninhadas.
2. **Registros com stride 0x18:** algumas regiões podem conter registros de 24 bytes que se alinham com coordenadas e outros atributos.
3. **Índices de malha como UInt32:** uma região de `charsel1.ccs` forma uma lista coerente de 96 índices, todos no intervalo 0–24, compatível com 32 triângulos sobre 25 vértices.
4. **Cores de quatro bytes:** valores como `6A 6A 6A FF`, `66 66 66 FF` e `69 69 69 FF` podem representar cor por vértice, mas a ordem de canais não foi validada.
5. **Referências de textura:** nomes `TEX_...` parecem identificadores ou referências de recursos; não foi demonstrado que os dados da imagem estejam dentro do CCS.

## Procedimento de análise recomendado

1. Preservar os arquivos originais localmente e registrar tamanho e hash SHA-256.
2. Registrar cada offset em hexadecimal e decimal, tamanho do intervalo e método usado.
3. Comparar blocos com mesmo comprimento, primeiro em bytes e depois em campos tipados.
4. Confirmar listas de índices verificando limites, conectividade e triangulação.
5. Validar a interpretação visual importando exportações OBJ em um visualizador 3D.
6. Validar UVs com uma textura de teste sintética, não com imagens protegidas.
7. Repetir os testes em vários arquivos/objetos antes de generalizar o layout.
8. Manter scripts de análise pequenos e reproduzíveis, com logs dos parâmetros usados.

## Regras para conclusões

Nunca inferir a semântica de um campo apenas porque seu valor parece um float ou um inteiro plausível. Os mesmos quatro bytes podem representar tipos diferentes. Marcar cada conclusão como observada, hipótese ou pendente e documentar quais testes poderiam refutá-la.
