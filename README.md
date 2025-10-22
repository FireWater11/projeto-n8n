# Projeto-n8n Steam Promotions Notifier
---
Desenvolvido por: Luidi Augusto
Curso Aprender e Crescer(treinamento de n8n)

# O que o código faz?

# 1. Agendamento (Schedule Trigger)

Executa automaticamente todos os dias ao meio-dia (12h) (no prototipo a partir de um clique)

# 2. Busca de Promoções (HTTP Request)

Consulta a API do CheapShark para pegar as 60 melhores ofertas da Steam(somente Steam, devido ao id da loja ser = 1
Retorna lista de jogos em promoção

# 3. Processamento em Lotes

Divide os jogos em grupos de 2 para não sobrecarregar as APIs (tambem devido ao limite do Webhook da API do Discord)
Para cada jogo, busca informações detalhadas

# 4. Enriquecimento de Dados (2 etapas)
Primeira busca (preço BR):

Consulta API Steam para pegar preço em Reais (em Reais, nao poderia ser pego diretamente da API do CheapShark, por que os valores sao retornados em dolar, e o valor convertido nao bate com o preço real)
Pega porcentagem de desconto oficial da Steam

Segunda busca (detalhes completos):

Nome do jogo
Preço original vs preço final
Avaliações dos usuários (total de recomendações)
Nota do Metacritic (caso disponível)

# 5. Sistema de Recomendação Inteligente
Calcula automaticamente se vale a pena comprar baseado em:
Status gerados:

"COMPRE AGORA" - Desconto ≥90% OU preço ≤R$3 OU (desconto ≥75% E preço ≤R$10)
"VALE MUITO A PENA" - Desconto ≥75%
"VALE A PENA" - Desconto ≥60%
"BOM NEGÓCIO" - Desconto ≥50%
"VALE SE VOCÊ QUISER" - Desconto ≥40%
"SÓ SE ESTIVER NA WISHLIST" - Desconto ≥25%
"NÃO VALE A PENA" - Desconto ≥10%
"ESPERE PROMOÇÃO MELHOR" - Desconto <10%

Avisos especiais:

"NÃO RECOMENDADO" - Metacritic <60 e desconto <75%
"PESQUISE ANTES" - Menos de 100 avaliações e desconto <50%

# 6. Classificação de Popularidade
Categoriza jogos por total de avaliações:

Extremamente Popular - 100k+ avaliações
Muito Popular - 50k+ avaliações
Popular - 10k+ avaliações
Bem Avaliado - 5k+ avaliações
Poucas Avaliações - 100-500

# 7. Envio para Discord

Formata mensagens em lotes de 5 jogos
Limita mensagens a 1900 caracteres (limite da API do Discord)
Envia via webhook para canal específico(nesse caso um Grupo de teste)
Devido a limitações, so é enviado 25 jogos por vez iniciado, então é algo para demonstração e teste, caso fosse algo mais profissional, seria necessaria outra API/Webhook.

