# Visão Web

Ecossistema web do Visão (TRE-MA/SEASU-COINF-STIC) - Trilha B do
projeto: Gerenciamento Web, Mobile (leitura) e Painel de TV, construídos
sobre Google Apps Script + a mesma planilha já usada pela Visão Desktop
(repositório irmão: [Visao-TRE](https://github.com/gamcastro/Visao-TRE)).

## Status

**Fase 1 (dados) - concluída.** A Visão Desktop (WinForms e WPF) publica
automaticamente o resultado de cada varredura na aba `INVENTARIO` da
planilha, via um Web App do Apps Script dedicado
(`apps_script_publicar_inventario.gs`, no repositório `Visao-TRE`) - é a
base de dados que as telas abaixo vão consumir.

**Fase 2 (dashboard Web) - em andamento.**

## Conteúdo deste repositório

- `dados_municipios_ma.geojson` - malha dos 217 municípios do Maranhão
  (geometria + nome), montada a partir de duas APIs públicas do IBGE:
  - Geometria: `servicodados.ibge.gov.br/api/v3/malhas/estados/21` (formato GeoJSON, por município)
  - Nomes: `servicodados.ibge.gov.br/api/v1/localidades/estados/21/municipios`
  - Cada `feature.properties` tem `codarea` (código IBGE do município) e
    `nome` (nome oficial, com acentuação) - usado pra ligar cada Zona
    Eleitoral (campo `Sede` da aba `Zonas`) ao município correspondente
    no mapa.

## Arquitetura planejada (ver plano completo no repo `Visao-TRE`)

- **Sem OAuth customizado nas views** - Apps Script Web App implantado
  com "Executar como: Usuário com acesso ao app da Web" + "Quem pode
  acessar: Qualquer pessoa em TRE-MA..." (restrição nativa de domínio do
  próprio Apps Script, confirmada com o mesmo padrão já usado pelo DICON
  Web) - login automático do Google, sem escrever nenhum código de OAuth.
- **3 views, 1 Web App**: `?app=web` (dashboard completo), `?app=mobile`
  (espelho responsivo, só leitura), `?app=tv` (tela cheia + polling).
- **Paleta por campanha** (4 estados por zona): cinza (nunca escaneada),
  vermelho (escaneada, 0 máquinas prontas), laranja (1 pronta), verde (2+
  prontas). Município com várias zonas pintado pelo pior status, 1 pino
  por zona sobreposto ao polígono do município.
- Publicado/gerenciado via `clasp` (CLI oficial do Apps Script).
