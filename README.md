# RetroPortLab

Pesquisa técnica e desenvolvimento experimental de ferramentas para compreender formatos de arquivos de jogos e estudar a viabilidade de ports nativos.

> **Estado:** pesquisa exploratória de formatos e geometria. Ainda não existe um port jogável. Resultados preliminares devem ser validados antes de orientar uma implementação.

## Objetivos

- Inventariar recursos extraídos legalmente de uma cópia do usuário.
- Documentar formatos binários e estruturas ainda não totalmente compreendidas.
- Desenvolver ferramentas de análise reproduzíveis.
- Investigar a extração/conversão de geometria, texturas, áudio e dados de interface.
- Avaliar uma arquitetura multiplataforma, começando por PC Windows; Android e Nintendo Switch são objetivos posteriores, sujeitos a viabilidade técnica e às regras de cada plataforma.

## Estado atual

A pesquisa inicial usa recursos CCS do diretório `21btlsel` de *Naruto Shippuuden - Narutimate Accel 3* (PSP). O trabalho está concentrado em identificar estruturas `CCMF`, possíveis registros de vértices e listas de índices.

Principais resultados preliminares:

- Foram identificados 32 marcadores `CCMF` no arquivo `charsel1.ccs` analisado.
- Uma região em `charsel1.ccs` apresenta uma malha candidata coerente com 25 vértices, 96 índices e 32 triângulos.
- Os arquivos OBJ de teste `mesh_grid_A.obj` e `mesh_grid_B.obj` foram exportados para inspeção visual.
- Em `custom.ccs`, foram encontrados 28 marcadores `CCMF`; quatro blocos consecutivos de 0x198 bytes têm campos semelhantes e diferenças localizadas.
- Referências textuais como `TEX_sel01` foram encontradas, mas isso não comprova que os pixels da textura estejam embutidos no arquivo CCS.

Consulte [as notas de engenharia reversa](docs/REVERSE_ENGINEERING.md), [as notas de formato](docs/FORMAT_NOTES.md) e o [roteiro](docs/ROADMAP.md).

## Estrutura do repositório

- `docs/` — caderno de pesquisa, hipóteses e próximos passos.
- `reports/` — relatórios textuais revisáveis; não incluir dumps grandes ou recursos originais sem necessidade.
- `scripts/` — analisadores e utilitários cuja procedência e funcionamento tenham sido verificados.

## Princípios de documentação

Cada observação deve ser classificada como:
- **Observado:** bytes, offsets, tamanhos ou contagens medidos diretamente.
- **Interpretação:** explicação provável, ainda não confirmada.
- **Pendente:** teste necessário para distinguir hipóteses.

Não publicar ISOs, arquivos originais do jogo, áudio, imagens ou outros recursos protegidos. Publicar apenas documentação e ferramentas próprias, evitando material derivado que não seja necessário para explicar a pesquisa.

## Ambiente de referência

O ambiente de análise registrado usa Windows, PowerShell, Python e Blender. Ferramentas CRI foram utilizadas para extrair arquivos do contêiner CPK. A configuração de máquina e versões específicas deve ser registrada novamente quando necessária para reproduzir um resultado.

## Aviso

Este repositório documenta pesquisa de interoperabilidade e formatos. Não inclui os arquivos comerciais do jogo nem pretende substituir sua cópia legítima. A possibilidade de distribuição e execução em cada plataforma depende dos requisitos técnicos, licenças e regras aplicáveis.
