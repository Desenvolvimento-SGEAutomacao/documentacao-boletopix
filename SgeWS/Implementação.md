# Implantação do BoletoPix / Bolecode / Boleto híbrido

Funcionalidade do SgeWS responsável por registrar o boleto com QrCode

### Bancos integrados com o Sge

- Itau
- Santander

## Registro dos boletos
### Fluxo

- Ao imprimir um boleto, o Sge grava um arquivo Xml na pasta SgeWS/Boletos/Envio
- Esse arquivo Xml contém os dados do título e do banco de dados
- Depois de gerado esse arquivo, o Sge chama o SgeWs
- O SgeWS consome esse arquivo Xml para iniciar o processo
- Ao final do processo, o SgeWS grava um arquivo Xml e a imagem do QrCode na pasta SgeWS/Boletos/Retorno
- Esse arquivo Xml de retorno contém o caminho da imagem do QrCode caso a requisição tenha sido completada com sucesso
- Caso a requisição tenha dado erro, no lugar do caminho da imagem do QrCode é gravado o motivo do erro
- Ao final do processo a imagem do QrCode é exibida no relatório caso tenha sido sucesso
- Ou uma mensagem de erro é exibida em um MessageBox em caso de erro na requisição
- Caso ocorra algum erro, o relatório **NÃO** é carregado, pois o boleto não foi registrado
- Nesses casos em que ocorrer algum erro, entre em contato com o suporte da SGE

## Instruções de cobrança dos boletos
### Instruções suportadas para envio

- Data de vencimento
- Valor
- Baixa

### Fluxo

- Dado um boleto já registrado, ao fazer alteração de valor ou data de vencimento será enviado a instrução de cobrança para o respectivo banco
- Para ocorrer o envio o relatório deve ser impresso novamente(exibido na tela)
- Caso o envio seja feito com sucesso o boleto será exibido na tela com o QrCode
- Caso ocorra algum erro, o relatório **NÃO** é carregado, pois o boleto não foi alterado
- Nesses casos em que ocorrer algum erro, entre em contato com o suporte da SGE
- O envio de exclusões(baixas) se dará por um serviço que ficará sendo executado de forma automática.
- Para mais detalhes do envio de baixas consulte : [SgeBaixaBoleto](../SgeBaixaBoleto/SgeBaixaBoleto.md)

## Pré Requisitos no Sge

### 1 - Versão do Sge

- **Necessário Update 3277 (Mandatório)**

### 2 - Relatórios de boleto

- Atualizar os 3 relatórios de boletos
- Extrair na pasta relatórios informada nas configurações do sistema
- Relatórios atualizados disponíveis em : [Relatórios](Arquivos/Relatorios.rar)

### 3 - Executável do SgeWS

- Para o suporte SGE: Disponivel no FTP em Atualizações Sge -> SgeIB Versão 1.30Z -> SgeIBFull -> SgeWS
- Para os clientes SGE: Disponivel em: [SgeWS](Arquivos/SgeWS230824.rar)
- Extrair o executável no mesmo diretório em que se encontra o Sge

### 4 - Cadastro de conta corrente

- A partir do dia 23/08/2024, apenas o campo de chave pix (se solicitado pelo banco) e a flag **Pix no boleto** serão necessárias para o funcionamento
- As credenciais serão armazenadas por nós, da SGE de forma a gerenciar melhor as credenciais dos clientes

![alt text](Imagens/ContaCorrente.png)

### Client Id e Secret Id 

- São chaves referente a conta corrente do cliente, necessários para registrar os boletos nas APIs do bancos, esses dados são fornecidos pelo banco do cliente
- Informá-las para a SGE para o correto funcionamento do serviço

### Caminho do certificado digital e senha

- Certificado digital usado para emissão de boleto
- Armazenar o certificado em uma pasta na rede acessível para todos os usuários do Sge
- NÃO é necesário instalar o certificado, apenas armazenar em uma pasta acessível na rede
- Informá-las para a SGE para o correto funcionamento do serviço

### Chave pix

- Alguns bancos obrigam a informação da chave pix, nesse caso deve-se informar qual a chave pix
- Santander é obrigatório
- Itau não é obrigatorio, caso não informado, será usado o CNPJ da empresa favorecida atrelado a conta corrente como chave pix
- Informar no campo da tela do cadastro de conta corrente