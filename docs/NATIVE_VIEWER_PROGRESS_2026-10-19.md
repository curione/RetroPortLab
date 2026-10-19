# Progresso do visualizador nativo — 2026-10-19

Este documento consolida o progresso recente relatado no ambiente local. Não substitui os relatórios técnicos detalhados que permanecem em `C:\RetroPortLab\reports`; esses arquivos locais não estavam disponíveis através da conexão GitHub usada para esta atualização e, portanto, não foram copiados integralmente.

## Infraestrutura implementada e validada localmente

### Renderizador Win32/WGL/OpenGL 1.1
- Foi criado um alvo separado `RetroPortViewer`, preservando o executável CLI de análise.
- Diagnósticos observaram criação de contexto, pintura, rasterização e sucesso de `SwapBuffers`.
- A rasterização sintética foi usada como teste de infraestrutura, não como validação do formato do jogo.

### Imagens PNG
- O modo `--png <caminho>` usa WIC para decodificar PNG e converter os pixels para RGBA.
- O PNG de teste local tinha dimensões de 144×80; a captura Win32 foi comparada com amostras da imagem original, com erro médio de cor reportado de 1,31 por canal em 36 amostras.
- Um PNG sintético verificou orientação, proporção e transparência.
- O hash do PNG original foi confirmado como inalterado no teste reportado.

### Propriedade da textura
- `RgbaImage` permanece independente de Win32 e OpenGL.
- `GlTexture2D` mantém propriedade exclusiva do identificador OpenGL e não permite cópia ou movimentação.
- Operações verificam HDC, HGLRC e thread; o caminho normal diagnosticado libera a textura antes de destruir o contexto WGL.
- Não foi forçada uma falha real do driver em `glTexImage2D`; esse caso permanece sem validação direta.

### Múltiplas texturas
- O modo explícito `--multi-texture-test` cria duas imagens sintéticas distintas e as exibe lado a lado.
- A captura visual confirmou cores e proporções esperadas; os IDs de textura observados foram distintos (1 e 2).
- A ordem de destruição foi diagnosticada como correta.

### OBJ texturizado
- O modo `--obj <arquivo.obj> --texture <imagem.png>` aplica PNG a uma malha OBJ.
- O parser valida UVs por canto, aceita índices positivos e negativos e rejeita referências inválidas, UVs incompletas no modo texturizado e faces não triangulares.
- Um fixture sintético com quatro quadrantes confirmou visualmente a orientação UV.
- O OBJ experimental extraído dos dados do jogo continua sem UVs completas; nenhuma UV foi inventada para ele.

### Testes relatados
A última validação reportada compilou os alvos e passou em 6/6 testes CTest. Os modos PNG, OBJ sem textura, OBJ texturizado, sintético, múltiplas texturas e diagnóstico foram exercitados em etapas de validação. Isso registra resultados fornecidos pelo ambiente local, não uma execução feita durante este commit.

## Limites técnicos atuais

- O renderizador valida a infraestrutura nativa, mas ainda não carrega diretamente recursos gráficos CCS/CCMF.
- A interpretação de malhas experimentais não está confirmada por uma chamada gráfica real do jogo.
- O EBOOT permanece com payload não identificado por análise estática; não foi executado nem extraído.
- A auditoria local concluiu que falta uma captura reproduzível em runtime PSP autorizado, com evento de desenho, buffers e proveniência até offsets dos arquivos CCS.

## Próxima etapa sugerida

Priorizar evidência independente sobre a estrutura CCS/CCMF ou obter o pré-requisito de captura PSP autorizado. Evitar novas funcionalidades genéricas do renderizador até que uma hipótese sobre recursos reais possa ser testada de forma falsificável.

## Limitação de sincronização

Este resumo e o índice central foram preparados com base nos resultados fornecidos na conversa e nos documentos que já estavam no GitHub. Os relatórios detalhados locais de implementação/auditoria não foram sincronizados individualmente nesta alteração porque seus conteúdos completos não estavam disponíveis através da conexão remota. Não se afirma que todos os arquivos locais tenham sido publicados.
