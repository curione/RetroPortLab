# RetroPortLab — índice central de descobertas

Atualizado em 2026-10-19. Este índice separa resultados observados de hipóteses e aponta para a documentação versionada neste repositório.

## Estado geral

O foco atual é exclusivamente a prova de conceito nativa para Windows. Android e Nintendo Switch ficam adiados. **Ainda não existe um port jogável do jogo**: existe uma infraestrutura experimental de visualização e ferramentas de análise, enquanto a interpretação dos recursos gráficos proprietários permanece incompleta.

## Descobertas e ferramentas documentadas no repositório

- [Caderno de engenharia reversa](REVERSE_ENGINEERING.md) — observações de CCS/CCMF, offsets e hipóteses com limitações explícitas.
- [Notas de formato](FORMAT_NOTES.md) — notas sobre assinaturas e estruturas ainda não compreendidas.
- [Inventário de recursos](RESOURCE_INVENTORY.md) — visão geral dos recursos investigados.
- [Análise de recorrência CCMF](CCMF_REPETITION_ANALYSIS.md) — comparação de trechos repetidos sem assumir semântica.
- [Roteiro do projeto](ROADMAP.md) — etapas e critérios para validar interpretações.
- [Progresso do visualizador nativo (2026-10-19)](NATIVE_VIEWER_PROGRESS_2026-10-19.md) — resumo consolidado dos marcos recentes do renderizador e das limitações restantes.

## Fatos observados

- Em `charsel1.ccs`, foram inventariadas 32 ocorrências literais de `CCMF`; em `custom.ccs`, 28. A presença do marcador não prova que ele delimite um recurso completo.
- O inventário local de recursos reportou 1.336 arquivos extraídos e 1.092 com `CCSF` no offset `0x08`. Esses números são resultados relatados da análise local, não uma especificação do formato.
- Há uma região de bytes interpretada experimentalmente como uma malha de 25 vértices e 96 índices (32 triângulos), mas o layout e a semântica dos dados CCS continuam sem confirmação independente.
- Em `custom.ccs`, os tamanhos usados no inventário coincidem com a distância até o próximo marcador e o último intervalo vai até o fim do arquivo. Isso é consequência do método de segmentação e não prova limites reais de recursos.
- A investigação estática do EBOOT confirmou um cabeçalho `~PSP`, mas não identificou um payload ELF legível, arquitetura executável, entry point real ou tabelas ELF.
- A auditoria local não encontrou uma ferramenta de captura PSP GE já disponível para rastrear desenho, buffers e proveniência até offsets CCS.
- Uma auditoria de referências nominais reportou que 590 dos 595 identificadores CCS únicos em `ccsName.strtbl` também aparecem em `texName.strtbl` com o prefixo `TEX_`. Isso indica uma convenção de nomenclatura compartilhada, mas não comprova referência estrutural nem associação a uma textura específica. O relatório detalhado permanece local em `reports/native_windows_ccs_cross_resource_reference_audit_20261018.md` e ainda não foi sincronizado integralmente para este repositório.

## Estado do visualizador nativo

O progresso local reportado inclui:
- leitor CCS e modos CLI de inspeção/comparação;
- renderizador Win32/WGL com OpenGL 1.1;
- modo PNG via WIC, conversão RGBA e captura visual validada;
- `GlTexture2D` com propriedade exclusiva e verificações de HDC, HGLRC e thread;
- teste explícito de múltiplas texturas;
- carregamento OBJ com UVs por canto, índices positivos/negativos e modo OBJ texturizado por PNG;
- validações de compilação, capturas e CTest relatadas como aprovadas (mais recentemente 6/6).

Esses resultados demonstram a infraestrutura de visualização sintética, PNG e OBJ. **Não** demonstram que o formato de vértices, índices, UVs ou texturas de CCS/CCMF foi decodificado corretamente.

## Hipóteses ainda abertas

- limites reais e semântica dos blocos CCMF;
- layout/stride de vértices e interpretação de campos candidatos;
- formato e proveniência de índices;
- formato das texturas internas e relação entre nomes `TEX_...` e dados de imagem; a correspondência nominal CCS/TEX observada não prova essa relação;
- relação entre recursos CCS e chamadas gráficas reais do jogo;
- viabilidade de reconstruir lógica, animações, cenas, áudio, interface e controles para um port completo.

## Próxima dependência técnica

Para avançar na relação CCS/TEX, falta uma referência estrutural ou uma observação de uso que ligue um identificador a dados de textura concretos; a ausência de correspondência externa para os nomes `TEX_sel_*` em `charsel1.ccs` permanece sem explicação. A evidência mais forte seria uma captura reproduzível em um runtime PSP autorizado que associe um evento de desenho ao conteúdo dos buffers e à proveniência dos bytes nos arquivos CCS. Até existir esse pré-requisito, não tratar hipóteses binárias como especificação e não repetir buscas sem uma hipótese nova e falsificável.

## Política de publicação

Este repositório deve conter apenas documentação e ferramentas próprias necessárias para explicar a pesquisa. Não publicar ISOs, executáveis protegidos, recursos originais/extraídos do jogo, capturas ou modelos experimentais derivados dos recursos do jogo. Os relatórios locais detalhados só devem ser adicionados depois de revisar seu conteúdo e confirmar que não contêm esses materiais.

