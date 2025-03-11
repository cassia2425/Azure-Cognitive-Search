# Azure-Cognitive-Search
Utilizando AI Search para indexação e consulta de Dados


Repositório criado para entregar o projeto final da etapa: Azure Cognitive Search - Utilizando AI Search para indexação e consulta de Dados.

Três **recursos** são importantes para esse experimento:

- Azure AI Search
- Azure AI services
- Storage account

# Requerimentos do projeto
### Como Entregar esse projeto?



# Link útil

1. https://aka.ms/ai900-ai-search

# Crie um arquivo readme.md descrevendo o passo a passo para se configurar uma pesquisa, assim como seus insights, possibilidades de ferramentas que se beneficiam com esse tipo de ferramenta e aprendizados adquiridos durante o processo.



### Passo a passo para se configurar uma pesquisa

* Criar um recurso Azure AI Search
* Criar um recurso Azure AI services
* Criar um Storage Account
* Adicionar documentos ao Azure Storage
* Indexar os documentos
* Consultar o índice
* Revisar o knowledge store

|

#### Crie um recuros Azure AI Search

1. Fique logado no [Portal Azure](https://portal.azure.com).
2. Clique em **+ Create a resource**, procure e crie um **Azure AI Search** com as configurações abaixo:

	* **Subscription**: Your Azure subscription.
	* **Resource group**: Select or create a resource group with a unique name.
	* **Service name**: A unique name.
	* **Location**: Choose any available region.
	* **Pricing tier**: Basic.



#### Criar um recurso Azure AI services

3. Retorne ao início do [Portal Azure](https://portal.azure.com) e clique novamente em **+ Create a resource**, procure e crie um **Azure AI services** com as configurações abaixo:

	* **Subscription**: Your Azure subscription.
	* **Resource group**: The same resource group as your Azure AI Search resource.
	* **Region**: The same location as your Azure AI Search resource.
	* **Name**: A unique name.
	* **Pricing tier**: Standard S0.
	* **By checking this box I acknowledge that I have read and understood all the terms below**: Selected.


#### Criar um Storage Account

4. Retorne ao início do [Portal Azure](https://portal.azure.com) e clique novamente em **+ Create a resource**, procure e crie um **storage account** com as configurações abaixo:

	* **Subscription**: Your Azure subscription.
	* **Resource group**: The same resource group as your Azure AI Search and Azure AI services resources.
	* **Storage account name**: A unique name.
	* **Location**: Choose any available location.
	* **Performance**: Standard.
	* **Redundancy**: Locally redundant storage (LRS).



5. Após criar um **storage account**, no menu lateral esquerdo, acesse as configurações do seu storage account e altere a opção **Allow Blob anonymous access** para **Enabled** e salve. ⚠️



#### Adicionar documentos ao Azure Storage

6. Dentro do seu recurso **Azure Storage Account**, no menu lateral esquerdo, acesse **Containers**, selecione **+ Container**. Vai abrir um menu lateral à direita. Preencha as informações como fornecido abaixo:

* **Name**: coffee-reviews.
* **Public access level**: Container (anonymous read access for containers and blobs).
* **Advanced**: no changes.



7. Em uma nova janela do seu navegador, acesse esse [link](https://aka.ms/mslearn-coffee-reviews) para fazer o download dos arquivos de exemplo.

8. Ainda na sessão de **Containers**, acesse o container recém criado **coffee-reviews** e clique em **Upload**. Uma janela à direita se abrirá e você deverá selecionar os arquivos baixados e descompactados do passo anterior no seu computador.



9. Após isso, clique em Upload, dessa janela, para adicionar os documentos com as opiniões dos clientes para o container.


#### Indexar os documentos

Depois de armazenar os documentos, você poderá usar o **Azure AI Search** para extrair insights dos documentos. O portal do Azure fornece um *Import data wizard*. Com esse wizard, você pode criar automaticamente um index (índice) e um indexer (indexador) para fontes de dados suportadas. Você usará o wizard para criar um index e importar seus documentos de opiniões do armazenamento para o index do **Azure AI Search**.

10. No [Portal Azure](https://portal.azure.com), navegue até o recurso criado **Azure AI Search** e selecione o botão **Import data**.



11. Na guia **Connect to your data**, observe a lista em drop-down **Data Source**, selecione **Azure Blob Storage**. Complete as informações como mostrado abaixo:

	* **Data Source**: Azure Blob Storage
	* **Data source name**: coffee-customer-data
	* **Data to extract**: Content and metadata
	* **Parsing mode**: Default
	* **Connection string**: Select Choose an existing connection. Select your storage account, select the coffee-reviews container, and then click Select.
	* **Managed identity authentication**: None
	* **Container name**: this setting is auto-populated after you choose an existing connection.
	* **Blob folder**: Leave this blank.
	* **Description**: Reviews for Fourth Coffee shops.



12. Clique em **Next: Add cognitive skills (Optional)**.



13. Na seção **Attach Cognitive Services**, selecione o seu recuros Azure AI services.



14. Na seção **Add enrichments**:

	* Change the Skillset name to **coffee-skillset**.
	* Select the checkbox **Enable OCR and merge all text into merged_content field**.
	* Ensure that the Source data field is set to **merged_content**.
	* Change the Enrichment granularity level to **Pages (5000 character chunks)**.
	* Don’t select *Enable incremental enrichment*.
	* Select the following enriched fields: ⚠️ **(ver imagem 14.2)**.


15. Na seção **Save enrichments to a knowledge store**, selecione:

	* Image projections
		* ⚠️ Uma mensagem de alerta aparecerá em vermelho (ver imagem 15.2) perguntando sobre *Storage Account Connection String*, siga esses passos:
			1. Select **Choose an existing connection**. 
			2. Choose the storage account you created earlier.
			3. Click on **+ Container** to create a new container called knowledge-store with the privacy level set to Private, and select Create.
			4. Select the knowledge-store container, and then click Select at the bottom of the screen.
	* Documents
	* Pages
		* Key phrases
		* Entities
	* Image details
		* Image references


16. Ainda na seção **Save enrichments to a knowledge store**, selecione **Azure blob projections: Document**. Aparecerá o container recém criado knowledge-store, não altere esse nome mostrado.



17. Observe todas as configurações realizadas e selecione **Next: Customize target index**.



18. Na etapa seguinte, altere o **Index name** para **coffee-index**.

19. Garanta que o campo **Key** esteja configurado para **metadata_storage_path**. Deixe o **Suggester name** em branco e **Search mode** autopreenchido.

20. Observe com atenção as informações preenchidas das etapas 18 e 19 até aqui.



21. Selecione o checkbox **filterable** para todos os campos que já estão selecionados por padrão. Observe a imagem.



22. Clique em **Next: Create an indexer**.

23. Altere o *Indexer name* para **coffee-indexer**. Deixe o *Schedule* configurado para **Once**. Verifique se o **Base-64 Encode Keys** está selecionado em **Advanced options**. Clique em **Submit**.



24. Retorne à página do seu recurso **Azure AI Search**. No menu lateral esquerdo, abaixo de **Search Management**, selecione **Indexers**. Selecione o recém-criado **coffee-indexer**, aguarde até visualizar o **Status** indicar success ✅. Acesse o indexer **coffee-indexer** para visualizar os detalhes.



#### Consultar o índice

Use o **Search Explorer** para escrever e testar consultas. Você pode usar o **Search Explorer** para escrever consultas e revisar resultados em **JSON**.

25. Na página de Overview do recurso **Search service** que você criou, selecione **Search explorer** nas guias superiores.



26. Observando que o **coffee-index** é o index selecionado, altere a visualização de query view para **JSON view**, copie e cole o trecho de código abaixo e clique em **Search**.

```json
{
    "search": "*",
    "count": true
}
```

A consulta retorna todos os documentos, incluindo uma contagem de todos os documentos no campo **@odata.count**. Essa consulta está descrita no formato JSON.



27. Filtre por localidade. No campo **JSON query editor**, copie e cole o trecho de código abaixo e clique em **Search**.

```json
{
 "search": "locations:'Los Angeles'",
 "count": true
}
```

A consulta retorna filtros de review baseado na localidade **Los Angeles**. Observe o número 2 no campo **@odata.count**.


28. Agora filtre por **sentimento**. No campo **JSON query editor**, copie e cole o trecho de código abaixo e clique em **Search**.

```json
{
 "search": "sentiment:'negative'",
 "count": true
}
```

A consulta retorna filtros de view com **sentimento negativo**. Observe o número 1 no campo **@odata.count**.

Veja como os resultados são classificados por **@search.score**. Essa é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão próximos os resultados correspondem à consulta fornecida.

Observe os comentários negativos em **content**. Esses comentários estão associados aos **key phrases**. Isso ajuda a entender a causa dos reviews negativos.



#### Revisar o knowledge store

Quando você executou o *Import data wizard*, você criou um **knowledge store**. Dentro dele, você vai encontrar os dados enriquecidos extraídos pelas habilidades da IA que persistem na forma de projeções e tabelas.

29. Na home do [Portal Azure](https://portal.azure.com), navegue de volta ao seu **Azure storage account**. No menu lateral esquerdo, selecione **Containers**, Clique no container **knowledge-store**.


30. Selecione qualquer pasta, depois clique no arquivo **objectprojection.json**. Selecione **Edit** e observe o JSON gerado para um dos documentos de review dos clientes direto do seu **Azure data store**.



31. Retorne ao seu **Storage account Containers** por meio do *breadcrumb* na guia superior esquerda da tela.



32. Selecione o container **coffee-skillset-image-projection**. Clique em qualquer arquivo e depois selecione qualquer arquivo .jpg. Clique em **Edit** para visualizar uma imagem armazenada no documento fornecido no **Azure data store**.



33. Retorne novamente ao seu **Storage account Containers**. Selecione **Storage browser** no menu lateral esquerdo, depois selecione **Tables**.

Há uma tabela para cada entidade no index. 



34. Selecione a tabela **coffeeSkillsetKeyPhrases**.

Observe os **key phrases** que o **knowledge store** conseguiu capturar do conteúdo das reviews. Muitos dos campos são keys, portanto você pode vincular as tabelas como um banco de dados relacional. A última coluna mostra os **key phrases** que foram extraídos pelo skillset.



### Insights


Agora, é possível utilizar o **Azure Cognitive Search** para indexar e pesquisar dados relacionados ao comportamento e saúde do cachorro. Por exemplo, posso armazenar dados de atividade, como tempo de sono, tempo de brincadeira e padrões alimentares, e usar o **Azure Cognitive Search** para pesquisar esses dados e obter insights sobre o bem-estar do cachorro.

### Possibilidades de ferramentas que se beneficiam com esse tipo de ferramenta

Você pode usar câmeras IP ou outras câmeras de segurança conectadas à internet para fornecer uma visão ao vivo do que está acontecendo em casa enquanto você está fora.

Integre recursos de reconhecimento de imagem e vídeo ao seu sistema para identificar automaticamente seu cachorro e outras pessoas ou objetos em casa. Isso pode ajudar a me alertar se algo incomum acontecer ou se o meu cachorro precisar de atenção específica.

Outra possibilidade é permitir que veterinários possam entender problemas do animal com objetivos personalizados de tratamento. Nesse caso, as clínicas se beneficiam de uma ferramenta específica para esse segmento.

### Aprendizados adquiridos

O passo a passo do guia para utilizar os três recursos me ajudou a entender o poder da tecnologia do **AI Search**. Os modelos de exemplo para analisar sentimento é simples e muito claro para entendermos e tirarmos ideias importantes para usar no dia a dia.


