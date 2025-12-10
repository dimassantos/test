1.1 Marshall:
1.1.1 Controller:
	Base URL: /asphalt/dosages/marshall
	Descrição geral: Este módulo gerencia o fluxo de criação,  verificação, cálculo e salvamento de dosagens Marshall, desde a inicialização até a última etapa. Cada uma dessas etapas da dosagem (do passo 1 ao 8) possui endpoints próprios, responsáveis por processar e armazenar dados de maneira progressiva.
	1.1.1.1 Submódulo: Inicialização da dosagem Marshall

Método GET(URL: /all/:id): Retorna todas as dosagens Marshall associadas a um usuário.

Método POST(URL /verify-init/:id): O que faz: Verifica se é possível iniciar uma dosagem com os dados enviados.

Método GET (URL /material-selection/:id): Lista todos os materiais do usuário que possuem ensaios necessários.

Método GET (URL /:id): Retorna os dados completos de uma dosagem pelo ID.

Método POST (URL /save-material-selection-step/:id): Salva a etapa de seleção de materiais da dosagem.


	1.1.1.2 Submódulo: Composição Granulométrica

Método POST (URL: /step-3-data): Retorna os dados iniciais necessários para a etapa 3.

Método POST(URL: /calculate-granulometry): Calcula os dados de granulometria. 

Método POST(URL: /save-granulometry-composition-step/:userId): Salva os dados da etapa 3.




1.1.1.3 Submódulo: Ensaio com Ligante

Método POST (URL: /calculate-step-4-data): Calcula os dados da etapa 4 (binder trial).

Método POST (URL: /save-binder-trial-step/:userId): Salva os dados da etapa 4.



1.1.1.4 Submódulo:  Massa Específica e Densidade Máxima

Método POST (URL: /get-specific-mass-indexes): Retorna índices de massa específica para os agregados.

Método POST (URL: /calculate-step-5-dmt-data): Calcula dados de DMT (massa específica aparente da mistura).

Método POST (URL: /calculate-step-5-gmm-data): Calcula dados de GMM (massa específica máxima teórica).

Método POST (URL: /calculate-step-5-rice-test): Calcula o ensaio de Rice (densidade máxima da mistura).

Método POST (URL: /save-maximum-mixture-density-step/:userId): Salva os dados da densidade máxima da mistura.


1.1.1.5 Submódulo: Parâmetros Volumétricos

Método POST (URL: /set-step-6-volumetric-parameters): Define os parâmetros volumétricos da mistura.

Método POST (URL: /save-volumetric-parameters-step/:userId): Salva os parâmetros volumétricos.


	


1.1.1.6 Submódulo: Teor Ótimo de Ligante

Método POST (URL: /set-step-7-optimum-binder): Calcula o teor ótimo de ligante.

Método POST (URL: /step-7-plot-dosage-graph): Gera o gráfico de dosagem com ótimo teor.

Método POST (URL: /step-7-get-expected-parameters): Retorna os parâmetros esperados para o teor ótimo.

Método POST (URL: /save-optimum-binder-content-step/:userId): Salva os dados do teor ótimo de ligante.


1.1.1.7 Submódulo: Confirmação Final

Método POST (URL: /confirm-specific-gravity): Confirma a massa específica na etapa final.

Método POST (URL: /confirm-volumetric-parameters): Confirma os parâmetros volumétricos finais.

Método POST (URL: /save-confirmation-compression-data-step/:userId): Salva os dados de compressão final.


1.1.1.8 Submódulo: Finalização da Dosagem

Método POST (URL: /save-marshall-dosage/:userId): Salva a dosagem Marshall completa no banco de dados.

Método DELETE (URL: /:id): Exclui uma dosagem Marshall pelo ID.


1.1.2 Como o Controller se relaciona com o Frontend: Sabendo que o Controller vai controlar todas as requisições e chamar o Service, também temos, no backend: 

	

	Isso quer dizer que todos os endpoints começam com /asphalt/dosages/marshall, e dentro de cada um deles, existem outros, cada um responsável por uma etapa da dosagem.O backend se conecta com o front através de dois arquivos principais:

	marshallDosageService.ts: Contém apenas 4 operações simples, que são: 
	createMarshallDosage (data): 
	deleteMarshallDosage (id)
	getMarshallDosageByUserId (userId)
	getMarshallDosage (dosageId)

	No backend, essas dosagens correspondem aos seguintes passos:

	1.1 GET (/all/id): Lista todas as dosagens do usuário. Para fins de uma melhor visualização, segue as seguintes imagens das funções no Frontend e suas respectivas chamadas no Backend:

	No Frontend:


No Backend:

	

	1.2 GET (/:id): É utilizado quando o usuário abre uma dosagem já existente, seja para edição ou consulta.

	Frontend:

	

	Backend:

	

	1.3 DELETE (/:id): Chamado quando o usuário exclui uma dosagem.

	Frontend:

	

	Backend:

	

	
2. Marshall_Service: Este é o arquivo que controla o fluxo da dosagem do frontend. Dependendo de cada etapa, ele decide qual rota chamar no backend, podendo ser qualquer um dos endpoints citados anteriormente.