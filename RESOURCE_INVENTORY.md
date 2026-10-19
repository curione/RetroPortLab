# Inventário de recursos analisados

Este inventário registra nomes, tamanhos e observações sem distribuir os arquivos comerciais originais.

## Ambiente local

Diretório de trabalho informado durante a pesquisa:

`C:\RetroPortLab\analysis\selection_resources\21btlsel\`

Arquivos CCS citados:

| Arquivo | Tamanho/estado conhecido | Observações |
|---|---|---|
| `charsel1.ccs` | 2,187,468 bytes | 32 marcadores CCMF; análise de malhas candidatas |
| `custom.ccs` | 153,364 bytes | 28 marcadores CCMF; blocos de 0x198 bytes comparados |
| `mapsel1.ccs` | tamanho não registrado aqui | varredura de referências de textura |
| `network.ccs` | tamanho não registrado aqui | varredura de referências de textura |
| `setting.ccs` | tamanho não registrado aqui | varredura de referências de textura |
| `vs.ccs` | tamanho não registrado aqui | varredura de referências de textura |

Outros nomes citados no processo de extração: `data.cpk`, `bgm.afs`, `evvoice.afs`, `plvoice.afs` e `stream.afs`. Esses arquivos não são incluídos neste repositório.

## Referências de textura encontradas

Uma varredura textual registrou nomes como:

- `TEX_sel01`
- `TEX_seleff01`
- `TEX_xbg00`
- `TEX_sel02`
- `TEX_sel_asw`
- `TEX_sel_ymt`

A presença dos nomes não confirma onde os pixels estão armazenados nem qual é o formato da imagem.

## Artefatos de análise mencionados

Estes artefatos foram produzidos ou citados localmente, mas não estão incluídos aqui:

- `reports\charsel1_heatmap_512.png`
- `reports\charsel1_heatmap_1024.png`
- `reports\charsel1_heatmap_2048.png`
- `reports\vertices_1ADAB4.csv`
- `reports_v2\mesh_grid_A.obj`
- `reports_v2\mesh_grid_B.obj`

Antes de adicionar qualquer relatório futuro, revisar se ele contém bytes de recursos protegidos, imagens extraídas, caminhos de usuário, metadados privados ou dados desnecessários.

## Procedência e reprodutibilidade

Os resultados resumidos aqui foram fornecidos a partir da análise local no Windows. Para tornar o processo totalmente reproduzível, a próxima etapa é versionar os scripts próprios, registrar suas versões/dependências e anexar logs textuais contendo hashes SHA-256 dos arquivos analisados. Os hashes ainda não foram registrados neste documento.
