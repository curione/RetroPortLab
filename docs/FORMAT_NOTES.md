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


## Observação adicional: registros de 24 bytes em `custom.ccs` (2026-10-16)

Nos blocos `0x7638`, `0x77D0`, `0x7968` e `0x7B00`, foram observados quatro registros com stride de `0x18`, iniciando relativamente em `+0x0A0`, `+0x0B8`, `+0x0D0` e `+0x0E8`. A interpretação provisória por registro é:

| Offset relativo dentro do registro | Interpretação candidata |
|---|---|
| `+0x00` | Float variável; pode ser uma coordenada de textura |
| `+0x04` | Quatro bytes semelhantes a cor empacotada (ex.: `66 66 66 FF`) |
| `+0x08` | X, aproximadamente -64 ou +64 |
| `+0x0C` | Y, aproximadamente +12/-12 ou +13/-13 |
| `+0x10` | Z, normalmente zero; cerca de -0.00005 no bloco `0x7B00` |
| `+0x14` | Float 1 ou 0, alternando entre os dois lados do retângulo; pode ser a segunda coordenada de textura |

A disposição dos valores é consistente com quatro cantos de retângulo. O primeiro float muda entre os pares superior/inferior; o último alterna por coluna. Isso torna plausível que os dois campos sejam UVs, mas ainda não houve validação com textura sintética no Blender. A ordem dos canais de cor também permanece desconhecida.

Valores observados no primeiro campo dos quatro registros, na ordem `+0x0A0, +0x0B8, +0x0D0, +0x0E8`:
- `0x7638`: 0.8125, 0.8125, 1.0, 1.0.
- `0x77D0`: 0.625, 0.625, 0.8125, 0.8125.
- `0x7968`: 0.4375, 0.4375, 0.625, 0.625.
- `0x7B00`: 0.234375, 0.234375, 0.4375, 0.4375.

O bloco `0x7B00` também difere por Y próximo de ±13, Z próximo de -0.00005 e pelo float próximo de +0.00005 em `+0x028`. O campo em `+0x18C` contém 20, 21, 22 e 23 nos quatro blocos; ainda não foi identificado seu significado.

**Status:** layout de registro candidato observado; UV, semântica dos campos finais e função gráfica ainda não confirmados. Não generalizar esse layout para todos os CCMF.
