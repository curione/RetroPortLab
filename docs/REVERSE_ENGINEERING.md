# Caderno de engenharia reversa

Este documento consolida as observações disponíveis até 2026-10-16. Os offsets são relativos ao início do arquivo, salvo indicação contrária. Todos os resultados vêm de análise estática dos arquivos locais; devem ser reproduzidos antes de serem tratados como especificação definitiva.

## Arquivos examinados

No diretório extraído `21btlsel` foram analisados:

- `charsel1.ccs` — 2,187,468 bytes (`0x2160CC`).
- `custom.ccs` — 153,364 bytes (`0x25714`).
- Também foram inventariados `mapsel1.ccs`, `network.ccs`, `setting.ccs` e `vs.ccs`.

Os recursos originais não estão incluídos neste repositório.

## `charsel1.ccs`

### Marcadores e pesquisa de texturas

- Foram encontrados 32 marcadores `CCMF`.
- Os marcadores `CLT_sel_asw` e `TEX_sel_asw` aparecem em offsets próximos de `0xEEC` e `0xF0C`.
- A sequência de nomes de referências `TEX_sel_ymt` foi observada em torno de `0x1B8C`, seguida por um intervalo maior antes de uma entrada dummy.
- Uma varredura não encontrou assinaturas diretas reconhecíveis de DDS, PNG, TIM2 ou GIM; também não identificou um BMP plausível. Dois offsets com bytes semelhantes a início JPEG não apresentaram um terminador JPEG confirmado nos 64 KiB seguintes. Isso não prova que texturas estejam ausentes: podem estar em outro arquivo, comprimidas, armazenadas em formato proprietário ou referenciadas externamente.

### Malha candidata perto de `0x001ADE90`

A região que começa em `0x001ADE90` foi interpretada como um bloco candidato de geometria. O próximo marcador CCMF foi observado em `0x001AF748`, resultando em tamanho de bloco de 6,328 bytes (`0x18B8`). O bloco seguinte tem 6,184 bytes (`0x1828`).

Observações da região:

