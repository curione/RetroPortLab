# Roteiro do RetroPortLab

## Fase 1 — Consolidar a pesquisa atual

- [x] Inventariar arquivos CCS de seleção extraídos.
- [x] Identificar ocorrências CCMF em `charsel1.ccs` e `custom.ccs`.
- [x] Encontrar ao menos uma lista candidata de índices coerente com 25 vértices e 32 triângulos.
- [x] Exportar uma malha candidata para OBJ e inspecioná-la no Blender (artefatos locais).
- [x] Comparar quatro blocos de 0x198 bytes em `custom.ccs`.
- [ ] Reexecutar as contagens com scripts versionados e guardar logs reproduzíveis.
- [ ] Confirmar o layout dos registros de 24 bytes e o mapeamento UV.

## Fase 2 — Ferramentas de análise

- [ ] Versionar o analisador Python, após revisar e remover caminhos locais e dependências não documentadas.
- [ ] Criar testes automatizados para detecção de marcadores e limites de bloco.
- [ ] Adicionar exportação OBJ com validação explícita de vértices, UVs e índices.
- [ ] Produzir relatório por arquivo com hashes, offsets, tamanhos e contagens.
- [ ] Adicionar detecção de estruturas fora do intervalo e avisos de baixa confiança.

## Fase 3 — Recursos gráficos

- [ ] Determinar onde os dados de textura realmente residem.
- [ ] Mapear referências `TEX_...` para recursos identificáveis.
- [ ] Validar formato de cor, orientação e coordenadas UV usando texturas de teste.
- [ ] Identificar geometria de mais de um arquivo para evitar conclusões baseadas em um único exemplo.

## Fase 4 — Prova de conceito no PC

- [ ] Definir um renderizador de teste independente do jogo original.
- [ ] Carregar malhas exportadas e texturas sintéticas.
- [ ] Criar uma cena de teste que comprove o pipeline de leitura e renderização.
- [ ] Separar código de parsing, representação intermediária e renderização.

## Fase 5 — Viabilidade de port

- [ ] Identificar dependências de lógica, física, animação, áudio, interface e controle.
- [ ] Determinar o que pode ser reimplementado e o que depende de formatos ou comportamento ainda desconhecidos.
- [ ] Estabelecer primeiro um alvo Windows para depuração.
- [ ] Avaliar Android e Nintendo Switch separadamente, incluindo desempenho, APIs, ferramentas oficiais e requisitos legais/técnicos.

## Critérios de conclusão

Um formato só deve ser considerado compreendido quando o parser conseguir processar vários exemplos, validar limites e gerar saídas verificáveis. Uma malha exibida com sucesso é um marco de análise, não prova de que o jogo completo possa ser portado.
