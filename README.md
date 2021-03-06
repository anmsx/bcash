# Bcash

Integração com Bcash(antigo Pagamento Digital)

## Como usar

# Criando um item

	items = [] 
	items << Bcash::Item.new({id: 1, description: 'teste', amount: 2, price: 30.0})

Você pode criar quando itens for necessários para enviar para o Bcash

Você pode validar se o item criado é valido

	item = Bcash::Item.new({id: 1, description: 'teste', amount: 2, price: 30.0})
	item.valid?

Valide o item, igual ao Rails.

# Criando um pacote

Com os itens criados, crie um pacote para o envio

	package = Package.create(items)

No pacote você pode alterar o valor do frete e o tipo de integração do Bcash
	
	Package.create(items, 30.0, BCASH::PAD)

# Gerando pagamento

Crie um pagamento

	payment = Bcash::Payment.new(package, {:email_loja => 'test@test.com'}) 

Podemos colocar nas opções uma url de retorno para o Bcash voltar ao site desejado.

	Bcash::Payment.new(package, {:email_loja => 'test@test.com', :url_retorno => 'http://www.teste.com.br'})

Você pode encontrar todas as opções no [site do bcash](https://www.bcash.com.br/desenvolvedores/integracao-loja-online.html) e procure por campos opcionais

# Gerando formulário

	payment.html

Você terá o formulário pronto para envio, você também pode alterar o input do de envio para a maneira que desejar

	payment.html{tag_content('button', 'Pagar!')}

# Retorno automático

Se você utilizou a opção de url de retorno, você pode capturar o POST enviado pelo Bcash

	notification = Bcash::Notification(params)
	notification.id_transacao

Você pode visualizar todos os parametros na [página de desenvolvimento do bcash](https://www.bcash.com.br/desenvolvedores/integracao-retorno-automatico-loja-online.html).

## Verificar transação

Você pode visualizar as transações, consultado o status do seu pedido, para isso obtenha a chave de acesso

	Logue em sua conta no site do bcash -> Ferramentas -> Sua chave acesso

Com a chave de acesso podemos utilizar a classe transação de duas maneiras

	transaction = Bcash::Transaction.new(email, token, id_transacao, id_pedido)
	transaction.get

Ou

	transaction = Bcash::Transaction.new
	transaction.email = 'teste@teste.com.br'
	transaction.token = '1234567890'
	transaction.id_transaction = '123'
	transaction.id_order = '1234'
	transaction.get

Agora você tem todos os parametros para consulta

	transaction.id_transacao
	transaction.status

Você pode ver todos HTTP code e parametros de retorno no [manual de transação](https://www.bcash.com.br/site/manual/Bcash_Manual_Integracao_Consultar_Dados_Transacao.pdf).

## Instalação

Adicione ao seu Gemfile:

    gem 'bcash'

E execute:

    $ bundle

## Contribuindo

1. Fork 
2. Cria seu branch (`git checkout -b my-new-feature`)
3. Commit suas mudanças (`git commit -am 'Add some feature'`)
4. Envie o branch (`git push origin my-new-feature`)
5. Rode os testes
6. Cria um novo Pull Request