- Uma sequência candidata de vértices começa em `base + 0x0D00`, com passo de 0x18 bytes (24 bytes).
- Foram interpretados 25 vértices, com coordenadas X e Y organizadas como uma grade de 5×5. X observado aproximadamente: -85, -42.5, 0, 42.5, 85; Y: 64, 32, 0, -32, -64. O valor Z é próximo de zero.
- Uma sequência candidata de índices começa em `base + 0x0F4C`, com 96 valores UInt32 (384 bytes), até o offset relativo `+0x10CC).
- Os índices observados variam de 0 a 24; as duas listas comparadas para A/B eram idênticas.
- 96 índices agrupados em trios representam 32 triângulos. Os trios examinados não repetiam o mesmo índice dentro do triângulo.
- A geometria resultante é consistente com uma grade de 5×5 vértices, subdividida em 16 células e dois triângulos por célula.
- Foram exportados OBJ de teste `mesh_grid_A.obj` e `mesh_grid_B.obj`; cada arquivo reportado tinha 25 posições, 25 coordenadas UV e 32 faces. A inspeção no Blender mostrou uma grade plana.

**Limitações:** a função visual da malha não foi determinada. Ela pode representar um fragmento de interface ou outra superfície. A atribuição UV para o último vértice foi inferida e ainda precisa de confirmação. Os OBJ de teste são artefatos locais não incluídos aqui.

### Outras sequências candidatas

Foram observadas muitas ocorrências do padrão de 32 bits `FFFFFFFF` no campo a +8 de registros candidatos: uma varredura registrou 4,744 ocorrências e 55 sequências. Esse padrão sozinho não prova que cada sequência seja um array de vértices.

Exemplos de regiões investigadas:

- `0x001A8924`
- `0x001AB378`
- `0x001ADAB4` — 32 registros / 768 bytes; uma sequência associada apresentou 48 índices e 16 triângulos sem índices fora do intervalo.
- `0x001AEB84`
- `0x001AF054`
- `0x001AD55C` foi usada como início de uma sequência nomeada provisoriamente `Malha_9B`.

Comparações A/B/C reportadas apresentaram listas de índices iguais e coordenadas UV iguais em casos específicos. As diferenças máximas de XYZ medidas em uma comparação foram aproximadamente 1.5e-05 ou 3.8e-06. Isso sugere cópias quase idênticas em alguns campos, mas não esclarece a semântica dos blocos.

## `custom.ccs`

### Marcadores CCMF

Foram registrados 28 marcadores nos offsets:

`0x608, 0x89C, 0xA74, 0xBA4, 0xF30, 0x1124, 0x12B4, 0x1CB8, 0x23C4, 0x26A0, 0x2C6C, 0x3528, 0x40D4, 0x4F7C, 0x50E0, 0x52D0, 0x54E8, 0x6620, 0x7638, 0x77D0, 0x7968, 0x7B00, 0x7C98, 0x7DE8, 0x7F18, 0x80E8, 0x8258, 0x8388`.

Os blocos iniciados em `0x7638`, `0x77D0`, `0x7968` e `0x7B00` têm distância de 0x198 bytes entre marcadores consecutivos (408 bytes).

### Campos candidatos de geometria

Nos quatro blocos, valores em offsets relativos `+0x078` e `+0x098` foram registrados como 4 e 2, respectivamente. Isso é compatível com contagens de vértices e triângulos, mas a interpretação ainda não está confirmada.

Registros candidatos foram examinados em `+0x0A0`, `+0x0B8`, `+0x0D0` e `+0x0E8`, com passo 0x18. Os campos incluem floats, sequências de quatro bytes semelhantes a cores e coordenadas X/Y/Z. A geometria se assemelha a um quadrilátero com X próximo de ±64 e Y próximo de ±12 ou ±13. A interpretação exata de todos os campos, incluindo UVs, não está estabelecida.

Os valores no campo `+0x0FC` variaram conforme o registro, com sequência observada incluindo 2, 0, 3 e 1; pode haver dados de índices/atributos nessa região, mas a topologia ainda precisa ser validada pelo layout completo.

### Comparação dos blocos

Os quatro blocos têm os primeiros 32 bytes iguais, com exceção do bloco `0x7B00`, em que `+0x028` contém `17 B7 51 38` (float aproximado de 5e-05), em vez de zeros.

As comparações contra `0x7638` encontraram diferenças localizadas em campos de floats, bytes semelhantes a cores e valores próximos do fim do bloco. Os offsets `+0x18C` foram observados como 20, 21, 22 e 23 nos quatro blocos. No bloco `0x7B00`, `+0x188` foi 82, enquanto nos demais foi 100. A semântica desses campos é desconhecida.

Outros campos em `+0x118` a `+0x154` contêm valores que, interpretados como floats/inteiros, incluem 0.8, 1.0, 0.9 e pequenos inteiros. Ainda não há evidência suficiente para nomeá-los como parâmetros específicos de renderização.

O bloco iniciado em `0x8388` termina no fim do arquivo e mede 119,692 bytes (`0x1D38C`). Ele pode conter dados compostos ou estruturas aninhadas; não deve ser presumido como um único objeto.

## Referências textuais de textura

Nomes como `TEX_sel01`, `TEX_seleff01`, `TEX_xbg00` e `TEX_sel02` aparecem em referências no CCS. São indícios de recursos nomeados, não prova de que a imagem correspondente esteja embutida no mesmo arquivo.

## Critério de confiança

- **Observação:** marcador, byte, tamanho, contagem ou valor medido.
- **Hipótese:** interpretar um campo como posição, UV, cor, índice, contagem ou parâmetro.
- **Não concluído:** finalidade da malha, mapeamento UV definitivo, formato integral dos recursos de textura e semântica dos campos desconhecidos.


### Análise detalhada dos quatro blocos de 0x198 bytes (atualização 2026-10-16)

Foi feita uma leitura em palavras de 4 bytes dos intervalos relativos `+0x090` a `+0x0FC` dos blocos `0x7638`, `0x77D0`, `0x7968` e `0x7B00`. A saída hexadecimal confirma quatro registros com início aparente em `+0x0A0`, `+0x0B8`, `+0x0D0` e `+0x0E8`, espaçados por `0x18` (24 bytes).

Cada registro de 24 bytes tem o padrão observado:
- dword em `+0x00` do registro: valor interpretável como float;
- dword em `+0x04`: bytes semelhantes a cor empacotada, por exemplo `6A 6A 6A FF`, `66 66 66 FF` e `69 69 69 FF`;
- dwords em `+0x08`, `+0x0C`, `+0x10`: coordenadas interpretáveis como X, Y e Z;
- dword em `+0x14`: float 0 ou 1 nos exemplos.

Valores decodificados observados:

| Bloco | Relativo | Float inicial | Bytes de cor | X aprox. | Y aprox. | Z aprox. | Último float |
|---|---:|---:|---|---:|---:|---:|---:|
| `0x7638` | `+0x0A0` | 0.8125 | `6A 6A 6A FF` | -64 | 12 | 0 | 1 |
| `0x7638` | `+0x0B8` | 0.8125 | `66 66 66 FF` | 64 | 12 | 0 | 0 |
| `0x7638` | `+0x0D0` | 1.0 | `66 66 66 FF` | -64 | -12 | 0 | 1 |
| `0x7638` | `+0x0E8` | 1.0 | `66 66 66 FF` | 64 | -12 | 0 | 0 |
| `0x77D0` | `+0x0A0` | 0.625 | `66 66 66 FF` | -64 | 12 | 0 | 1 |
| `0x77D0` | `+0x0B8` | 0.625 | `66 66 66 FF` | 64 | 12 | 0 | 0 |
| `0x77D0` | `+0x0D0` | 0.8125 | `66 66 66 FF` | -64 | -12 | 0 | 1 |
| `0x77D0` | `+0x0E8` | 0.8125 | `66 66 66 FF` | 64 | -12 | 0 | 0 |
| `0x7968` | `+0x0A0` | 0.4375 | `66 66 66 FF` | -64 | 12 | 0 | 1 |
| `0x7968` | `+0x0B8` | 0.4375 | `69 69 69 FF` | 64 | 12 | 0 | 0 |
| `0x7968` | `+0x0D0` | 0.625 | `66 66 66 FF` | -64 | -12 | 0 | 1 |
| `0x7968` | `+0x0E8` | 0.625 | `66 66 66 FF` | 64 | -12 | 0 | 0 |
| `0x7B00` | `+0x0A0` | 0.234375 | `66 66 66 FF` | -64 | 13 | aprox. -0.00005 | 1 |
| `0x7B00` | `+0x0B8` | 0.234375 | `66 66 66 FF` | 64 | 13 | aprox. -0.00005 | 0 |
| `0x7B00` | `+0x0D0` | 0.4375 | `66 66 66 FF` | -64 | -13 | aprox. -0.00005 | 1 |
| `0x7B00` | `+0x0E8` | 0.4375 | `66 66 66 FF` | 64 | -13 | aprox. -0.00005 | 0 |

Os floats de posição são aproximações decimais de valores codificados em binário; a exibição arredondada de PowerShell não deve ser confundida com valores binários exatos. No bloco `0x7B00`, os três campos Z observados contêm os bytes `15 B7 51 B8`, que correspondem aproximadamente a `-0.00005`; o primeiro campo em `+0x028` contém `17 B7 51 38`, aproximadamente `+0.00005`, enquanto os blocos anteriores têm zero nesse local.

A organização espacial dos quatro registros — X próximo de `-64, +64, -64, +64`, Y próximo de `+12, +12, -12, -12` (ou `±13` no quarto bloco) — é compatível com os quatro cantos de um retângulo. O float inicial varia por linha vertical, enquanto o último float alterna entre 1 e 0 por coluna horizontal. **Hipótese a testar:** esses dois floats podem formar as duas coordenadas de textura (UV, possivelmente em ordem V/U); a interpretação não foi validada por renderização com textura de teste. Os bytes semelhantes a cor também não tiveram a ordem de canais confirmada.

O dword em `+0x18C` dos blocos contém 20, 21, 22 e 23, respectivamente; em `+0x188`, os valores observados são 100 nos três primeiros blocos e 82 no bloco `0x7B00`. A semântica desses campos continua desconhecida.

Esta atualização refina a descrição anterior dos registros como candidatos a vértices, mas **não confirma** o formato geral de todos os blocos CCMF nem a função visual dos retângulos.
