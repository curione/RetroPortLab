# Análise de recorrência de trechos CCMF em `charsel1.ccs`

**Data da análise:** 2026-10-18  
**Escopo:** análise binária estática, somente leitura. O arquivo original do jogo não é distribuído neste repositório.

## Arquivo e inventário

- Arquivo analisado: `charsel1.ccs`, obtido localmente a partir de uma cópia do usuário.
- Tamanho observado: 2.187.468 bytes (`0x2160CC`).
- Ocorrências literais de `CCMF`: 32.
- Foram testadas 64 janelas de 112 bytes: duas por marcador, nos deslocamentos relativos `+0x384` e `+0x804`. Todas estavam dentro dos limites do arquivo.

Os deslocamentos relativos são posições de busca experimentais, não campos de uma estrutura presumida.

## Referência de 112 bytes

A sequência de referência tem SHA-256:

```text
51dfa76cfb5e1eaafcdcd3a11316fa9a8ba305344606b9471966cb8f10714496
```

A busca global encontrou exatamente quatro ocorrências dessa sequência:

| Marcador associado observado | Deslocamento relativo | Offset absoluto |
|---|---:|---:|
| `0x001ADE90` | `+0x384` | `0x001AE214` |
| `0x001ADE90` | `+0x804` | `0x001AE694` |
| `0x001AF748` | `+0x384` | `0x001AFACC` |
| `0x001AF748` | `+0x804` | `0x001AFF4C` |

O texto `TEX_seleff01` começa 36 bytes (`+0x24`) depois do início de cada uma das quatro ocorrências. A mesma string foi encontrada uma quinta vez em `0x0000228C`, mas os bytes ao redor não constituem outra cópia do trecho de referência. Na janela alinhada a partir de `0x00002268`, o maior trecho contíguo de bytes iguais tem 31 bytes.

Não foram encontradas outras cópias integrais dos 112 bytes, nem correspondências parciais de 96 a 111 bytes, na busca global descrita.

## Contextos compartilhados

### Ocorrências em `+0x384`

As regiões abaixo são idênticas byte a byte:

- `[0x001ADE9D, 0x001AE2C0)`
- `[0x001AF755, 0x001AFB78)`

Comprimento: **1.059 bytes** (`0x423`). Os intervalos correspondem aos contextos dos marcadores `0x001ADE90` e `0x001AF748`, respectivamente, em deslocamentos relativos `+0x0D` até `+0x430`, fim exclusivo.

Nos 64 bytes anteriores às ocorrências em `+0x384`, os contextos são iguais. Nos 64 bytes posteriores, 61 bytes coincidem; os três bytes divergentes ficam no início do contexto posterior, em posições relativas `+0x430..+0x432` dos marcadores.

### Ocorrências em `+0x804`

As regiões abaixo são idênticas byte a byte:

- `[0x001AE675, 0x001AE740)`
- `[0x001AFF2D, 0x001AFFF8)`

Comprimento: **203 bytes** (`0xCB`), nos deslocamentos relativos `+0x7E5` até `+0x8B0`, fim exclusivo. Essa sequência inclui 31 bytes anteriores e 60 posteriores aos 112 bytes de referência.

Entre ocorrências em `+0x384` e ocorrências em `+0x804`, a sequência comum contígua termina nos 112 bytes centrais; os bytes imediatamente adjacentes não prolongam essa igualdade.

## Resultado da varredura das posições relativas

Para cada um dos 32 marcadores, foram examinadas janelas de 112 bytes em `+0x384` e `+0x804`. Apenas as quatro janelas listadas acima coincidiram integralmente com a referência. As 64 janelas tinham 112 bytes disponíveis, sem truncamento.

Em outras janelas foram observadas strings `TEX_` distintas, como `TEX_sel00`, `TEX_sel01`, `TEX_sel_txt00`, `TEX_sel_txt01`, `TEX_selbg00` e `TEX_sel_scroll`. A presença dessas strings, por si só, não demonstra que a janela tenha a mesma função ou layout.

## Fatos, hipóteses e limitações

### Fatos observados

- Há 32 ocorrências literais de `CCMF` no arquivo examinado.
- A sequência de referência de 112 bytes ocorre exatamente quatro vezes no arquivo, nas posições listadas.
- As quatro ocorrências contêm `TEX_seleff01` no deslocamento interno `+0x24`.
- As duas ocorrências em `+0x384` estão dentro de uma região compartilhada de 1.059 bytes.
- As duas ocorrências em `+0x804` estão dentro de uma região compartilhada de 203 bytes.
- A ocorrência adicional de `TEX_seleff01` em `0x0000228C` não é uma cópia integral da referência.

### Hipóteses plausíveis

A recorrência em deslocamentos relativos iguais e os contextos compartilhados são compatíveis com reutilização de conteúdo em regiões organizadas de maneira semelhante.

### Não demonstrado

Estes resultados não demonstram que os marcadores `CCMF` delimitem recursos completos, nem que `+0x384` ou `+0x804` sejam ponteiros, tamanhos, IDs ou campos semânticos. Também não identificam o trecho como geometria, material, textura incorporada ou outro tipo de recurso.

### Próximo teste

Repetir a busca em outro arquivo CCS já extraído, começando pela validação de tamanho, hash e inventário de marcadores. Comparar as sequências de bytes e os contextos sem presumir que os arquivos compartilhem o mesmo layout.

## Reprodutibilidade e distribuição

Os offsets, comprimentos, contagens e hash da sequência de referência são transcritos dos resultados da análise local. O relatório não inclui o conteúdo binário do jogo. Os hashes de duas janelas não correspondentes ficaram incompletos na transcrição original e foram omitidos deliberadamente; não são necessários para as conclusões acima.
