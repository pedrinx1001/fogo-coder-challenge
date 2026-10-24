# F.O.G.O. — Ferramenta Onisciente de Gestão e Operações
Coder Challenge 2026 · Etapa Bônus (Operação Next)

## Como executar
1. Descompacte o .zip.
2. Abra `fogo/index.html` no navegador (Chrome, Edge, Brave ou Firefox).

Não há instalação, build nem servidor: é um único arquivo HTML com CSS e JavaScript puro (sem bibliotecas externas e sem internet).
Os ícones são SVG embutidos (traço de 1,8 px, grade 24×24). O mapa é SVG.

## Persistência (limitação)
Os dados ficam no `localStorage` do navegador (chave `fogo3`), ou seja, **somente naquele navegador e naquela máquina**. Não existe banco de dados.
Para levar os dados a outra máquina use Inventário → Exportar/Importar JSON. "Restaurar dados de exemplo" recria os 64 equipamentos e 14 agentes demonstrativos.

## Dados demonstrativos
Equipamentos, agentes, histórico de manutenção, histórico de 90 dias de ocorrências e a "telemetria" (caixa "Simular telemetria") são **gerados para demonstração**.
O mapa é esquemático (coordenadas projetadas em um plano), não um mapa geográfico real; isso está indicado na própria tela.

## Regras de negócio (única fonte de verdade: `S.eq` e `S.ag`)
- **Total** = operacionais + indisponíveis + ainda não instalados.
- **Indisponíveis** = mau funcionamento + em manutenção + desativado + furtado.
- **Disponibilidade** = operacionais ÷ instalados (equipamentos "ainda não instalados" ficam fora do denominador). Ex.: 41 ÷ 55 = 74,5%.
- **Agentes em atividade** = agentes com situação "ativo" entre os exibidos.
- **Manutenção:** imediata (marcada como imediata, ou mau funcionamento), vencida (data anterior a hoje), próxima (até 7 dias), agendada, sem pendência.
- **Alertas:** no máximo um por equipamento, na maior gravidade: furtado ou mau funcionamento com manutenção imediata = crítico; manutenção imediata, mau funcionamento sem manutenção marcada ou manutenção vencida = alto; manutenção próxima ou desativado = atenção. Há também alertas de região quando a disponibilidade fica abaixo do limite.
- **Situação da região:** `CFG.disp = [90, 80, 65]` no código. Disponibilidade < 90% ou 3+ alertas altos = Atenção; < 80% = Elevado; < 65% = Crítico. `CFG.dias = 7` define "manutenção próxima".

## Funcionalidades
**Central de operações:** indicadores, filtros (busca, tipo, situação, manutenção, região, órgão, função do agente), mapa com ícones vetoriais por tipo, agrupamento de marcadores ao afastar, zoom e arraste, destaque do item selecionado, dica ao passar o mouse, alertas priorizados que levam ao item no mapa, painel de detalhes por grupos (com campos específicos por tipo), troca de situação e registro de manutenção concluída, visão por região (clique filtra o mapa).
**Inventário:** cadastro, edição, exclusão com confirmação e busca de equipamentos e agentes, com validação (campos obrigatórios, CEP, GPS, série e matrícula únicas). Exporta JSON/CSV; importa JSON.
**Cidade preditiva:** risco das próximas 24 h por região e recomendação de agentes, comportamento fora do padrão (z-score) e tendência de 14 dias, equipamentos com manutenção recorrente e mapa de calor semanal. Cada bloco explica por que a estimativa foi gerada, quais dados usou e as limitações.

## Uso de IA
Desenvolvido com apoio do **Claude (Anthropic)**, que escreveu e revisou o código a partir do enunciado. O participante testou e deve ser capaz de explicar a estrutura e as regras acima.

## Testes realizados
Execução automatizada em Chromium (Playwright): 41 verificações, 0 falhas, sem erros no console. Cobrem os cálculos (reconciliação do total, soma das regiões, percentuais), seleção de marcador e alerta, filtros, busca, cadastro/edição/remoção, validações, persistência após recarregar, página preditiva e ausência de rolagem horizontal em 1920, 1440, 1024, 768 e 390 px.
