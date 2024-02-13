# cognitive-search-in-azure

O CONTEÍUDO DESTE REPOSITORIO DESCREVE O PASSO A PASSO PARA REALIZAR UMA PESQUISA COGNITIVA NO AZURE:

Criar um recurso de pesquisa de IA do Azure

    Inicie sessão no portal do Azure.

    Clique no botão + Criar um recurso, procure a Pesquisa de IA do Azure e crie um recurso de Pesquisa de IA do Azure com as seguintes configurações:
        Assinatura: Sua assinatura do Azure.
        Grupo de recursos: Selecione ou crie um grupo de recursos com um nome exclusivo.
        Nome do serviço: um nome único.
        Localização : Escolha qualquer região disponível.
        Nível de preços: Básico

    Selecione Revisão + criar e depois de ver a resposta Validação de Sucesso, selecione Criar.

    Após a implantação ser concluída, selecione Ir para recurso. Na página de visão geral da Pesquisa de IA do Azure, você pode adicionar índices, dados de importação e índices criados por pesquisa.

Criar um recurso de serviços de IA do Azure

Você precisará fornecer um recurso de serviços de IA do Azure que está no mesmo local que seu recurso de Pesquisa de IA do Azure. Sua solução de pesquisa usará esse recurso para enriquecer os dados no datastore com insights gerados por IA.

    Voltar à página inicial do portal do Azure. Clique no+Criar um recursobotão e busca porServiços de IA do Azure( , . e Selecione aCriare aServiços de IA do Azure- Plano. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:
        Assinatura: Sua assinatura do Azure.
        Grupo de recursos: o mesmo grupo de recursos do seu recurso de Pesquisa de IA do Azure.
        Região: O mesmo local do seu recurso de Pesquisa de IA do Azure.
        Nome : Um nome único.
        Nível de preços : Standard S0
        Ao marcar esta caixa reconheço que li e entendi todos os termos abaixo : Selecionado

    Selecione Revisão + criar. Depois de ver a resposta Validação Passada, selecione Criar.
    Aguarde a conclusão da implantação e visualize os detalhes da implantação.

Criar uma conta de armazenamento

    Retorne à página inicial do portal do Azure e, em seguida, selecione o botão + Criar um recurso.
    Pesquisar porconta de armazenamento, e criar aConta de armazenamentorecurso com as seguintes configurações:
        Assinatura: Sua assinatura do Azure.
        Grupo de recursos: o mesmo grupo de recursos da sua Pesquisa de IA do Azure e dos recursos de serviços de IA do Azure.
        Nome da conta de armazenamento: um nome único.
        Localização: Escolha qualquer local disponível.
        Desempenho : Padrão
        Redundância : Armazenamento redundante local (LRS)

    Clique em Revisar e, em seguida, clique em Criar. Aguarde a conclusão da implantação e, em seguida, vá para o recurso implantado.
    Na conta Armazenamento do Azure que você criou, no painel de menus à esquerda, selecione Configuração (em Configurações).
    Altere a configuração para permitir acesso anônimo de Blob a Habilitado e selecione Salvar.

Carregar documentos para o armazenamento do Azure

    No painel de menus à esquerda, selecione Containers.

    Screenshot that shows the storage blob overview page.

    Selecione + Container. Um painel no lado direito abre.
    Insira as seguintes configurações e clique emCriar:
        Nome : coffee-reviews
        Nível de acesso público: Container (acesso de leitura anônimo para contêineres e blobs)
        Avançado : sem alterações.

    Em uma nova aba do navegador, baixe ocomentários sobre café zippedA partir dehttps://aka.ms/mslearn-coffee-reviews, e extrair os arquivos para oComentários sobre aPast da pasta.

    No portal do Azure, selecione o recipiente de resenhas de café. No contêiner, selecione Upload.

    Screenshot that shows the storage container.

    No painel Carregar blob, selecione Selecionar um arquivo.

    Na janela Explorador, selecione todos os arquivos na pasta de comentários, selecione Abrir e, em seguida, selecione Carregar.

    Screenshot that shows the files uploaded to the Azure container.
    Depois que o upload for concluído, você pode fechar o painel Upload blob. Seus documentos estão agora em seu recipiente de armazenamento de resenhas de café.

Indexação dos documentos

Depois de armazenar os documentos, você pode usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um assistente de importação de dados. Com este assistente, você pode criar automaticamente um índice e um indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice de pesquisa IA do Azure.

    No portal do Azure, navegue até o recurso de pesquisa de IA do Azure. Na página Visão geral, selecione Importar dados.

    Screenshot that shows the import data wizard.
    Sobre oConecte-se aos seus dadosPágina, naFonte de dadoslista, selecioneArmazenamento de Blobs do Azure- A . (í a , , , , , í , Preencha os detalhes do armazenamento de dados com os seguintes valores:
        Fonte de dados : Armazenamento de Blobs do Azure
        Nome da fonte de dados : café-clientista-dados
        Dados para extrair : Conteúdo e metadados
        Modo de análise : Padrão
        Corda de ligação : ?Selecionar Escolher uma ligação existente. Selecione sua conta de armazenamento, selecione o recipiente de revisão de café e clique em Selecionar.
        Autenticação de identidade gerenciada: Nenhum
        Nome do recipiente: esta definição é auto-preenchida depois de escolher uma ligação existente.
        Pastas de blocos: Deixe este espaço em branco.
        : Avaliações para Fourth Coffee shops.

    Selecione Próximo: Adicione habilidades cognitivas (opcional).

    Na seção Serviços Cognitivos Acessar, selecione o recurso de serviços de IA do Azure.
    NoAdicionam enriquecimentosSeção:
        Mude o nome Skillset para coffee-skillset.
        Selecione a caixa de seleçãoHabilite OCR e mescle todo o texto no campo mesclado_content( , . e

            Nota É importante selecionar Ativar OCR para ver todas as opções de campo enriquecidas.

        Certifique-se de que o campo de dados de origem esteja definido para mesclar_content.
        Altere o nível de granularidade do enriquecimento para Páginas (5000 blocos de caracteres).
        Não selecione Ativar enriquecimento incremental

        Selecione os seguintes campos enriquecidos:
        Habilidade Cognitiva 	Parâmetro de medição 	Nome do campo
        Extrair nomes de localização 	  	Localizações
        Extrair frases-chave 	  	frases-chave
        Detecte o sentimento 	  	O sentimento
        Gerar tags a partir de imagens 	  	imageTags (tags)
        Gerar legendas a partir de imagens 	  	Capção de imagem
    Under (s)Salve enriquecimentos em uma loja de conhecimento, seleccionar:
        Projeções de imagem
        Documentos de um
        Páginas
        Frases de chave
        Entidades
        Detalhes da imagem
        Referências de imagem

        Nota Se um aviso solicitando uma corda de conexão de conta de armazenamento aparecer.

        Screenshot that shows the Storage account connection screen warning with 'Choose an existing connection' selected.
            Selecione Escolher uma conexão existente. Escolha a conta de armazenamento que você criou anteriormente.
            Clique em + Container para criar um novo contêiner chamado knowledge-store com o nível de privacidade definido como Privado e selecione Criar.
            Selecione o contêiner do conhecimento-loja e clique em Selecionar na parte inferior da tela.

    Selecione as projeções do Azure blob: Documento. Uma configuração para o nome do contêiner com o contêiner do conhecimento-loinibe exibe popula-ulado automaticamente. Não altere o nome do container.

    Selecione Avançar: Personalizar o índice de destino. Altere o nome do índice para o índice de café.

    Certifique-se de que a chave está definida como metadata_storage_path. Deixe o Sugerir nome em branco e modo de pesquisa autopopulado.

    Revise as configurações padrão dos campos de índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

    Screenshot that shows the customize index pane with the index name entered and 'Filterable' selected for a default index field.

    Selecione Avançar: Criar um indexador.

    Altere o nome do Indexador para cafeicultor.

    Deixe o calendário definido para uma vez.

    Expanda as opções avançadas. Certifique-se de que a opção Base-64 Encode Keys esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.
    Selecione aSubmeterpara criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
        Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
        Executa o conjunto de habilidades das habilidades cognitivas para gerar campos mais enriquecidos.
        Mapeia os campos extraídos para o índice.

    Retorne à sua página de recursos de Pesquisa de IA do Azure. No painel esquerdo, em Gerenciamento de Pesquisa, selecione Indexadores. Selecione o recém-criador de café. Aguarde um minuto e seleccione o &orarr; Atualize até que o Status indique o sucesso.

    Selecione o nome do indexador para ver mais detalhes.

    Screenshot that shows the coffee-indexer Indexer successfully created.

Consultar o índice

Use o Explorador de Pesquisa para escrever e testar consultas. O Search Explorer é uma ferramenta incorporada no portal do Azure que lhe dá uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search Explorer para escrever consultas e revisar os resultados em JSON.

    Na página Visão geral do seu serviço de pesquisa, selecione Pesquisar explorador na parte superior da tela.

    Screenshot of how to find Search explorer.

    Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a exibição para a visualização JSON.

    Screenshot of the Search explorer.

No campo do editor de consulta JSON, copiar e colar:
Código do código

{
    "search": "*",
    "count": true
}

    Selecione a pesquisa. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo ?odata.count. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.
    Agora vamos filtrar por localização. NoEditor de consulta JSONcopiar e colar no campo:
    Código do código

{
 "search": "locations:'Chicago'",
 "count": true
}

Selecione aPesquisar- A . (í a questão: es. , , , í , , . A consulta pesquisa todos os documentos no índice e filtros para comentários com um local de Chicago. Você deveria ver3No@odata.countCampo.
Agora vamos filtrar pelo sentimento. NoEditor de consulta JSONcopiar e colar no campo:
Código do código

    {
     "search": "sentiment:'negative'",
     "count": true
    }

    Selecione aPesquisar- A . (í a questão: es. , , , í , , . A consulta pesquisa todos os documentos no índice e filtra comentários com um sentimento negativo. Você deveria ver1No@odata.countCampo.

        NotaVeja como os resultados são classificados por@search.score- A . (í a questão: es. , , , í , , . Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão de perto os resultados correspondem à consulta dada.

    Um dos problemas que podemos resolver é porque pode haver certas revisões. Vamos dar uma olhada nas frases-chave associadas à revisão negativa. O que você acha que pode ser a causa da revisão?

Revise a loja do conhecimento

Vejamos o poder da loja de conhecimento em ação. Quando você executou o assistente de dados Importar, você também criou uma loja de conhecimento. Dentro da loja de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA persistirem na forma de projeções e tabelas.

    No portal do Azure, navegue de volta para a sua conta de armazenamento do Azure.

    No painel de menus à esquerda, selecione Containers. Selecione o recipiente da loja do conhecimento.

    Screenshot of the knowledge-store container.

    Selecione qualquer um dos itens e, em seguida, clique no arquivo objectprojection.json.

    Screenshot of the objectprojection.json.

    Selecione Editar para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

    Screenshot of how to find the edit button.

    Selecione o armazenamento blob breadcrumb na parte superior esquerda da tela para retornar aos contêineres da conta de armazenamento.

    Screenshot of the storage blob breadcrumb.

    Nos Contentores, selecione o recipiente de café-habilidade-imagem-projeção. Selecione qualquer um dos itens.

    Screenshot of the skillset container.

    Selecione qualquer um dos arquivos .jpg. Selecione Editar para ver a imagem armazenada no documento. Observe como todas as imagens dos documentos são armazenadas dessa maneira.

    Screenshot of the saved image.

    Selecione o armazenamento blob breadcrumb na parte superior esquerda da tela para retornar aos contêineres da conta de armazenamento.

    Selecione o navegador Armazenamento no painel esquerdo e selecione Tabelas. Há uma tabela para cada entidade no índice. Selecione a mesa caféSkillsetKeyPhrases.

    Veja as frases-chave que a loja de conhecimento foi capaz de capturar a partir do conteúdo nas resenhas. Muitos dos campos são teclas, então você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.
