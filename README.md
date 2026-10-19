# RetroPortLab

Pesquisa técnica e desenvolvimento experimental de ferramentas para compreender formatos de arquivos de jogos e estudar a viabilidade de ports nativos.

> **Estado:** pesquisa exploratória de formatos e infraestrutura de visualização nativa. Ainda não existe um port jogável. Resultados preliminares devem ser validados antes de orientar uma implementação.

## Objetivos

- Inventariar recursos extraídos legalmente de uma cópia do usuário.
- Documentar formatos binários e estruturas ainda não totalmente compreendidas.
- Desenvolver ferramentas de análise reproduzíveis.
- Investigar geometria, texturas, áudio e dados de interface.
- Focar inicialmente no PC Windows; Android e Nintendo Switch estão adiados e dependem de viabilidade técnica e requisitos aplicáveis.

## Estado atual

A pesquisa usa recursos CCS de *Naruto Shippuuden - Narutimate Accel 3* (PSP). O renderizador experimental nativo Win32/WGL/OpenGL já valida a apresentação de PNGs e malhas OBJ texturizadas com recursos sintéticos. Isso comprova a infraestrutura do visualizador, não a interpretação do formato gráfico proprietário.

Principais documentos:

- [Índice central de descobertas](docs/RETROPORTLAB_FINDINGS_INDEX.md)
- [Progresso do visualizador nativo (2026-10-19)](docs/NATIVE_VIEWER_PROGRESS_2026-10-19.md)
- [Caderno de engenharia reversa](docs/REVERSE_ENGINEERING.md)
- [Notas de formato](docs/FORMAT_NOTES.md)
- [Inventário de recursos](docs/RESOURCE_INVENTORY.md)
- [Análise de recorrência CCMF](docs/CCMF_REPETITION_ANALYSIS.md)
- [Roteiro](docs/ROADMAP.md)

## Princípios de documentação

Classifique cada resultado como observado, inferência ou hipótese. Não trate a presença de `CCMF` como prova de limite de recurso. Não publique ISOs, executáveis protegidos, recursos originais ou extraídos do jogo, capturas ou modelos experimentais derivados quando não forem necessários para explicar a pesquisa.

## Aviso

Este repositório documenta pesquisa de interoperabilidade e formatos. Não inclui os arquivos comerciais do jogo nem pretende substituir sua cópia legítima. A possibilidade de distribuição e execução em cada plataforma depende dos requisitos técnicos, licenças e regras aplicáveis.
